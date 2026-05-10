---
title: Lagrangian Neural Network
tags: [lagrangian-mechanics, architecture-pattern, scalar-parameterization, conservation-laws, physics-as-architecture, hessian-inversion]
sources: [raw/papers/interpretability/LNN.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: high
hard_constraint_possible: yes
---

# Lagrangian Neural Network

## One-Line Intuition

An architecture pattern for learning conservative dynamical systems from arbitrary (non-canonical) coordinates: parameterize a scalar Lagrangian `L_θ(q, q̇)` with an MLP, derive accelerations `q̈` by inverting the velocity-Hessian from the Euler-Lagrange equation. Energy conservation is structural; canonical momenta are *not* required.

## Why This Was Invented

[[Hamiltonian-Neural-Network]] requires canonical `(q, p)` coordinate pairs. For many systems, canonical momenta are unknown, hard to compute, or non-trivial functions of `q̇` (e.g., relativistic, charged particle in magnetic field). LNN works directly with the natural observables `(q, q̇)` — generalized position and velocity — paying for it with `O(d³)` Hessian inversion per forward pass.

Introduced in [[LNN]]; the pattern generalizes to *Lagrangian Graph Networks* for spatially-extended systems and composes with Rayleigh-dissipation extensions for non-conservative dynamics.

## The Math

The architecture in three lines:

1. **Network**: `L_θ : ℝ^{2d} → ℝ` — scalar function of position + velocity. Activation must have non-zero second derivative (softplus, tanh — *not* ReLU).
2. **Dynamics** (via [[Euler-Lagrange-Equation]] solved for `q̈`):
   ```
   q̈ = (∇_q̇ ∇_q̇^T L_θ)^(-1) [ ∇_q L_θ − (∇_q ∇_q̇^T L_θ) q̇ ]
   ```
   Computed via second-order autograd. Pseudoinverse (`pinv`) for numerical safety against singular mass matrices.
3. **Loss**: MSE between predicted `q̈` and observed `q̈_true`.

**Design parameters:**

| Knob | Typical value | Trade-off |
|---|---|---|
| Activation | softplus | Twice-differentiable; ReLU broken (∂² = 0); sigmoid/tanh also OK |
| Depth × width | 4 × 500 | Bigger nets fit complex `L`; init is delicate |
| Init scheme | custom (App. C of LNN paper) | Kaiming/Xavier insufficient due to Hessian-inversion nonlinearity |
| Optimizer | Adam, lr=1e-3 decaying | Standard; no special tricks |
| Forward cost | `O(d³)` | Hessian + matrix inversion per step; scaling concern for large d |
| Coordinate frame | arbitrary `(q, q̇)` | The whole point — no canonical structure required |
| Integrator | RK4 typical; variational integrators correct match | Variational (Marsden-West) preserves discrete Euler-Lagrange exactly |

### Code Correspondence (JAX)

```python
import jax, jax.numpy as jnp

def lnn_acceleration(L_fn, params, q, q_t):
    """q̈ from a learned Lagrangian L_θ(q, q̇) via Euler-Lagrange."""
    L = lambda q, q_t: L_fn(params, q, q_t)
    M     = jax.hessian(L, 1)(q, q_t)                       # mass matrix ∇_q̇∇_q̇^T L
    grad_q = jax.grad(L, 0)(q, q_t)                          # ∇_q L
    cross = jax.jacobian(jax.jacobian(L, 1), 0)(q, q_t)      # ∇_q∇_q̇^T L
    return jnp.linalg.pinv(M) @ (grad_q - cross @ q_t)

def lnn_loss(L_fn, params, q, q_t, q_tt_true):
    q_tt_pred = lnn_acceleration(L_fn, params, q, q_t)
    return jnp.mean((q_tt_pred - q_tt_true) ** 2)
```

Torch equivalent needs `torch.autograd.grad(..., create_graph=True)` cascades and is much messier — JAX's higher-order autograd composition is the natural language for LNN.

## Toy Example

For a 1-D mass-spring with true `L = ½ q̇² − ½ q²` (T − V):

```python
# Before training: random network → q̈ predictions are noise
# After training on (q, q̇, q̈) tuples → q̈ → -q (simple harmonic motion)
```

Contour plots of trained `L_θ(q, q̇)` recover the kinetic-minus-potential structure: positive parabola in `q̇`, negative parabola in `q`. Differs from true `L` by an additive constant + a *gauge* term `df/dt` that doesn't affect dynamics (Lagrangians are equivalence classes).

## Connection to SiS / CTPC

LNN is the **arbitrary-coordinate alternative** to [[Hamiltonian-Neural-Network]] for the conservative-dynamics corrector in CTPC.

- **Direct fit for raw observables.** TLE-derived `(r, v)` pairs are *not* canonical under the J2-perturbed Hamiltonian (which is what we're trying to learn). LNN takes them directly without a canonical transform.
- **Non-quadratic kinetic energies.** Relativistic corrections, charged-particle dynamics, anything where `T` isn't `½ q̇^T M(q) q̇`. LNN handles all of these; HNN requires constructing canonical momenta first; DeLaN-style restricted Lagrangians fail entirely.
- **Composition with frozen physics.** Same decomposition as HNN: SGP4 (frozen, hard physics) + LNN (learned conservative residual) + dissipative term. Dissipative term on Lagrangian side: *Rayleigh dissipation function* `D(q̇)`, modifying Euler-Lagrange to `d/dt(∂L/∂q̇) − ∂L/∂q + ∂D/∂q̇ = 0`. Symmetric to PHNN's Port-Hamiltonian formulation.
- **Cost is the practical concern.** `O(d³)` Hessian inversion. For 6-D orbital state, free; for full perturbation-rich N-body, needs graph sparsity (Lagrangian Graph Network) to be tractable.

See [[Hamiltonian-vs-Lagrangian-Duality]] for the full HNN-vs-LNN-for-CTPC decision page.

## Connections

- [[LNN]] — the introducing paper
- [[Lagrangian-Mechanics]] — foundational physics
- [[Euler-Lagrange-Equation]] — math object behind the forward dynamics
- [[Hamiltonian-vs-Lagrangian-Duality]] — synthesis with HNN
- [[Hamiltonian-Neural-Network]] — Hamiltonian counterpart
- [[Port-Hamiltonian-Neural-Network]] — dissipative extension on the Hamiltonian side; Lagrangian counterpart uses Rayleigh dissipation
- [[PeRCNN]] — physics-as-architecture sibling for spatial PDEs
- [[PhyArch]] — orthogonal physics-as-architecture sibling: hardwires *spatial symmetries* (parity, periodicity) instead of energy formalism. The two are complementary, not competing.

## Caveat: formalism-level vs all-physics constraint

LNN's structural guarantee is **formalism-level**: any output is guaranteed to come from *some* Lagrangian, so dynamics are automatically conservative *for that learned Lagrangian*. This is not the same as "all physics is enforced." Specifically:

- **Energy structure: enforced** (modulo `L̂ ≈ L` approximation + non-symplectic integration).
- **Spatial symmetries (periodicity, reflection, rotation): NOT enforced.** LNN's MLP can produce parity-violating Lagrangians as easily as parity-respecting ones.

The [[PhyArch-Double-Pendulum-Benchmark]] (Bilal's own work) confirms this empirically — LNN's reflection error is 19.43, *worst* of three models. For systems where spatial symmetries matter as much as energy conservation, LNN alone is insufficient. Composition options:

- **PhyArch + Lagrangian formalism.** Apply parity-split features inside an LNN's scalar Lagrangian net so `L_θ` itself is parity-respecting. Result: both formalism + symmetry hardwired. Untested but architecturally clean.
- **Pure PhyArch** (no Lagrangian formalism). Acceleration directly from invariant-coefficient × equivariant-basis assembly. Loses the energy-conservation formalism but the empirical PhyArch DP result suggests this isn't actually a loss — symmetry compliance protects energy *more effectively* than the Lagrangian formalism does in practice.

## Open Questions

- **Variational integrators.** Symplectic integrators preserve `H` for HNN; the Lagrangian analogue is a *variational integrator* (Marsden-West discrete mechanics) preserving the discrete Euler-Lagrange equation. Pairing LNN with variational integrators is unstudied in the original paper but is the structurally correct match.
- **Lagrangian Graph Network for sparse N-body.** Per-node `L_density` summed over grid neighbors → sparse Hessian → linear-time inversion. For SDA N-body with weak inter-body coupling, the same sparsity should make full-perturbation modeling tractable. Untested for orbital problems specifically.
- **Activation expressiveness.** Softplus's second derivative is `σ(x)(1−σ(x))` — bell-shaped, vanishes at extremes. May limit ability to represent Lagrangians with sharp features (relativistic kinetic energy near `q̇ → c`).
- **Init scheme generalization.** Custom σ formula derived empirically for 2-input systems. Whether it scales to 6-D orbital state or higher is unstudied.

## Sources

- `raw/papers/interpretability/LNN.pdf` — Cranmer, Greydanus, Hoyer, Battaglia, Spergel, Ho 2020 (ICLR Workshop on Deep Differential Equations); paper that introduced this pattern.
