---
title: FourCastNet
tags: [neural-operator, weather-forecasting, AFNO, fourier-neural-operator, vision-transformer, space-weather, sda, Q6]
sources:
  - raw/papers/sda/FourCastNet.pdf   # Pathak et al. 2022, arXiv:2202.11214v1
created: 2026-05-22
updated: 2026-05-22
sis_relevance: medium
hard_constraint_possible: no
---

# FourCastNet (Pathak et al. 2022)

## One-Line Intuition

A global data-driven weather forecast model — a Fourier-neural-operator transformer trained on ERA5 reanalysis — that matches the ECMWF physics-based forecast at 0.25° resolution while running **~45,000× faster**, making thousand-member ensembles cheap.

## Why This Was Invented

Numerical weather prediction (NWP) is accurate but enormously expensive, which caps operational ensembles at ~50 members. Prior data-driven weather models ran at coarse resolution (5.625°, ~500 km) — a 32×64 grid for the whole globe — which washes out tropical cyclones, atmospheric rivers, and near-surface winds. FourCastNet (Pathak et al., NVIDIA + LBNL, Feb 2022) was the first DL model to forecast at the **full 0.25° ERA5 resolution** (~30 km, a 720×1440 grid) and to make a direct head-to-head comparison with the operational ECMWF IFS.

## The Model

**AFNO — Adaptive Fourier Neural Operator** (Guibas et al. 2022): a Vision Transformer backbone whose spatial token-mixing is done as a *global convolution in the Fourier domain*. Self-attention is `O(N²)` in the number of tokens — infeasible at 720×1440; AFNO's FFT-based mixing is `O(N log N)`, which is what makes full-resolution training feasible. AFNO ≈ FNO (Li et al. 2021) + ViT.

Per AFNO layer, on patch-embedded tokens `X ∈ ℝ^{h×w×d}` (patch size `p = 8`):
```
z_{m,n} = [DFT(X)]_{m,n}                          (1)  transform to Fourier domain
z̃_{m,n} = S_λ( MLP(z_{m,n}) )                     (2)  shared MLP + soft-threshold/shrinkage
y_{m,n} = [IDFT(Z̃)]_{m,n} + X_{m,n}               (3)  back to patch domain + residual
```
`S_λ(x) = sign(x)·max(|x|−λ, 0)` promotes sparsity. Spatial mixing (1–3) is followed by channel mixing; `L` layers, then a linear decoder.

- **20 prognostic variables** at 5 vertical levels (surface `U10/V10/T2m/sp/mslp`; `T/U/V/Z/RH` at 1000/850/500 hPa; `Z` at 50 hPa; integrated `TCWV`). Trained on **ERA5 reanalysis** (1979–2015 train, 2016–17 val, 2018+ test), 6-hour steps.
- **Two-step training:** single-step pre-train (`X(k) → X(k+1)`), then two-step autoregressive fine-tune. A separate diagnostic model forecasts precipitation (log-transformed, hard to model directly).
- Inference is **autoregressive free-running**.

## Key Results

- Matches IFS on RMSE and Anomaly Correlation Coefficient up to ~3-day lead for large-scale variables; **beats IFS at short lead (<48 h)** on small-scale variables (precipitation, surface winds).
- **~45,000× faster** than NWP on a node-hour basis; a week-long forecast in <2 s; ~12,000× less energy than one IFS forecast.
- 8× higher resolution than prior DL weather models; resolves Super Typhoon Mangkhut, Hurricane Michael's rapid intensification, and atmospheric rivers.
- Speed enables **1000+-member ensembles** (Gaussian-perturbed initial conditions, EnKF-style); the ensemble mean improves ACC/RMSE at longer lead times.

## Connection to SiS / CTPC

**FourCastNet is a methodological template for a Q6 space-weather covariate model.** Per [[CTPC-Design-Rationale]] Q6, the CTPC corrector needs space-weather covariates because the dominant *non-conservative* perturbation — atmospheric drag — depends on thermospheric density, which is set by solar activity (see [[Orbital-Perturbations]]). FourCastNet is the existence proof that a neural operator can do global atmospheric forecasting at NWP-competitive accuracy, orders of magnitude faster. A FourCastNet-style operator trained on *thermospheric* reanalysis would be exactly the density-covariate predictor feeding the drag residual.

**It is the first neural operator to land in the wiki.** [[PIML-Survey]] §3.4 flagged neural operators (FNO, DeepONet, CNO) as a wiki gap; the AFNO is FNO + ViT + Fourier soft-thresholding, so this ingest partially closes that gap.

**Speed → ensembles → UQ.** CTPC's probabilistic design ([[CTPC-Design-Rationale]] D3) needs calibrated uncertainty. A covariate model fast enough for 1000-member ensembles lets you Monte-Carlo over density forecasts → an ensemble of drag accelerations → propagated state uncertainty.

**Contrast with physics-as-architecture — `hard_constraint_possible: no`.** FourCastNet is *purely data-driven*: no conservation laws, no physics priors, IFS's 150+ physically-guided variables replaced by 20 learned channels. This is the **opposite pole** from the SiS design hierarchy. Its 3-day skill cliff and absence of conservation structure are precisely the failure surface a physics-encoded operator (hard-coding mass continuity, hydrostatic balance) would address. FourCastNet is therefore as much a *cautionary contrast* as a template: it shows the data-driven ceiling that physics-as-architecture aims to break through.

## Connections

- [[ClimaX]], [[Aurora]] — sibling foundation-model weather forecasters; the Q6 covariate-model family in `raw/papers/sda/`
- [[SWMF]] — the physics-based ("white-box") counterpole to this data-driven model; names MSIS, the empirical thermospheric drag covariate
- [[PIML-Survey]] — §3.4 flagged neural operators as a wiki gap; FourCastNet is the AFNO instance
- [[Orbital-Perturbations]] — atmospheric drag depends on thermospheric density → space-weather covariates
- [[CTPC-Design-Rationale]] — Q6 (space-weather covariates), D3 (probabilistic corrector / ensembles)
- [[Orbital-Mechanics-Agent]] — owns `raw/papers/sda/`, the Q6 paper folder

## Open Questions

- **Thermosphere ≠ troposphere.** FourCastNet forecasts terrestrial weather (ERA5, surface to stratosphere). Orbital drag needs *thermospheric* density at ~200–600 km. The methodology transfers; the data and variables do not — thermospheric density, the physics-based-modeling domain of [[SWMF]] (and its GITM thermosphere component), is the actual target.
- **No physics structure.** A physics-encoded operator — hard-coding mass continuity, hydrostatic balance — would be the SiS-aligned variant; FourCastNet hard-encodes nothing.
- **The 3-day skill cliff.** For multi-day orbital forecasts the covariate model's own horizon limit propagates into the drag estimate; the covariate model's uncertainty must be carried, not ignored.

## Sources

- Pathak, J., Subramanian, S., Harrington, P., Raja, S., Chattopadhyay, A., Mardani, M., Kurth, T., Hall, D., Li, Z., Azizzadenesheli, K., Hassanzadeh, P., Kashinath, K., Anandkumar, A. (2022). *FourCastNet: A Global Data-driven High-resolution Weather Model using Adaptive Fourier Neural Operators.* arXiv:2202.11214v1. Pages 1–14 read for this ingest (abstract; §1 Introduction; §2 Training Methods, incl. §2.1 AFNO model description with Eqs 1–3, §2.2 Training, §2.3 Precipitation Model, §2.4 Inference; §3 Results through §3.5). Pages 15+ (further results, compute comparison §4, appendices) skimmed.
