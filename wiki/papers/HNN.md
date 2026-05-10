---
title: HNN — Hamiltonian Neural Networks (Greydanus et al. 2019)
tags: [hamiltonian-mechanics, conservation-laws, physics-as-architecture, neural-ode-adjacent, energy-conservation, reversible-network]
sources: [raw/papers/interpretability/HNN.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: high
hard_constraint_possible: yes
---

# HNN — Hamiltonian Neural Networks (Greydanus et al. 2019)

> Greydanus, Dzamba, Yosinski. *Hamiltonian Neural Networks*. NeurIPS 2019. arXiv:1906.01563.

## One-Line Intuition

Don't learn the dynamics — learn a *scalar* Hamiltonian `H(q,p)` from data, then derive dynamics by taking its symplectic gradient via in-graph autograd. Energy conservation is a structural property of the architecture, not a loss term.

## Why This Was Invented

Standard NN dynamics models (Eq. 1: `(q₁,p₁) = (q₀,p₀) + ∫ S(q,p) dt`) learn an approximate vector field `S` directly. They tend to drift over time because they don't enforce conservation laws — the baseline mass-spring NN spirals inward, accumulating "energy loss" from numerical-style errors (Fig. 1).

[[Hamiltonian-Mechanics]] already gives a clean way to enforce conservation: pick a scalar `H` and the dynamics `dq/dt = ∂H/∂p, dp/dt = −∂H/∂q` automatically conserve `H`. So instead of *learning* the conservation law (and getting it slightly wrong), the authors *bake it into the architecture* by parameterizing `H_θ` and deriving dynamics through Hamilton's equations.

This is the time-evolution analogue of [[PeRCNN]]'s frozen FD conv: where PeRCNN encodes spatial differential operators as architecture, HNN encodes the symplectic time-evolution structure as architecture.

## The Math

The canonical equations of motion (Eq. 2):

```
dq/dt = ∂H/∂p     dp/dt = −∂H/∂q
```

The HNN parameterizes `H_θ : ℝ^{2N} → ℝ` with an MLP and computes the [[Symplectic-Gradient]] via autograd. The loss (Eq. 3) supervises *gradients* of `H_θ` against observed time derivatives:

```
L_HNN = ‖∂H_θ/∂p − dq/dt‖² + ‖∂H_θ/∂q + dp/dt‖²
```

Crucially, `H_θ` itself is *never directly supervised* — only its partial derivatives. The learned `H_θ` therefore equals true energy up to an additive constant (energy is relative — see Footnote 3 of paper).

Integration uses standard ODE solvers (4th-order Runge-Kutta in `scipy.integrate.solve_ivp`, error tolerance `1e-9`).

### Code Correspondence

```python
import torch, torch.nn as nn

class HNN(nn.Module):
    """H_θ : (q,p) → scalar. Dynamics derived via autograd."""
    def __init__(self, dim, hidden=200):
        super().__init__()
        self.H = nn.Sequential(
            nn.Linear(2*dim, hidden), nn.Tanh(),
            nn.Linear(hidden, hidden), nn.Tanh(),
            nn.Linear(hidden, 1),
        )

    def time_derivative(self, x):
        x = x.requires_grad_(True)
        H = self.H(x).sum()
        dH = torch.autograd.grad(H, x, create_graph=True)[0]
        dq, dp = dH.chunk(2, dim=-1)
        return torch.cat([dp, -dq], dim=-1)         # symplectic gradient

def hnn_loss(model, x, dxdt_true):
    return ((model.time_derivative(x) - dxdt_true) ** 2).mean()
```

3 layers × 200 hidden units × `tanh` activations is the paper's default; ~2000 gradient steps on synthetic toys.

## Toy Example

**Mass-spring (Task 1, Eq. 4):** `H = ½kq² + p²/(2m)`, with `k = m = 1`. 25 trajectories of 30 noisy `(q,p)` observations, total energies sampled uniformly from `[0.2, 1.0]`. Both HNN and baseline reach similar L2 train/test loss, but on the *energy MSE* metric the HNN beats the baseline by **~450×** (Table 1: `0.38e-3` vs `170e-3`).

The phase portrait tells the same story (Fig. 1, Fig. A.2a): the baseline's predicted orbit spirals inward; the HNN's orbit closes on itself.

## Connection to SiS / CTPC

HNN is the **conservation half** of SiS for ODE systems. It maps directly to the Keplerian core of CTPC:

| SiS layer | HNN realization |
|---|---|
| Hard constraint → architecture | Hamilton's equations baked into the computation graph (autograd of scalar `H_θ`) |
| Soft constraint → loss | None; loss is data-driven on time derivatives |
| Learned residual → ML corrector | `H_θ` itself is the learned scalar |

Specifically for CTPC:

- **Two-body Keplerian dynamics are exactly Hamiltonian** — the reduced-mass formulation in Eq. 6 of the paper is *the* orbital-mechanics use case. HNN is directly applicable to the predictor side.
- **Liouville's theorem gives free reversibility** — propagating an orbit forward and integrating back is bijective. Useful for orbit determination (recover epoch state from observations).
- **The Riemann-gradient counterfactual** (Sec. 6, Fig. 5) is a clean way to ask "what if we applied Δv?" — a maneuver-simulation primitive that doesn't require retraining.

**Critical limitation: HNN cannot represent dissipation.** Real pendulum (Task 3) is Hamiltonian + friction; HNN over-conserves energy because conservation is its inductive bias. **Two architectural routes extend HNN to dissipative dynamics**, and they make different trade-offs — see [[Hamiltonian-vs-Lagrangian-Duality]] § "A Fourth Axis: Dissipation Route":

- **Helmholtz route — [[D-HNN]]** (Sosanya & Greydanus 2022; same Greydanus). Adds a second scalar `D_θ`; combines via [[Helmholtz-Decomposition]]. Empirically beats HNN by ~4 orders of magnitude on damped-spring task. Limitation: does *not* structurally guarantee `Ḣ ≤ 0`.
- **J-R-structure route — [[Port-Hamiltonian-Neural-Network]]** (Dissipative SymODEN; Zhong, Dey, Chakraborty 2020; ingested 2026-05-10). Uses `(J − R)∇H` form with PSD `R` via Cholesky parameterization. Structural `Ḣ ≤ 0` guarantee at the vector-field level (RK4 caveat for discrete time).

The SiS dissipation constraint `Ḣ ≤ 0` requires the J-R route, not the Helmholtz route — see [[Dissipative-Hamiltonian-Neural-Network]] § "Why D-HNN is not enough for SiS" for the precise architectural analysis.

## Connections

- [[Hamiltonian-Neural-Network]] — the architecture pattern, separable from this paper's specific experiments
- [[Hamiltonian-Mechanics]] — foundational physics theory
- [[Symplectic-Gradient]] — the math object that produces Hamilton's equations
- [[PeRCNN]] — sister physics-as-architecture paper for spatial PDE structure (this paper is the time-evolution analogue)
- [[D-HNN]] — Sosanya & Greydanus 2022; Helmholtz-route dissipative extension of HNN by the same Greydanus
- [[Dissipative-Hamiltonian-Neural-Network]] — the D-HNN architecture pattern, separated from D-HNN paper's experiments
- [[Port-Hamiltonian-Neural-Network]] — J-R-structure-route dissipative extension; gives `Ḣ ≤ 0` structurally
- [[LNN]] — Lagrangian counterpart paper; co-authored by Greydanus
- [[Lagrangian-Neural-Network]] — Lagrangian-side architecture; works in arbitrary (non-canonical) coordinates
- [[Hamiltonian-vs-Lagrangian-Duality]] — synthesis: when to pick HNN vs LNN for CTPC; also covers the Helmholtz-vs-J-R-route axis for adding dissipation

## Open Questions

- **Dissipation gap (substantially closed 2026-05-10 across both routes).** Fundamental limitation explicitly acknowledged in §3.2: "we would need to model it separately from the HNN." [[D-HNN]] (Sosanya & Greydanus 2022) closes the *Helmholtz-decomposition route* — empirically dominant over HNN on damped spring + real pendulum + ocean currents but *without* `Ḣ ≤ 0`. [[Dissipative-SymODEN]] (Zhong, Dey, Chakraborty 2020) closes the *J-R-structured route* via Cholesky-PSD parameterization of the dissipation matrix — gives `Ḣ ≤ 0` structurally at the vector-field level. **Q8 of [[CTPC-Design-Rationale]] now splits into Q8a (Helmholtz route — closed by D-HNN), Q8b-vector-field (J-R route at continuous-time — closed by Dissipative SymODEN), and Q8b-discrete-time (exact discrete passivity — open, needs structure-preserving integrator).**
- **Non-canonical coordinates.** Pixel pendulum (Task 5) needs the auxiliary loss `‖z_p − (z_q^t − z_q^{t+1})‖²` (Eq. 7) to coerce the autoencoder latent space into canonical `(q,p)` form. Poisson-bracket structure isn't learned — it has to be enforced. Open question: can equivariant or symplectic-by-construction encoders eliminate this auxiliary loss?
- **Chaotic systems.** Three-body (App. B) shows energy stability but trajectory divergence. For SDA N-body perturbations, HNN gives long-term `H` stability but not orbit stability. Whether this is "good enough" depends on downstream use (collision probability vs. exact ephemeris).
- **Computational cost of gradient supervision.** Each forward needs an autograd pass; each backward goes through that gradient. ~2× cost vs. standard supervision. Tractable on a desktop CPU for these toys; budget question for production CTPC at SDA scale.
- **Real pendulum train loss is *worse* than baseline (Table 1, Task 3).** Paper handwaves this with "test-set distribution differs from train" (App. A), but it's a real issue when applying to noisy / partially-conservative real-world data. Worth scrutinizing when applying to TLE-derived state observations.

## Sources

- `raw/papers/interpretability/HNN.pdf` — Eq. 2 (Hamilton's equations), Eq. 3 (HNN loss), Eq. 4–6 (mass-spring, pendulum, two-body Hamiltonians), Eq. 7 (auxiliary canonical-coordinate loss), Tables 1–2 (quantitative results), Figs. 1–5, App. A–C.
- Code: <https://github.com/greydanus/hamiltonian-nn>
