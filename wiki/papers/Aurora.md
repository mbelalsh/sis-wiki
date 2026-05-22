---
title: Aurora
tags: [foundation-model, earth-system, weather-forecasting, atmospheric-chemistry, swin-transformer, perceiver, transfer-learning, space-weather, sda, Q6]
sources:
  - raw/papers/sda/Aurora_FoundationModel.pdf   # Bodnar et al. 2025, Nature 641:1180-1187
created: 2026-05-22
updated: 2026-05-22
sis_relevance: medium
hard_constraint_possible: no
---

# Aurora (Bodnar et al. 2025)

## One-Line Intuition

A 1.3-billion-parameter foundation model for the **whole Earth system** — pretrained on >1 million hours of geophysical data — that finetunes cheaply to forecast air quality, ocean waves, tropical-cyclone tracks, and high-resolution weather, and is the **first AI model to beat the operational numerical systems** in all four domains at a fraction of their cost.

## Why This Was Invented

[[FourCastNet]] and [[ClimaX]] are atmospheric — terrestrial weather and climate. But the SiS-relevant point of foundation models is *cross-domain generality*, and the previous data-driven wave (Pangu-Weather, GraphCast, FourCastNet, ClimaX) had largely centred on medium-range weather at 0.25°, leaving air chemistry, ocean dynamics, and wave modelling untouched. Aurora (Bodnar et al., Microsoft Research + collaborators, *Nature* 2025) asks whether **one** pretrained backbone can absorb the dynamics common to *all* of these geophysical systems and then specialize — and answers yes, decisively, by surpassing each domain's bespoke operational numerical system.

## The Model

A three-part encoder–processor–decoder, run autoregressively. With state `Xᵗ ∈ ℝ^{V×H×W}` the rollout is:
```
Φ(X^{t-1}, X^t)   = X̂^{t+1}
Φ(X^t, X̂^{t+1})  = X̂^{t+2}        ...  predictions fed back in
```

- **3D Perceiver encoder.** Converts *heterogeneous* inputs — arbitrary variable sets, pressure levels, and resolutions — into one standardized **3D latent representation**. All variables are treated as `H×W` images, split into `P×P` patches, embedded by variable-specific linear maps. A Perceiver module then compresses the variable number `C` of physical pressure levels down to a fixed `L = 3` latent levels. The latent grid is tagged with Fourier-expansion encodings of patch position, **patch area**, and absolute time — the patch-area encoding is what lets one model run at different resolutions.
- **Processor — multiscale 3D Swin Transformer U-Net.** A 48-layer backbone (3 stages, symmetric down/up-sampling) using *local windowed* self-attention with window shifting — emulating local computations in numerical integration. The U-Net structure handles multiple spatial scales; depth (48 vs. 16 in prior work) is affordable because the Perceiver shrank the level dimension.
- **3D Perceiver decoder.** Reverses the encoder — disaggregates latent levels back to any requested pressure levels, variable-specific linear decoding back to the lat–lon grid.

**Training — three stages.** (1) **Pretraining:** 150,000 steps on 32 A100 GPUs (~2.5 weeks), MAE objective, latitude-weighted (Eq 1, surface + atmospheric terms), bf16. Trained on a mixture of **analysis, reanalysis, forecast, reforecast, and climate-simulation** datasets (ERA5, HRES, IFS/GFS ensembles, GEFS reforecasts, CMIP6, MERRA-2, CAMS) — the >1M-hour corpus. (2) **Short-lead-time finetuning:** finetune the whole architecture with 1–2 rollout steps. (3) **Rollout (long-lead) finetuning:** uses **LoRA** (low-rank adaptation of the backbone's attention), the pushforward trick, and a replay buffer — parameter-efficient adaptation of a very large model to long-horizon dynamics.

## Key Results

State-of-the-art in **four** domains, each beating the domain's operational numerical system:

- **Air pollution** (5-day, 0.4°): outperforms CAMS atmospheric-chemistry simulations on **74%** of targets (89% at 3-day lead) — first AI attempt at global atmospheric-composition forecasting at this scale. ~**100,000× speed-up** over CAMS (~0.6 s per hour of lead on one A100).
- **Ocean waves** (10-day, 0.25°): exceeds the IFS HRES-WAM numerical wave model on **86%** of targets.
- **Tropical-cyclone tracks** (5-day): surpasses **all seven** operational forecasting centres on **100%** of targets — the first time an ML model beats full operational TC forecasts.
- **High-resolution weather** (10-day, 0.1° ≈ 10 km): surpasses IFS HRES on **92%** of targets and improves extreme-event performance; outperforms IFS and GraphCast at 0.25° on 91%+ of targets.

**Scaling:** validation loss follows `L(N) ∝ N^{-α}` — a ~6% reduction per 10× model size; pretraining on *more diverse* data systematically improves performance, especially for **extreme** values. Each downstream finetuning took a small team ~4–8 weeks (vs. years for a numerical model).

## Connection to SiS / CTPC

**Aurora is the strongest Q6 space-weather covariate-model template — and the one whose generality argument most directly licenses an SDA application.** Per [[CTPC-Design-Rationale]] Q6, the corrector needs space-weather covariates because atmospheric drag — the dominant *non-conservative* perturbation ([[Orbital-Perturbations]]) — tracks thermospheric density. Aurora's distinguishing claim is *cross-geophysical-domain* transfer: one pretrained backbone finetuned to air chemistry, waves, cyclones. The thermosphere is just another geophysical domain; Aurora's own Discussion explicitly lists extending the model to "broader domains closely tied to weather." A thermospheric-density finetune is in-scope by the paper's own logic.

**The 3D Perceiver encoder is the right tool for space-weather data.** Space-weather observations are heterogeneous in variable, *altitude*, and resolution: in-situ density at irregular satellite altitudes, ground indices (F10.7, Kp, Dst), solar-wind streams. Aurora's Perceiver encoder was built precisely to fold arbitrary variables / pressure levels / resolutions into one latent grid — a better structural match than [[ClimaX]]'s fixed gridded variable tokenization or [[FourCastNet]]'s fixed 20-channel input. Aurora also handles **missing data** (waves undefined over land, via extra density channels) — directly relevant to sparse, gappy space-weather records.

**It sets the data-driven bar — `hard_constraint_possible: no`.** Like its two siblings, Aurora bakes *no physics into the architecture*: the Swin U-Net and Perceiver are generic; physics enters only statistically, through the reanalysis/simulation pretraining corpus. It is the **opposite pole** from the SiS hierarchy — but as the strongest data-driven Earth-system model in existence, it is the concrete performance bar a *physics-as-architecture* orbital corrector must justify itself against. Aurora's own scaling result (6% per 10× params) and its extreme-event weaknesses are the argument for SiS: brute-force scale buys diminishing returns, and conservation structure is what extreme-event reliability needs.

## Connections

- [[FourCastNet]], [[ClimaX]] — the other two Q6 foundation-model forecasters in `raw/papers/sda/`. Three architectures: AFNO (FourCastNet), ViT + variable tokenization (ClimaX), 3D Swin U-Net + Perceiver (Aurora).
- [[SWMF]] — the physics-based ("white-box") counterpole; the fourth paper in the Q6 `raw/papers/sda/` cluster
- [[PIML-Survey]] — foundation models / transformers for physical systems
- [[Orbital-Perturbations]] — atmospheric drag depends on thermospheric density → space-weather covariates are a Q6 need
- [[CTPC-Design-Rationale]] — Q6 (space-weather covariates), D3 (probabilistic corrector / ensembles)
- [[Orbital-Mechanics-Agent]] — owns `raw/papers/sda/`, the Q6 paper folder

## Open Questions

- **Is a thermosphere finetune actually viable?** Aurora's air-quality finetune used CAMS — a rich, gridded, operational chemistry product. Thermospheric-density observations are far sparser and less standardized; whether Aurora's missing-data machinery stretches that far is the open SDA question.
- **0.1° terrestrial ≠ thermospheric scales.** Aurora's 10-km horizontal resolution is for surface-to-stratosphere weather. Orbital drag needs density at 200–600 km altitude — the *vertical* extent and physics are entirely different; only the encoder/decoder machinery transfers.
- **1.3B parameters — is that the Q6 budget?** Aurora's scaling law (6%/10×) suggests a useful covariate model could be far smaller. What is the minimum-viable size for a thermospheric-density predictor good enough to feed the drag residual?
- **No physics structure — same gap as the two siblings.** A version hard-encoding thermospheric continuity / energy balance would be SiS-aligned; Aurora hard-encodes nothing.

## Sources

- Bodnar, C., Bruinsma, W. P., Lucic, A., Stanley, M., Allen, A., Brandstetter, J., Garvan, P., Riechert, M., Weyn, J. A., Dong, H., Gupta, J. K., Thambiratnam, K., Archibald, A. T., Wu, C.-C., Heider, E., Welling, M., Turner, R. E., Perdikaris, P. (2025). *A Foundation Model for the Earth System.* Nature 641, 1180–1187. DOI 10.1038/s41586-025-09005-y. Open access (CC BY-NC-ND 4.0). Pages read for this ingest: main article pp 1180–1187 (abstract; introduction; the four domain sections — atmospheric chemistry & air pollution, ocean wave dynamics, tropical-cyclone tracking, high-resolution weather; Discussion); Methods (problem statement & autoregressive rollout, the Aurora model — 3D Perceiver encoder, multiscale 3D Swin Transformer U-Net backbone, 3D Perceiver decoder; training methods — MAE objective Eq 1, pretraining, short-lead-time and rollout/LoRA finetuning; datasets; verification metrics); Extended Data Figs 1–3 (pretraining-data and model-size scaling; encoder/decoder schematic).

Third of the Q6 space-weather paper ingest wave (FourCastNet → ClimaX → Aurora → SpaceWeather), 2026-05-22.
