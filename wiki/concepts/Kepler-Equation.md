---
title: Kepler's Equation
tags: [orbital-mechanics, kepler-equation, anomalies, orbital-period, vis-viva, astrodynamics, foundational]
sources: [raw/books/classical-mechanics/Fundamentals_of_Astrodynamics_VersieRepository_250115.pdf]
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: yes
---

# Kepler's Equation

## One-Line Intuition

`E − e sin E = M` is the transcendental bridge between *time* and *position* in an orbit: it converts elapsed time (carried by the mean anomaly `M`, which ticks uniformly) into the geometric angle (the eccentric anomaly `E`, hence the true anomaly `θ`).

## Why This Was Invented

The [[Two-Body-Problem|two-body]] orbital equation `r = p/(1 + e cos θ)` gives the *shape* of the orbit but says nothing about *when* the body is where. Timing is hard because Kepler's second law makes the body race through pericenter and crawl through apocenter — `θ(t)` is not an elementary function. Kepler (c. 1618) introduced an auxiliary angle, the *eccentric anomaly* `E`, that linearizes the time relation into the simple transcendental form `E − e sin E = M`. Every orbit propagator that advances a satellite in time through its two-body arc — SGP4, GMAT's Keplerian core, any TLE-to-state conversion — solves Kepler's equation.

## The Math

**Orbit geometry (Wakker §6.1).** An ellipse has semi-major axis `a` (size), eccentricity `e` (shape), and semi-minor axis `b = a√(1−e²)`. Pericenter and apocenter radii (Eq 6.4): `r_p = a(1−e)`, `r_a = a(1+e)`. The semi-latus rectum links to `a` (Eq 6.2): `p = a(1−e²)`, so (Eq 6.3) `r = a(1−e²)/(1 + e cos θ)`.

**Energy fixes size — the vis-viva integral (Eq 6.13, 6.14, 6.21).** The total specific energy `ℰ = V²/2 − μ/r` is constant and negative for a bound orbit, and it determines the semi-major axis alone:
```
a = −μ / (2ℰ)              V² = μ (2/r − 1/a)   ← vis-viva integral (Eq 6.21)
```
The vis-viva equation gives speed from position without integrating the orbit.

**Period — Kepler's third law (Eq 6.25, 6.27, 6.29).**
```
T = 2π √(a³/μ)         n = 2π/T = √(μ/a³)         a³/T² = μ/(4π²) = constant
```
`n` is the *mean motion* (mean angular rate). Kepler's third law `a³/T² = const` is *derived* here from Newton, not assumed.

**The three anomalies.** Three angles measure progress around the orbit, all zero at pericenter:
- **True anomaly `θ`** — the actual geometric angle from pericenter. Non-uniform in time.
- **Eccentric anomaly `E`** — an auxiliary angle on the circumscribing circle. Related to radius by `r = a(1 − e cos E)` (Eq 6.33) and to `θ` by (Eq 6.35): `tan(θ/2) = √((1+e)/(1−e)) · tan(E/2)`.
- **Mean anomaly `M`** — *uniform in time*: `M = n(t − τ)`, with `τ` the time of pericenter passage. `M` is a fictitious angle, but it is the one that ticks at a constant rate.

**Kepler's equation (Eq 6.36)** ties the time-uniform angle to the geometric one:
```
E − e sin E = M = n (t − τ)
```
Going *position → time* is trivial (plug in `E`). Going *time → position* — the case orbit propagation needs — requires solving this transcendental equation for `E` given `M`.

### Code Correspondence

```python
import numpy as np

def kepler_propagate(a, e, t, tau, mu=398_600.0):
    """Two-body propagation: time t -> (radius r, true anomaly theta)."""
    n = np.sqrt(mu / a**3)                 # Eq 6.27: mean motion
    M = n * (t - tau)                      # Eq 6.36: mean anomaly (uniform in time)

    E = M                                  # Newton-Raphson on  E - e sinE - M = 0
    for _ in range(50):                    # (Wakker §6.6)
        E -= (E - e*np.sin(E) - M) / (1 - e*np.cos(E))

    r = a * (1 - e*np.cos(E))              # Eq 6.33: radius from eccentric anomaly
    theta = 2*np.arctan2(np.sqrt(1+e)*np.sin(E/2),   # Eq 6.35: E -> true anomaly
                         np.sqrt(1-e)*np.cos(E/2))
    return r, theta
```

## Toy Example

Take an eccentric orbit (`e = 0.25`, `a = 9000 km`) and step `M` uniformly from `0` to `2π`. Plotting `θ` against `M` gives an S-curve, not a straight line: the body covers most of `θ` quickly near pericenter and barely advances near apocenter. Wakker's Figure 6.8 makes the operational point — a satellite with apogee at 200,000 km altitude spends *half its period* within a 24°-wide arc of true anomaly around apogee. Kepler's equation is exactly what encodes that non-uniformity.

## Connection to SiS / CTPC

**Kepler's equation is how the CTPC Predictor advances a satellite in time within its two-body core.** A [[Two-Body-Problem|two-body]] arc is propagated by: elapsed time → mean anomaly `M = n(t−τ)` → solve `E − e sin E = M` → true anomaly and radius → Cartesian state. SGP4 and GMAT's Keplerian core do exactly this; it is `hard_constraint_possible: yes` — exact, parameter-free two-body propagation (per [[CTPC-Design-Rationale]] D1).

**The TLE connection.** A Two-Line Element set (CLAUDE.md domain vocabulary, the Space-Track state representation that feeds SGP4) literally stores the **mean anomaly `M₀`**, the **mean motion `n`**, and the **eccentricity `e`** at an epoch. Kepler's equation is the function that turns those three numbers into a position. The mean anomaly is chosen as the stored time-element precisely *because* it is uniform in time — `M = M₀ + n(t − t₀)` is exact in the two-body model.

**Link to the along-track instability.** [[Two-Body-Problem]] §5.8 showed a Keplerian orbit is dynamically unstable along-track. Kepler's equation localizes that: the mean anomaly `M` *is* the along-track time coordinate, and a small error in `n` (hence in `a`, via `n = √(μ/a³)`) produces a mean-anomaly error growing **linearly** as `Δn·(t−t₀)`. This is the mechanism behind the Transverse component dominating the RTN error budget ([[CTPC-Design-Rationale]] D6) — and it is exactly the residual signal the ML Corrector must learn.

## Connections

- [[Two-Body-Problem]] — supplies the conic-section orbit whose timing Kepler's equation resolves
- [[Newtonian-Mechanics]] — the two-body equation of motion underneath
- [[Hamiltonian-Mechanics]] — action-angle variables are the canonical formalization of `(M, ...)` as a uniformly-advancing coordinate
- [[Orbital-Mechanics-Agent]] — the Layer-1 agent this page serves
- [[CTPC-Design-Rationale]] — D1 (the Predictor), D6 (RTN frame / along-track error)
- [[CTPC-KDD-Submission]] — SGP4/GMAT propagate the two-body core via Kepler's equation; TLEs carry `M`, `n`, `e`

## Open Questions

- **Lambert's problem.** Wakker §6.7: the time-of-flight between two *positions* (rather than between a position and time) is Lambert's theorem — the basis of preliminary orbit determination (Curtis Ch 5, a later ingest). Not covered on this page.
- **Series convergence for high eccentricity.** The Fourier/Bessel series `E(M)`, `θ(M)` (Wakker Eq 6.41, 6.45) converge only for `e < 0.663` (Laplace) and converge slowly for `e > 0.5`. Most satellite orbits have `e ≪ 0.3`, so the iterative solution is fine; high-eccentricity transfer orbits need care.
- **Perturbed periods.** The sidereal period `T = 2π√(a³/μ)` is the two-body value. Real orbits have anomalistic and draconic periods that differ under J2 — Wakker §23.3, part of the [[Orbital-Perturbations]] picture.
- **Hyperbolic / parabolic analogs.** Kepler's equation has distinct forms for `e ≥ 1` (Wakker Ch 7–8) — relevant to interplanetary and hyperbolic-flyby regimes, outside the LEO SDA scope.

## Sources

- `raw/books/classical-mechanics/Fundamentals_of_Astrodynamics_VersieRepository_250115.pdf` — Wakker, *Fundamentals of Astrodynamics*, **Ch 6 "Elliptical and circular orbits"** (pp 157–180). Orbit geometry §6.1 (Eq 6.2–6.5); energy and `a` (Eq 6.13–6.14); circular orbit §6.2 (Eq 6.18–6.19); velocity and period §6.3 (vis-viva Eq 6.21, Eq 6.22–6.25); mean motion (Eq 6.26–6.27); Kepler's third law §6.4 (Eq 6.28–6.29); Kepler's equation §6.5 (eccentric anomaly Eq 6.31–6.33, true↔eccentric Eq 6.35, Kepler's equation Eq 6.36); graphical/analytical solution §6.6 (Newton–Raphson, Fourier/Bessel series Eq 6.41, 6.45); Lambert's theorem §6.7. Second ingest of the Orbital-Mechanics-Agent corpus wave, 2026-05-21.
