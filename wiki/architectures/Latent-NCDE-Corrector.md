---
title: Latent NCDE Corrector
tags: [latent-ncde, probabilistic-corrector, encoder-decoder, student-t, architecture-pattern, ctpc]
sources: [raw/papers/my-papers/KDD_submission.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: critical
hard_constraint_possible: partial
---

# Latent NCDE Corrector

## One-Line Intuition

An encoder-decoder architecture pattern for **continuous-time probabilistic correction** of a deterministic forecaster: NCDE encoder maps past observed errors to a latent posterior; NCDE decoder driven by the deterministic predictor's forecasts maps samples from the latent to time-varying Student-t parameters. Trained with a strictly-proper loss (NLL + CRPS + KL) so calibration consistency follows from the training objective.

## Why This Was Invented

For long-horizon open-loop forecasting (no observations during the forecast regime), error accumulates without bounds and uncertainty estimation matters as much as accuracy. Existing options for time-series correctors fall short:

- **Discrete-time** (ARIMA-MLP/LSTM, HopCast): can't handle irregular sampling
- **Latent ODE**: continuous-time but uncontrolled dynamics; severely overconfident at long horizons
- **Deterministic NCDE correctors**: continuous-time + irregular sampling but no uncertainty
- **Latent SDE**: probabilistic but no Predictor-Corrector decomposition; can't wrap around an existing physics simulator

The Latent NCDE Corrector pattern unifies these requirements: continuous-time, irregular-sampling-friendly, full-covariance probabilistic, with an encoder-decoder structure that supports Predictor-Corrector decomposition. Introduced in [[CTPC-KDD-Submission]] for spacecraft trajectory forecasting; the pattern generalizes to any deterministic forecaster + irregularly-sampled error history setting.

## The Math

The architecture in four stages:

### Stage 1 — NCDE encoder over observed errors

Past errors `e(t_0:T')` of the deterministic Predictor are spline-interpolated into a control path `X_e(t) ∈ ℝ^{d+1}` (state + time). The encoder evolves a hidden state via [[Neural-Controlled-Differential-Equation]]:

```
z_e(t) = z_e(t_0) + ∫ f_θ_e(z_e(s)) dX_e(s)        for t ∈ (t_0, t_T']
```

`f_θ_e : ℝ^{h_e} → ℝ^{h_e × (d+1)}` is an MLP. Hidden states across time are aggregated via a learned weight network `f_θ_w`:

```
z_f = ∫ w(t) ⊙ z_e(t) dt  /  ∫ w(t) dt
```

A linear projection `W_{h_e→L}` maps `z_f` to the parameters of a `L`-dimensional diagonal Gaussian latent posterior:

```
(μ_L, Σ_L) ← W_{h_e→L} z_f
```

### Stage 2 — Latent sampling

Draw K samples from the latent posterior:

```
s_L^(k) ∼ N(μ_L, Σ_L)        for k = 1, ..., K
z_d^(k)(t_T'+1) = W_{L→h_d} s_L^(k)
```

Each sample becomes the initial hidden state of a separate decoder trajectory.

### Stage 3 — NCDE decoder over Predictor's forecasts

Driven by the deterministic Predictor's forecasts `x̂(t_T'+1:T)` as control path `X_d(t)`:

```
z_d^(k)(t) = z_d^(k)(t_T'+1) + ∫ f_θ_d(z_d^(k)(s)) dX_d(s)    for t ∈ (t_T'+1, t_T]
```

K decoder trajectories → ensemble-like effect through a single decoder.

### Stage 4 — Probabilistic prediction head

At each forecast time `t`, a probabilistic MLP `f_θ_p` maps the decoder hidden state to Student-t parameters:

```
(μ_t^(k), L_t^(k)) = f_θ_p(z_d^(k)(t))
Σ_t^(k) = L_t^(k) (L_t^(k))^T               (Cholesky parameterization)
ê(t) | z_d^(k)(t) ∼ T_ν(μ_t^(k), Σ_t^(k))
```

`L_t` is lower-triangular with strictly positive diagonal (Softplus on diag entries). `ν > 0` learned as `ν = ν_min + softplus(ν_raw)` with `ν_min = 4.5` (avoids degenerate heavy tails; `ν > 4` ensures finite kurtosis).

### Loss (strictly proper combination)

```
L_total = L_NLL + L_CRPS + L_KL
```

- **NLL** — Student-t negative log-likelihood (Eq. 7-8 of source paper)
- **CRPS** — closed-form for samples (Eq. 10 of source paper)
- **KL** — between latent posterior `N(μ_L, Σ_L)` and prior `N(0, I)` (Eq. 11 of source paper)

Each is strictly proper; their sum preserves strict properness → calibration consistency at population minimum.

### Inference: moment aggregation

Collapse the K-sample mixture via law of total variance:

```
μ̄_t = (1/K) Σ_k μ_t^(k)
Σ̄_t = (1/K) Σ_k Σ_t^(k)                         within-sample (aleatoric)
     + (1/K) Σ_k (μ_t^(k) − μ̄_t)(μ_t^(k) − μ̄_t)^T   between-sample (latent-induced)
```

The between-sample term is critical for calibration at long horizons (Table 3 of source paper).

### Code Correspondence (PyTorch / JAX skeleton)

```python
class LatentNCDECorrector(nn.Module):
    def __init__(self, d_state, h_e, h_d, L, hidden=128):
        super().__init__()
        self.encoder = NCDE(in_ch=d_state+1, h=h_e, hidden=hidden)
        self.weight_net = MLP(h_e, hidden, h_e)
        self.latent_proj = Linear(h_e, 2 * L)              # outputs (μ_L, log σ_L²)
        self.latent_to_decoder = Linear(L, h_d)
        self.decoder = NCDE(in_ch=d_state+1, h=h_d, hidden=hidden)
        self.pred_head = ProbMLP(h_d, hidden, d_state)     # outputs (μ_t, L_t entries, ν_raw)

    def forward(self, t_obs, e_obs, t_fc, x_hat_fc, K=8):
        X_e = cubic_spline(t_obs, e_obs)
        z_e = self.encoder(z0=zeros, control=X_e, t=t_obs)
        w = self.weight_net(z_e).softmax(dim=0)
        z_f = (w * z_e).sum(dim=0) / w.sum(dim=0)
        mu_L, sigma_L = self.latent_proj(z_f).chunk(2, dim=-1)

        outputs = []
        for k in range(K):
            s_L = mu_L + sigma_L * torch.randn_like(sigma_L)
            z_d_init = self.latent_to_decoder(s_L)
            X_d = cubic_spline(t_fc, x_hat_fc)
            z_d = self.decoder(z0=z_d_init, control=X_d, t=t_fc)
            mu_t, L_t, nu = self.pred_head(z_d)            # parameters at each forecast time
            outputs.append((mu_t, L_t))

        return outputs                                      # K trajectories of (μ_t, L_t)

    def aggregate(self, outputs):
        mu_bar = mean over K of mu_t
        Sigma_within = mean over K of L_t @ L_t.T
        Sigma_between = mean over K of outer(mu_t - mu_bar)
        return mu_bar, Sigma_within + Sigma_between
```

Reference implementation: JAX + Diffrax in the source paper (`raw/papers/my-papers/KDD_submission.pdf`, Appendix I).

## Design Parameters

| Knob | Typical value | Trade-off |
|---|---|---|
| Latent dim `L` | 8–32 | More dims → more expressive posterior; harder to regularize |
| Encoder hidden `h_e` | 64–128 | Standard NCDE sizing |
| Decoder hidden `h_d` | 64–128 | Same |
| K (latent samples) | 8–32 at inference | More samples → better latent-induced variance estimate; linear cost |
| `ν_min` | 4.5 | Floor avoiding degenerate heavy tails; `ν > 4` ensures finite kurtosis |
| Vector field activation | Softplus or Tanh | Standard NCDE choice; second-derivative non-zero matters |
| Spline interpolation | Cubic (natural) | Source paper uses cubic; linear works but loses smoothness |
| Loss weighting | `λ_NLL = λ_CRPS = λ_KL = 1` | Source paper uses unit weights; tuning may help in different regimes |

## Toy Example

Smallest realistic instance: 1D position, observed error history at irregular times, deterministic forecast of position. Encoder takes 4-channel control path (time + 3D position-error placeholder + missingness, or just `(t, e)` for 1D). Decoder takes deterministic forecast as control. Output: `(μ_t, σ_t)` over forecast horizon.

For the actual spacecraft case (CTPC-CDE++ in the source paper):

- `d_state = 3` (RTN position error or ECI position error)
- Control paths: 4-channel (time + 3D state)
- `L = 16`, `h_e = h_d = 128`, K = 8 at inference
- 64% MSE reduction at 4-day horizon vs GMAT alone, with `d̄² ≈ 1` calibration

## Connection to SiS / CTPC

**This is the architectural backbone of CTPC.** The Predictor-Corrector decomposition is realized concretely as:

- **Predictor** = any deterministic forecaster (GMAT for orbital; could be PeRCNN for PDEs, etc.). Frozen.
- **Corrector** = Latent NCDE Corrector. Learned. Probabilistic. Continuous-time.

Compared to the existing physics-as-architecture options in the wiki:

- [[Hamiltonian-Neural-Network]] / [[Lagrangian-Neural-Network]] — physics-as-arch *for the dynamics*. The Latent NCDE Corrector is physics-as-arch *for the residual dynamics* — orthogonal axis.
- [[PeRCNN]] — physics-as-arch for PDE *fields*. Latent NCDE Corrector is for *time-evolving error states*.
- [[PhyArch]] — physics-as-arch via *symmetry hardwiring*. Currently the Latent NCDE Corrector is a *generic* time-series model with no physics priors inside the decoder vector field. The natural composition: replace the decoder's `f_θ_d` with a PhyArch-style symmetry-hardwired vector field. See [[CTPC-Design-Rationale]] § "Decisions Deferred to PhyArch-CTPC".

For the SiS dissipation constraint `Ḣ ≤ 0`: the Latent NCDE Corrector *can* produce dissipative residuals (it has no constraint preventing them), but doesn't *guarantee* dissipation. PHNN-style hardwiring of dissipation in the decoder is an open extension.

## Connections

- [[CTPC-KDD-Submission]] — the introducing paper
- [[Neural-Controlled-Differential-Equation]] — foundational concept; the encoder and decoder both use NCDEs
- [[CTPC-Design-Rationale]] — design decisions justified + open architectural questions
- [[PhyArch]] — orthogonal sibling that hardwires symmetries; PhyArch-Latent-NCDE-Corrector is the open composition
- [[Hamiltonian-Neural-Network]] / [[Lagrangian-Neural-Network]] — alternative correctors targeting conservative dynamics rather than residuals
- [[PeRCNN]] — Predictor-Corrector sibling for PDEs

## Open Questions

- **Symmetry-hardwired decoder vector field.** Replace the generic NCDE `f_θ_d` with [[PhyArch]]-style parity-split + manipulator-skeleton structure. Open architectural details: how to compose NCDE control paths with parity-typed features, training stability with second-order autograd through the resulting Hessian-inversion-style assembly.
- **CBM concept bottleneck.** Insert a [[Concept-Bottleneck-Models|CBM]] layer between the decoder hidden state `z_d(t)` and the prediction head, with named orbital-physics concepts (J2, drag, SRP, third-body, RTN components). Concrete design: [[CBM-CTPC-Composition]]. **Year 1 milestone of the PhyArch-CTPC roadmap** ([[CTPC-Design-Rationale]] Part III); promotes concept-closure invariance from "asserted" to "verifiable."
- **Analytic covariance propagation.** Replace the K-sample Monte Carlo aggregation (Algorithm 1, lines 8–15 of [[CTPC-KDD-Submission]]) with analytic propagation per [[Analytic-Covariance-Propagation|Wright et al. AISTATS 2024]]. Eliminates MC sampling noise; enables propagation of TLE input uncertainty through the encoder. Concrete design: [[Analytic-Sigma-CTPC-Composition]]. **Year 2 milestone of the PhyArch-CTPC roadmap**; promotes information invariance from "partial (mean only)" to "verified for full distribution." Open contribution: deriving the analytic-Σ-NCDE theorem (extending Wright's MLP result to RK4-integrated NCDEs) is potentially a publishable methods paper in its own right.
- **Variance-head equivariance.** Currently `L_t` is unconstrained beyond positive-diagonal. Parity-typing the rows of `L_t` so `Σ_t = L_t L_t^T` transforms correctly under symmetry actions is the next step.
- **Loss weighting with hard architectural priors.** When the architecture itself enforces some structural properties, the corresponding loss terms become redundant or potentially detrimental. E.g., if the architecture enforces dissipation, do you still need a dissipation-promoting loss term? Untested.
- **Multi-step rollout in training.** Source paper trains by predicting the entire forecast horizon at once given the encoder context. Multi-step rollout with detached gradients + BPTT through the rollout is an alternative; trade-off vs. computational cost is open.
- **Beyond GMAT.** The pattern generalizes to any deterministic forecaster. Validating with PeRCNN-style PDE forecasters (atmospheric drag fields), differentiable robotics simulators, or molecular dynamics codes is open.

## Sources

- `raw/papers/my-papers/KDD_submission.pdf` — Shahid, Jiang, Sarkar, Fleming 2026 (KDD '26). Sec. 4 (architecture details), Sec. 4.2 (training), Sec. 4.3 (inference), Algorithm 1 (full procedure), Sec. 5 (theoretical guarantees), Appendix A (moment aggregation).
- Kidger et al. 2020, *Neural Controlled Differential Equations for Irregular Time Series* (foundational NCDE paper; see [[Neural-Controlled-Differential-Equation]]).
