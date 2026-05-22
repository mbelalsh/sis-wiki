---
title: Two-Body Problem
tags: [orbital-mechanics, two-body-problem, kepler, conic-sections, conservation-laws, astrodynamics, foundational]
sources: [raw/books/classical-mechanics/Fundamentals_of_Astrodynamics_VersieRepository_250115.pdf]
created: 2026-05-21
updated: 2026-05-21
sis_relevance: critical
hard_constraint_possible: yes
---

# Two-Body Problem

## One-Line Intuition

Two gravitating point masses reduce to a single body moving in an inverse-square central field; the orbit is a conic section with the central body sitting at a focus, and three things are conserved exactly — energy, angular momentum, and the eccentricity vector.

## Why This Was Invented

Kepler (1609–1619) extracted three empirical laws from Tycho Brahe's observations — orbits are ellipses with the Sun at a focus, the radius sweeps equal areas in equal times, and the period squared scales as the semi-major axis cubed — but he had no *dynamical cause* for them. Newton (*Principia*, 1687) supplied the cause: `F = ma` plus universal gravitation `F ∝ −1/r²` *derives* all three of Kepler's laws. Wakker Ch 5 reverses the historical order and derives Kepler from Newton.

The two-body problem matters because it is the **exactly solvable core of all orbital mechanics**. Everything else — J2 oblateness, atmospheric drag, third-body lunisolar attraction, solar radiation pressure — is a *perturbation* of this conic-section solution. A space-domain-awareness pipeline that does not start from an exact two-body propagator is starting from the wrong place.

## The Math

**Equation of motion.** With `μ = G m_k` the gravitational parameter of the central body (Wakker Eq 5.4; Earth `μ ≈ 398,600 km³/s²`, Sun `μ ≈ 1.327×10¹¹ km³/s²`), the relative motion obeys (Eq 5.3):
```
d²r̄/dt² = −(μ / r³) r̄
```
This is just Newton's second law for the inverse-square force — see [[Newtonian-Mechanics]].

**Conservation law 1 — energy (the vis-viva integral).** Dotting Eq 5.3 with the velocity and integrating gives (Eq 5.5):
```
½ V² − μ/r = ℰ        (ℰ = total energy per unit mass, constant; ℰ < 0 ⇔ bound orbit)
```
`½V²` is kinetic energy per unit mass; `−μ/r` is the gravitational potential per unit mass.

**Conservation law 2 — angular momentum.** Crossing Eq 5.3 with `r̄` and integrating gives (Eq 5.6):
```
r̄ × V̄ = H̄          (H̄ = specific angular momentum, constant — fixes the orbital plane)
```
In the orbital plane with polar angle `φ`, this is `r²φ̇ = H` (Eq 5.7), which immediately yields Kepler's second law: the radius sweeps area at the constant rate `dA/dt = ½H` (Eq 5.8 — true for *any* central force, not just inverse-square).

**Conservation law 3 — the eccentricity vector.** A *third* constant, special to the inverse-square law (Eq 5.32):
```
ē = (1/μ) [ (V² − μ/r) r̄ − (r̄·V̄) V̄ ]
```
`ē` has magnitude `e` and points toward pericenter. It is the **Laplace–Runge–Lenz vector** — its conservation is what makes the inverse-square orbit a *closed* curve that does not precess.

**The orbit — a conic section.** Solving the equations of motion gives the orbital equation (Eq 5.22), with semi-latus rectum `p = H²/μ` and true anomaly `θ` (angle from pericenter):
```
r = (H²/μ) / (1 + e cos θ) = p / (1 + e cos θ)
```
The eccentricity `e` sets the conic type: `e = 0` circle, `0 < e < 1` ellipse, `e = 1` parabola, `e > 1` hyperbola. The central body sits at a **focus**. The constants `(p, e, ω)` — size, shape, orientation — are fixed in time; this is exactly Kepler's first law.

### Code Correspondence

```python
import numpy as np
from scipy.integrate import solve_ivp

mu = 398_600.0  # Earth gravitational parameter, km^3/s^2   (Wakker Eq 5.4)

def two_body(t, s):
    r, v = s[:3], s[3:]
    a = -mu * r / np.linalg.norm(r)**3       # Eq 5.3:  r̈ = -μ r / r³
    return np.concatenate([v, a])

r0 = np.array([7000.0, 0.0, 0.0])            # LEO, ~620 km altitude
v0 = np.array([0.0, 7.546, 0.0])             # near-circular speed √(μ/r)
sol = solve_ivp(two_body, (0, 12000), np.concatenate([r0, v0]),
                rtol=1e-12, atol=1e-12)

r, v = sol.y[:3], sol.y[3:]
energy = 0.5*np.sum(v**2, 0) - mu/np.linalg.norm(r, axis=0)  # Eq 5.5:  ½V² − μ/r
h_vec  = np.cross(r.T, v.T)                                  # Eq 5.6:  r̄ × V̄
# energy and |h_vec| stay flat to ~1e-9 over the integration —
# conservation is a structural property of the dynamics, not something fitted.
```

## Toy Example

Integrate a single LEO orbit (`r ≈ 7000 km`, near-circular). Three plots tell the whole story: (1) the trajectory closes into an ellipse in the plane normal to `H̄`; (2) the specific energy `ℰ` is a flat line; (3) the magnitude `|H̄|` is a flat line. Perturb the initial speed by 1% and the ellipse changes *shape* but `ℰ` and `|H̄|` are still each individually flat — the new orbit conserves its own (different) constants. The conserved quantities are not approximately held; they are held to integrator precision because they are algebraic consequences of Eq 5.3.

## Connection to SiS / CTPC

**The two-body problem is the conservative Hamiltonian core of the CTPC Predictor.** Per [[CTPC-Design-Rationale]] D1, GMAT/SGP4 integrate exactly this — `r̈ = −μr/r³` — plus perturbations. It is `hard_constraint_possible: yes` in the strongest sense: the constraint is not encoded into a neural architecture, it *is* the frozen physics propagator. Two-body motion is exactly [[Hamiltonian-Mechanics|Hamiltonian]] (`H = ½|p|²/m − μm/r`), so energy and angular momentum are conserved by construction — the Predictor never needs a loss penalty to respect them.

**The stability finding (Wakker §5.8) is directly load-bearing for CTPC.** Wakker draws a sharp distinction:

- **Orbital (geometric) stability** — Poincaré's notion: a small change in initial conditions leaves the orbit's *shape and orientation* nearly unchanged. A Keplerian orbit *is* orbitally stable (Eq 5.42: stable for force exponent `n > −3`; Newtonian gravity has `n = −2`).
- **Dynamical (isochronous) stability** — Lyapunov's notion: does the body stay near where it *would have been* at a given time? A Keplerian orbit is **dynamically unstable**. A purely radial perturbation `Δr` produces an along-track angular error that grows **linearly without bound** (Eq 5.39): `Δφ = −2(μ/r₀⁵)^{1/2} (Δr) t`.

Two consequences for SiS:
1. **This is the physics reason long-horizon orbital forecasting accumulates error** — and why Wakker explicitly notes it "leads to a numerical instability in orbit computations." It is the root cause behind [[CTPC-Design-Rationale]] D3's motivation (open-loop forecast error grows unbounded → calibrated uncertainty is mandatory).
2. **It explains why the along-track (Transverse) direction dominates the RTN error budget** — the instability is *along-track by construction*. This grounds D6's choice of the RTN frame: the error is structurally largest in the `t̂` direction, so a frame that isolates that direction is the right modeling coordinate. It also connects to the [[Neural-ODE-Agent]]'s high-Lyapunov-regime caveat for chaotic orbital dynamics.

The eccentricity vector `ē` and energy `ℰ` are also the natural conserved quantities a structure-preserving orbital corrector should not destroy — the standing question "does your method preserve energy, `H`, and `e`?" traces to here.

## Connections

- [[Newtonian-Mechanics]] — the two-body equation of motion is Newton's second law for the inverse-square force
- [[Hamiltonian-Mechanics]] — two-body motion is exactly Hamiltonian; energy + angular momentum conserved by construction
- [[Kepler-Equation]] — converts the conic-section orbit into a position-versus-time relation
- [[Orbital-Elements]] — the six integration constants that label this orbit
- [[Orbital-Perturbations]] — the small forces (J2, drag, third-body, SRP) that perturb this conservative core
- [[Orbital-Mechanics-Agent]] — the Layer-1 agent for which this is the first Tier-1 corpus page
- [[Classical-Mechanics-Agent]] — Layer-0 parent; owns the first-principles central-force derivation (Goldstein Ch 3)
- [[CTPC-Design-Rationale]] — D1 (the Predictor as hard physics), D3 (unbounded forecast error → UQ), D6 (RTN frame)
- [[CTPC-KDD-Submission]] — GMAT propagates exactly this conservative core plus perturbations

## Open Questions

- **Kepler's third law and the orbital period.** `T² ∝ a³` and the semi-major-axis form of vis-viva (`V² = μ(2/r − 1/a)`, `ℰ = −μ/2a`) are proved in Wakker Ch 6, not Ch 5 — the **next ingest** in the Orbital-Mechanics-Agent wave.
- **Orbital elements.** The conic constants `(p, e, ω)` plus the plane orientation give the classical Keplerian element set; the singularity-free equinoctial set is Wakker Ch 11 — a later ingest. This page deliberately stops at the in-plane conic.
- **Perturbations.** Real orbits are two-body *plus* J2/J3, drag, third-body, SRP (Wakker Ch 20–23). The conservative perturbations enter the potential; drag and SRP are non-conservative — see the [[Orbital-Mechanics-Agent]] commitments.
- **The eccentricity vector and regularization.** Wakker §5.7 notes `ē` is used in the regularization of orbit computations (Ch 10) — relevant to numerically stable differentiable propagators for CTPC Q4.
- **Relativistic precession.** Wakker §5.10: the Schwarzschild correction adds `Δω = 6πμ/(H²c²)` per revolution (Eq 5.57; Mercury 43″/century). Negligible for LEO SDA, but the regime where `p ≠ m q̇` and the Lagrangian-side analog would be needed.

## Sources

- `raw/books/classical-mechanics/Fundamentals_of_Astrodynamics_VersieRepository_250115.pdf` — Wakker, *Fundamentals of Astrodynamics*, **Ch 5 "Two-body problem"** (pp 117–156). Equation of motion Eq 5.3–5.4; conservation laws §5.1 (energy Eq 5.5, angular momentum Eq 5.6–5.7, areal velocity Eq 5.8); shape of the orbit §5.2 (orbital equation Eq 5.22, `p = H²/μ`); conic sections §5.3; Kepler's laws §5.4; velocity components and flight path angle §5.6; eccentricity vector / Laplace–Runge–Lenz §5.7 (Eq 5.32–5.33); stability of Keplerian orbits §5.8 (orbital vs dynamical stability, along-track instability Eq 5.39, geometric-stability condition Eq 5.42); Roche limit §5.9; relativistic effects §5.10 (Eq 5.57). First ingest of the Orbital-Mechanics-Agent corpus wave, 2026-05-21.
