---
title: GITM (Global Ionosphere–Thermosphere Model)
tags: [thermosphere, ionosphere, space-weather, atmospheric-drag, GITM, physics-based-model, sda, Q6]
sources:
  - raw/Proposals/NOAA_Proposal/weather_modeling/GITM_Ridley.pdf   # Ridley, Deng & Tóth 2006, JASTP 68:839-864
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: yes
---

# GITM — Global Ionosphere–Thermosphere Model (Ridley, Deng & Tóth 2006)

## One-Line Intuition

A first-principles, **non-hydrostatic** model of Earth's thermosphere and ionosphere — it solves the actual continuity / momentum / energy conservation equations on an *altitude* grid — and it is the physics-based alternative to the empirical **MSIS** density model that satellite-drag calculations have always leaned on. GITM is also the Upper-Atmosphere component of [[SWMF]].

## Why This Was Invented

LEO satellites fly *inside* the thermosphere (~100–600 km), and its neutral mass density — which sets atmospheric drag — is hard to model. The empirical incumbent, **MSIS** (Hedin), is a spherical-harmonic fit to decades of satellite and remote-sensing data, driven by the solar `F10.7` flux and the `Ap` activity index; it gives good *average* conditions but, being a fit, averages away storm-time dynamics. The first-principles thermosphere GCMs (TGCM → TIEGCM, CTIP, CMAT) instead compute density / momentum / energy self-consistently — but every one of them uses a **pressure-based** coordinate and **assumes hydrostatic equilibrium**. Ridley, Deng & Tóth (University of Michigan, 2006) built GITM to drop both assumptions, so the model can be correct in regimes where hydrostatic balance fails: the auroral region under strong Joule heating, and the storm-time equatorial fountain.

## The Model

**Two structural departures from every other thermosphere GCM:**
1. **Altitude grid, not a pressure grid.** Vertical spacing is set by scale heights (< 3 km low, > 10 km high), 40 vertical levels typical.
2. **Non-hydrostatic.** GITM solves the **vertical momentum equation explicitly** rather than assuming `∂p/∂r = −ρg`. This is a *hard structural choice*, not a soft correction — the vertical dynamics is left exact and unconstrained.

Other features:
- **3-D spherical grid**, stretchable in latitude and altitude, fixed longitude resolution; adjustable resolution (grid-point count specifiable per direction). Runs **1-D** (single column, horizontal transport neglected) or **3-D**; MPI-parallel with block-based 2-D domain decomposition.
- **Each neutral species gets its own vertical velocity** — O, O₂, N(⁴S/²D/²P), N₂, NO, H, He. Above ~120 km, turbulent mixing dies and species fall off at their *individual* scale heights instead of sharing one. Ion species: O⁺(⁴S/²D/²P), O₂⁺, N₂⁺, NO⁺, N⁺, H⁺, He⁺.
- **Log primitive variables.** GITM evolves `ℛ = ln(ρ)` and `𝒩ₛ = ln(Nₛ)`: density varies *exponentially* with height, so its log varies *linearly* — far better-conditioned numerically. Normalized temperature `𝒯 = p/ρ`.
- **Explicit advection + explicit chemistry** → small time step (~2–4 s, vs ~5 min for hydrostatic TGCMs) but full parallelism; ~3× faster than real time on 32 processors.
- **Ion momentum is solved algebraically** — the LHS `ρ dv/dt` is dropped as negligible, giving a closed-form ion velocity (Eq 26).
- **Drivers:** solar EUV heating from `F10.7` (daily value + 81-day mean) via the Hinteregger / Tobiska irradiance models; Joule (ion-neutral frictional) heating; auroral particle precipitation and high-latitude electric fields from swappable empirical models (AMIE, Weimer, Foster, Heppner–Maynard …) keyed to `Dst`/`Kp`/season; magnetic field as ideal dipole or realistic IGRF + APEX coordinates.
- **Three initialization modes:** ideal atmosphere, MSIS + IRI, or restart from a previous run.

## Key Results

- **Validated against MSIS / IRI** (§5): an equinox 3-D run initialized from MSIS+IRI converges to a stable diurnal solution after ~1.5 days. GITM's dayside maximum temperature is 1207 K vs MSIS's 1183 K — a 2% difference. GITM is *more* hemispherically symmetric than MSIS, and it reproduces the observed **midnight temperature maximum (MTM)** — a feature MSIS misses.
- **Honest about its limits.** Standalone GITM gets the low-latitude electron density wrong (a single equatorial peak vs IRI's two off-equator peaks) because it lacks a self-consistent **equatorial dynamo electric field** — the equatorial fountain effect needs that field, and it is only available when GITM runs *coupled inside [[SWMF]]*.
- GITM is the University of Michigan thermosphere model and the **Upper-Atmosphere (UA) component of SWMF** (Tóth coauthors both); coupled to BATS-R-US it provides thermosphere ⇄ magnetosphere interaction.

## Connection to SiS / CTPC

**GITM is the most directly Q6-relevant paper in the space-weather cluster.** The paper's opening list of why thermosphere–ionosphere modeling matters begins with "(1) examining increased satellite drag due to heating of the atmosphere." Thermospheric neutral mass density *is* the covariate the CTPC drag residual needs — atmospheric drag is the dominant *non-conservative* perturbation ([[Orbital-Perturbations]]; [[CTPC-Design-Rationale]] Q6).

**The Q6 covariate-model menu is now complete — and it is the SiS design hierarchy.** Three ways to model thermospheric density:

| Model | Category | Mechanism | Q6 role |
|---|---|---|---|
| **MSIS** | empirical | `F10.7`+`Ap` spherical-harmonic fit to data | the drag incumbent; cheap, "average" |
| **GITM** | physics-based | first-principles non-hydrostatic conservation laws | captures storm-time dynamics MSIS averages out |
| **[[FourCastNet]] / [[ClimaX]] / [[Aurora]]** | black-box ML | learned neural operators | fast, flexible, but no physics structure |

That is exactly the §2 taxonomy of [[SWMF]] — empirical / black-box-ML / physics-based — which *is* the SiS design hierarchy. Physics-as-architecture is the deliberate synthesis: ML speed with GITM's conservation laws hard-encoded.

**`hard_constraint_possible: yes`.** GITM's governing equations are conservation laws — continuity (Eqs 3/8/12), momentum (Eqs 4/9/14), energy (Eqs 5/16/22). A physics-encoded neural thermosphere model would hard-code these; GITM is the white-box reference whose structure such an architecture bakes in. And the **non-hydrostatic choice is itself a physics-as-architecture instinct** — GITM does not penalize non-hydrostatic behaviour away with a soft term; it picks the formulation (altitude grid, explicit vertical momentum) in which the vertical physics is exact. The **log-density primitive variable** (`ℛ = ln ρ`) is the same instinct in miniature: choose the coordinate in which an exponential structure becomes linear — the coordinate-choice argument of [[Orbital-Elements]] (equinoctial elements, non-degenerate charts).

**Active SiS work.** The NOAA-proposal folder this PDF lives in (`raw/Proposals/NOAA_Proposal/`) also holds an **FNO-vs-MSIS** comparison (proposal figures `03_fno_vs_msis_snapshots`, `04_fno_minus_msis_error`) and `MSIS_Hedin.pdf` — i.e. a Fourier-Neural-Operator surrogate benchmarked against MSIS. That is precisely the "neural surrogate trained on / benchmarked against a physics thermosphere model" recipe flagged in [[SWMF]]'s open questions, now an active experiment.

## Connections

- [[SWMF]] — GITM is SWMF's Upper-Atmosphere component; same UMich group (Tóth coauthors both); SWMF's §2 empirical/black-box/physics-based taxonomy frames the Q6 menu
- [[Orbital-Perturbations]] — thermospheric neutral density sets atmospheric drag, the dominant non-conservative perturbation in LEO
- [[CTPC-Design-Rationale]] — Q6 (space-weather covariates for the drag residual)
- [[FourCastNet]], [[ClimaX]], [[Aurora]] — the data-driven pole of the Q6 covariate-model menu; GITM and MSIS are the physics-based and empirical poles
- [[PIML-Survey]] — physics-informed ML; the hybrid that would hard-encode GITM's conservation laws into a fast neural surrogate
- [[Orbital-Mechanics-Agent]] — owns the Q6 covariate thread

## Open Questions

- **MSIS vs GITM accuracy for drag specifically.** The paper benchmarks GITM against MSIS on *temperature* and electron density — not on *neutral mass density*, which is what drag actually needs. The drag-relevant benchmark is exactly what the NOAA-proposal FNO-vs-MSIS figures appear to target; ingesting that proposal material would close the loop.
- **Which hard constraint?** GITM's conservation laws are *neutral-fluid* continuity + non-hydrostatic momentum + energy, plus ion chemistry/advection. A Q6 neural architecture should hard-encode mass continuity and energy balance — **not** MHD (that is BATS-R-US's domain, the wrong constraint for the neutral thermosphere, as flagged in [[SWMF]]).
- **Cost vs. surrogate.** GITM runs ~3× real time on 32 cores — far heavier than a neural operator. The hybrid: train a physics-encoded neural surrogate on GITM (and/or MSIS) output → a fast, conservation-respecting Q6 covariate model. This is the live FNO-vs-MSIS direction.
- **Driver uncertainty bounds the covariate.** GITM needs `F10.7`, `Kp`, `Dst`, IMF as inputs. A CTPC covariate model's useful horizon is capped by the *forecast horizon of those drivers*; that uncertainty must propagate into the drag residual, not be dropped.
- **Does standalone GITM suffice?** GITM's low-latitude *ionosphere* is wrong without the SWMF dynamo coupling. But if a Q6 model needs only *neutral density* for drag — not ionospheric structure — the standalone version may be adequate. Worth pinning down before committing to the heavier coupled run.

## Sources

- Ridley, A. J., Deng, Y., Tóth, G. (2006). *The global ionosphere–thermosphere model.* Journal of Atmospheric and Solar-Terrestrial Physics 68(8), 839–864. doi:10.1016/j.jastp.2006.01.008. Located at `raw/Proposals/NOAA_Proposal/weather_modeling/GITM_Ridley.pdf` (the paper lives in Bilal's NOAA-proposal working folder, alongside `MSIS_Hedin.pdf` and FNO-vs-MSIS proposal figures). Pages 1–15 read for this ingest: abstract; §1 Introduction (MSIS, IRI, and the TGCM/TIEGCM/CTIP/CMAT/GAIM lineage); §2 Model description (§2.1 neutral dynamics, continuity/momentum/energy Eqs 1–22; §2.2 ion advection Eqs 23–32; §2.3 chemistry Eqs 33–41; §2.4 electron temperature Eqs 42–48); §3 numerical schemes; §4 GITM-vs-TGCM differences (non-hydrostatic vs hydrostatic); §5 test results through §5.1 equinox simulation and the MSIS/IRI comparison. Pages 16–26 (solstice and 1-D test simulations, further validation, conclusions) not read for this ingest.

Q6 follow-up ingest, 2026-05-22 — the dedicated GITM ingest flagged as the natural next step in [[SWMF]]'s open questions.
