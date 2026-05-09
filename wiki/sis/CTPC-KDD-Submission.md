---
title: CTPC — Continuous-Time Probabilistic Correctors (Shahid et al. 2026, KDD submission)
tags: [ctpc, latent-ncde, probabilistic-corrector, predictor-corrector, gmat, spacecraft-trajectory, sis-paper, uncertainty-quantification, sda]
sources: [raw/papers/my-papers/KDD_submission.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: critical
hard_constraint_possible: partial
---

# CTPC — Continuous-Time Probabilistic Correctors (Shahid et al. 2026, KDD submission)

> Shahid, Jiang, Sarkar, Fleming. *Continuous-Time Probabilistic Correctors (CTPC) for Physics-Based Long-Horizon Trajectory Forecasting*. KDD '26 submission.

> **The defining CTPC paper.** Throughout the wiki, [[CTPC]] has been a forward reference; this is the source. See [[CTPC-Design-Rationale]] for the synthesis with [[PhyArch-Double-Pendulum-Benchmark]] and the operational design rationale.

## One-Line Intuition

A Predictor-Corrector framework where **GMAT (or any deterministic physics simulator) is the frozen Predictor, and a Latent NCDE is the probabilistic Corrector** that models the Predictor's residuals in continuous time. The Corrector wraps around the Predictor without modifying it — corrects the deterministic forecast AND produces calibrated full-covariance uncertainty ellipsoids. Training loss `NLL + CRPS + KL` is strictly proper, so calibration consistency is a structural property of the objective.

## Why This Was Done

High-fidelity physics simulators like GMAT [Hughes 2016] produce accurate deterministic spacecraft forecasts but lack calibrated uncertainty estimates. In long-horizon open-loop regimes (no observations available for correction), errors accumulate without bounds — and a model that *usually* respects physics but *occasionally* violates it can trigger false collision warnings or miss real ones. For Space Domain Awareness (SDA), the safety implication is direct: reliable uncertainty matters as much as accuracy.

Existing approaches fall short:

- **Hybrid ARIMA-MLP / ARIMA-LSTM** [Zhang 2003, Xu 2022] — discrete time, regular sampling required, no full predictive covariance
- **HopCast** [Shahid & Fleming 2025] — retrieval-based, regular sampling only, no covariance
- **Deterministic NCDE Correctors** [Shahid, Koirla, Fleming 2025] — continuous-time, irregular sampling supported, but no uncertainty quantification
- **Latent ODE / GRU-ODE-Bayes / Latent SDE** — continuous-time but uncontrolled dynamics; severely overconfident in long-horizon open-loop regimes (this paper's empirical finding)

CTPC unifies these threads: continuous-time, irregular-sampling-friendly, *probabilistic* with full covariance, with a Predictor-Corrector decomposition that retains the underlying physics simulator.

## The Math

### Problem Setup

Spacecraft state in ECI frame: `x_ECI(t) ∈ ℝ³` (position component; GMAT internally propagates full Cartesian state). Observed warm-up interval `[t_0, t_T']`; open-loop forecast horizon `(t_T', t_T]` (up to 4 days). GMAT produces deterministic forecasts `x̂_ECI(t)`. Define forecast error:

```
e_ECI(t) = x_ECI(t) − x̂_ECI(t)
```

Errors over `[t_0, t_T']` are observed; the corrector must predict the distribution of errors over `(t_T', t_T]`:

```
ê_ECI(t) | e_ECI(t_0:T'), x̂_ECI(t_T'+1:T) ∼ T_ν(μ_t, Σ_t)
```

The corrected forecast and uncertainty ellipsoid:

```
x̂_ECI^corr(t) = x̂_ECI(t) + μ_t
ε_α(t) = { e ∈ ℝ³ : (e − μ_t)^T Σ_t^(−1) (e − μ_t) ≤ χ²_3(α) }
```

### CTPC-CDE++ Architecture (the most general variant)

Encoder, decoder, and prediction head are described below. See `paa_dp.py`-style code skeleton in [[Latent-NCDE-Corrector]].

**Encoder** ([[Neural-Controlled-Differential-Equation]]). Processes past errors via NCDE driven by a spline-interpolated control path `X_e(t)` of `e_ECI(t_0:T')`:

```
z_e(t) = z_e(t_0) + ∫ f_θ_e(z_e(s)) dX_e(s)        for t ∈ (t_0, t_T']
```

Hidden states are aggregated via a learned weight network (`f_θ_w`):

```
z_f = ∫ w(t) ⊙ z_e(t) dt  /  ∫ w(t) dt
```

A linear projection maps `z_f` to a diagonal Gaussian latent: `(μ_L, Σ_L) ← W_{h_e→L} z_f`.

**Decoder** ([[Neural-Controlled-Differential-Equation]]). NCDE driven by GMAT's deterministic forecasts as control path `X_d(t)`:

```
z_d(t) = z_d(t_T'+1) + ∫ f_θ_d(z_d(s)) dX_d(s)     for t ∈ (t_T'+1, t_T]
```

Initial hidden state samples from the latent: `s_L ∼ N(μ_L, Σ_L)`, `z_d(t_T'+1) = W_{L→h_d} s_L`. K samples → K decoder trajectories → ensemble-like effect.

**Prediction head.** A probabilistic MLP maps `z_d(t)` to Student-t parameters:

```
(μ_t, L_t) = f_θ_p(z_d(t))
Σ_t = L_t L_t^T                  (Cholesky parameterization, full covariance)
ê_ECI(t) | z_d(t) ∼ T_ν(μ_t, Σ_t)
```

`L_t` is lower-triangular with strictly positive diagonal. `ν > 0` is learned as `ν = ν_min + softplus(ν_raw)` with `ν_min = 4.5` (avoids degenerate heavy tails; `ν > 4` ensures finite kurtosis).

### Loss

Three-term strictly-proper loss (Eq. 12):

```
L_total = L_NLL + L_CRPS + L_KL
```

- **L_NLL** — Student-t negative log-likelihood (heavy-tailed orbital errors per Mashiku 2012)
- **L_CRPS** — Continuous Ranked Probability Score; encourages well-calibrated distributions
- **L_KL** — KL divergence between latent posterior `q(z_0) = N(μ_L, Σ_L)` and prior `p(z_0) = N(0, I)`

Each term is strictly proper individually (Lemma D.3); their sum preserves strict properness.

### Theoretical Guarantees

Under standard Lipschitz + bounded-variation assumptions:

- **Lemma 5.1** (decoder stability): `E‖z_d(t)‖² ≤ M_0 exp(2L_d ‖X_d‖_TV)` — at-most-exponential growth.
- **Theorem 5.2** (calibration consistency): At population minimum, `E[r_t^T Σ_t^(−1) r_t] = L`, i.e., normalized squared Mahalanobis distance equals dimensionality. **The strictly proper loss uniquely identifies the true conditional distribution** — calibration is structural, not empirical.
- **Theorem 5.3** (error reduction): `E‖ẽ_ECI(t)‖² = E‖e_ECI(t)‖² − E‖μ_t‖²`. Corrector strictly reduces expected forecast error unless `μ_t = 0`.
- **Theorem 5.4** (covariance growth): `‖Σ_t‖ ≤ K_1 exp(2L_d ‖X_d‖_TV) + K_2 ∫ exp(...) ds`. Rules out both covariance collapse and uncontrolled explosion.
- **Theorem D.6** (input-output stability): bounded perturbations in past observations → bounded perturbations in `(μ_t, Σ_t)` over the entire forecast horizon.

### CTPC Variants (Table 1 of paper)

| Variant | Encoder | Decoder | Pred. Head | Loss | Irregular Sampling | Missing Features | Frame-Robust |
|---|---|---|---|---|---|---|---|
| Latent ODE★ (baseline) | ODE-RNN | NODE | Deterministic | MSE+KL | ✓ | ✗ | ✗ |
| Latent ODE† (baseline) | ODE-RNN | NODE | Deterministic | CRPS+KL | ✓ | ✗ | ✗ |
| CTPC-ODE | ODE-RNN | NODE | Probabilistic | NLL+CRPS+KL | ✓ | ✗ | ✗ (RTN only) |
| CTPC-CDE | ODE-RNN | NCDE | Probabilistic | NLL+CRPS+KL | ✓ | ✗ | ✓ |
| **CTPC-CDE++** | NCDE | NCDE | Probabilistic | NLL+CRPS+KL | ✓ | ✓ | ✓ |

NCDE-based decoders generalize across coordinate frames because the control path adapts; NODE decoders fail in ECI (Appendix G).

## Code Correspondence

The training and inference algorithms (Algorithm 1 of paper):

```python
# Training (per epoch)
e_ECI = x_ECI - x_hat_ECI                                     # observed errors
X_e = CubicSpline(t_obs, e_ECI[obs])                          # encoder control path
z_e = NCDE_encoder(z_e_init=0, X_e)                           # past error → latent dynamics
w = WeightNet(z_e)
z_f = trapezoidal(w * z_e) / trapezoidal(w)                    # weighted aggregation
mu_L, Sigma_L = LatentProjection(z_f)                          # latent posterior

# K samples for ensemble-like uncertainty
for k in range(K):
    s_L = sample(N(mu_L, Sigma_L))
    z_d_init = LatentToDecoder(s_L)
    X_d = CubicSpline(t_forecast, x_hat_ECI[forecast])         # GMAT forecasts as control path
    z_d = NCDE_decoder(z_d_init, X_d)                          # forecast latent dynamics
    mu_t_k, L_t_k = PredictionHead(z_d)                        # Student-t params

loss = NLL(Student_t(mu_t_k, L_t_k), e_ECI_true) + CRPS(...) + KL(N(mu_L, Sigma_L) || N(0,I))
```

At inference, the K-sample mixture is collapsed via moment aggregation (law of total variance):

```
mu_bar = (1/K) Σ_k mu_t_k
Sigma_bar = (1/K) Σ_k Sigma_t_k                                # within-sample (aleatoric)
          + (1/K) Σ_k (mu_t_k - mu_bar)(mu_t_k - mu_bar)^T     # between-sample (latent-induced)
```

This is the deep-ensembles variance decomposition [Lakshminarayanan et al. 2017] but achieved with K samples through *one* decoder rather than K separate models.

## Empirical Results

NASA CDDIS spacecraft ephemeris data, 6 rolling test windows in 2017, train on ~1.5 months (2500-min trajectories every 15 min) → test on next 15 days (5760-min trajectories every 6 hours). RTN-frame errors for main results.

### Headline numbers (Table 2, averaged across 6 periods)

| Model | MSE @ 4-day (% reduction) | d̄² @ 4-day | −log\|Σ̄\| @ 4-day |
|---|---|---|---|
| GMAT (baseline) | 0.2704 — | — | — |
| GMAT + Latent ODE★ | 0.1631 (39%) | **20,000** | 38.03 |
| GMAT + Latent ODE† (CRPS+KL) | 0.1706 (36%) | 3,408 | 22.60 |
| GMAT + CTPC-ODE | 0.1073 (60%) | 1.14 | 16.24 |
| GMAT + CTPC-CDE | 0.0994 (63%) | 1.11 | 16.51 |
| **GMAT + CTPC-CDE++** | **0.0967 (64%)** | **1.15** | 16.39 |

Three-orders-of-magnitude calibration improvement (d̄² from 20,000 → ~1.1) while simultaneously reducing MSE by 64%. CTPC-CDE++ is the best on accuracy + supports missing features.

### Coordinate-frame robustness (Appendix G)

CTPC-ODE in ECI: **fails** — MSE reductions negligible/negative, d̄² ≥ 13 across all horizons. The NODE decoder lacks a control path so dynamics can't condition on the frame.

CTPC-CDE in ECI: 24% MSE reduction at 4-day horizon, d̄² ≈ 214. Worse than RTN (d̄² ≈ 1.1) but still functional. **Architectural property**: NCDE decoders adapt to coordinate transforms via the control path; NODE decoders don't.

### Latent-induced variability (Table 3, Appendix F)

Comparing total variance Σ̄ (with between-sample term) vs. mean predictive variance only (within-sample only): total variance gives d̄² ≈ 1.0–1.5; mean-only gives d̄² ≈ 0.7–2.0 depending on horizon, systematically more overconfident at long horizons. **Latent sampling provides the ensemble-like effect critical for long-horizon calibration.**

## Connection to SiS / CTPC

**This is the CTPC paper.** It defines the framework that the rest of the wiki has been forward-referencing. Key SiS-philosophy mappings:

- **Predictor (GMAT) = the frozen physics layer.** Operational analog of a [[Physics-Based-FD-Convolutional-Layer]] but for orbital ODEs. Hard-encoded conservation laws via physics simulation; no trainable parameters.
- **Decoder dynamics (NCDE) = learned residual.** Currently a *generic* time-series model. Open question (resolved by [[CTPC-Design-Rationale]]): can [[PhyArch]]-style symmetry-hardwired vector fields replace this generic NCDE? See deferred-decisions section there.
- **Probabilistic head (Student-t with Cholesky) = uncertainty quantification.** A new piece in the SiS toolkit — physics-as-architecture compose-able with formally-calibrated UQ.

The relationship to [[PhyArch-Double-Pendulum-Benchmark]]:

- PhyArch DP validates **deterministic** symmetry-hardwiring on a controlled toy.
- CTPC validates **probabilistic** Predictor-Corrector decomposition on real-world orbital data.
- **Both make the same coordinate-choice argument**: PhyArch's `(u_i, t_i, ĝ)` ≈ CTPC's RTN basis. Choose coordinates where the natural physics structure is exposed.
- **Composition path forward**: PhyArch-CTPC = symmetry-hardwired Latent NCDE Corrector. Concrete architectural details in [[CTPC-Design-Rationale]].

## Connections

- [[CTPC-Design-Rationale]] — synthesis with PhyArch DP, decisions made + decisions deferred to PhyArch-CTPC
- [[Latent-NCDE-Corrector]] — the architecture pattern, separable from this paper's GMAT-specific application
- [[Neural-Controlled-Differential-Equation]] — foundational concept (Kidger et al. 2020)
- [[PhyArch-Double-Pendulum-Benchmark]] — Bilal's deterministic counterpart; validates symmetry-hardwiring on a toy
- [[PhyArch]] — the methodology pattern
- [[Hamiltonian-Mechanics]] — GMAT is a real-world Hamiltonian propagator (orbital mechanics is Hamiltonian + dissipative perturbations)
- [[PeRCNN]] — sibling Predictor-Corrector decomposition for spatial PDEs (frozen FD conv = hard physics; Π-block = learned residual)
- [[HNN]] / [[LNN]] — alternative architectures for the conservative core

## Open Questions

(See [[CTPC-Design-Rationale]] § "Decisions Deferred to PhyArch-CTPC" for full treatment.)

- **PhyArch-style symmetry hardwiring inside the corrector.** Currently the NCDE decoder is a generic vector field. PhyArch-style parity-split + manipulator-skeleton structure should improve OOD generalization analogously to what the PhyArch DP benchmark showed in the deterministic case.
- **Variance-head equivariance.** The Cholesky `L_t` is unconstrained beyond positive-diagonal. Constraining its rows by parity (so `Σ_t = L_t L_t^T` transforms correctly under symmetry actions) is the next architectural step for PhyArch-CTPC.
- **Differentiable physics + end-to-end adaptation** (paper's own future-work item ii). Differentiable GMAT (or open-source equivalent) + symmetry-hardwired corrector + probabilistic head, trained end-to-end. The composition gateway.
- **Formal verification + safety guarantees** (paper's own future-work item iii). PhyArch's hard structural guarantees + CTPC's strict-properness theorems → potential for *provably safe* CTPC variants. The `formal-verification/` raw papers (already in the wiki) become directly relevant.
- **Parameter uncertainty + space weather covariates** (paper's own future-work item i). Conditional CTPC on environmental covariates.
- **Conjunction probability sensitivity.** Listed in limitations. The downstream operational metric — calibrated uncertainty → reliable conjunction probability bounds.
- **HNN / LNN / PHNN integration.** GMAT *is* a Hamiltonian propagator. A natural three-layer composition: SGP4-like frozen-Hamiltonian-core + PHNN-style dissipative-residual + Latent-NCDE-style probabilistic-head. Open until PHNN is ingested.

## Sources

- `raw/papers/my-papers/KDD_submission.pdf` — Shahid, Jiang, Sarkar, Fleming 2026 (KDD '26 submission). Sec. 3 (problem setup), Sec. 4 (methodology + Latent NCDE architecture + Algorithm 1), Sec. 5 (theoretical analysis: Lemma 5.1 + Theorems 5.2–5.4), Sec. 6 (CTPC variants), Sec. 7 (evaluation metrics), Sec. 8 (results: Table 2 RTN, ablations, decoder comparison), Appendix A (moment aggregation / law of total variance), Appendix D (Theorem D.6 system-level stability), Appendix E (ECI↔RTN transform), Appendix G (ECI results: NCDE decoder coordinate-frame robustness), Appendix H (training/test rolling-window setup).
- Code + dataset: provided upon acceptance per the paper.
