---
title: Euler-Lagrange Equation
tags: [lagrangian-mechanics, calculus-of-variations, foundational, action-principle]
sources: [raw/papers/interpretability/LNN.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: high
hard_constraint_possible: yes
---

# Euler-Lagrange Equation

## One-Line Intuition

The condition that a trajectory `q(t)` makes the action `S = ∫ L(q, q̇, t) dt` stationary. Equivalent to: `d/dt (∂L/∂q̇) − ∂L/∂q = 0`. This is what *replaces* `F = ma` in the Lagrangian formulation of mechanics — and what [[LNN]] solves numerically to derive accelerations from a learned Lagrangian.

## Why This Was Invented

Lagrange (1788) and Euler (mid-1700s) generalized the calculus of variations to derive equations of motion from a *single* scalar action principle. Two payoffs:

1. **Coordinate independence.** Same equation in any generalized coordinates — Cartesian, polar, joint angles. Newton's `F = ma` requires re-deriving force expressions in each coordinate system.
2. **Constraint handling.** Holonomic constraints (bead on a wire, rigid body) handled by Lagrange multipliers without ad-hoc force decomposition.

For machine learning, Euler-Lagrange is the bridge between "scalar Lagrangian function" and "trajectory dynamics." Given a learned `L_θ(q, q̇)`, the equation tells you how to compute accelerations `q̈`.

## The Math

**Derivation (sketch).** The action `S[q] = ∫_{t_0}^{t_1} L(q, q̇, t) dt`. Vary the path: `q(t) → q(t) + ε η(t)` with `η(t_0) = η(t_1) = 0`. Stationarity `δS/δε|_{ε=0} = 0` for arbitrary `η`. Integration by parts on the `q̇` term, boundary terms vanish by endpoint conditions. What remains:
```
∫ [ ∂L/∂q − d/dt (∂L/∂q̇) ] · η dt = 0
```
For arbitrary `η`, the bracket must vanish:
```
d/dt (∂L/∂q̇_j) − ∂L/∂q_j = 0       for j = 1, …, d
```

**Vectorized form** (Eq. 4 of [[LNN]] paper):
```
d/dt ∇_q̇ L = ∇_q L
```

**Solved for q̈.** Expand the time derivative by chain rule:
```
(∇_q̇ ∇_q̇^T L) q̈ + (∇_q ∇_q̇^T L) q̇ = ∇_q L
```

The matrix `M(q, q̇) := ∇_q̇ ∇_q̇^T L` is the **velocity-Hessian** of `L` — for mechanical systems with `L = ½ q̇^T M(q) q̇ − V(q)`, this is just the (q-dependent) mass matrix `M(q)`.

Inverting:
```
q̈ = M^(-1) [ ∇_q L − (∇_q ∇_q̇^T L) q̇ ]                              (LNN Eq. 6)
```

This is the operation [[LNN]] computes via second-order autograd at every forward pass. `O(d³)` cost from the matrix inversion.

**Non-degeneracy condition.** The forward-dynamics computation requires `M` to be invertible. Lagrangians where `M` is singular are called *degenerate* — they correspond to systems with constraints baked into the Lagrangian (gauge theories). For non-degenerate Lagrangians, the Legendre transform to [[Hamiltonian-Mechanics]] is well-defined; for degenerate ones, you need Dirac's constrained-Hamiltonian formalism.

### Code Correspondence (JAX, from LNN App. A)

```python
import jax, jax.numpy as jnp

def euler_lagrange_acceleration(L_fn, q, q_t):
    """Solve d/dt(∂L/∂q̇) = ∂L/∂q for q̈ given L(q, q̇)."""
    M     = jax.hessian(L_fn, 1)(q, q_t)                      # ∇_q̇∇_q̇^T L  (mass matrix)
    grad_q = jax.grad(L_fn, 0)(q, q_t)                         # ∇_q L
    cross = jax.jacobian(jax.jacobian(L_fn, 1), 0)(q, q_t)     # ∇_q∇_q̇^T L
    return jnp.linalg.pinv(M) @ (grad_q - cross @ q_t)
```

The full forward dynamics in 4 lines — this is the canonical example of why higher-order autograd matters for physics-informed ML.

## Toy Example

**Free particle: `L = ½ m q̇²`.**
- `∂L/∂q̇ = m q̇` → `d/dt(m q̇) = m q̈`
- `∂L/∂q = 0`
- Euler-Lagrange: `m q̈ = 0` → constant velocity. ✓

**Particle in gravity: `L = ½ m q̇² − m g q`.**
- `∂L/∂q̇ = m q̇` → `m q̈`
- `∂L/∂q = −m g`
- Euler-Lagrange: `m q̈ = −m g` → `q̈ = −g`. ✓ (Worked example in App. B of LNN paper.)

**Pendulum: `L = ½ m l² θ̇² − m g l (1 − cos θ)`.**
- `∂L/∂θ̇ = m l² θ̇` → `m l² θ̈`
- `∂L/∂θ = −m g l sin θ`
- Euler-Lagrange: `θ̈ = −(g/l) sin θ`. ✓

## Connection to SiS / CTPC

The Euler-Lagrange equation is the **mathematical object that makes [[Lagrangian-Neural-Network]] possible**:

- LNN forward pass = solve Euler-Lagrange for `q̈` given a learned scalar `L_θ`
- Loss = MSE between predicted and observed `q̈`
- Conservation of energy = automatic consequence of `∂L/∂t = 0` (Noether)
- For CTPC: same role as [[Symplectic-Gradient]] plays for [[Hamiltonian-Neural-Network]]; pick whichever fits the available coordinates

**Variational integrators.** Discrete-time analogue: instead of integrating `q̈` with RK4, use a *discrete Euler-Lagrange equation* (Marsden-West discrete mechanics) that preserves the discrete action exactly. Pairs naturally with LNN; preserves conservation properties to machine precision over arbitrarily long horizons. Not used in the original paper — open extension.

**Rayleigh dissipation.** For non-conservative systems, augment Euler-Lagrange:
```
d/dt(∂L/∂q̇) − ∂L/∂q + ∂D/∂q̇ = 0
```
where `D(q̇)` is the *Rayleigh dissipation function*. This is the Lagrangian counterpart to [[Port-Hamiltonian-Neural-Networks]]'s explicit dissipation operator — the symmetric extension for friction/drag/SRP in CTPC.

## Connections

- [[Lagrangian-Mechanics]] — the framework where this equation lives
- [[Lagrangian-Neural-Network]] — the NN architecture that solves it numerically
- [[LNN]] — paper that ties it to ML
- [[Hamiltonian-Mechanics]] — equivalent formulation via Legendre transform
- [[Symplectic-Gradient]] — the Hamiltonian-side analogue
- [[Hamiltonian-vs-Lagrangian-Duality]] — when to compute dynamics via E-L vs symplectic gradient

## Open Questions

- **Variational integrators.** Discrete-time Euler-Lagrange that preserves the discrete action exactly. Standard in computational mechanics (Marsden-West) but unused in the LNN paper. Pairing should improve long-horizon stability.
- **Constrained Lagrangians.** Holonomic: Lagrange multipliers on the constraint manifold. Non-holonomic: D'Alembert vs. variational principles diverge. For SiS attitude dynamics (unit-quaternion constraint), the choice matters.
- **Field theory generalization.** Euler-Lagrange for `L(φ, ∂_μ φ)` — covariant equations of motion for fields. The Lagrangian Graph Network in the LNN paper is the discretized version of this. Bridge to [[PeRCNN]]-style PDE learning is open.
- **Singular mass matrices.** When `M = ∇_q̇ ∇_q̇^T L` is non-invertible, the Lagrangian is degenerate (gauge theories, constrained systems). LNN uses pseudoinverse `pinv` as a fallback — proper handling needs Dirac's constrained-Hamiltonian formalism. Untested for SiS use cases.

## Sources

- `raw/papers/interpretability/LNN.pdf` — Eqs. 3–6 (E-L equation through to solved-for-q̈ form), App. A (4-line JAX implementation), App. B (worked example: ball in gravity).
- `raw/books/classical-mechanics/GoldsteinPooleSafkoClassicalMechanics.pdf` — primary reference Ch. 2, *not yet ingested*.
