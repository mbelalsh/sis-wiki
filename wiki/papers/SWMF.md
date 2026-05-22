---
title: Space Weather Modeling Framework (SWMF)
tags: [space-weather, MHD, physics-based-model, SWMF, BATS-R-US, atmospheric-drag, sda, Q6]
sources:
  - raw/papers/sda/SpaceWeather.pdf   # Gombosi et al. 2021, arXiv:2105.13227v1
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: yes
---

# Space Weather Modeling Framework (SWMF) — Gombosi et al. 2021

## One-Line Intuition

A 25-year, **physics-based** modeling framework that simulates the entire Sun-to-Earth space-weather chain — corona, heliosphere, magnetosphere, ionosphere — from first-principles MHD conservation laws, and runs operationally at NOAA's Space Weather Prediction Center. It is the **physics-as-architecture counterpole** to the data-driven weather foundation models [[FourCastNet]] / [[ClimaX]] / [[Aurora]].

## Why This Was Invented

Space weather — solar flares, CMEs, geomagnetic storms — threatens power grids, GPS, and satellites, and the *extreme* events that matter most (e.g. the March 1989 storm, `Dst = −589 nT`, that collapsed the Hydro-Québec grid) are **rare**. That rarity is exactly what defeats empirical and machine-learning models: they "struggle with out-of-sample predictions" (Gombosi et al., §2.3). Physics-based models instead solve the governing equations directly, so they extrapolate to unprecedented events. The catch is cost: a Sun-to-Earth model couples many sub-domains, each with different physics at different scales, and must run faster than real time. The Space Weather Modeling Framework (Gombosi et al., U. Michigan CSEM + NASA Goddard, 2021) is the review of how one university group sustained that multi-disciplinary effort for a quarter century.

## The Three Categories of Space-Weather Model

The paper's §2 taxonomy is the single most SiS-relevant idea in it — it *is* the SiS design hierarchy, stated for space weather:

| Category | What it is | Strength | Weakness |
|---|---|---|---|
| **Empirical** | aggregate historical data into parametrized fits; little/no physics | cheap, good where data is dense | poor where data is sparse; no extrapolation |
| **Black-box (ML)** | machine-learned predictors (NN for Dst/AL, TEC maps, flare prediction) | flexible, fast | **"not interpretable… do not help us understand the underlying physics"** |
| **Physics-based** | directly solve the governing equations (MHD) | predicts rare extremes; extrapolates | computationally expensive |

The paper is explicit that ML models are non-interpretable — and SiS is *Safe, Interpretable, Simple*. Physics-as-architecture is precisely the synthesis the taxonomy leaves open: keep the ML speed/flexibility, but bake the physics-based conservation laws into the model so it is no longer a black box.

## The Framework

- **BATS-R-US** (Block Adaptive-Tree Solar-wind Roe-type Upwind Scheme) — the core code. A high-performance generalized **MHD** solver with adaptive mesh refinement, born from extending modern CFD (van Leer / Roe high-order Godunov schemes) to magnetized plasma. Configurable for ideal/resistive, semi-relativistic, anisotropic, Hall, multi-species, and multi-fluid extended magnetofluid (XMHD) equations. Conservation laws (mass, momentum, energy, magnetic flux) are the architecture.
- **SWMF** — couples ~a dozen physics domains via a modular framework (>1M lines of Fortran 2008 / C++): each heliophysics domain has its own purpose-built model; the framework couples them for the problem at hand. Open-source core (`github.com/MSTEM-QUDA`); runnable on request through NASA's CCMC.
- **AWSoM / AWSoM-R** — the Alfvén Wave Solar-atmosphere Model: solar corona + inner heliosphere, XMHD with separate ion/electron temperatures and self-consistent Alfvén-wave turbulence. AWSoM-R's Threaded-Field-Line Model reformulates the stiff transition-region 3D problem as cheap 1D problems along field lines → faster than real time on ~200 cores.
- **SWMF/Geospace** — the magnetosphere–ionosphere configuration: global magnetosphere (BATS-R-US) ⇄ inner magnetosphere (Rice Convection Model) ⇄ ionospheric electrodynamics (Ridley Ionosphere Model), optional radiation belts. Drivers are **solar wind, IMF, and F10.7 solar radio flux**. An operational version has run **24/7 at NOAA/SWPC since 2016** (transitioned 2015, upgraded to v2 in 2020); it reproduces Dst, cross-polar-cap potential, and ground magnetic perturbations with operationally useful skill.

## Connection to SiS / CTPC

**MSIS — named in §2.1 — is the literal Q6 drag covariate.** The paper's empirical-model example is the **MSIS** (Mass Spectrometer and Incoherent Scatter) model of the upper atmosphere: it gives thermospheric temperature and the densities of N₂, O, O₂, He, Ar, H, and is "often used… in calculations of satellite orbital decay caused by atmospheric drag." That is exactly the covariate [[CTPC-Design-Rationale]] Q6 needs — the dominant *non-conservative* perturbation, atmospheric drag, scales with thermospheric density ([[Orbital-Perturbations]]). MSIS is the empirical incumbent; SWMF's Upper Atmosphere component — [[GITM]], the Global Ionosphere–Thermosphere Model — is the physics-based alternative; [[FourCastNet]]/[[ClimaX]]/[[Aurora]] are the black-box-ML route. CTPC's Q6 design question is *which* of these three to build on — and the SiS answer is none of them alone.

**This paper is the physics pole; the foundation-model trio is the data pole — SiS sits between, by design.** In the §2 taxonomy, FourCastNet/ClimaX/Aurora are *black-box models* and SWMF is *physics-based*. Physics-as-architecture is the deliberate hybrid: take the foundation-model speed (a neural operator is ~10⁴–10⁵× faster than a physics simulator — see [[Aurora]]'s 100,000× speed-up over CAMS) but hard-encode the MHD/continuity conservation laws SWMF solves, so the covariate model extrapolates to extreme storms instead of failing out-of-sample. `hard_constraint_possible: yes` — SWMF is the existence proof that space-weather physics is well-posed as conservation laws, which is the prerequisite for baking them into a layer.

**F10.7 is both an SWMF driver and the standard drag index.** SWMF/Geospace takes F10.7 solar radio flux as an input; F10.7 is also the canonical solar driver in empirical drag models. It is the natural scalar covariate to feed a CTPC drag-residual corrector — a concrete, immediately-available Q6 input.

## Connections

- [[GITM]] — SWMF's Upper-Atmosphere (thermosphere/ionosphere) component; the drag-relevant piece, ingested as the Q6 follow-up
- [[FourCastNet]], [[ClimaX]], [[Aurora]] — the data-driven ("black-box" in this paper's taxonomy) Q6 weather foundation models; SWMF is their physics-based counterpole
- [[Orbital-Perturbations]] — atmospheric drag is the dominant non-conservative perturbation; it scales with thermospheric density, which space weather drives
- [[CTPC-Design-Rationale]] — Q6 (space-weather covariates for the drag residual)
- [[PIML-Survey]] — physics-informed ML; the hybrid that bridges this paper's physics pole and the foundation-model data pole
- [[Orbital-Mechanics-Agent]] — owns `raw/papers/sda/`, the Q6 paper folder

## Open Questions

- **GITM, not BATS-R-US, is the drag-relevant piece** — *now ingested → [[GITM]]*. SWMF's headline physics is corona/heliosphere/magnetosphere MHD; LEO orbital drag needs *thermospheric neutral density* at 200–600 km, which is the GITM (Upper Atmosphere) component. The dedicated GITM ingest (Ridley, Deng & Tóth 2006) is on the [[GITM]] page: a first-principles, non-hydrostatic thermosphere model — the physics-based alternative to the empirical MSIS drag covariate. Residual open question moves there: MSIS-vs-GITM accuracy on *neutral mass density* specifically.
- **Simulator → covariate coupling.** SWMF is a forward simulator, not a corrector. How does a physics-simulator density field enter the CTPC corrector — as a direct covariate, as a prior, or as the pretraining target for a neural surrogate (the [[ClimaX]]/[[Aurora]] "simulations-as-corpus" recipe)?
- **Cost vs. a neural surrogate.** SWMF runs faster than real time but is still far heavier than a neural operator. The hybrid question: train a physics-encoded neural surrogate *on* SWMF/GITM output, then use it as the fast, conservation-respecting Q6 covariate model.
- **Is MHD even the right hard constraint for the thermosphere?** SWMF's conservation laws are MHD (magnetized plasma). The neutral thermosphere is better described by neutral-fluid continuity/Navier–Stokes (GITM is "non-hydrostatic fluid"). The hard constraint a Q6 architecture should bake in is mass continuity + hydrostatic/energy balance, not full MHD.

## Sources

- Gombosi, T. I., Chen, Y., Glocer, A., Huang, Z., Jia, X., Liemohn, M. W., Manchester, W. B., Pulkkinen, T., Sachdeva, N., Al Shidi, Q., Sokolov, I. V., Szente, J., Tenishev, V., Toth, G., van der Holst, B., Welling, D. T., Zhao, L., Zou, S. (2021). *What Sustained Multi-Disciplinary Research Can Achieve: The Space Weather Modeling Framework.* Submitted to *Journal of Space Weather and Space Climate*; arXiv:2105.13227v1. CC BY 4.0. Pages read for this ingest: front matter & abstract; §1 Introduction; §2 Evolution of Space Weather Models (§2.1 empirical — MSIS, Tsyganenko; §2.2 black-box / ML; §2.3 physics-based, global MHD history); §3 Origins of BATS-R-US & SWMF (Fig 1 multiphysics overview); §4 The SWMF Today (§4.1 AWSoM/AWSoM-R + Threaded-Field-Line Model, Fig 2–3; §4.2 SWMF/Geospace configuration, Fig 4–5; §4.2.2 operational use at NOAA/SWPC and CCMC). §5–§9 and Appendices A–E (BATS-R-US fundamentals, SWMF modules, physics/conservation laws, algorithms, MHD-EPIC) not yet read — a deep ingest of the GITM thermosphere component is flagged in Open Questions.

Fourth and final of the Q6 space-weather paper ingest wave (FourCastNet → ClimaX → Aurora → SWMF), 2026-05-22. Note: [[FourCastNet]]'s pre-ingest forward-reference `[[SpaceWeather-ML-Survey]]` was a wrong guess at this file's identity — the paper is a physics-based MHD framework review, not an ML survey. Reference corrected to `[[SWMF]]` on ingest.
