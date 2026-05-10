---
title: CTPC Design Rationale (Decisions Made + Decisions Deferred to PhyArch-CTPC)
tags: [ctpc, sis-design, design-rationale, phyarch, synthesis, research-roadmap, afrl, actionable-interpretability, barbiero-symmetries, dissipation-route]
sources: [raw/papers/my-papers/KDD_submission.pdf, raw/notes/PhyArch_DoublePendulum.pdf, raw/papers/interpretability/Interpretability&Symmetry.pdf, raw/papers/interpretability/ConceptBottleneckModels.pdf, raw/papers/uncertainty-propagation/AnalyticUncertaintyProp.pdf, raw/papers/port-hamiltonian/DissipativeHNN.pdf, raw/papers/port-hamiltonian/DissipativeSymODEN-Zhong2020.pdf]
created: 2026-05-09
updated: 2026-05-10
sis_relevance: critical
hard_constraint_possible: yes
---

# CTPC Design Rationale (Decisions Made + Decisions Deferred to PhyArch-CTPC)

> **Purpose:** justify past architectural choices for [[CTPC-KDD-Submission]] (the deployed framework) AND map open research questions forward toward **PhyArch-CTPC** (the next-generation symmetry-hardwired variant). Functions both as a reviewer-facing rationale doc and as a research-planning document. Updated repeatedly as the project evolves.

> **Sources synthesized:**
> - [[CTPC-KDD-Submission]] — probabilistic Predictor-Corrector framework on real CDDIS spacecraft data
> - [[PhyArch-Double-Pendulum-Benchmark]] — deterministic symmetry-hardwiring benchmark on a controlled toy system
> - [[Actionable-Interpretability-Symmetries]] (Barbiero et al. 2026) — the four-symmetry framework operationalized in Part III. **Ingested 2026-05-10.** Part III was substantially revised after this ingest to correct four substantive errors in the original framing — see "Corrections from Barbiero Ingest (2026-05-10)" section in Part III.
> - [[Concept-Bottleneck-Models]] (Koh et al. ICML 2020) — closes concept-closure invariance AND information invariance (one architectural mechanism, two Barbiero symmetries) via [[CBM-CTPC-Composition]] (Year 1 milestone). Ingested 2026-05-09.
> - [[Analytic-Covariance-Propagation]] (Wright et al. AISTATS 2024) — refines inference equivariance to the full predictive distribution via [[Analytic-Sigma-CTPC-Composition]] (Year 2 milestone). Ingested 2026-05-09. *Note: originally framed as closing information invariance; corrected 2026-05-10 — see Part III corrections section.*

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

Each layer hardwires a different aspect of the physics.

**Q8 splits into three independently-tractable sub-questions** (split formalized 2026-05-10 after [[D-HNN]] and [[Dissipative-SymODEN]] ingests):

### Q8a. Helmholtz-decomposition route to dissipative HNN — *closed by D-HNN ingest 2026-05-10*

**Closed by:** [[D-HNN]] (Sosanya & Greydanus 2022). Architecture: two scalar subnetworks `H_θ(q,p)` + `D_θ(q,p)` combined via Helmholtz decomposition (`dx/dt = symplectic_grad(H) + grad(D)`).

**What this gives CTPC:**

- Structurally complete decomposition (any smooth field decomposes uniquely into the two components).
- Counterfactual generalization for free: multiplying `D_θ` by a scalar `α` simulates a different friction coefficient *without retraining*. This is the architectural template for dissipation-side concept-closure compliance — see § "Architectural Hook for Dissipation-Side Concept-Closure (D-HNN template)" in Part III below.
- Empirical validation (~4 orders of magnitude better than HNN on damped spring; positive results on real damped pendulum + ocean currents).

**What this does NOT give CTPC** — and why Q8b is needed: D-HNN does **not** structurally guarantee `Ḣ ≤ 0`. The energy rate `dH/dt = ∇H · ∇D` along D-HNN dynamics has indeterminate sign because the two scalars are unconstrained relative to each other. For SiS deployment safety, this is insufficient — see [[Dissipative-Hamiltonian-Neural-Network]] § "Why D-HNN is not enough for SiS" for the precise architectural analysis.

### Q8b-vector-field. J-R-structured route at continuous-time — *closed by Dissipative SymODEN ingest 2026-05-10*

**Closed by:** [[Dissipative-SymODEN]] (Zhong, Dey, Chakraborty 2020). Architecture: four neural networks `(M⁻¹_θ1, V_θ2, D_θ4, g_θ3)` parameterizing a port-Hamiltonian form `dx/dt = (J − D(q))∇H + g(q)u` with the restricted Hamiltonian `H = ½ pᵀ M⁻¹(q) p + V(q)`. **The load-bearing primitive: Cholesky parameterization** of `D = LLᵀ` makes the dissipation matrix PSD-by-construction. Energy rate is `dH/dt = −∇HᵀD∇H ≤ 0` *structurally* — passivity is built into the algebraic form, not learned. See [[Port-Hamiltonian-Neural-Network]] for the SiS-canonical architectural treatment.

**Why this is a structural match for orbital, not a compromise.** Dissipative SymODEN restricts the Hamiltonian to the *mechanical form* `H = ½ pᵀ M⁻¹(q) p + V(q)` — the same restriction LNN explicitly criticized DeLaN for. **For orbital dynamics, this restriction is exactly what physics already provides:**

```
H_orbital = |p|²/(2m) + V_J2(r) + V_J3(r) + V_3body(r) + V_SRP(r) + ...
```

The kinetic energy is exactly quadratic in `p`; perturbations enter through `V(q)` (potential) or `D(q)` (drag dissipation) — never through non-quadratic `T(p)` terms. **Dissipative SymODEN's mechanical-form restriction is therefore a structural match for SiS, not a compromise.** The architecture matches orbital physics; expressivity is not sacrificed for the orbital domain. (For a relativistic GEO-precision regime where `p ≠ m·q̇`, the LNN-side analog would be needed instead — see [[Hamiltonian-vs-Lagrangian-Duality]].)

**Native control-input channel `g(q)u` — forward-looking asset.** The architecture supports an external control input via `g_θ3(q) u`. **For current CTPC, set `u = 0`** (no actuator commands during the prediction window). For a future SiS extension to integrated maneuver planning (joint state forecasting + Δv recommendation), the `u` channel is exactly the right architectural slot for thrust / Δv commands. This is a free asset banked into the architecture, not a current need.

### Q8b-discrete-time. Exact discrete passivity — *open; needs structure-preserving integrator*

**The remaining gap.** Dissipative SymODEN uses **RK4** as its numerical integrator, *not* a structure-preserving integrator. Despite the name "SymODEN", the symplectic structure is in the *vector field form*, not the integrator. Consequence: the `Ḣ ≤ 0` guarantee derived above holds at the level of the continuous-time port-Hamiltonian ODE; the discrete RK4 trajectory only satisfies it *approximately* — accurate to RK4's `O(Δt⁴)` truncation error per step.

For SiS deployment-safety claims, the architectural `Ḣ ≤ 0` guarantee is real *up to integrator error*. For a structurally-exact discrete passivity guarantee, three candidate structure-preserving integrators are flagged in [[Port-Hamiltonian-Neural-Network]] § "Integrator Caveat" as concrete next-stage targets:

- **Discrete gradient method** (Gonzalez 1996; McLachlan-Quispel-Robidoux 1999) — replaces `∇H` with a discrete-gradient operator that preserves the energy/dissipation balance exactly at the discrete level.
- **Average-Vector-Field (AVF) method** (Quispel-McLaren 2008) — second-order energy-preserving integrator; extended versions handle dissipation.
- **Gauss-collocation methods** (s-stage Gauss-Legendre) — implicit Runge-Kutta that is symplectic for Hamiltonian systems with substantially smaller energy drift than explicit methods on dissipative perturbations.

**Status:** Open until follow-up ingest of any of the three. When that happens, this section becomes the resolution point.

### Composition with the rest of CTPC (Q8a and Q8b)

Both routes plug into the Latent NCDE decoder's vector field `f_θ_d(z_d)`. The composition gateway is the same as Q1 (PhyArch composition). The architectural difference between routes is *internal* to the dissipation block, not at the composition interface.

Three-axis design space for the corrector (see [[Hamiltonian-vs-Lagrangian-Duality]] § "A Fourth Axis: Dissipation Route" for the full treatment):

| Axis | Choices | What it controls |
|---|---|---|
| Conservative formalism | HNN / LNN | Which scalar (`H` or `L`) to parameterize |
| Symmetry hardwiring | PhyArch / no | Whether spatial symmetries are baked into architecture |
| Dissipation route | Helmholtz / J-R | Whether dissipation is added via second scalar (Q8a route) or via PSD damping matrix (Q8b route) |

**The principled triple for SiS is `(HNN, PhyArch, J-R)`.** HNN-side conservative core (orbital is canonical, mechanical-form Hamiltonian fits exactly); PhyArch hardwiring of spatial symmetries inside the four port-Hamiltonian networks; J-R route via Dissipative-SymODEN-style Cholesky parameterization for `Ḣ ≤ 0`. **PhyArch-CTPC + PHNN-proper composition** is now the natural next paper — both architectural pieces are ingested, the composition is the open implementation work.

**Composition with PhyArch.** PhyArch hardwires *spatial symmetries* (parity-split, geometric features) — independent of the dissipation route. PhyArch + J-R route gives:
- Spatial symmetries from PhyArch (forced).
- `Ḣ ≤ 0` from J-R structure (forced).
- Conservation of `H` from the `(J − R)∇H` form (forced when `R = 0`).

This is the principled hardwiring stack for an orbital corrector.

### Composition with the rest of CTPC (both Q8a and Q8b)

Both routes plug into the Latent NCDE decoder's vector field `f_θ_d(z_d)`. The composition gateway is the same as Q1 (PhyArch composition). The architectural difference between routes is *internal* to the dissipation block, not at the composition interface.

Three-axis design space for the corrector (see [[Hamiltonian-vs-Lagrangian-Duality]] § "A Fourth Axis: Dissipation Route" for the full treatment):

| Axis | Choices | What it controls |
|---|---|---|
| Conservative formalism | HNN / LNN | Which scalar (`H` or `L`) to parameterize |
| Symmetry hardwiring | PhyArch / no | Whether spatial symmetries are baked into architecture |
| Dissipation route | Helmholtz / J-R | Whether dissipation is added via second scalar (Q8a route) or via PSD damping matrix (Q8b route) |

**The principled triple for SiS is `(LNN-or-HNN, PhyArch, J-R)`.** LNN-vs-HNN driven by coordinate frame; J-R driven by deployment-safety.

---

# Part III — PhyArch + CBM-CTPC + Analytic-Σ-CTPC under the Barbiero Framework

[[Actionable-Interpretability-Symmetries|Barbiero et al. 2026]] is a *position paper* arguing that interpretability research should be defined via four specific symmetries: **inference equivariance** (a user can predict the model's outputs), **information invariance** (existence of a compressed sufficient statistic), **concept-closure invariance** (sound translation between concept vocabularies), and **structural invariance** (model's hypothesis class matches the user's mental model). The paper *posits* that these four suffice; it does not establish them as field consensus. This Part III maps PhyArch + CBM-CTPC + Analytic-Σ-CTPC onto the Barbiero framework, with explicit user-relativity caveats.

> **Audit note (2026-05-10).** This Part III was originally drafted (2026-05-09) from discussion context, before the Barbiero paper was ingested. After the canonical ingest ([[Actionable-Interpretability-Symmetries]]), four substantive errors were identified and corrected. **The visible audit trail is at the end of this Part III** ("Corrections from Barbiero Ingest"). The corrections are not embarrassing — they demonstrate the wiki's verification workflow (CLAUDE.md "Never assert a claim without grounding it in a raw source") catching prior overstatement when the source ingest happened.

## The Four Symmetries Mapped to PhyArch / CTPC (corrected after Barbiero ingest)

| Barbiero symmetry | Paper definition (formal) | PhyArch + CBM-CTPC + Analytic-Σ-CTPC instantiation |
|---|---|---|
| **Inference equivariance** | Inference `P_{Y\|X}(Y \| X = x)` is equivariant w.r.t. user mental-model class `Hm` under translations `τ_X : X → X[h]` and `τ_Y : Y → Y[h]` if there exists `P_{Y\|X}^{[h]} ∈ Hm` such that translate-then-predict equals predict-then-translate (diagram commutativity, paper Symmetry 1) | **For mean prediction**: ✓ via [[PhyArch]] parity-split assembly + geometric features. Algebraically enforced under `Z₂` (DP) or `SO(2)` (orbital) for users who understand the relevant symmetry. **For full predictive distribution**: currently approximate due to K-sample MC noise on the covariance; concrete verification plan via [[Analytic-Sigma-CTPC-Composition]] (Year 2) makes mean and covariance both exactly equivariant under input frame transformations. |
| **Information invariance** | Compressed sufficient statistic `Z` with `H(Z) ≪ H(X)` and `I(Y;X\|Z) = 0`. Markov factorization `P_{Y\|X} = P_{Y\|Z} ∘ P_{Z\|X}` (paper Symmetry 2). About **information bottleneck / minimal sufficient statistic**, NOT about full-distribution invariance under input encodings. | **NOT YET** in current PhyArch + CTPC — geometric features are an *embedding* (4 angles → 12 features), not compression. **Concrete verification plan**: the `k=9` concept bottleneck in [[CBM-CTPC-Composition]] IS the compressed sufficient-statistic Z. Information invariance is closed when concept accuracy verifies that `I(Y;X\|ĉ) ≈ 0` (Test 1 of CBM-CTPC verification protocol). |
| **Concept-closure invariance** | Sound translation `τ_C : T → T'` between concept vocabularies preserving the underlying object set — `(T, M) ↔ (T', M)`. About *vocabulary translation soundness* (paper Symmetry 3 + Section 2.3.1, citing CBMs as the mechanism). **Interventions are a downstream consequence** in Section 4, not the symmetry itself. | **NOT YET** in current PhyArch + CTPC — no concept layer with vocabulary structure. **Concrete verification plan**: [[CBM-CTPC-Composition]] enforces concept-closure by construction. The k=9 concept set has named physics semantics; alternative vocabularies translating `drag ↔ atmospheric_dissipation` preserve the underlying physical mechanism. |
| **Structural invariance** | Model's hypothesis class matches **the user's mental model** `Hm` (paper Symmetry 4 + Section 2.4.1). User-relative — "linearity, monotonicity, or sparsity are not first principles but instantiations chosen to match a given user." | **For an orbital-mechanics-trained user (the AFRL operator)**: ✓ via PhyArch's manipulator-equation skeleton — the user's mental model IS the manipulator equation. **For a generic ML practitioner**: weaker; the relevant structural form is different. **Open issue:** CBM-CTPC prediction head `f: ĉ → ŷ` needs explicit structural commitment (linear / monotonic / manipulator-skeleton) — paper Section 2.4.1 explicitly disqualifies CBMs with free-form MLP task predictors. See [[CBM-CTPC-Composition]] § Open Design Decisions. |

## Where Other Methods Stand (corrected after Barbiero ingest)

Symmetry assignments below reflect the *paper's actual definitions* and explicit (dis)qualifications in Sections 2.2.1, 2.3.1, and 2.4.1.

| Method | Inference equivariance | Information invariance | Concept closure | Structural invariance |
|---|---|---|---|---|
| Post-hoc explainers (SHAP, LIME) | N/A (post-hoc, not a model itself) | ✗ — paper §2.2.1 explicitly disqualifies | N/A | N/A |
| Rudin sparse models on raw inputs | ✓ for mathematically literate user | ✓ (sparsity → compression) | ✗ — paper §2.3.1 disqualifies decision trees / additive models on non-concept spaces | ✓ (sparse / linear / decision rule = well-defined hypothesis class) |
| Equivariant NNs (EGNN, Tensor Field Networks) | ✓ for the relevant group | ⚠️ partial (group-equivariant networks remain high-dim) | ✗ (no concept alignment) | ⚠️ user-dependent |
| Concept Bottleneck Models (Koh et al. 2020) | ✓ for trained user | **✓** (k-dim bottleneck IS compression) | **✓** — paper §2.3.1 explicitly cites CBMs as the mechanism | ✗ if `f: c → y` is free-form MLP — paper §2.4.1 explicitly disqualifies; ✓ if `f` has explicit structural commitment |
| **PhyArch + CTPC (current, 2026-05)** | ✓ for mean (parity-split + geometric features) | **NOT YET** (no compressed bottleneck) | **NOT YET** (no concept layer) | ✓ for orbital-mechanics-trained user (manipulator-skeleton) |
| **PhyArch + CBM-CTPC (Year 1, post-verification)** | ✓ for mean | **✓** (CBM bottleneck IS the compressed Z) | **✓** (CBM concept layer with verified semantics) | ✓ contingent on structurally-committed head (open decision) |
| **PhyArch + CBM-CTPC + Analytic-Σ-CTPC (Year 2 end-state)** | **✓ for full distribution** (mean + covariance both exactly equivariant under input transforms) | ✓ | ✓ | ✓ contingent on structurally-committed head |

**Key observations from the corrected table:**

- **SHAP / LIME** are explicitly disqualified by the paper for failing information invariance (§2.2.1: "operates in the original input space and does not guarantee the existence of a lower-dimensional representation Z"). Other rows are N/A because post-hoc explainers are not models in the framework's sense.
- **Rudin sparse models** satisfy three of four for the right user — but fail concept closure when applied on raw input spaces, which is the paper's specific disqualification.
- **CBMs alone** satisfy concept closure AND information invariance (the bottleneck IS the compressed Z) — a single architectural mechanism for two symmetries. They fail structural invariance with free-form MLP heads (paper §2.4.1 disqualification).
- **PhyArch + CTPC alone** satisfies inference equivariance (for mean) and structural invariance (for physics-trained user) but lacks the bottleneck needed for information invariance and concept closure. **Year 1 (CBM-CTPC) and Year 2 (Analytic-Σ-CTPC) compositions close these gaps.**
- The end-state (PhyArch + CBM-CTPC + Analytic-Σ-CTPC) is the only entry checking all four boxes — *for the orbital-mechanics-trained user*. This is the user-relative form of the publishable claim below.

This table reads as the **gap-filling response** to the paper's Section 6 diagnostic: "the concept-based interpretability community has largely focused on concept-closure invariance... but has paid comparatively little attention to structural invariance." PhyArch's manipulator-skeleton brings structural invariance to the concept-based interpretability community; the orbital domain is the operational instantiation.

## The Publishable Claim (softened after Barbiero ingest)

> **PhyArch + CBM-CTPC + Analytic-Σ-CTPC is a candidate practical instantiation of [[Actionable-Interpretability-Symmetries|Barbiero et al. 2026]]'s four-symmetry framework in safety-critical dynamical systems, with concrete verification protocols for each symmetry, contingent on a target user whose mental model includes orbital-mechanics structure.** The composition addresses Barbiero §6's explicit gap diagnostic — that the concept-based interpretability community has neglected structural invariance — by combining CBM-style concept-closure-and-information-invariance (one bottleneck, two symmetries) with PhyArch-style structural invariance (manipulator-equation skeleton matching the orbital-mechanics-trained user's mental model). Validation: [[PhyArch-Double-Pendulum-Benchmark]] (deterministic, controlled toy) for inference equivariance + structural invariance; [[CTPC-KDD-Submission]] (probabilistic, real spacecraft data) for the same two on real OOD trajectories. The remaining symmetries close via [[CBM-CTPC-Composition]] (Year 1) and [[Analytic-Sigma-CTPC-Composition]] (Year 2).

The claim is publishable independently of any single experiment as a framework-level architectural argument. **Three explicit caveats**:

1. The Barbiero framework is itself a *position paper* (arXiv Jan 2026), not yet established as field consensus. Claims that lean on the framework being authoritative inherit this dependency.
2. Structural invariance is *user-relative*. The "orbital-mechanics-trained user" target is the AFRL operator class — defensible operationally, but the claim doesn't extend to a generic ML practitioner.
3. The four symmetries are not yet all empirically verified — they have *concrete verification protocols* (Year 1 + Year 2) but the protocols haven't been run.

## Current Status of Each Symmetry (after Barbiero ingest)

### Inference equivariance — *for mean*, ✓ now; *for full distribution*, Year 2

PhyArch's parity-split assembly + geometric features make the mean prediction algebraically equivariant under the relevant symmetry group. For DP this is exact under `Z₂` reflection; for orbital it's exact under `SO(2)` rotation around Earth's pole (J2-broken from full `SO(3)`). The full predictive distribution (mean + covariance) currently has approximate covariance equivariance due to K-sample MC noise. [[Analytic-Sigma-CTPC-Composition]] makes both exact via analytic moment propagation — the Year 2 milestone.

### Information invariance — Year 1 milestone via the CBM bottleneck

Geometric features in PhyArch are an *embedding*, not a compression — they don't satisfy information invariance per the paper's definition (`H(Z) ≪ H(X)`). The k=9 concept bottleneck in [[CBM-CTPC-Composition]] IS the compressed sufficient statistic Z; **the bottleneck closes information invariance simultaneously with concept-closure invariance** (one architectural mechanism, two symmetries). Verification: Test 1 of CBM-CTPC verification protocol checks that `I(Y;X|ĉ) ≈ 0`.

### Concept-closure invariance — Year 1 milestone via the CBM concept layer

PhyArch's coefficient networks `aᵢⱼ(z_even)` correspond to manipulator-equation entries by construction, but this is *structural* alignment (the `aᵢⱼ` indices map to physics quantities), not *concept-vocabulary* alignment in the Barbiero sense (sound translation between concept symbol sets). [[CBM-CTPC-Composition]] adds explicit named-concept supervision (J2, drag, SRP, etc.) — the bottleneck enforces concept-closure by construction (Koh et al. 2020 cited as the mechanism in paper §2.3.1). Verification: Tests 2–4 of CBM-CTPC verification protocol check intervention semantics, composability, and sanity.

### Structural invariance — *user-relative*; ✓ for orbital-mechanics-trained user

PhyArch's manipulator-equation skeleton matches the user's mental model when the user is an orbital-mechanics-trained operator (the AFRL deployment case). For a generic ML practitioner the relevant structural form is different; the claim doesn't transfer.

**Open issue.** Per Barbiero §2.4.1, CBMs with free-form MLP task predictors fail structural invariance ("hypothesis space lacks any well-defined structure"). For CBM-CTPC, the prediction head `f: ĉ → ŷ` therefore needs an explicit structural commitment. The choice (linear / monotonic / manipulator-skeleton) is flagged as an **open design decision** in [[CBM-CTPC-Composition]] § Open Design Decisions; the right answer is user-dependent and is part of the Year 1 implementation work.

### Status table

| Symmetry | Pre-Year-1 status | Year 1 status | Year 2 status | Closing mechanism |
|---|---|---|---|---|
| Inference equivariance (mean) | ✓ | ✓ | ✓ | PhyArch parity-split |
| Inference equivariance (full distribution) | ⚠️ approximate (MC noise) | ⚠️ approximate | ✓ exact | Analytic-Σ propagation |
| Information invariance | NOT YET | ✓ | ✓ | CBM bottleneck (k=9 compressed Z) |
| Concept-closure invariance | NOT YET | ✓ | ✓ | CBM concept layer |
| Structural invariance | ✓ for physics user | ✓ contingent on head commitment | ✓ contingent on head commitment | PhyArch manipulator-skeleton + structurally-committed head |

## Architectural Hook for Dissipation-Side Concept-Closure (D-HNN Template)

> **Design insight, not just a paper observation.** Surfaced 2026-05-10 during [[D-HNN]] ingest. **Architectural decomposition of dynamics into scalar fields gives a free hook for counterfactual interventions on the parameters of those scalar fields.** This is a design-level statement about *how* CTPC will satisfy concept-closure compliance on the *dissipation side* — independent of whether the implementation uses D-HNN's Helmholtz route (Q8a) or PHNN-proper's J-R route (Q8b).

### The mechanism (D-HNN as the worked example)

[[D-HNN]] trains two scalar subnetworks `(H_θ, D_θ)` jointly. At inference, replacing `D_θ → α · D_θ` simulates dynamics under a counterfactual friction coefficient — *without retraining*. The model only ever saw one friction coefficient during training but recovers the full one-parameter family ([[D-HNN]] Fig. 3). The conservative `H_θ` is untouched, so the conservative dynamics remain correctly modeled under the counterfactual.

### Why this counts as concept-closure compliance under [[Actionable-Interpretability-Symmetries|Barbiero et al. 2026]]

Concept-closure invariance (paper §2.3) requires sound translation between concept vocabularies — `τ_C : T → T'` is sound if for any pair of concepts `C = (T, M)` and `C' = (T', M)` (same underlying object set), the closure diagram commutes. The dissipation-coefficient concept space is exactly this kind of structure: two named concepts (`friction`, `α · friction`) refer to the same underlying physical object (the dissipation force field) related by a multiplicative scaling. **The architecture supports the translation by construction** — `α · D_θ` produces dynamics consistent with the rescaled physical reality, not just a numerical perturbation.

Critically, this is *cleaner* than concept-closure via the [[CBM-CTPC-Composition]] k=9 concept bottleneck for *parametric* concepts. The CBM bottleneck enforces concept-closure on *categorical* concepts (J2 vs J3 vs drag vs SRP — discrete physical mechanisms); D-HNN's `α`-scaling enforces it on *parametric* concepts (the magnitude of a fixed mechanism). They are *complementary*, not redundant.

### The transferable design template

The Helmholtz-route is one realization. The same architectural pattern transfers to the J-R route (Q8b-vector-field, now closed by [[Dissipative-SymODEN]]):

**General principle:** parameterize each *individually-interpretable* dissipation channel as a *separate* PSD operator inside `D(q)`:
```
D(q) = α_drag · D_drag(q) + α_SRP · D_SRP(q) + α_thermal · D_thermal(q) + ...
```
with each `D_i(q) = L_i Lᵢᵀ` Cholesky-PSD-parameterized (per Dissipative SymODEN §3.5) and each `α_i` a learnable scalar magnitude. At inference, intervening on `α_drag` or `α_SRP` independently simulates counterfactual environments — same `α`-scaling mechanism, but applied to PSD operators rather than to a free scalar `D_θ`.

This preserves:
- **Q8b-vector-field's `Ḣ ≤ 0` guarantee** (sum of PSD matrices is PSD; Cholesky factors compose cleanly).
- **D-HNN's counterfactual-intervention hook** (each `α_i` is independently scalable).
- **CBM-CTPC's named-concept semantics** (each `D_i` is the concept "`drag`", "`SRP`", "`thermal`" — a decomposition that aligns with the k=9 concept set).

**Realizability check (post-Dissipative-SymODEN ingest 2026-05-10).** [[Dissipative-SymODEN]] uses a *single* Cholesky-PSD `D_θ4(q)` matrix. The channel-decomposed extension above is a *straightforward generalization* — sum of Cholesky-PSD matrices is PSD, training adds one parameter per channel for the `α_i` scalars (or per-channel PSD constraint via separate Cholesky factors). **The extension is concrete and implementable** as a Year 1 architectural commitment; it does not require additional theory.

### Implication for the CTPC architecture

**For Year 1 (CBM-CTPC, [[CBM-CTPC-Composition]]):** the prediction head `f: ĉ → ŷ` decomposes into per-concept dissipation operators. The Year 1 implementation should *not* use a monolithic `R(x)` — it should use the channel-decomposed form so that the CBM concept layer's interventions on individual concepts (e.g., `c_drag = 0` to ablate drag) translate cleanly into architectural interventions on the corresponding `R_drag` term.

**For Year 2 (Analytic-Σ-CTPC, [[Analytic-Sigma-CTPC-Composition]]):** the analytic moment-propagation result transfers per-channel — propagating `Σ` through `R(x) = Σ_i α_i R_i(x)` is just the linear superposition of per-channel propagation, since `R` enters the dynamics linearly.

**For Year 3+ (formal verification):** each `R_i` being PSD-by-parameterization is a *machine-checkable* algebraic property. The intervention semantics (`α_i · R_i` simulates `α_i`-scaled `i`-th dissipation) is also machine-checkable. The Q8b architecture composed with channel decomposition is a strong candidate for the formal-verification target ([[CTPC-Design-Rationale]] Part II Q5).

### Status

**Open design decision — deferred to Year 1 implementation.** Whether each individual `R_i` is parameterized as full Cholesky, low-rank-plus-diagonal, or block-structured is an implementation choice driven by the d-dimension scaling. Flagged for Year 1 architecture review. The *insight* (channel-decomposed `R(x)` for free counterfactual interventions) is the design commitment; the *parameterization* of each channel is the implementation detail.

This is the kind of architectural decision that goes in [[CBM-CTPC-Composition]] § Open Design Decisions alongside the prediction-head structural commitment (linear / monotonic / manipulator-skeleton).

## Two-Year Research Arc (corrected after Barbiero ingest)

| Time | Milestone | Status | Closes |
|---|---|---|---|
| **Now (2026-05)** | PhyArch DP benchmark + CTPC KDD submission | ✓ Done | Inference equivariance for mean (PhyArch parity-split); structural invariance for orbital-mechanics-trained user (manipulator-skeleton) |
| **Now (2026-05-09)** | CBM-CTPC composition design ([[CBM-CTPC-Composition]]) — architecture, 9-concept set, independent training scheme, four-test verification protocol | ✓ Designed (implementation pending) | **BOTH concept-closure AND information invariance** (the k=9 bottleneck IS the compressed sufficient statistic Z): from "not yet" to "concrete verifiable plan" |
| **Year 1** | Implement CBM-CTPC; run verification protocol; report concept accuracy + intervention-success rates + concept-alignment metrics. **Plus:** decide structural commitment of prediction head `f: ĉ → ŷ` (linear / monotonic / manipulator-skeleton) | Open | Concept-closure + information invariance: from *concrete plan* to *empirically verified* |
| **Now (2026-05-09)** | Analytic-Σ-CTPC composition design ([[Analytic-Sigma-CTPC-Composition]]) — replace K-sample MC with [[Analytic-Covariance-Propagation\|Wright et al. 2024]] analytic moment propagation, enable TLE uncertainty propagation, frame-equivariance verification protocol | ✓ Designed (implementation pending) | **Inference equivariance refined to full distribution** (NOT information invariance per Barbiero — that's Year 1's job): from *partial (mean only)* to *concrete verifiable plan* |
| **Year 2** | Implement Analytic-Σ-CTPC; run frame-equivariance test (`Σ_t^ECI = R^T Σ_t^RTN R` to machine precision); derive analytic-Σ-NCDE theorem (potential standalone publication) | Open | Inference equivariance for full predictive distribution: from *concrete plan* to *empirically verified* |
| **End-state** | Full Barbiero et al. 2026 framework instantiation for orbital-mechanics-trained users + formal verification | Aspirational | All four symmetries empirically verified + formally proven (links to Part II Q5) |

**Sequencing rationale (corrected):**

- **CBM bottleneck first (Year 1).** Closes TWO symmetries at once — concept-closure AND information invariance — because the bottleneck IS the compressed sufficient-statistic Z required by Barbiero's Symmetry 2. Plus makes the model debuggable: you cannot safely deploy a model whose internal concepts you cannot inspect.
- **Structurally-committed head (within Year 1).** Per Barbiero §2.4.1, free-form MLP heads disqualify CBM from structural invariance. The head commitment (linear / monotonic / manipulator-skeleton) needs to be made during the CBM-CTPC implementation. Open design decision in [[CBM-CTPC-Composition]].
- **Analytic-Σ propagation second (Year 2).** Refines inference equivariance from "mean exact + covariance approximate (MC noise)" to "full distribution exactly equivariant under input transformations." This is *not* about closing information invariance (which Year 1 already did); it's about extending an existing symmetry from one moment to all moments.
- **Formal verification last.** Worth the effort only when the architectural pieces are stable. Premature verification of an unstable architecture produces re-work. Connect to Part II Q5 once the architectural pieces are settled.

The end-state is a CTPC variant where:

1. Architecture instantiates all four Barbiero symmetries for orbital-mechanics-trained users.
2. Each instantiation is empirically verified by per-symmetry verification protocols.
3. The whole pipeline is formally verified against a specification of "safe orbital correction."

This is the AFRL-deployable form. The current 2026-05 state is the architectural foundation for that deployable form, not the deployable form itself.

## Cross-References (Part III specific)

- [[Actionable-Interpretability-Symmetries]] — Barbiero et al. 2026 canonical paper page. **Now ingested 2026-05-10.** This is the source against which the Part III corrections were validated.
- [[Concept-Bottleneck-Models]] — Koh et al. ICML 2020 concept-bottleneck pattern, cited by Barbiero §2.3.1 as the mechanism for concept-closure invariance. **Ingested 2026-05-09.**
- [[CBM-CTPC-Composition]] — concrete Year 1 milestone design: how to insert a CBM into the Latent NCDE Corrector. Closes BOTH concept-closure AND information invariance simultaneously (the k=9 bottleneck IS the compressed sufficient statistic Z). **Created 2026-05-09.**
- [[Analytic-Covariance-Propagation]] — Wright et al. AISTATS 2024 paper providing the technical machinery for analytic moment propagation through neural networks. **Ingested 2026-05-09.**
- [[Analytic-Sigma-CTPC-Composition]] — concrete Year 2 milestone design refining inference equivariance from "mean exact" to "full distribution exact" via analytic Σ propagation. **Created 2026-05-09; reframed 2026-05-10** — does NOT close Barbiero's information invariance (which is Year 1's job); refines inference equivariance for the full distribution.
- [[Physics-Based-FD-Convolutional-Layer]] — the discrete analog of "freeze what's known structurally" for spatial PDEs; PhyArch's parity-split assembly is the symmetry-respecting analog for ODEs.

---

## Corrections from Barbiero Ingest (2026-05-10)

> **Why this section exists.** Part III was originally drafted (2026-05-09) before [[Actionable-Interpretability-Symmetries|Barbiero et al. 2026]] was ingested as a wiki page. The four-symmetry framework summarized in Part III came from discussion context (Bilal's verbal description), not from the paper itself. After the canonical ingest (2026-05-10), four substantive errors were identified and corrected.
>
> **The visible audit trail is intentional.** This is a demonstration that the wiki's verification workflow (CLAUDE.md *Foundational Reference* + *Page Conventions* + "Never assert a claim without grounding it in a raw source or existing wiki page") works as designed: catching prior overstatement when the canonical source ingest happened. An AFRL reviewer reading this section sees intellectual honesty and a self-correcting documentation pipeline, not sloppiness.

### Correction 1 — Information invariance is about compression, not full-distribution invariance under input transformations

**Original framing (2026-05-09):** Information invariance was framed as "same prediction under redundant / equivalent input encodings," with the gap framed as "covariance not yet" closeable by analytic moment propagation (Year 2 milestone via [[Analytic-Sigma-CTPC-Composition]]).

**What the paper actually says (Barbiero §2.2):** Information invariance requires a compressed sufficient statistic `Z` with `H(Z) ≪ H(X)` AND `I(Y;X|Z) = 0`. This is about the *information bottleneck / minimal sufficient statistic* property — Markov factorization `P_{Y|X} = P_{Y|Z} ∘ P_{Z|X}`.

**Implications for the milestones:**

- **CBM-CTPC (Year 1) closes BOTH concept-closure invariance AND information invariance simultaneously** — the k=9 concept bottleneck IS the compressed sufficient-statistic Z that information invariance requires. This is the *most important correction*: the original framing double-counted the symmetries by attributing concept-closure to CBM and information invariance to analytic Σ. In fact the bottleneck closes both. **One architectural mechanism, two Barbiero symmetries.** See [[CBM-CTPC-Composition]] for the elevated framing.
- **Analytic-Σ-CTPC (Year 2) does NOT close Barbiero's information invariance.** What it actually refines is *inference equivariance applied to the full predictive distribution*: the mean is currently frame-equivariant via geometric features; the covariance is approximate due to K-sample MC noise. Year 2 makes both exactly equivariant — extending inference equivariance from "mean only" to "full distribution under input transformations." Still a valuable contribution; just not the Barbiero symmetry it was originally claimed to close.

### Correction 2 — Concept-closure invariance is about sound translation, not intervention compositionality

**Original framing:** Concept-closure invariance was framed as "operating on internal concepts produces predictions that respect those concept manipulations (intervention compositionality)."

**What the paper actually says (Barbiero §2.3):** Concept-closure invariance is about *sound translation between concept vocabularies* — `τ_C : T → T'` is sound if for any pair of concepts `C = (T, M)` and `C' = (T', M)` (same underlying object set), the closure diagram commutes. Example: `τ_C = {black → noir}` is sound because both refer to the same set of black objects. *Interventions* are a downstream consequence in §4 (Bayesian inversion on aligned concept-based models), not the symmetry definition itself.

**Implications:** The CBM mapping is still correct — paper §2.3.1 explicitly cites CBMs as enforcing concept-closure by construction. The intervention story in [[CBM-CTPC-Composition]]'s verification protocol is a downstream consequence enabled by the framework, not the symmetry definition. The verification protocol's intervention tests are valid checks of the *framework's downstream consequences*, but concept-closure invariance itself is about vocabulary translation soundness.

### Correction 3 — Structural invariance is user-relative, not "physics-form-mimicking"

**Original framing:** Structural invariance was framed as "architecture mirrors the system's structural form" — implying a universal property of physics-mimicking architectures.

**What the paper actually says (Barbiero §2.4):** Structural invariance requires the model's hypothesis class to match the *user's mental model* `Hm`. The paper emphasizes (§2.4.1): "linearity, monotonicity, or sparsity are not first principles but instantiations of structural properties chosen to match a given user." User-relative.

**Implications:**

- All claims about PhyArch+CTPC's structural invariance must specify the target user class. PhyArch's manipulator-skeleton satisfies structural invariance *for an orbital-mechanics-trained user* (the AFRL operator); for a generic ML practitioner the relevant structural form is different.
- Barbiero §2.4.1 explicitly disqualifies CBMs with DNN task predictors: "concept-based models whose mappings are arbitrary, such as employing DNNs, have a hypothesis space that lacks any well-defined structure and therefore do not satisfy this symmetry." For CBM-CTPC, the prediction head `f: ĉ → ŷ` therefore needs an explicit structural commitment (linear / monotonic / manipulator-skeleton). This is now flagged as an open design decision in [[CBM-CTPC-Composition]] § Open Design Decisions.

### Correction 4 — The publishable claim was overstated

**Original claim:** "PhyArch + CTPC is the first practical instantiation of actionable interpretability in safety-critical dynamical systems, where all four symmetries identified in Barbiero et al. 2026 are enforced architecturally."

**Two problems:**

1. **"First practical instantiation"** claims primacy in a framework whose primacy isn't established. Barbiero et al. is a *position paper* (says so in title — "Position: Actionable Interpretability Must Be Defined in Terms of Symmetries") arguing this framework SHOULD be the standard, not that it IS. Claiming "first instantiation" of an argued-for framework is fragile.
2. **"All four enforced architecturally"** needs requalification because (a) information invariance requires *compression*, which only the CBM bottleneck provides — not PhyArch alone; (b) structural invariance is user-relative; (c) inference equivariance currently holds only for the mean, not the full predictive distribution.

**Corrected claim** (now in "The Publishable Claim" section above, post-correction):

> "PhyArch + CBM-CTPC + Analytic-Σ-CTPC is a candidate practical instantiation of [[Actionable-Interpretability-Symmetries|Barbiero et al. 2026]]'s four-symmetry framework in safety-critical dynamical systems, with concrete verification protocols for each symmetry, contingent on a target user whose mental model includes orbital-mechanics structure."

Drops "first." Acknowledges position-paper status. Makes user-relativity explicit. Still defensible to AFRL operators (the target physics-trained user class). **Three explicit caveats are now stated in the publishable-claim section above** (position-paper status, user-relativity, verification not yet executed).

### Connection to Barbiero §6 Call to Action

The paper's §6 explicitly identifies a gap: "the concept-based interpretability community has largely focused on concept-closure invariance, but has paid comparatively little attention to structural invariance. As a result, concept-based models often exhibit arbitrary behaviour (e.g., when using DNNs as task predictors) or are weak classifiers (e.g., when using linear models as task predictors)."

PhyArch + CBM-CTPC is *gap-filling for that exact deficit*: combining CBM's concept-closure + information invariance with PhyArch's structural commitment (manipulator-equation skeleton matching the orbital-mechanics-trained user). This framing is stronger than the original "first practical instantiation" claim because it's framework-internal — the contribution is bringing structural invariance to concept-based methods in a specific operational domain, addressing a specific deficit identified by the framework's own authors.

### Summary of corrections impact

| Where | Pre-2026-05-10 framing | Corrected framing |
|---|---|---|
| Information invariance | Closed by Year 2 (analytic Σ propagation) | Closed by Year 1 (CBM bottleneck = compressed Z) |
| Concept-closure invariance | About intervention compositionality | About sound translation between concept vocabularies |
| Structural invariance | About architecture mirroring physical equations | About hypothesis class matching user's mental model (user-relative) |
| Publishable claim | "First practical instantiation" | "Candidate practical instantiation, contingent on physics-trained user" |
| Year 1 milestone (CBM-CTPC) | Closes 1 symmetry (concept-closure) | Closes 2 symmetries (concept-closure + information invariance) |
| Year 2 milestone (Analytic-Σ-CTPC) | Closes information invariance for full distribution | Refines inference equivariance from mean to full distribution |
| Open design decisions | (none flagged at this level) | Prediction head structural commitment for CBM-CTPC (linear / monotonic / manipulator-skeleton) |

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
- [[Hamiltonian-vs-Lagrangian-Duality]] — sister synthesis page on the Hamiltonian↔Lagrangian axis (now also covers the Helmholtz-vs-J-R dissipation-route axis)
- [[PeRCNN]] — sibling Predictor-Corrector decomposition for spatial PDEs
- [[Pi-Block-Polynomial-Approximator]] — alternative inductive-bias architecture (polynomial form vs. PhyArch's symmetry hardwiring)
- [[D-HNN]] — Sosanya & Greydanus 2022; closes Q8a (Helmholtz route) and supplies the architectural template for dissipation-side concept-closure compliance. Ingested 2026-05-10.
- [[Dissipative-Hamiltonian-Neural-Network]] — D-HNN architecture pattern; § "Why D-HNN is not enough for SiS" is the precise architectural analysis behind the Q8a/Q8b split.
- [[Dissipative-SymODEN]] — Zhong, Dey, Chakraborty 2020; closes Q8b-vector-field via Cholesky-PSD parameterization of the dissipation matrix. Ingested 2026-05-10.
- [[Port-Hamiltonian-Neural-Network]] — the SiS-canonical dissipative block (architecture pattern from Dissipative SymODEN, separated from the paper's experiments); contains the Integrator Caveat naming Q8b-discrete-time candidate integrators.
- [[Port-Hamiltonian-Systems]] — math/physics foundation for the J-R route.
- [[Helmholtz-Decomposition]] — math foundation of the Q8a route.
- [[Rayleigh-Dissipation-Function]] — classical-mechanics primitive that both routes generalize.
- [[Concept-Bottleneck-Models]] — Koh et al. ICML 2020; mechanism for verifying concept-closure invariance (Part III). Ingested 2026-05-09.
- [[CBM-CTPC-Composition]] — concrete Year 1 milestone design applying CBM to the Latent NCDE Corrector with orbital-physics concepts. Created 2026-05-09.
- [[Analytic-Covariance-Propagation]] — Wright et al. AISTATS 2024; technical machinery for full-distribution information invariance. Ingested 2026-05-09.
- [[Analytic-Sigma-CTPC-Composition]] — concrete Year 2 milestone design replacing K-sample MC with analytic propagation, enabling TLE uncertainty and frame-equivariance verification. Created 2026-05-09.

## Sources

- `raw/papers/my-papers/KDD_submission.pdf` — Shahid, Jiang, Sarkar, Fleming 2026 (KDD '26 submission). The deployed CTPC framework. Used in Parts I–II.
- `raw/notes/PhyArch_DoublePendulum.pdf` — Bilal's deterministic benchmark notes. Validates symmetry-hardwiring on a controlled toy. Used in Parts I–III.
- `raw/papers/interpretability/Interpretability&Symmetry.pdf` — Barbiero et al. 2026, the four-symmetry framework for actionable interpretability. **Not yet ingested as a separate wiki page**; framework summarized in Part III from Bilal's reading. Promote to canonical source treatment on next ingest.
- `raw/papers/interpretability/ConceptBottleneckModels.pdf` — Koh et al. ICML 2020. The mechanism for empirically verifying concept-closure invariance. Ingested 2026-05-09; see [[Concept-Bottleneck-Models]] and [[CBM-CTPC-Composition]] for the full treatment.
- `raw/papers/uncertainty-propagation/AnalyticUncertaintyProp.pdf` — Wright et al. AISTATS 2024. The technical machinery for analytic moment propagation through neural networks; underpins the Year 2 milestone for verifying information invariance on the full predictive distribution. Ingested 2026-05-09; see [[Analytic-Covariance-Propagation]] and [[Analytic-Sigma-CTPC-Composition]] for the full treatment.
- `raw/papers/port-hamiltonian/DissipativeHNN.pdf` — Sosanya & Greydanus 2022. Closes Q8a (Helmholtz-decomposition route to dissipative HNN); supplies the `α · D_θ` counterfactual-intervention template elevated in Part III as "Architectural Hook for Dissipation-Side Concept-Closure". Does *not* close Q8b (the J-R-structure route required by SiS `Ḣ ≤ 0`). Ingested 2026-05-10; see [[D-HNN]] and [[Dissipative-Hamiltonian-Neural-Network]] for the full treatment.
- `raw/papers/port-hamiltonian/DissipativeSymODEN-Zhong2020.pdf` — Zhong, Dey, Chakraborty 2020 (ICLR Workshop). Closes Q8b-vector-field via Cholesky-PSD parameterization of the dissipation matrix (`D = LLᵀ`); restricted Hamiltonian `½ pᵀ M⁻¹(q) p + V(q)` is a structural match for orbital, not a compromise; native control-input channel `g(q)u` as a forward-looking asset for future maneuver-planning extensions. Q8b-discrete-time still open (paper uses RK4, not a structure-preserving integrator). Ingested 2026-05-10; see [[Dissipative-SymODEN]] and [[Port-Hamiltonian-Neural-Network]] for the full treatment, including the Integrator Caveat with three candidate integrators (discrete gradient method, AVF, Gauss-collocation).
- `CLAUDE.md` § Core Philosophy — the SiS design hierarchy that this page operationalizes.
