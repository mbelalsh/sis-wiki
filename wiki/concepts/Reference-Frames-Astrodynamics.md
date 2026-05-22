---
title: Reference Frames (Astrodynamics)
tags: [astrodynamics, reference-frames, RTN, RSW, ECI, ECEF, clohessy-wiltshire, hill-equations, relative-motion, orbital-mechanics]
sources: [raw/books/classical-mechanics/Fundamentals_of_Astrodynamics_VersieRepository_250115.pdf]
created: 2026-05-21
updated: 2026-05-21
sis_relevance: critical
hard_constraint_possible: yes
---

# Reference Frames (Astrodynamics)

## One-Line Intuition

A position is meaningless without a frame to express it in — and orbital mechanics uses a small, fixed cast of them: inertial **ECI** (where orbital elements live), Earth-fixed **ECEF** (where ground stations live), orbit-plane **perifocal**, and the orbit-relative **RTN** frame, the one CTPC models its corrector errors in.

## Why This Was Invented

The same orbit looks different in different frames, and each task wants a different one: a telescope reports an observation in a topocentric frame; the catalogue stores [[Orbital-Elements|orbital elements]] referred to ECI; an error analysis wants a frame riding *with* the satellite. The skill is to pick the frame in which the structure you care about is simplest — exactly the [[CTPC-Design-Rationale|physics-as-architecture]] instinct, applied to coordinates. The orbit-relative **RTN frame** exists because the *relative* motion of two nearby satellites — or of a real satellite versus its nominal orbit — has a clean linear description there (the Clohessy–Wiltshire equations) that it has in no inertial frame.

## The Math

**ECI — geocentric equatorial inertial** (Wakker §11.3). Origin at Earth's centre, `XY` = equatorial plane, `+X` → vernal equinox ♈, `+Z` → north pole. Non-rotating (quasi-inertial) — the frame in which Newton's law `r̈ = −μr/r³` holds and in which orbital elements are defined.

**ECEF — Earth-Centred Earth-Fixed.** Rotates *with* the Earth; `+X` through the Greenwich meridian. Linked to ECI by the Greenwich sidereal time `θ_GM`. The frame for ground-station positions and sub-satellite tracks.

**Perifocal.** Orbit-plane frame, `+X` toward pericenter, `+Z` along the angular momentum — the natural frame of the in-plane conic.

**RTN — Radial / Transverse / Normal** (Wakker Ch 9; CLAUDE.md also calls it **RSW** = Radial / along-track / cross-track — the same frame). Origin *at the reference satellite*, rotating with it at the orbital mean motion `n`:
- `R̂` — **radial**, outward along the position vector;
- `T̂` — **along-track** (transverse), along the direction of motion;
- `N̂` — **cross-track** (normal), along the orbit normal, completing the right-handed triad.

**The dynamics in the RTN frame — the Clohessy–Wiltshire equations.** For a second body near a reference satellite on a circular orbit, the linearized relative motion is (Wakker Eq 9.7):
```
ẍ − 2n ẏ − 3n² x = f_x      (radial)
ÿ + 2n ẋ        = f_y      (along-track)
z̈ + n² z        = f_z      (cross-track)
```
First derived by Hill (1878), rediscovered by Clohessy & Wiltshire (~1960) for rendezvous. The `2n` terms are **Coriolis** accelerations, the `n²` terms **centrifugal** — both artefacts of working in a rotating frame. Three facts make RTN the right modeling frame:

1. **Cross-track decouples.** `z̈ + n²z = f_z` is an independent harmonic oscillator — cross-track error never mixes into the orbit plane.
2. **Radial and cross-track are purely periodic** (period = the orbital period); only the **along-track** coordinate carries a term that **grows linearly with time** (Wakker Eq 9.12 — the drift term). RTN *isolates the unstable direction into its own coordinate.*
3. The unforced in-plane relative orbit is a **2:1 ellipse** — along-track amplitude twice the radial, eccentricity `e = ½√3` — centred on (or drifting along) the reference satellite.

### Code Correspondence

```python
import numpy as np

def eci_to_rtn(r, v):
    """RTN rotation matrix from the reference satellite's ECI state.
       Rows are the RTN basis vectors expressed in ECI."""
    R_hat = r / np.linalg.norm(r)              # radial — outward
    h     = np.cross(r, v)
    N_hat = h / np.linalg.norm(h)              # cross-track — orbit normal
    T_hat = np.cross(N_hat, R_hat)             # along-track — completes the triad
    Q = np.vstack([R_hat, T_hat, N_hat])       # ECI → RTN
    return Q

# A position error transforms by rotation; a covariance by conjugation:
#   e_RTN = Q @ e_ECI
#   Sigma_RTN = Q @ Sigma_ECI @ Q.T        # the frame-equivariance identity (CTPC Q2)
```

## Toy Example

Put a second satellite 1 km radially above a 400 km-circular reference satellite and integrate the Clohessy–Wiltshire equations. The cross-track coordinate `z` (if perturbed) just oscillates. In the radial–along-track plane the relative trajectory traces the 2:1 CW ellipse — but its centre *slides* steadily in the along-track direction: a purely radial offset converts into an unbounded along-track drift. Plot `x(t)` (bounded, oscillating) against `y(t)` (oscillation + linear ramp) and the asymmetry is the whole story — and it is the same along-track instability that [[Two-Body-Problem]] §5.8 and [[Kepler-Equation]] surfaced, now displayed directly as a coordinate of the modeling frame.

## Connection to SiS / CTPC

**RTN is the CTPC corrector frame — this page is the source for [[CTPC-Design-Rationale]] D6.** CTPC trains and evaluates the corrector on errors expressed in RTN, for two reasons this chapter makes precise:

- **Near-stationarity.** For a near-circular orbit the RTN axes ride with the satellite, so the residual-error distribution is approximately *stationary* — statistically similar all the way round the orbit. In ECI the same error is *non-stationary*: the ECI↔body rotation is time-varying, so the error distribution rotates with the orbit. Stationary errors are far easier to learn. RTN buys that stationarity by construction.
- **It isolates the unstable direction.** The Clohessy–Wiltshire solution shows radial and cross-track errors stay bounded and periodic, while *along-track* (Transverse) carries the linear drift. RTN therefore puts the one structurally-unstable, hardest-to-predict error component into a single named coordinate — exactly the residual the [[Latent-NCDE-Corrector|ML Corrector]] most needs to model. This is why the frame choice is not cosmetic.

**Frame transformations are exact rotations — hence `hard_constraint_possible: yes`.** ECI ↔ RTN is an orthogonal rotation `Q` built deterministically from the state; a position error transforms as `e_RTN = Q e_ECI` and a covariance as `Σ_RTN = Q Σ_ECI Qᵀ`. That conjugation identity is precisely the **frame-equivariance** property [[CTPC-Design-Rationale]] Q2 asks the variance head to respect, and the empirical test `Σ^ECI = Rᵀ Σ^RTN R` to machine precision (the [[Analytic-Sigma-CTPC-Composition]] verification). Frame-equivariance is hard-encodable because the frame map is an exact rotation, not a learned approximation.

**The `g(q)u` maneuver channel.** Wakker §9.4 works out relative motion after an impulsive Δv: a tangential burn produces an along-track displacement `y_2π = −6π ΔV/n` after one revolution — enormous leverage. This relative-motion-after-impulse analysis is the astrodynamics basis for the maneuver-planning interface the port-Hamiltonian `g(q)u` channel banks ([[CTPC-Design-Rationale]] Q8b).

## Connections

- [[Orbital-Elements]] — the classical elements are referred to the ECI frame defined here
- [[Two-Body-Problem]] — RTN's along-track drift is the §5.8 dynamical instability, shown as a coordinate
- [[Kepler-Equation]] — the mean anomaly is the along-track time coordinate the CW drift term tracks
- [[Orbital-Perturbations]] — perturbing accelerations enter the CW equations as the forcing terms `f_x, f_y, f_z`
- [[Orbital-Mechanics-Agent]] — the Layer-1 agent this page serves
- [[CTPC-Design-Rationale]] — D6 (RTN corrector frame), Q2 (variance-head frame-equivariance), Q8b (`g(q)u` maneuver channel)
- [[CTPC-KDD-Submission]] — the corrector is trained and evaluated in RTN
- [[Analytic-Sigma-CTPC-Composition]] — `Σ^ECI = Rᵀ Σ^RTN R` is the frame-equivariance verification test

## Open Questions

- **CW linearization limits.** The Clohessy–Wiltshire equations assume a *circular* reference orbit and small separations — Wakker's rule of thumb: accurate for `nt < 2π` and `|x|/r₁ < 8×10⁻³`, `|y|/r₁, |z|/r₁ < 6×10⁻²`. Eccentric-reference and large-separation regimes need higher-order relative-motion theories.
- **J2-modified relative motion.** The CW equations use the Newtonian potential only; J2 makes the RTN frame itself precess (nodal regression — see [[Orbital-Perturbations]]). J2-relative-motion models matter for long-baseline formations.
- **Time systems for the observation pipeline.** ECEF ↔ ECI needs Greenwich sidereal time, hence UT1; TLE epochs are UTC. The UTC/TAI/TT/UT1 chain (Wakker §11.4) is the practical glue not detailed here.
- **RTN vs LVLH vs Frenet conventions.** Several closely-related orbit-relative frames exist with differing axis orders/signs; pinning the exact convention used in CTPC code against this page is a consistency check.

## Sources

- `raw/books/classical-mechanics/Fundamentals_of_Astrodynamics_VersieRepository_250115.pdf` — Wakker, *Fundamentals of Astrodynamics*. **Ch 9 "Relative motion of two satellites"** (pp 203–217): the rotating RTN frame (radial / along-track / cross-track axes, §9.1); the Clohessy–Wiltshire / Hill equations (Eq 9.1–9.7); analytical solution §9.2 (Eq 9.8–9.13); characteristics of unperturbed relative motion §9.3 (the 2:1 CW ellipse, Eq 9.16, 9.27–9.28, `e = ½√3`; along-track drift); relative motion after an impulsive shot §9.4 (tangential / radial / normal Δv, Eq 9.39–9.43, `y_2π = −6π ΔV/n`). **Ch 11 §11.3** (pp 250–252) for the ECI / ECEF / perifocal / topocentric frame definitions. Sixth ingest of the Orbital-Mechanics-Agent corpus wave, 2026-05-21.
