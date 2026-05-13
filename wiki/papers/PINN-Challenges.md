---
title: Characterizing Possible Failure Modes in PINNs (PINN Challenges)
tags: [physics-informed, soft-constraints, optimization, loss-landscape, sis-foundational]
sources:
  - raw/papers/physics-informed/PINNChallenges.pdf   # Krishnapriyan, Gholami, Zhe, Kirby, Mahoney 2021 (NeurIPS)
created: 2026-05-12
updated: 2026-05-12
sis_relevance: critical
hard_constraint_possible: yes
---

# PINN Challenges (Krishnapriyan et al. 2021)

## One-Line Intuition

The empirical smoking gun for "soft physics constraints are pathological": vanilla PINNs
achieve **~100% error on simple closed-form PDEs** (convection with `β = 30`, reaction
with `ρ = 5`, reaction-diffusion with `ν = 2`) — not because the network lacks
expressivity, but because the PDE-based soft loss makes the optimization landscape
ill-conditioned. This is the foundational paper SiS cites when arguing
"physics-as-architecture, not loss penalty."

## Why This Was Invented

By 2021 PINNs (Raissi et al. 2019) had become a default approach in SciML — solve PDEs by
adding `λ_F · ||F(u)||²` to the training loss, where `F` is the PDE residual operator.
The community was claiming success on increasingly ambitious problems (Navier-Stokes,
cardiac activation, multi-physics). But the PINN community had not systematically
characterized *when PINNs fail*. Krishnapriyan et al. demonstrate that PINNs fail on
**embarrassingly simple problems** as soon as the PDE coefficients exit a narrow easy
regime — and diagnose *why*: the soft constraint approach makes the loss landscape worse.

## The Math — and Why It Fails

**The standard PINN formulation** (Eq. 3 of the paper):
```
min_θ ℒ(u) + λ_F · ℒ_F(u)        where ℒ_F = ||F(u)||² is the PDE residual
```

The paper studies three canonical problems, all with **closed-form analytic solutions**:

1. **1D convection**: `∂u/∂t + β · ∂u/∂x = 0`. Analytic: `u(x,t) = F⁻¹(F(h(x)) · e^{-iβkt})`.
2. **1D reaction**: `∂u/∂t = ρ · u(1 − u)` (logistic).
3. **1D reaction-diffusion**: `∂u/∂t − ν ∂²u/∂x² − ρ · u(1 − u) = 0`.

**Result** (Figure 1, Figure 2, Table 1):

| Problem | Coefficient | Vanilla PINN relative error |
|---|---|---|
| 1D convection | `β = 1` | **0.78%** |
| 1D convection | `β = 20` | **75%** |
| 1D convection | `β = 30` | **90%** |
| 1D convection | `β = 40` | **96%** |
| 1D reaction-diffusion | `ν = 5, ρ = 5` | **93%** |

The PINN works fine for `β = 1` and catastrophically fails for `β > 10` — *for a linear
hyperbolic PDE with a simple Fourier-series analytic solution.* The architecture is a
4-layer fully-connected NN with 50 neurons per layer; the paper sweeps over learning
rates `1e-4` to `2.0`, uses L-BFGS, averages 10 random seeds.

**The diagnosis (§4, §5).** The failure is **not architecture/capacity** — the paper shows
the same architecture *can* solve the problem with curriculum or seq2seq training (§5).
The failure is **optimization landscape geometry**:

- Hessian-eigenvector loss-landscape analysis (Figure 3) shows the landscape becomes
  increasingly complex, non-symmetric, and ridden with local minima as `β` increases.
- Increasing the regularization weight `λ_F` makes the landscape **harder to optimize**,
  not easier. There's an irresolvable trade-off: reduce `λ_F` for tractable optimization
  → solutions don't satisfy PDE; increase `λ_F` for physics satisfaction → optimization
  gets stuck in high-loss local minima.
- The PDE-based regularizer is **structurally different** from L1/L2 weight decay (which
  is a nice convex set). `ℒ_F` is a differential operator; ill-conditioning is intrinsic.

**Two proposed remedies (§5):**

1. **Curriculum regularization.** Start training at low `β/ρ/ν` (easy problem), gradually
   increase. ~2 orders of magnitude error reduction (Table 1: `β = 30`,
   regular 89.7% → curriculum 2.0%).

2. **Sequence-to-sequence training.** Predict one `Δt` step at a time instead of the
   entire space-time at once. Use predicted `u(t = Δt)` as initial condition for next
   segment. ~2 orders of magnitude error reduction on reaction-diffusion.

Both remedies **keep the soft-constraint formulation** but mitigate the landscape
ill-conditioning. They don't *fix* the underlying problem — they manage around it.

### Code Correspondence

```python
import torch, torch.nn as nn

# A vanilla PINN: MLP that takes (x, t), outputs û; loss combines data + PDE residual
class PINN(nn.Module):
    def __init__(self):
        super().__init__()
        self.net = nn.Sequential(nn.Linear(2, 50), nn.Tanh(),
                                 nn.Linear(50, 50), nn.Tanh(),
                                 nn.Linear(50, 50), nn.Tanh(),
                                 nn.Linear(50, 1))
    def forward(self, x, t):
        return self.net(torch.cat([x, t], dim=-1))

def pinn_loss(model, x_data, t_data, u_data, x_phys, t_phys, beta, lambda_F):
    # Data-fit term
    L_data = ((model(x_data, t_data) - u_data) ** 2).mean()
    # PDE-residual term: ∂u/∂t + β ∂u/∂x = 0 for convection
    x_phys.requires_grad_(True); t_phys.requires_grad_(True)
    u_phys = model(x_phys, t_phys)
    u_t = torch.autograd.grad(u_phys.sum(), t_phys, create_graph=True)[0]
    u_x = torch.autograd.grad(u_phys.sum(), x_phys, create_graph=True)[0]
    L_F = ((u_t + beta * u_x) ** 2).mean()
    return L_data + lambda_F * L_F   # THE soft constraint — the paper's pathology lives in λ_F · L_F
```

This is the formulation Krishnapriyan et al. demonstrate fails. **For `β > 10` no choice
of `λ_F` and no amount of hyperparameter tuning recovers the analytic solution.**

## Three Things Worth Calling Out

1. **The "expressivity is not the problem" demonstration is the key contribution.** It's
   tempting to attribute PINN failures to NN limitations. The paper kills that hypothesis:
   the same NN architecture achieves 2-OOM lower error under curriculum training. The
   architecture is fine; the soft constraint is wrong.

2. **The trade-off is irresolvable within the soft-constraint formulation.** Tune
   `λ_F` down → optimize well, ignore physics. Tune `λ_F` up → respect physics, fail to
   optimize. There is no `λ_F*` that gives both. This is what
   "ill-conditioned by construction" means in practice.

3. **PINNs that DO work need to be moved away from the vanilla formulation.** The
   curriculum and seq2seq fixes change the *training procedure*, not the architecture.
   The deeper SiS conclusion: change the *architecture* (hard constraints baked in) and
   the training procedure becomes irrelevant. PhyArch / port-Hamiltonian / G-CNN
   approaches don't need curricula because they don't have an ill-conditioned soft
   regularizer to optimize through.

## Connection to SiS / CTPC

**This is the foundational citation for the SiS design hierarchy** (CLAUDE.md "Core
Philosophy"):

> 1. Hard constraints → baked into architecture
> 2. Soft constraints → loss function penalties (only when hard encoding is intractable)
> 3. Learned residuals → ML correctors for unmodeled dynamics

Krishnapriyan et al. supply the empirical evidence for *why* step 1 is preferred over
step 2:

1. **PhyArch is the architectural alternative to PINN soft constraints for ODE state
   spaces.** Where PINN encodes physics in `λ_F · ||F(u)||²`, PhyArch encodes it in the
   parity-split assembly + geometric-feature embedding. PhyArch's experimental result
   (0% failure rate on OOD double pendulum, lowest energy drift despite not modeling
   energy) is exactly the kind of result PINNs cannot reach because of the
   loss-landscape pathology this paper identifies. See [[PhyArch-Double-Pendulum-Benchmark]].

2. **Port-Hamiltonian Neural Networks bake `Ḣ ≤ 0` as an algebraic identity.** The Cholesky-
   PSD parameterization `D = LLᵀ` makes `dH/dt = −∇HᵀD∇H ≤ 0` true at every
   `(q, p, θ)` — no `λ_F` to tune, no optimization-landscape pathology. See
   [[Port-Hamiltonian-Neural-Network]].

3. **G-CNN bakes equivariance as an algebraic identity.** `[L_u f ★ ψ] = L_u [f ★ ψ]` holds
   at random init — no `λ_F` to tune. See [[Group-Equivariant-CNN]].

4. **PeRCNN is the closest spiritual analog in the PDE setting.** Where PINN puts physics in
   the loss, PeRCNN puts the differential operator structure directly in the conv kernel
   (frozen FD stencils + physics-based padding for BCs). PeRCNN doesn't have this paper's
   failure mode. See [[PeRCNN]].

5. **The curriculum / seq2seq remedies are conceptually adjacent to CTPC's design choices.**
   CTPC's Predictor-Corrector decomposition (D1 of [[CTPC-Design-Rationale]]) implicitly
   does a kind of "curriculum": the Predictor handles the easy part (J2 + drag from GMAT,
   known analytically), the Corrector models the hard residual. CTPC's continuous-time
   formulation (D2) is conceptually closer to seq2seq than to "predict the entire
   trajectory at once." Both SiS design decisions land on the *correct side* of this
   paper's diagnoses.

**Hard vs soft constraint classification.** This paper is *about the failure of soft
constraints*. The SiS thesis is that hard constraints (architectural) avoid this entire
class of failure modes. CTPC adopts hard constraints wherever possible:

| SiS hard-constraint mechanism | Avoids PINN failure mode? |
|---|---|
| PhyArch parity-split symmetry | ✓ Equivariance is algebraic identity |
| Port-Hamiltonian Cholesky-PSD `D = LLᵀ` | ✓ Ḣ ≤ 0 is algebraic identity |
| G-CNN group convolution | ✓ Equivariance is algebraic identity |
| PeRCNN frozen-stencil FD conv | ✓ Differential operator hardcoded |
| Latent NCDE Corrector | ⚠️ Probabilistic loss (NLL + CRPS + KL) is soft, but on calibration not physics; no PDE-residual penalty |

The Latent NCDE Corrector's training loss is "soft" in the sense that it's a likelihood
objective, but it doesn't enforce a *physics* constraint via penalty. Physics in CTPC
comes from the Predictor (GMAT) which is exact, not learned. So CTPC sidesteps this
paper's failure mode entirely.

## Connections

- [[PhyArch]] — SiS architectural alternative to PINN soft constraints for ODE state spaces.
- [[Port-Hamiltonian-Neural-Network]] — `Ḣ ≤ 0` as algebraic identity, the SiS-canonical
  hard-constraint instantiation for dissipative dynamics.
- [[PeRCNN]] — PDE counterpart to SiS hard-constraint approach (PINN's natural rival in
  the PDE setting).
- [[Group-Equivariant-CNN]] — equivariance as algebraic identity, another SiS hard-constraint
  exemplar.
- [[CTPC-Design-Rationale]] — Core Design Philosophy section's "hierarchy" is the SiS
  response to this paper's diagnoses.
- [[Geometric-Priors]] — the curse-of-dimensionality argument under which hard physical
  priors are essential, not optional.

## Open Questions

1. **Are there SiS subproblems where hard encoding is intractable and we must accept the
   soft-constraint pathology?** Atmospheric drag tracking from sparse measurements,
   space-weather coupling, multi-fidelity sensor fusion — these have known physics but
   not yet a clean hard-architectural encoding. The paper's diagnosis suggests we should
   prefer Predictor-Corrector decomposition with the Predictor handling the
   hard-encodable part and the Corrector handling residuals as a learned function rather
   than imposing residual physics via penalty.

2. **Curriculum-style training for CTPC.** The paper's curriculum remedy (start with easy
   PDE coefficients, ramp up) has an analog for CTPC: start training at short horizons,
   ramp to long horizons. Whether this materially helps CTPC training stability is an
   empirical question.

3. **Hessian-eigenvector landscape analysis for the Latent NCDE Corrector.** The paper's
   diagnostic technique (perturb model parameters along top-2 Hessian eigenvectors, plot
   loss surface) could be applied to CTPC training to verify the loss landscape is *not*
   pathological under hard architectural priors. Would be a satisfying empirical
   confirmation of the SiS thesis.

## Sources

- Krishnapriyan, A. S., Gholami, A., Zhe, S., Kirby, R. M., Mahoney, M. W. (2021).
  *Characterizing possible failure modes in physics-informed neural networks.*
  NeurIPS 2021. All 13 pages of the main paper read (Abstract, §1 Introduction, §2
  Related Work, §3 Failure Modes — convection + reaction-diffusion, §4 Diagnosis — loss
  landscape geometry, §5 Remedies — curriculum + seq2seq, §6 Conclusion). Appendix
  (additional figures, reaction problem details, hyperparameter sweep, eigenvector
  landscape methodology) referenced but not separately summarized.
