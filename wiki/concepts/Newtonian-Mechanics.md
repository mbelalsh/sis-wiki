---
title: Newtonian Mechanics
tags: [classical-mechanics, newtonian, foundational, reference-frames, conservation-laws]
sources: [raw/books/classical-mechanics/GoldsteinPooleSafkoClassicalMechanics.pdf, raw/books/classical-mechanics/Arnold_1989_MMCM.pdf]
created: 2026-05-21
updated: 2026-05-21
sis_relevance: critical
hard_constraint_possible: yes
---

# Newtonian Mechanics

> **Seed stub.** Created 2026-05-21 during the [[Classical-Mechanics-Agent]] build to complete
> the three-formalism Tier-1 corpus ([[Newtonian-Mechanics]] + [[Lagrangian-Mechanics]] +
> [[Hamiltonian-Mechanics]]). Content is standard classical mechanics; Goldstein Ch 1 and
> Arnold Ch 1–2 are the canonical references, **not yet ingested**. A future ingest will
> expand this page with verified equation numbers.

## One-Line Intuition

Specify the forces on a body and Newton's second law turns them into a trajectory: `F = m a` is a second-order ODE whose solution, given an initial position and velocity, is the entire future and past of the motion.

## Why This Was Invented

Before Newton (*Principia*, 1687), terrestrial motion (projectiles, pendulums) and celestial motion (planets, the Moon) were described by separate, largely empirical rules — Kepler's laws for orbits, Galileo's kinematics for falling bodies. Newton unified them: **one law of motion plus one law of gravitation reproduces both.** The payoff is a *predictive, deterministic* framework — the state `(r, ṙ)` at one instant determines the state at every other instant by integrating an ODE.

The Newtonian formulation is the natural starting point because it is the most *direct*: you write down the actual physical forces. Its weaknesses — coordinate-dependence and the awkwardness of constraint forces — are exactly what motivated the [[Lagrangian-Mechanics|Lagrangian]] and [[Hamiltonian-Mechanics|Hamiltonian]] reformulations. All three predict identical trajectories; they differ only in which mathematical object you write down first.

## The Math

**The three laws.**
1. **Inertia.** A body's velocity is constant unless a net force acts. This defines the *inertial frames* in which laws 2–3 hold.
2. **`F = ma`.** Net force equals mass times acceleration: `F = m r̈`. More fundamentally, `F = dp/dt` with momentum `p = m ṙ`.
3. **Action–reaction.** Forces come in equal-and-opposite pairs: `F_{ij} = −F_{ji}`.

**State and equations of motion.** A single particle has state `(r, ṙ) ∈ ℝ⁶`. For `N` interacting particles:
```
m_i r̈_i = Σ_{j≠i} F_{ij} + F_i^ext        i = 1, …, N
```

**Conservative forces.** A force is *conservative* if `F = −∇V(r)` for a scalar potential `V`. Gravity, electrostatics, and the spring force are conservative; drag and friction are not. This split — conservative force from a potential, non-conservative force handled separately — is the seed of the entire physics-as-architecture philosophy.

**Conservation laws** (each follows from a symmetry — formalized by Noether's theorem in the [[Lagrangian-Mechanics|Lagrangian]] picture):
- **Linear momentum** `P = Σ m_i ṙ_i` is conserved if there is no net external force (space-translation symmetry).
- **Angular momentum** `L = Σ r_i × m_i ṙ_i` is conserved if there is no net external torque (rotational symmetry).
- **Energy** `E = T + V` with `T = Σ ½ m_i |ṙ_i|²` is conserved if all forces are conservative and time-independent (time-translation symmetry).

**Galilean invariance.** Newton's laws take the same form in every inertial frame; frames related by `r → r + v t` (constant boost) are physically equivalent. This is the symmetry group of Newtonian mechanics.

### Code Correspondence

```python
import numpy as np
from scipy.integrate import solve_ivp

def newton_flow(force, mass, state0, t_span):
    """Integrate m r̈ = F(r) given state0 = [r, v]."""
    def rhs(t, s):
        r, v = np.split(s, 2)
        a = force(r) / mass          # a = F / m   (Newton's second law)
        return np.concatenate([v, a])  # d/dt [r, v] = [v, a]
    return solve_ivp(rhs, t_span, state0, rtol=1e-10, atol=1e-12)

# Two-body gravity (reduced one-body form): r̈ = -μ r / |r|³
mu = 1.0
sol = newton_flow(
    force = lambda r: -mu * r / np.linalg.norm(r)**3,  # F = -∇V,  V = -μ/|r|
    mass  = 1.0,
    state0 = np.array([1.0, 0.0, 0.0, 0.95]),  # position (1,0), velocity (0,0.95)
    t_span = (0, 20),
)
# sol traces a closed ellipse — a Keplerian orbit, energy + angular momentum conserved
```

## Toy Example

**The Kepler orbit (one-body reduction of the two-body problem).** Two gravitating masses reduce, after separating the center-of-mass motion, to a single particle of reduced mass `μ_r = m₁m₂/(m₁+m₂)` moving in the central potential `V(r) = −G m₁ m₂ / |r|`. Newton's equation `r̈ = −μ r/|r|³` (with `μ = G(m₁+m₂)`) integrates to a conic section — ellipse, parabola, or hyperbola — with energy and angular momentum exactly conserved. This is the conservative core that [[Orbital-Mechanics-Agent|orbital mechanics]] perturbs with J2, drag, and third-body terms.

**Projectile with drag (the conservative/dissipative split made concrete).** `m r̈ = −m g ẑ − b |ṙ| ṙ`. The gravity term `−m g ẑ = −∇V` is conservative; the quadratic-drag term `−b|ṙ|ṙ` is not — it removes energy, `dE/dt = −b|ṙ|³ ≤ 0`. The trajectory needs both pieces, and they enter through *different* mechanisms. This is the Newtonian-picture preview of the [[Rayleigh-Dissipation-Function|Rayleigh dissipation function]] and the port-Hamiltonian `(J − R)` split.

## Connection to SiS / CTPC

**Newtonian mechanics is the hard-constraint layer of CTPC.** Per [[CTPC-Design-Rationale]] D1 and the Core Design Philosophy: the Predictor (GMAT / SGP4) *is* Newton's equations integrated — `r̈ = −μ r/|r|³ + perturbations` — with zonal harmonics, lunisolar third-body terms, drag, and SRP added as known forces. It has no trainable parameters: the physics is baked in by construction. This is hard-constraint-possible **yes** in the strongest sense — the constraint is not encoded into a neural architecture, it *is* the frozen numerical propagator.

Three specific ties:
- **The orbital equation of motion** `r̈ = −μ r/|r|³ + a_pert` is Newton's second law. The conservative perturbations (J2, J3, third-body) enter as `−∇V`; the dissipative ones (drag, SRP) do not — they are the residual the [[Latent-NCDE-Corrector|ML Corrector]] is left to model.
- **Conservation as architecture.** Orbital mechanics is "Hamiltonian + small dissipative perturbations" ([[CTPC-Design-Rationale]] Q8). The Predictor respects energy and angular-momentum conservation because it integrates the real forces — not because a loss penalizes drift. That is the SiS hierarchy step 1.
- **The foundation the other two formalisms are built on.** [[Lagrangian-Mechanics]] and [[Hamiltonian-Mechanics]] are reformulations of *this* — derived from `F = ma` via D'Alembert's principle and the Legendre transform respectively. Every HNN/LNN/PHNN claim ultimately reduces to a Newtonian statement.

## Connections

- [[Lagrangian-Mechanics]] — reformulation via the action principle; coordinate-free, handles constraints cleanly; derived from Newton via D'Alembert's principle
- [[Hamiltonian-Mechanics]] — phase-space reformulation; the Legendre transform of the Lagrangian
- [[Euler-Lagrange-Equation]] — the Lagrangian-picture equation of motion that replaces `F = ma`
- [[Rayleigh-Dissipation-Function]] — the standard way to carry non-conservative forces (drag, friction) into the Lagrangian/Hamiltonian formalisms
- [[Two-Body-Problem]] — the inverse-square central-force orbit in full: conic sections + energy / angular-momentum / eccentricity-vector conservation
- [[Classical-Mechanics-Agent]] — the Layer-0 expert agent for which this page is Tier-1 corpus
- [[Orbital-Mechanics-Agent]] — Layer-1 agent applying Newtonian mechanics to the orbital domain
- [[CTPC-Design-Rationale]] — D1: the Predictor as the hard Newtonian physics layer
- [[CTPC-KDD-Submission]] — GMAT is a real-world Newtonian propagator

## Open Questions

- **Constraint forces.** Holonomic constraints (a bead on a wire, a rigid body, the unit-quaternion constraint for spacecraft attitude) require *constraint forces* that do no work but are awkward to write down in the Newtonian picture — they are unknowns solved alongside the motion. This is the single biggest reason to move to the [[Lagrangian-Mechanics|Lagrangian]] formulation. Non-holonomic constraints (rolling without slipping) are subtler still.
- **When to leave the Newtonian picture.** Direct force models and inertial-frame problems are cleanest in Newtonian form; constrained or arbitrary-coordinate problems want Lagrangian; phase-space / canonical-structure / symplectic-integrator problems want Hamiltonian. The decision rule belongs on [[Hamiltonian-vs-Lagrangian-Duality]].
- **Goldstein Ch 1 / Arnold Ch 1–2 not yet ingested.** `raw/books/classical-mechanics/GoldsteinPooleSafkoClassicalMechanics.pdf` Ch 1 (Elementary Principles, incl. constraints and D'Alembert's principle) and `Arnold_1989_MMCM.pdf` Ch 1–2 (the geometric/experimental foundation) are the canonical references. A future ingest will replace this seed stub with verified equation numbers.

## Sources

- `raw/books/classical-mechanics/GoldsteinPooleSafkoClassicalMechanics.pdf` — Ch 1 *Elementary Principles* (pp 1–33; constraints, D'Alembert's principle, §1.5 dissipation function). **Canonical reference — not yet ingested.**
- `raw/books/classical-mechanics/Arnold_1989_MMCM.pdf` — Part I *Newtonian Mechanics*, Ch 1–2 (pp 3–54; experimental facts, investigation of the equations of motion). **Canonical reference — not yet ingested.**
- This page is a seed stub written from standard classical-mechanics knowledge to bootstrap the [[Classical-Mechanics-Agent]] corpus; no equation or section numbers are cited beyond chapter-level ranges verified against [[book-routing-map]].
