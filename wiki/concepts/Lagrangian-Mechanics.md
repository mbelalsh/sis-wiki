---
title: Lagrangian Mechanics
tags: [classical-mechanics, lagrangian, conservation-laws, action-principle, foundational]
sources: [raw/papers/interpretability/LNN.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: critical
hard_constraint_possible: yes
---

# Lagrangian Mechanics

## One-Line Intuition

A reformulation of classical mechanics centered on a scalar function `L(q, q̇) = T − V` (kinetic minus potential energy). Physical trajectories are those that make the *action* `S = ∫L dt` stationary; equivalently, they satisfy the [[Euler-Lagrange-Equation]]. Works in any generalized coordinates — no canonical-momentum requirement.

## Why This Was Invented

Newton's `F = ma` is coordinate-dependent and awkward for systems with constraints (pendulum on a rotating frame, double pendulum, rigid body with multiple joints). Lagrange (1788) reformulated mechanics around a scalar action principle: nature picks the path that extremizes `S = ∫(T − V) dt`. The resulting equations of motion are coordinate-invariant — they look the same in Cartesian, polar, spherical, or any other choice of generalized coordinates `q = (q_1, …, q_d)`.

For machine learning, this is the natural framework because:

1. The physically meaningful quantity is a single *scalar* function `L(q, q̇)` — much smaller hypothesis space than learning a `2d`-dimensional vector field directly.
2. Conservation of energy follows automatically when `L` has no explicit time-dependence (Noether's theorem applied to time-translation symmetry).
3. The coordinates `(q, q̇)` are observable directly — unlike canonical momenta `p = ∂L/∂q̇`, which require knowing `L` to compute.

[[LNN]] exploits all three properties to learn dynamics as a parameterized Lagrangian.

## The Math

**Generalized coordinates.** A system with `d` degrees of freedom has configuration `q = (q_1, …, q_d)`. The choice of coordinates is free — Cartesian, polar, joint angles, anything. Velocities `q̇ = dq/dt`.

**Lagrangian.** A scalar function `L(q, q̇, t)`. For mechanical systems, `L = T − V` (kinetic minus potential energy). Common forms:

- Free particle: `L = ½ m q̇²`
- Particle in potential: `L = ½ m q̇² − V(q)`
- Relativistic free particle: `L = m c² (1 − √(1 − q̇²/c²))` (or equivalent gauge forms)
- Rigid body: `T = ½ q̇^T M(q) q̇` where `M` is the inertia matrix (this is the DeLaN-restricted form)

**Action.** `S[q] = ∫_{t_0}^{t_1} L(q, q̇, t) dt` — a functional of the trajectory.

**Action principle (Hamilton's principle).** Physical trajectories are those for which `δS = 0` — small variations of the path leave the action stationary.

**Euler-Lagrange equation.** Stationarity of `S` is equivalent to:
```
d/dt (∂L/∂q̇_j) − ∂L/∂q_j = 0   for j = 1, …, d
```
See [[Euler-Lagrange-Equation]] for the derivation and how to solve for `q̈`.

**Conservation from symmetry (Noether's theorem).** Each continuous symmetry of `L` corresponds to a conserved quantity:

- Time-translation symmetry (`∂L/∂t = 0`) → energy conservation
- Spatial-translation symmetry → linear momentum conservation
- Rotational symmetry → angular momentum conservation

**Connection to [[Hamiltonian-Mechanics]].** Define canonical momentum `p_i = ∂L/∂q̇_i`, then Legendre-transform: `H(q, p) = Σ p_i q̇_i − L(q, q̇)`. The two formalisms are equivalent for non-degenerate Lagrangians (where the velocity-Hessian `∂²L/∂q̇_i ∂q̇_j` is invertible — the same matrix LNN inverts).

### Code Correspondence

```python
import numpy as np
from scipy.integrate import solve_ivp

def lagrangian_dynamics(dLdq, d2Ldqdotdqdot, d2Ldqdqdot, x0, t_span):
    """Integrate the Euler-Lagrange equation forward."""
    d = len(x0) // 2
    def rhs(t, x):
        q, q_t = x[:d], x[d:]
        M = d2Ldqdotdqdot(q, q_t)
        rhs_term = dLdq(q, q_t) - d2Ldqdqdot(q, q_t) @ q_t
        q_tt = np.linalg.solve(M, rhs_term)
        return np.concatenate([q_t, q_tt])
    return solve_ivp(rhs, t_span, x0, rtol=1e-9)
```

## Toy Example

**Mass-spring (1 DoF).** `L = ½ m q̇² − ½ k q²`. Then `∂L/∂q̇ = m q̇`, `∂L/∂q = −k q`, so Euler-Lagrange gives `m q̈ + k q = 0` — simple harmonic motion. Same dynamics as the Hamiltonian formulation, derived from a single scalar `L`.

**Pendulum.** `L = ½ m l² θ̇² − m g l (1 − cos θ)`. Gives `θ̈ = −(g/l) sin θ` directly.

**Double pendulum.** `L = ½(m_1+m_2) l_1² θ̇_1² + ½ m_2 l_2² θ̇_2² + m_2 l_1 l_2 θ̇_1 θ̇_2 cos(θ_1 − θ_2) + (m_1+m_2) g l_1 cos θ_1 + m_2 g l_2 cos θ_2`. Velocity-Hessian is a non-trivial 2×2 matrix → must be inverted to get `(θ̈_1, θ̈_2)`. This is the LNN paper's headline benchmark.

## Connection to SiS / CTPC

Lagrangian mechanics is the **coordinate-free foundation** for SiS architectures targeting conservative dynamics:

- **Position-velocity is the natural observable.** TLE data, optical observations, radar tracks all give `(r, v)`. Lagrangian formulation works directly; Hamiltonian formulation requires constructing canonical momenta first.
- **Coordinate freedom matches CTPC.** CTPC uses RTN/RSW frames for the corrector. These are not canonical (no symplectic structure baked in), but they *are* perfectly good generalized coordinates for a Lagrangian formulation.
- **Decomposition for CTPC.** Conservative core (SGP4 + Keplerian + zonal perturbations) → Lagrangian. Dissipative residual (drag, SRP) → Rayleigh dissipation function `D(q̇)`. Same decomposition philosophy as Hamiltonian + Port, on the symmetric Lagrangian side.
- **Action-principle interpretability.** "What trajectory minimizes the action under the learned Lagrangian?" is a more physically meaningful question than "what trajectory follows the learned vector field?" The action is a single-valued functional that's human-interpretable.

## Connections

- [[Euler-Lagrange-Equation]] — the equation of motion derived from the action principle
- [[Lagrangian-Neural-Network]] — the architecture that parameterizes `L` with a neural net
- [[LNN]] — paper that introduced the NN parameterization
- [[Hamiltonian-Mechanics]] — Legendre-dual formulation; `H(q,p) = p·q̇ − L(q,q̇)`
- [[Hamiltonian-vs-Lagrangian-Duality]] — design-decision synthesis for choosing between formulations
- [[Symplectic-Gradient]] — the Hamiltonian-side analogue of the Euler-Lagrange operator

## Open Questions

- **Lagrangians and constraints.** Holonomic constraints (rigid body, unit quaternion for attitude) handled cleanly with Lagrange multipliers. Non-holonomic (rolling without slipping, certain wheel dynamics) are subtler — D'Alembert vs. variational principles diverge.
- **Gauge equivalence.** `L` and `L + df/dt` give identical dynamics for any function `f(q, t)`. The learned `L_θ` from LNN isn't unique — only the equivalence class matters. Open question whether this gauge freedom helps or hurts optimization.
- **Goldstein / Arnold not yet ingested.** `raw/books/classical-mechanics/GoldsteinPooleSafkoClassicalMechanics.pdf` Ch. 1–2 (Lagrangian formulation) and `Arnold_1989_MMCM.pdf` (mathematical methods of classical mechanics, geometric treatment) are the primary references. Future ingest will substantially expand this page.
- **Field-theoretic Lagrangians.** Continuous systems use Lagrangian *density* `ℒ(φ, ∂_μ φ)` integrated over space-time. The wave equation in the LNN paper's Lagrangian Graph Network is the discretized version of this. Bridge to [[PeRCNN]]-style PDE learning is open synthesis territory.

## Sources

- `raw/papers/interpretability/LNN.pdf` — Sec. 2 review of Lagrangian formalism (Eqs. 2–3), Eq. 1 (Poisson-bracket relations defining canonical), App. B (worked example: ball in gravity).
- `raw/books/classical-mechanics/GoldsteinPooleSafkoClassicalMechanics.pdf` — primary reference, *not yet ingested*. Ch. 1–2 are the canonical treatment.
- `raw/books/classical-mechanics/Arnold_1989_MMCM.pdf` — alternative geometric/mathematical treatment, *not yet ingested*.
