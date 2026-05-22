---
title: ClimaX
tags: [foundation-model, weather-forecasting, climate, vision-transformer, variable-tokenization, transfer-learning, space-weather, sda, Q6]
sources:
  - raw/papers/sda/ClimaX.pdf   # Nguyen et al. 2023, arXiv:2301.10343v5
created: 2026-05-22
updated: 2026-05-22
sis_relevance: medium
hard_constraint_possible: no
---

# ClimaX (Nguyen et al. 2023)

## One-Line Intuition

The first *foundation model* for Earth's atmosphere — one Vision-Transformer backbone, pretrained self-supervised on physics-based climate **simulations**, that finetunes to a whole spread of otherwise-separate tasks (weather forecasting, climate projection, downscaling) including variables and resolutions it never saw in pretraining.

## Why This Was Invented

[[FourCastNet]] and its peers are *single-task* models: each is trained from scratch on one curated, homogeneous dataset for one spatiotemporal task, so each lacks the general-purpose utility a numerical model has. ClimaX (Nguyen et al., UCLA + Microsoft + Scaled Foundations, Jan 2023) asks the foundation-model question for climate: can one architecture pretrain on **heterogeneous** data — different variable sets, different spatio-temporal coverage, different physical groundings — and then finetune cheaply to many downstream tasks? The two obstacles it had to clear: (1) what counts as an "internet-scale" passive corpus for the atmosphere, and (2) an architecture that absorbs datasets whose very *list of variables* differs.

## The Model

**Backbone:** a Vision Transformer (8 attention layers, embedding `D = 1024`, MLP-ratio 4, 2-layer MLP prediction head). The two novel modules are what make heterogeneous data ingestible:

- **Variable tokenization.** Standard ViT treats the input's `V` variables as channels of one `V×p²` patch — fine for RGB, wrong for climate where `V` *changes between datasets* (CMIP6 models carry different variable sets). ClimaX instead tokenizes **each variable separately**: every variable's `H×W` map is patched independently, giving a `V × h × w × D` token grid. The model can now train on any dataset regardless of which variables it carries.
- **Variable aggregation.** Per-variable tokenization makes the sequence `V × h × w` long — attention is `O(N²)`, and up to `V = 48` variables is expensive *and* mixes tokens of wildly different physical meaning. So for **each spatial position**, a cross-attention reduces the `V` variable-embeddings to a single vector (query = learnable, keys/values = the `V` embeddings). Sequence length drops `V × h × w → h × w`; the surviving tokens have unified semantics.
- A **lead-time embedding** (single-layer MLP, scalar `Δt → D`) and position embedding are added before the backbone — this is what lets one model serve many forecast horizons.

**Pretraining — simulations as the corpus.** The key data move: pretrain not on observations but on **CMIP6 physics-based climate simulations** (5 sources: MPI-ESM, TaiESM, AWI-ESM, HAMMOZ, CMCC). The objective is *randomized forecasting* — given snapshot `X_t`, predict `X_{t+Δt}` with `Δt ~ U[6, 168]` hours — self-supervised, latitude-weighted MSE:
```
L  =  (1/(V·H·W)) Σ_v Σ_i Σ_j  L(i) · (X̃ᵛ,ⁱ,ʲ_{t+Δt} − Xᵛ,ⁱ,ʲ_{t+Δt})²       (1)
L(i) = cos(lat(i)) / [ (1/H) Σ_i' cos(lat(i')) ]                              (2)
```
`L(i)` down-weights polar grid cells, which cover less area on the sphere.

**Finetuning.** Four learnable blocks: token embeddings, the aggregation module, the attention blocks, the prediction head. If a downstream task uses *unseen* variables, the embedding + head are reinitialized; the aggregation + attention layers (the transferable "feature extractor") are finetuned or frozen. Finetuning is done on **ERA5 reanalysis**. Two grids: 5.625° (32×64) and 1.40625° (128×256). Trained on ≤ 80 V100 GPUs.

## Key Results

- **Global forecasting (ERA5):** ClimaX matches operational IFS even at short horizons and is **superior at 7-day+ lead**; beats the ResNet/UNet CNN baselines on all tasks, at both resolutions.
- **Regional forecasting (North America, ERA5-NA):** best method across all target variables and lead times — and a model pretrained at *lower* resolution still places second, showing the backbone transfers across spatial coverage and resolution.
- **Sub-seasonal-to-seasonal (ERA5-S2S, weeks 3–4 / 5–6):** lowest RMSE on all four variables; the margin over baselines *widens* at longer lead. Large gains over the from-scratch `Cli-ViT` — direct evidence the pretraining transfers.
- **Climate projection (ClimateBench):** state-of-the-art at mapping anthropogenic forcings (CO₂, SO₂, black carbon, CH₄) to annual-mean temperature, diurnal range, and precipitation — a task whose inputs *and* outputs are completely unlike pretraining. With only 754 training points, the frozen-attention variant often wins.
- **Climate downscaling (MPI-ESM 5.625° → ERA5 1.40625°):** lowest RMSE and mean bias closest to zero on all variables.
- **Scaling laws:** error decreases consistently with more pretraining data, larger models, and higher resolution; bigger models are also more *sample-efficient*.

## Connection to SiS / CTPC

**ClimaX is the foundation-model template for a Q6 space-weather covariate model — the data-heterogeneity counterpart to [[FourCastNet]].** Per [[CTPC-Design-Rationale]] Q6, the corrector needs space-weather covariates because atmospheric drag — the dominant *non-conservative* perturbation — tracks thermospheric density, which is driven by solar activity (see [[Orbital-Perturbations]]). Space-weather data is *intrinsically heterogeneous*: F10.7, Kp/Ap, sunspot number, solar-wind speed/density, Dst — different instruments, cadences, and variable sets, exactly the regime variable tokenization + variable aggregation were built for. FourCastNet contributes the *speed/resolution* recipe; ClimaX contributes the *heterogeneity/transfer* recipe, and the latter is the better fit for the messy multi-source space-weather corpus.

**Simulations-as-corpus is directly portable.** ClimaX's central trick — pretrain self-supervised on physics-based *simulations* (CMIP6), then finetune on scarce *observations* (ERA5) — maps cleanly onto SDA: pretrain on physics-based thermosphere models (TIE-GCM, NRLMSISE-style) where data is unlimited, finetune on sparse measured-density / drag observations. This matters because labelled orbital-drag residuals are scarce, and ClimaX's ClimateBench result (strong with only 754 points) is the existence proof that the recipe survives small downstream datasets.

**Physics enters through the data, not the architecture — `hard_constraint_possible: no`.** Like [[FourCastNet]], ClimaX is purely data-driven: variable aggregation is a *learned* cross-attention reduction, not a physical one; no conservation law, hydrostatic balance, or continuity equation is encoded. Pretraining on CMIP6 simulations is a *soft* echo of physics-as-architecture — the network absorbs physics statistically from simulator output rather than having it baked into a layer. This is the **opposite pole** from the SiS design hierarchy: useful as a covariate-model template, but it is precisely the kind of "physics-as-data / physics-as-loss" approach that physics-as-architecture is meant to improve on.

## Connections

- [[FourCastNet]] — sibling Q6 weather forecaster; AFNO/Fourier for resolution, where ClimaX is ViT for task/data generality. Complementary recipes.
- [[Aurora]] — the third Q6 foundation-model weather forecaster in `raw/papers/sda/`
- [[SWMF]] — the physics-based ("white-box") counterpole to all three data-driven foundation models in this Q6 cluster
- [[PIML-Survey]] — foundation models / transformers for physical systems
- [[Orbital-Perturbations]] — atmospheric drag depends on thermospheric density → space-weather covariates are a Q6 need
- [[CTPC-Design-Rationale]] — Q6 (space-weather covariates), D3 (probabilistic corrector)
- [[Orbital-Mechanics-Agent]] — owns `raw/papers/sda/`, the Q6 paper folder

## Open Questions

- **Variable tokenization for space-weather indices.** ClimaX tokenizes each variable as a *spatial `H×W` map*. Many space-weather covariates (F10.7, Kp, Dst) are **scalar time series**, not gridded fields — adapting variable tokenization to mixed scalar/gridded inputs is an open design question for a Q6 model.
- **Which simulator is the "CMIP6" of the thermosphere?** ClimaX's pretraining leverage came from CMIP6's ~100 climate models across 49 groups. The thermosphere has fewer, less diverse physics models (TIE-GCM, NRLMSISE, JB2008, DTM) — is there enough simulation diversity to pretrain a foundation model, or does the recipe thin out?
- **No physics structure — same gap as FourCastNet.** A variant that hard-encodes thermospheric continuity / energy balance into the architecture would be the SiS-aligned model; ClimaX hard-encodes nothing.
- **ViT `O(N²)` ceiling.** ClimaX runs at 5.625°/1.40625° because self-attention caps its resolution; [[FourCastNet]]'s AFNO reaches 0.25°. A high-resolution thermospheric covariate model may need the AFNO mixing rather than vanilla ViT.

## Sources

- Nguyen, T., Brandstetter, J., Kapoor, A., Gupta, J. K., Grover, A. (2023). *ClimaX: A Foundation Model for Weather and Climate.* arXiv:2301.10343v5 (ICML 2023). Pages 1–21 read for this ingest: abstract; §1 Introduction; §2 Background (data sources — CMIP6, ERA5; tasks; foundation models); §3 Approach (§3.1 input representation, §3.2 architecture — variable tokenization, variable aggregation, transformer; §3.3 datasets; §3.4 training — pretraining objective Eqs 1–2, finetuning); §4 Experiments (§4.2 forecasting — global/regional/S2S; §4.3 climate projection / ClimateBench; §4.4 downscaling; §4.5 scaling laws; §4.6 ablations); §5 Discussion. Appendices A–E skimmed.

Second of the Q6 space-weather paper ingest wave (FourCastNet → ClimaX → Aurora → SpaceWeather), 2026-05-22.
