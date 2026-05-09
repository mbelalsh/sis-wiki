---
title: CTPC Design Rationale (Decisions Made + Decisions Deferred to PhyArch-CTPC)
tags: [ctpc, sis-design, design-rationale, phyarch, synthesis, research-roadmap, afrl]
sources: [raw/papers/my-papers/KDD_submission.pdf, raw/notes/PhyArch_DoublePendulum.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: critical
hard_constraint_possible: yes
---

# CTPC Design Rationale (Decisions Made + Decisions Deferred to PhyArch-CTPC)

> **Purpose:** justify past architectural choices for [[CTPC-KDD-Submission]] (the deployed framework) AND map open research questions forward toward **PhyArch-CTPC** (the next-generation symmetry-hardwired variant). Functions both as a reviewer-facing rationale doc and as a research-planning document. Updated repeatedly as the project evolves.

> **Sources synthesized:**
> - [[CTPC-KDD-Submission]] — probabilistic Predictor-Corrector framework on real CDDIS spacecraft data
> - [[PhyArch-Double-Pendulum-Benchmark]] — deterministic symmetry-hardwiring benchmark on a controlled toy system

## Core Design Philosophy

CTPC operationalizes the SiS design hierarchy from `CLAUDE.md`:

1. **Hard constraints → architecture.** The Predictor (GMAT) IS the hard physics layer. No trainable parameters; encodes Newtonian dynamics + perturbations + conservation laws by construction.
2. **Soft constraints → loss.** None in the architecture; loss is strictly proper (`NLL + CRPS + KL`) so calibration consistency follows structurally, not via penalty.
3. **Learned residual → ML corrector.** The Latent NCDE Corrector models residuals only — what the physics simulator can't predict.

This decomposition has been validated empirically (CTPC's 64% MSE reduction + d̄² ≈ 1 calibration on real spacecraft data) and empirically against alternative architectures (PhyArch DP's 0% failure rate vs. PINN/LNN baselines that fail catastrophically OOD).

The two papers operate at different levels but make consistent architectural choices. This page makes those choices explicit.

---

# Part I — Decisions Made (Justified)

Each decision: the choice, the alternatives considered, the rationale, the supporting evidence.

## D1. Predictor-Corrector decomposition (vs. learn dynamics from scratch)

**Choice.** Keep GMAT as a frozen deterministic Predictor; learn only the residual `e(t) = x(t) − x̂(t)`.

**Alternatives.**
- (a) End-to-end learned dynamics (no physics simulator). Approach taken by Latent ODE / GRU-ODE-Bayes.
- (b) Physics-informed losses on a from-scratch model (PINN-style). Approach taken by PINN baseline in [[PhyArch-Double-Pendulum-Benchmark]].
- (c) Predictor-Corrector with a learned Predictor (as opposed to GMAT). Possible but discards decades of validated orbital-mechanics engineering.

**Rationale.** GMAT already encodes Newtonian dynamics, J2/J3 zonal harmonics, lunisolar perturbations, drag, SRP via established numerical methods. Learning these from scratch would (i) require massive labeled data the field doesn't have, (ii) discard well-tested physics engineering, (iii) be unable to extrapolate beyond training trajectories without violating known conservation laws. The residual signal is much smaller and structurally different from the full dynamics.

**Supporting evidence.**
- CTPC paper Table 2: 64% MSE reduction at 4-day horizon with the Predictor-Corrector decomposition.
- PhyArch DP paper §10.2: PINN/LNN learning dynamics from scratch fail catastrophically OOD (21.9% / 43.8% failure rates), whereas symmetry-hardwired residual-style architectures don't.
- Same architectural choice in [[PeRCNN]]: frozen FD conv layer (hard physics) + Π-block (learned residual).

## D2. Continuous-time corrector (vs. discrete-time)

**Choice.** [[Neural-Controlled-Differential-Equation]] for both encoder and decoder.

**Alternatives.**
- (a) Discrete RNN (GRU/LSTM) with timestep deltas as features. Pre-NCDE standard.
- (b) Hybrid ARIMA-LSTM as in earlier hybrid forecasting literature.
- (c) Latent SDE for stochastic continuous-time evolution.

**Rationale.** Real CDDIS spacecraft observations arrive at irregular times (varying gaps between TLE updates, ground-station passes). Discrete RNNs require regularization or interpolation hacks. NCDEs handle irregular sampling natively via the control path. Plus: the *forecast* regime is open-loop (no observations); a discrete-time model would need dummy timestep features for the entire forecast horizon, whereas the NCDE evolves smoothly conditioned on the Predictor's output.

**Supporting evidence.**
- CTPC paper §6 + Table 1: NCDE-based variants (CTPC-CDE, CTPC-CDE++) outperform NODE-based (CTPC-ODE) on coordinate-frame robustness (Appendix G).
- Foundational result: Kidger et al. 2020 (NCDE universal-approximation theorem) shows NCDE expressivity ≥ RNN expressivity.
- HopCast retrieval-based corrector (Bilal's earlier work, [Shahid & Fleming 2025]) explicitly limited to regularly-sampled data → motivated the NCDE shift.

## D3. Probabilistic corrector (vs. deterministic)

**Choice.** Probabilistic prediction head outputting Student-t parameters at each forecast time.

**Alternatives.**
- (a) Deterministic NCDE corrector (Bilal's earlier work, [Shahid, Koirla, Fleming 2025]).
- (b) Ensemble of deterministic correctors.
- (c) Variational Bayesian neural network.

**Rationale.** Long-horizon open-loop forecasting accumulates error without bounds. Without uncertainty estimates, downstream operational decisions (collision avoidance, conjunction analysis) lack risk reasoning. **For SDA, calibrated uncertainty matters as much as accuracy.** Single-trajectory deterministic models can't support Pc (collision probability) computations. Ensembles are wasteful — they retrain K models. The Latent NCDE structure achieves an ensemble-like effect by sampling K times from a single learned latent posterior (cheaper, principled via law of total variance).

**Supporting evidence.**
- CTPC paper §1: explicit safety motivation citing Kerr & Ortiz 2021 on conjunction analysis needs.
- CTPC paper Table 3 (Appendix F): law-of-total-variance decomposition — within-sample (aleatoric) + between-sample (latent-induced) — gives better calibration than mean-variance only.
- PhyArch DP paper §1.1: same safety motivation ("a model that usually respects physics but occasionally violates it can be catastrophic"). Operationally, both papers are aligned: SDA needs both correctness *and* uncertainty.

## D4. Student-t likelihood with `ν_min = 4.5` (vs. Gaussian)

**Choice.** Multivariate Student-t with `ν > 4.5` learned as `ν = ν_min + softplus(ν_raw)`.

**Alternatives.**
- (a) Gaussian (much simpler).
- (b) Mixture-of-Gaussians (more expressive but harder to train).
- (c) Normalizing flows (most expressive, much more expensive).

**Rationale.** Orbital errors are documented as heavy-tailed [Mashiku, Garrison, Carpenter 2012] — particle filters were proposed precisely because Gaussian approximations break down for orbit determination. Student-t captures heavy tails with a single learnable parameter `ν`. The `ν_min = 4.5` floor avoids degenerate heavy tails (ν > 2 → finite variance; ν > 4 → finite kurtosis), preventing the optimizer from collapsing to a Cauchy-like distribution that would dominate the loss. Mixtures and flows are over-engineering for this problem — heavy-tailedness is the only structural deviation from Gaussian.

**Supporting evidence.**
- CTPC paper Eq. 7-8: Student-t NLL formulation.
- Mashiku et al. 2012 cited as the orbital-mechanics-specific motivation.
- Murphy 2023 (PML book) discussed for the Student-t NLL form.

## D5. Loss = NLL + CRPS + KL (strictly proper combination)

**Choice.** Three-term loss: Student-t NLL + Continuous Ranked Probability Score + KL regularization on the latent posterior.

**Alternatives.**
- (a) NLL only — standard for likelihood-based training.
- (b) CRPS only — standard for proper-scoring-rule training.
- (c) MSE + KL (Latent ODE baseline objective) — standard variational training.
- (d) Pinball loss / quantile regression — alternative for distributional forecasting.

**Rationale.** Each term serves a distinct purpose:
- **NLL** trains the Student-t parameters to capture the conditional distribution shape.
- **CRPS** is sample-based and robust under heavy tails; complementary to NLL for sharpness.
- **KL** regularizes the latent posterior toward `N(0, I)`, preventing latent collapse and ensuring meaningful sampling at inference.

Crucially, **all three are strictly proper individually, so their sum is strictly proper** (Lemma D.3 of source paper). At population minimum, the resulting predictive distribution is the *true* conditional distribution — calibration consistency is structural (Theorem 5.2). This is much stronger than empirical "we got d̄² ≈ 1 in our tests."

**Supporting evidence.**
- Lemma D.3 of source paper (proof of strict properness).
- Theorem 5.2: `E[r_t^T Σ_t^(−1) r_t] = L` at population minimum.
- Empirical (Table 2): Latent ODE† (CRPS+KL only, no NLL) reduces d̄² by ~10× vs Latent ODE★ (MSE+KL) but remains miscalibrated (d̄² ≈ 3000+); CTPC variants with all three terms achieve d̄² ≈ 1. **The probabilistic head + NLL is doing most of the calibration work**, with CRPS providing additional sharpness.

## D6. RTN (Radial-Transverse-Normal) frame for residual modeling

**Choice.** Train and evaluate the Corrector on errors in the RTN frame (orbit-relative). Convert from ECI as needed (Appendix E.2 of source paper).

**Alternatives.**
- (a) ECI directly (Earth-Centered Inertial). Standard for raw observation data.
- (b) Equinoctial elements. Singularity-free orbital-mechanics representation.
- (c) Delaunay variables. Canonical for Hamiltonian formulation.

**Rationale.** RTN is *orbit-relative*: errors decompose naturally into radial (along orbit's local outward direction), along-track (orbital velocity direction), cross-track (orbit-normal). For near-circular orbits, RTN errors are approximately *stationary* in distribution — they look statistically similar across the orbit. ECI errors are *non-stationary* because the rotation matrix from ECI to body-fixed is time-varying; the residual distribution rotates with the orbit. **Stationary errors are far easier to learn.**

**Connection to PhyArch.** This is the orbital analog of PhyArch DP's geometric features `(u_i, t_i, ĝ)` — both moves do the same thing: choose a coordinate system where the natural physics structure is exposed. The DP paper embeds the torus into Euclidean space without singularities; CTPC embeds the rotating frame into a near-stationary one.

**Supporting evidence.**
- CTPC paper Table 4 (Appendix G): NCDE-based variants in ECI frame have worse calibration (d̄² ≈ 200+) than in RTN (d̄² ≈ 1.1). The architectural choice of NCDE provides robustness, but the *frame choice itself* is the dominant effect.
- Kerr & Ortiz 2021 standard-practice citation: "RTN is standard for orbit-relative error analysis."

## D7. CTPC-CDE++ (NCDE encoder + NCDE decoder + missing-features support)

**Choice.** Full NCDE on both sides + missingness channels in the encoder control path.

**Alternatives within the variant family** (all in Table 1 of source paper).
- CTPC-ODE: NODE decoder, RTN-only, no missing features.
- CTPC-CDE: NODE encoder + NCDE decoder, frame-robust, no missing features.
- CTPC-CDE++: NCDE encoder + decoder, frame-robust, supports missing features.

**Rationale.** Real spacecraft data has both irregular timing AND occasional missing measurements (channel dropouts, station-pass gaps). Encoder NCDE with missingness channels in the control path handles both natively. **The variant ablation in Table 1 isn't just for completeness — it isolates which architectural pieces matter for which capabilities.** This makes it a defensible reviewer-facing artifact: "we considered simpler alternatives and showed why each architectural choice was necessary."

**Supporting evidence.**
- CTPC paper §6: explicit progression from Latent ODE★ → Latent ODE† → CTPC-ODE → CTPC-CDE → CTPC-CDE++ with each step adding one capability.
- Table 2: CTPC-CDE++ achieves the best long-horizon MSE (0.0967 at 4-day, 64% reduction) and competitive calibration (d̄² ≈ 1.15).

---

# Part II — Decisions Deferred to PhyArch-CTPC

The deployed CTPC works. But **the decoder's vector field `f_θ_d(z_d)` is currently a generic NCDE** — no physics priors inside the corrector itself. PhyArch DP demonstrates empirically that hardwiring symmetries gives massive OOD-robustness gains (0% failure rate vs PINN's 21.9% / LNN's 43.8%). The natural next-generation architecture is **PhyArch-CTPC**, which composes both. This section maps the open architectural questions.

## Q1. Where does PhyArch's symmetry hardwiring go inside CTPC?

**The composition gateway:** replace the decoder's vector field `f_θ_d(z_d)` with a [[PhyArch]]-style symmetry-hardwired vector field. The 5-step PhyArch recipe applied to orbital residuals:

| PhyArch step | Orbital instantiation |
|---|---|
| 1. Identify symmetry group | Two-body Kepler: SO(3); J2-perturbed: broken to SO(2) (axial); fully perturbed: weakly broken further |
| 2. Build geometric features | RTN basis vectors `(r̂, t̂, n̂)`, scalar invariants `‖r‖`, `r·v`, `‖h‖`, `e` (eccentricity vector) |
| 3. Split by parity / equivariance | For SO(2): scalar invariants vs. components in `(r̂, t̂, n̂)` frame |
| 4. Coefficient networks on invariants | Small MLPs `s_R(I), s_T(I), s_N(I)` taking only invariants |
| 5. Assemble: coefficient × basis | `f_θ_d(z_d) = s_R · r̂ + s_T · t̂ + s_N · n̂` (or higher-order multilinear forms) |

**Open architectural details:**

- (a) The decoder operates on a hidden state `z_d`, not directly on orbital state. How do we get from `z_d` back to a physical reference frame for the parity split? Either (i) constrain `z_d` to live on a frame-typed space throughout, or (ii) use the control path's RTN frame to define the typing implicitly.
- (b) PhyArch DP's "manipulator equation skeleton" doesn't have a direct orbital analog. The orbital equations of motion are `r̈ = −μ r/‖r‖³ + perturbations` — different algebraic skeleton. Does the assembly mirror this skeleton, or just the equivariance structure?
- (c) Step 3 for SO(2) (rotational symmetry) needs irreducible representations of SO(2) (just sin/cos basis on the angular variable). For SO(3), it needs Wigner-D / spherical harmonics — a research direction in itself.

## Q2. How does the variance head respect equivariance?

**Currently:** the prediction head outputs `(μ_t, L_t)` where `L_t` is a 3×3 lower-triangular Cholesky factor. `L_t`'s rows are unconstrained beyond positive-diagonal.

**PhyArch-CTPC requires:** `Σ_t = L_t L_t^T` should transform correctly under symmetry actions. Specifically, under reflection (DP) or rotation (orbital):
- Variance entries (diagonal) are *even* — positive scalars, don't flip
- Off-diagonal cross-covariances *do* flip sign under odd-parity actions involving both indices
- For continuous SO(3), `Σ_t` should transform as a rank-2 tensor: `Σ_t' = R Σ_t R^T` for rotation matrix `R`

**Open architectural options:**

- (a) Parameterize `L_t` via parity-typed rows: each row is constrained to be either even or odd in the geometric features. Untested but tractable.
- (b) Parameterize `Σ_t` directly via the `(r̂, t̂, n̂)` basis: `Σ_t = σ_RR² r̂r̂^T + σ_TT² t̂t̂^T + σ_NN² n̂n̂^T + cross terms`, with each scalar coefficient learned from invariants only. More structured but less expressive.
- (c) Free-form `L_t` + a regularization penalty on equivariance violations. Soft-constraint fallback if hard parameterization is too restrictive.

## Q3. Position-level corrector vs. acceleration-level corrector

**Mismatch:**
- CTPC corrects positions: `e(t) = x(t) − x̂(t) ∈ ℝ³`.
- PhyArch DP outputs accelerations: `q̈ = a(q, q̇) ∈ ℝ²`.

**Reconciliation paths:**

- (a) **Reformulate PhyArch for position-level.** Treat the position residual as a state and learn its dynamics. Doable but loses some of PhyArch's elegance — the manipulator-equation skeleton operates on accelerations, not positions.
- (b) **Differentiable predictor.** Differentiable GMAT (or open-source Astrodynamics-Toolbox) exposes `r̈` directly. PhyArch operates on `r̈`-residuals; integrate the result to get position correction. This matches the KDD paper's own future-work item ii ("coupling CTPCs with differentiable physics for end-to-end adaptation").
- (c) **Hybrid: PhyArch outputs accelerations, integrated forward + composed with GMAT positions to produce position predictions.** Requires care with ODE solver consistency between PhyArch's RK4 / variational and GMAT's internal integrator.

**Recommendation:** path (b) is the architecturally cleanest. Forces the rest of the project to confront differentiable-physics integration, which is itself a worthwhile research direction.

## Q4. End-to-end training with differentiable physics

**Current:** GMAT is a black-box external simulator. Training of the Corrector treats `x̂(t)` as fixed inputs.

**End-to-end target:** Differentiable predictor → joint training of (predictor parameters + corrector parameters). Benefits:
- Predictor parameters (drag coefficients, J2 magnitudes) can adapt to trajectory-specific anomalies
- Corrector residuals get smaller as predictor improves
- Gradients flow through the full pipeline → joint optimization of accuracy + calibration

**Architectural options:**
- (a) **Replace GMAT with a differentiable equivalent.** Astrodynamics-Toolbox in Diffrax-style JAX, or differentiable-physics-engine wrapper around a simpler propagator.
- (b) **Surrogate the GMAT internals as a Neural ODE matched to GMAT outputs.** Then differentiate through the surrogate.
- (c) **Implicit differentiation through GMAT.** Treat GMAT as a fixed-point operator and use implicit-diff theorems. More mathematically elegant, computationally heavier.

KDD paper future-work item ii. Open whether the gains justify the engineering cost.

## Q5. Formal verification + provable safety guarantees

**Current:** CTPC has stability theorems (5.1–5.4, D.6) but they're *bounds*, not formal proofs of correctness in a verification system. PhyArch has *algebraic* guarantees (parity structure exact, periodicity exact) but they're informal.

**Combined target:** A CTPC variant with hard architectural guarantees (PhyArch-style) + strict-properness training (CTPC-style) + formal verification of both, in a system like Lean / Coq / dReal.

**Connection to existing wiki content:**
- `raw/papers/formal-verification/` (already in the wiki) has 14 papers on proof assistants + NN robustness verification. Future ingest target.
- The existing Z₂ parity result in PhyArch DP is *literally* formally verifiable — the algebra is straightforward.
- The strict-properness theorems in CTPC are *also* formally verifiable but harder (require continuous probability measures).

KDD paper future-work item iii. The combination of hard architectural constraints + strict-properness + formal verification is the path to provably-safe SDA.

## Q6. Multi-spacecraft + parameter uncertainty + space weather covariates

**Current:** CTPC trains and evaluates per-spacecraft with no environmental conditioning.

**Target:** Multi-spacecraft training (transfer across satellites), conditioning on environmental covariates (space weather indices, solar flux, magnetospheric state), explicit parameter uncertainty (drag coefficient distributions, J2 measurement error).

**Architectural options:**
- Add covariates as additional channels in the encoder control path
- Learn a satellite-specific embedding + space-weather embedding jointly
- Hierarchical Bayesian extension of the latent posterior

KDD paper future-work item i. Operationally critical for production SDA.

## Q7. Conjunction probability sensitivity (the operational metric)

**Current:** CTPC reports MSE + d̄² + log|Σ̄|. None directly measure how well the uncertainty supports collision-avoidance decisions.

**Target:** Pc (collision probability) sensitivity analysis. Given two CTPC-derived trajectory + covariance estimates, compute Pc via Foster method or Monte Carlo. Compare against ground-truth Pc (where known) and against GMAT-only Pc (which assumes Gaussian uncertainty often underestimating tail risk).

**Connection to existing wiki content:**
- CDM (Conjunction Data Message) format mentioned in CLAUDE.md domain vocabulary.
- CARA MATLAB → Python port listed in CLAUDE.md priority concept areas.
- Foster method = standard Pc computation.

KDD paper limitations explicitly call this out as a needed downstream evaluation. **This is the metric AFRL ultimately cares about** — if PhyArch-CTPC can show *Pc-level* improvements over GMAT, that's the deployment case.

## Q8. HNN / LNN / PHNN integration

**Current:** GMAT *is* effectively a Hamiltonian propagator (orbital mechanics is Hamiltonian + small dissipative perturbations). The Corrector models the residual as a generic time series.

**Target:** Three-layer composition:
- **Hard physics layer:** SGP4-style or differentiable Hamiltonian core. Conservation laws by construction.
- **Dissipative residual layer:** PHNN-style for drag/SRP/atmospheric heating.
- **Probabilistic head:** Latent NCDE Corrector for uncertainty quantification.

Each layer hardwires a different aspect of the physics. The architectural composition is open until [[Port-Hamiltonian-Neural-Networks]] is ingested. Once it is, **PhyArch-CTPC + PHNN composition** becomes the natural next paper.

---

## How This Page Should Be Used

**As an AFRL reviewer-facing document:** Part I is the justification for every architectural choice. Each decision has an alternatives-considered section with explicit reasoning + supporting evidence. Together they answer "why CTPC, and why this version of CTPC."

**As a research-planning document:** Part II is the open-question roadmap. Each question has concrete architectural options + connections to other wiki content + relationship to the broader PhyArch-CTPC vision. New ingests (PHNN, formal-verification papers, differentiable-physics tools) will close some of these questions and open others.

**Update protocol:** when a new ingest resolves an open question or surfaces a new design constraint, edit this page to reflect it. Cite the new wiki page that provides the resolution. Promote resolved questions from Part II to Part I (with rationale). Demote any Part I decision that's been superseded.

## Connections

- [[CTPC-KDD-Submission]] — the framework being justified
- [[PhyArch-Double-Pendulum-Benchmark]] — the deterministic counterpart establishing the symmetry-hardwiring case
- [[PhyArch]] — the methodology pattern being composed in
- [[Latent-NCDE-Corrector]] — the architectural backbone of CTPC
- [[Neural-Controlled-Differential-Equation]] — the temporal substrate
- [[Hamiltonian-Mechanics]] — foundational physics for the predictor side
- [[Hamiltonian-vs-Lagrangian-Duality]] — sister synthesis page on the Hamiltonian↔Lagrangian axis
- [[PeRCNN]] — sibling Predictor-Corrector decomposition for spatial PDEs
- [[Pi-Block-Polynomial-Approximator]] — alternative inductive-bias architecture (polynomial form vs. PhyArch's symmetry hardwiring)
- [[Port-Hamiltonian-Neural-Networks]] *(not yet ingested)* — needed to close Q8 (HNN/LNN/PHNN integration)

## Sources

- `raw/papers/my-papers/KDD_submission.pdf` — Shahid, Jiang, Sarkar, Fleming 2026 (KDD '26 submission). The deployed CTPC framework.
- `raw/notes/PhyArch_DoublePendulum.pdf` — Bilal's deterministic benchmark notes. Validates symmetry-hardwiring on a controlled toy.
- `CLAUDE.md` § Core Philosophy — the SiS design hierarchy that this page operationalizes.
