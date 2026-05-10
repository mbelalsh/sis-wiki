---
title: Analytic-Σ-CTPC Composition (Year 2 Design)
tags: [analytic-covariance, ctpc, phyarch, sis-design, year-2-milestone, information-invariance, tle-uncertainty, intervention]
sources: [raw/papers/uncertainty-propagation/AnalyticUncertaintyProp.pdf, raw/papers/my-papers/KDD_submission.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: critical
hard_constraint_possible: yes
---

# Analytic-Σ-CTPC Composition (Year 2 Design)

> **Forward-looking design document.** This page lays out the architecture, training/inference protocol, verification protocol, and open research questions for replacing the Latent NCDE Corrector's K-sample Monte Carlo aggregation with [[Analytic-Covariance-Propagation|Wright's analytic moment propagation]], plus enabling propagation of TLE input uncertainty through the full pipeline. **Year 2 milestone of the two-year PhyArch-CTPC research arc**, sequentially after [[CBM-CTPC-Composition]] (Year 1).
>
> **Important reframing (2026-05-10):** this design was originally framed as closing the *information invariance* gap from [[CTPC-Design-Rationale]] Part III. After the canonical [[Actionable-Interpretability-Symmetries|Barbiero]] ingest, that framing was identified as incorrect: information invariance per Barbiero is about *compressed sufficient statistics* (closed by the CBM bottleneck in Year 1), not about full-distribution invariance under input transformations. This Year 2 design instead **refines inference equivariance from "mean exact" to "full distribution exact"** under input frame transformations. The architectural and verification work below is unchanged; only the symmetry the work closes is reframed. See [[CTPC-Design-Rationale]] § Corrections from Barbiero Ingest for the audit trail.

## One-Line Intuition

Replace [[CTPC-KDD-Submission]]'s K-sample Monte Carlo over latent samples with **layer-by-layer analytic propagation of full covariance matrices** through the Latent NCDE Corrector. This produces an *exact* predictive covariance — no MC sampling noise — and enables propagation of **TLE input uncertainty** all the way to the output uncertainty ellipsoid. The empirical test: predictive covariance must transform exactly correctly under input frame transformations (ECI ↔ RTN), to machine precision. Currently false (because MC sampling noise breaks frame equivariance); becomes structurally enforced under analytic propagation. **What this verifies in the Barbiero framework:** *inference equivariance* (Symmetry 1) extended from the mean to the full predictive distribution — not information invariance.

## Why This Was Done

[[CTPC-Design-Rationale]] Part III identifies four [[Actionable-Interpretability-Symmetries|Barbiero]] symmetries. After the Year 1 [[CBM-CTPC-Composition]], the CBM bottleneck closes both concept-closure invariance AND information invariance (the bottleneck IS the compressed sufficient statistic Z). Structural invariance holds for orbital-mechanics-trained users (PhyArch's manipulator-skeleton + structurally-committed head). **Inference equivariance is partially closed** — the *mean* prediction is exactly equivariant under input frame transformations (geometric features handle this), but the *covariance* has K-sample MC sampling noise that breaks exact equivariance.

The deficit has two sources:

1. **Monte Carlo sampling noise.** The K-sample latent aggregation (CTPC paper Algorithm 1, lines 8–15) introduces Monte Carlo error that breaks exact frame equivariance — `Σ_t^ECI(t) ≠ R(t)^T · Σ_t^RTN(t) · R(t)` to machine precision because the K samples are different draws. This means inference equivariance for the *full predictive distribution* is approximate, not exact.
2. **TLE input uncertainty isn't propagated at all.** The encoder receives `e_ECI(t_0:T')` as deterministic past errors. The TLE-derived true state has its own measurement uncertainty `Σ_TLE` that should flow through the entire pipeline but currently doesn't.

**Wright et al. AISTATS 2024 ([[Analytic-Covariance-Propagation]]) provides the technical machinery to fix both.** Theorem 1 gives closed-form analytic covariance through nonlinear activations of Gaussian inputs; Algorithm 1 gives the layer-by-layer propagation procedure for full networks. The application to the Latent NCDE Corrector — refining inference equivariance from mean-only to full-distribution — is the Year 2 milestone of the PhyArch-CTPC roadmap.

## The Architecture

Current [[Latent-NCDE-Corrector]] (Bilal's [[CTPC-KDD-Submission]]) at inference:

```
TLE state (r, v) [deterministic]
  ↓
e_ECI(t_0:T') [deterministic past errors]
  ↓
encoder NCDE → (μ_L, Σ_L) [latent posterior]
  ↓
sample s_L^(k) ~ N(μ_L, Σ_L) for k = 1..K  [Monte Carlo]
  ↓
decoder NCDE → z_d^(k)(t)  [K trajectories]
  ↓
prediction head → (μ_t^(k), L_t^(k)) for each k
  ↓
moment aggregation (law of total variance over K samples) → (μ̄_t, Σ̄_t)
```

Analytic-Σ-CTPC at inference:

```
TLE state (r, v) with covariance Σ_TLE  [PROBABILISTIC INPUT — new]
  ↓
e_ECI(t_0:T') with propagated Σ_e(t)  [propagate Σ_TLE through observation history]
  ↓
encoder NCDE: analytic propagation → (μ_L, Σ_L)  [exact, accounts for Σ_TLE]
  ↓
[no sampling — use full distribution directly]
z_d_init: μ = W_{L→h_d} · μ_L,  Σ = W_{L→h_d} · Σ_L · W_{L→h_d}^T  [exact affine]
  ↓
decoder NCDE: analytic propagation → (μ_z(t), Σ_z(t))  [Algorithm 1]
  ↓
prediction head: analytic propagation → (μ_t, Σ_t_aleatoric)
  ↓
Σ_t_total = Σ_t_aleatoric + Σ_t_propagated  [single total covariance, no MC]
```

**Critical change**: K-sample loop → analytic propagation. Each K-sample step replaced by an analytic moment-propagation step.

## The Three Variance Sources

| Source | CTPC currently | Analytic-Σ-CTPC |
|---|---|---|
| **Aleatoric** (within-sample) | Student-t Cholesky `L_t L_t^T` from prediction head | **Same** — already analytic |
| **Latent-induced** (between-sample) | K-sample Monte Carlo via law of total variance (CTPC App. A) | **Analytic via `Σ_L` propagated through decoder** — exact, no K-sample noise |
| **Input (TLE) uncertainty** | **Not propagated at all** | **Newly enabled** — `Σ_TLE` propagated through encoder + decoder analytically |

**Decomposition under analytic propagation:**
```
Σ_t_total(t) = Σ_t_aleatoric(t)            [from prediction head Cholesky]
             + Σ_t_latent_induced(t)        [from Σ_L propagated through decoder]
             + Σ_t_TLE_propagated(t)        [from Σ_TLE propagated through encoder + decoder]
```

All three terms are exact under analytic propagation. The K-sample MC currently estimates the first two terms with sampling noise; the third term is currently missing.

## The Composition Sketch

### Step 1 — Replace Encoder MC with Analytic Propagation

The encoder NCDE solves `z_e(t) = z_e(t_0) + ∫ f_θ_e(z_e(s)) dX_e(s)` with control path `X_e` from past errors. With RK4 discretization, each integration step is a fixed sequence of MLP evaluations + linear combinations:

```
RK4 step:
  k1 = f_θ_e(z_e(t)) dX_e(t)               [MLP evaluation]
  k2 = f_θ_e(z_e(t) + k1/2) dX_e(t+δ/2)
  k3 = f_θ_e(z_e(t) + k2/2) dX_e(t+δ/2)
  k4 = f_θ_e(z_e(t) + k3) dX_e(t+δ)
  z_e(t+δ) = z_e(t) + (k1 + 2k2 + 2k3 + k4) / 6     [linear combination]
```

Each `f_θ_e` MLP evaluation gets analytic moment propagation per [[Analytic-Covariance-Propagation]] Algorithm 1. Each linear combination is an exact affine operation on `(μ, Σ)`. Compose throughout the integration to get analytic `(μ_z_e(t), Σ_z_e(t))` over `t ∈ (t_0, t_T']`.

### Step 2 — Latent Posterior Stays Analytic

The variational posterior `(μ_L, Σ_L) = W_{h_e → L} · z_f` is already an analytic affine transformation. With `(μ_z_e(t), Σ_z_e(t))` from step 1, the weighted aggregation `z_f = ∫ w(t) ⊙ z_e(t) dt / ∫ w(t) dt` is a linear (affine in the mean, quadratic in covariance) operation that propagates analytically.

### Step 3 — Replace Decoder MC with Analytic Propagation

Same as step 1, applied to the decoder NCDE driven by GMAT forecasts as control path. Initial condition: `(μ_z_d(t_T'+1), Σ_z_d(t_T'+1)) = (W_{L→h_d} μ_L, W_{L→h_d} Σ_L W_{L→h_d}^T)`. Propagate through RK4 + MLP `f_θ_d` evaluations.

### Step 4 — Prediction Head Analytic

The prediction head outputs `(μ_t, L_t, ν)` from `z_d(t)`. Currently `L_t` is a Cholesky factor giving `Σ_t_aleatoric = L_t L_t^T`. Under analytic propagation, the input to the head is itself a Gaussian distribution `N(μ_z_d(t), Σ_z_d(t))`. Two layers of moment propagation:

- Propagate `(μ_z_d(t), Σ_z_d(t))` through the head's MLP to get `(μ_pred(t), Σ_pred_propagated(t))` — the variance from input uncertainty
- Add the head's intrinsic aleatoric `Σ_aleatoric = L_t L_t^T` (currently a function of `μ_z_d(t)` only; under analytic propagation, it becomes a function of `(μ_z_d(t), Σ_z_d(t))`)

Total: `Σ_t_total(t) = Σ_pred_propagated(t) + Σ_aleatoric(t)`.

### Step 5 — Add TLE Uncertainty Source

The encoder control path `X_e` is a spline interpolant of past errors `e_ECI(t_0:T')`. Currently treated as deterministic. With TLE uncertainty:

```
e_ECI(t_i) = x_ECI_TRUE(t_i) − x_ECI_GMAT(t_i)
           = (x_TLE(t_i) + ε_TLE(t_i)) − x_ECI_GMAT(t_i)         where ε_TLE ~ N(0, Σ_TLE)
```

So `e_ECI(t_i)` itself is Gaussian with mean `x_TLE(t_i) − x_ECI_GMAT(t_i)` and covariance `Σ_TLE`. The spline interpolant becomes a *Gaussian process* with mean and covariance propagated from the TLE noise.

The encoder NCDE control path now has a stochastic term `dX_e(t)`. Wright's framework assumes a deterministic control path; extending to stochastic control paths is part of the open NCDE-specific theorem (Open Question 1 below).

A pragmatic first version: treat `Σ_TLE` as additional input variance to the encoder hidden state at the first integration step, propagate through the rest of the NCDE deterministically. Loses some exactness but tractable.

## Verification Protocol — How Information Invariance Becomes Empirically Verified

This is the protocol that promotes information invariance from **partial (mean only)** to **verified for full predictive distribution** in [[CTPC-Design-Rationale]] Part III.

### Test 1: Analytic-vs-MC Convergence

Necessary precondition. Run analytic propagation alongside K-sample MC at inference. As `K → ∞`, MC estimates should converge to analytic estimates. Specifically:
```
||μ̄_t^MC(K) − μ_t^analytic|| → 0
||Σ̄_t^MC(K) − Σ_t^analytic||_F → 0
```
The convergence rate should be `O(1/√K)` for unbiased MC. If not, either MC is biased or analytic has implementation errors.

### Test 2: Activation Covariance Accuracy

Same as [[Analytic-Covariance-Propagation]]'s Section 4.1: numerical-integration ground truth for activation covariance vs. K-truncated Theorem 1. Confirms that the chosen Taylor order K is sufficient for the activation function used in the NCDE.

### Test 3: TLE Uncertainty Propagation Sanity

Synthesize TLE state with known `Σ_TLE`. Propagate through Analytic-Σ-CTPC. Verify that:
- Output uncertainty grows with input uncertainty (monotonicity)
- Output uncertainty decreases with stronger learned latent posterior (the encoder should reduce uncertainty when past errors are informative)
- The total predictive ellipsoid contains the true forecast error with correct frequency (frequentist coverage at α = 0.95 should be ~95%)

### Test 4: Frame Equivariance ⇒ Inference Equivariance for Full Distribution Verified (REFRAMED 2026-05-10)

**This is the empirical test for *inference equivariance* on the full predictive distribution.** Frame equivariance of the predictive covariance under input frame transformations is exactly what inference equivariance demands when the output distribution is parameterized by `(μ, Σ)` rather than the mean alone. If this test passes, **inference equivariance is promoted from "verified for mean only" to "verified for full predictive distribution"** in [[CTPC-Design-Rationale]] Part III.

> **Reframing note (2026-05-10):** Originally this test was framed as the empirical check for *information invariance*. After the canonical [[Actionable-Interpretability-Symmetries|Barbiero]] ingest (2026-05-10), that framing was identified as incorrect: information invariance per Barbiero is about *compressed sufficient statistics* (`H(Z) ≪ H(X)` and `I(Y;X|Z) = 0`), which the CBM bottleneck in [[CBM-CTPC-Composition]] (Year 1) closes. Frame equivariance of `Σ_t` is a property of *inference equivariance* (Barbiero Symmetry 1) extended from the mean to the full predictive distribution, not a property of information invariance. The architectural and verification work below is unchanged; only the symmetry the work closes is reframed. See [[CTPC-Design-Rationale]] § Corrections from Barbiero Ingest for the full audit trail.

**Protocol:**
1. Sample TLE state `(r, v) ∈ ℝ⁶` with covariance `Σ_TLE^ECI` in ECI frame
2. Propagate analytically through Analytic-Σ-CTPC → `(μ_t^ECI, Σ_t^ECI(t))` over forecast horizon
3. Convert: `Σ_TLE^RTN = R(t_0)^T · Σ_TLE^ECI · R(t_0)`, propagate analytically → `(μ_t^RTN, Σ_t^RTN(t))`
4. **Verify**: `Σ_t^ECI(t) = R(t)^T · Σ_t^RTN(t) · R(t)` to machine precision for all `t` in the forecast horizon

**Pass criterion:** Frobenius norm of the difference is below `1e-10` (machine precision for double-precision arithmetic). Currently the K-sample MC introduces `O(1/√K)` Monte Carlo noise that breaks this exact equivariance.

**Why this is the test for inference equivariance on the full distribution, not a sanity check.** Barbiero's inference equivariance (Symmetry 1) is defined as commutativity of the diagram `P_{Y|X} → translate-then-predict = predict-then-translate`. For a model with parametric output distribution `(μ, Σ)`, "same prediction" under translation requires both `μ` and `Σ` to transform consistently. The mean is currently exactly equivariant via PhyArch's geometric features; the covariance is approximate due to MC noise. Test 4 verifies that under analytic propagation, the covariance also transforms exactly correctly — completing inference equivariance for the full distribution. **There is no separate Barbiero check for the covariance beyond this — frame equivariance under transformation IS what inference equivariance means for the second moment.**

If Test 4 passes, the "mean only" qualifier on inference equivariance in [[CTPC-Design-Rationale]] Part III lifts entirely. The publishable claim of "all four Barbiero symmetries verified for orbital-mechanics-trained users" becomes defensible without the inference-equivariance caveat (assuming Year 1 has also been verified).

### Test 5: Composition with CBM Bottleneck (Year 1 + Year 2)

Once both CBM-CTPC and Analytic-Σ-CTPC are implemented, run combined verification:
- Concept-closure invariance verified per [[CBM-CTPC-Composition]] § verification protocol
- Information invariance verified per Test 4 above
- Joint test: intervene on a concept (`ĉ_drag(t) ← 1.3 · ĉ_drag(t)`), propagate analytically; verify that the resulting modified `Σ_t` is consistent across frames (intervention + frame-change should commute up to machine precision)

This closes the four-symmetry verification: structural, inference equivariance, concept closure (Year 1), information invariance (Year 2).

## TLE Uncertainty Sources

**Operationally, where does `Σ_TLE` come from?** Several options, ordered by typical fidelity:

| Source | Description | Typical accuracy |
|---|---|---|
| **From TLE format directly** | TLEs include `bstar` (drag scaling) and ephemeris-uncertainty fields | Coarse; ~km-scale at LEO, growing with time since epoch |
| **Successive-TLE differences** | Take consecutive TLE snapshots of the same satellite, compute state differences, fit a covariance | Empirical; captures unmodeled errors but can over/underestimate |
| **Physics-based OD tools (CARA-style)** | Apply formal orbit-determination machinery (Kalman filter, batch least squares) with full force model | Best when available; expensive |
| **Conservative bound from cataloging operations** | Operationally-derived bounds from the catalog provider | Pessimistic; useful for safety margins |
| **High-fidelity reference (precise OD products)** | International GNSS Service ephemeris, JPL ephemeris derivatives | Gold standard but rare for arbitrary spacecraft |

**Initial implementation recommendation:** Start with successive-TLE differences. It's the cheapest, doesn't require external infrastructure, and gives a realistic-if-imperfect `Σ_TLE` for benchmarking. As the framework matures, swap in CARA-derived `Σ_TLE` for high-stakes spacecraft.

## Connection to SiS / CTPC

Analytic-Σ-CTPC closes the **Year 2 milestone** of the two-year research arc in [[CTPC-Design-Rationale]] Part III. **Reframed 2026-05-10** after the canonical [[Actionable-Interpretability-Symmetries|Barbiero]] ingest:

- **Year 1 (CBM-CTPC):** closes concept-closure invariance AND information invariance simultaneously (the bottleneck IS the compressed sufficient statistic Z). One architectural mechanism, two Barbiero symmetries.
- **Year 2 (Analytic-Σ-CTPC, this design):** refines inference equivariance from "mean exact" to "full distribution exact" under input frame transformations. Closes the inference-equivariance-for-covariance gap, NOT information invariance per Barbiero (which Year 1 already closed).
- **End-state:** all four Barbiero symmetries empirically verified for orbital-mechanics-trained users + formal verification (links to Q5 of Part II)

The composition with [[CBM-CTPC-Composition]] is *additive*, not orthogonal:
- CBM bottleneck closes concept-closure + information invariance with one mechanism
- Analytic propagation operates on the variance through that bottleneck — variance from CBM bottleneck flows analytically through prediction head; intervention experiments (Year 1) and frame-equivariance tests (Year 2) compose
- Test 5 above is the joint-verification step that ensures both pass simultaneously

The composition with [[PhyArch]] is also additive:
- PhyArch hardwires inference equivariance for the mean + structural invariance (for orbital-mechanics-trained user) via parity-split assembly + manipulator-skeleton
- Analytic propagation refines inference equivariance to also hold for the covariance, exactly equivariant under frame transformations
- The PhyArch-style coefficient networks `aᵢⱼ(z_even)` are MLPs to which Wright's machinery applies directly

## Forward Reference: Formal Verification Stub

> *Stub for Year 3+ work — not developed here.*

After empirical verification of information invariance via Test 4 above, the next progression is *formally* verifying frame equivariance of analytic propagation in a system like Lean, Coq, or dReal.

Concrete verification targets:

- **Algebraic identity:** prove that `R^T · NCDE_propagation(Σ, X) · R = NCDE_propagation(R^T Σ R, R^T X)` for any rotation matrix `R`. This is the formal statement of frame equivariance for analytic propagation.
- **Truncation error bound:** prove that the K-truncated Theorem 1 covariance has error `O(ρ^{K+1})` and is monotonic in K for the variance corollary.
- **Composition with CBM bottleneck:** prove that intervention on a concept and frame transformation commute under the composed CBM-Analytic-Σ-CTPC architecture.

Source materials in `raw/papers/formal-verification/` (already in the wiki, not yet ingested as separate pages) — same set referenced by [[CBM-CTPC-Composition]] § Forward Reference. Particularly relevant for Analytic-Σ:

- `FVerify_NODE.pdf` — closest existing analog (Neural ODE verification), would be the template for NCDE verification
- `17_efficient_robustness_verificat.pdf` — robustness verification for NN, includes covariance propagation lower-bound proofs

## Connections

- [[Analytic-Covariance-Propagation]] — the Wright et al. AISTATS 2024 paper providing the technical machinery
- [[CTPC-KDD-Submission]] — the deployed framework whose K-sample inference algorithm gets replaced
- [[CTPC-Design-Rationale]] — the synthesis page identifying information invariance as the remaining Barbiero gap; this page closes it for the full predictive distribution
- [[Latent-NCDE-Corrector]] — the architectural backbone; analytic propagation replaces the K-sample inference loop
- [[CBM-CTPC-Composition]] — Year 1 sibling milestone (concept-closure verification); Year 1 + Year 2 compose to close all remaining Barbiero gaps
- [[PhyArch]] — coexists; analytic propagation through parity-split assembly is straightforward
- [[Neural-Controlled-Differential-Equation]] — the substrate; the open NCDE-specific theorem (below) is a separate publishable contribution

## Open Questions

### **⭐ Potential contribution: Analytic-Σ-NCDE theorem**

**Wright et al. AISTATS 2024 covers MLPs.** NCDE (Kidger et al. 2020) has a continuous-time integral solved by RK4 (or other integrators). Each RK4 step is a fixed sequence of MLP evaluations + linear combinations — Wright applies to each MLP, then linear-propagates through the RK4 step. **The explicit derivation of "analytic covariance propagation through RK4-integrated NCDEs" is novel and not in the literature.**

This is a publishable methods paper in its own right, *separate from the CTPC application*. Title sketch: *"Analytic Covariance Propagation through Neural Controlled Differential Equations."* Contribution claim: extending [Wright et al. 2024]'s analytic-covariance theorem to continuous-time NCDEs solved by Runge-Kutta integrators, with explicit error bounds for the combined Taylor-truncation + RK4-discretization approximation.

Components needed:

1. **Per-step covariance propagation.** Each RK4 step: 4 MLP evaluations + linear combination. Apply Wright's Theorem 1 to each MLP evaluation; linear-propagate through the combination.
2. **Step-size error analysis.** RK4 has `O(δ^5)` local truncation error for the mean trajectory. The covariance trajectory has different error structure — needs explicit analysis.
3. **Stochastic control paths.** Wright assumes deterministic input distributions. NCDE control paths may themselves be stochastic (TLE uncertainty case). Extending to stochastic control paths involves Itô calculus on top of moment propagation.
4. **Symplectic / variational integrators.** If the NCDE structure is symplectic (e.g., from a Hamiltonian formulation), variational integrators preserve the symplectic 2-form. The covariance-propagation theory should preserve the corresponding structural property. Untested.

**This contribution stands alone** — useful for any NCDE-based UQ application beyond CTPC. Worth scoping as a separate paper that the CTPC-application paper can cite.

### Other open questions

- **Tanh / Softplus covariance derivations.** CTPC paper doesn't specify NCDE activation. If Tanh, need explicit Tanh derivative formulas (likely Hermite-polynomial expressible). If Softplus, the mean is `E[Softplus(y)] = σ φ(μ/σ) + μ Φ(μ/σ)` (similar to ReLU but smoother) — derivable via direct integration.
- **Prediction-head Cholesky parameterization.** Currently `L_t` is a free-form Cholesky. Under analytic propagation, the input to the head is a full Gaussian, not a deterministic vector. The resulting `Σ_t_aleatoric` should be a function of `(μ_z_d(t), Σ_z_d(t))`, not just `μ_z_d(t)`. Architectural detail: how to make the Cholesky head input-covariance-aware.
- **TLE uncertainty estimation.** Wright assumes `Σ_TLE` is given. Operationally getting it (TLE format / successive differences / OD tools) is a separate research thread. Initial implementation: successive-TLE differences. See "TLE Uncertainty Sources" section above.
- **Heavy-tailed input distributions.** Wright assumes Gaussian. CTPC's prediction head output is Student-t (with `ν > 4.5`). Input TLE noise might also be heavy-tailed [Mashiku 2012]. Extending Wright to Student-t inputs is open — possibly via a moment-matched Gaussian approximation, possibly via explicit Student-t propagation.
- **Computational budget at scale.** `O(K · n²)` per activation × ~5760 forecast timesteps × 4-day horizon × n=128 hidden dim ≈ 4 × 10^8 ops per K-th order Taylor term. Tractable on a single GPU. For higher hidden dimensions (e.g., scaling to multi-spacecraft fleets), the `n²` becomes the dominant term.
- **Composition with CBM bottleneck.** Year 1 (CBM) + Year 2 (analytic Σ) should compose: propagate variance through the concept bottleneck, then through the prediction head. The law-of-total-variance decomposition becomes within-concept + between-concept + parameter-uncertainty + propagated-input. Architectural design pending.
- **Variational posterior bottleneck.** Wright's BNN training results (Sec. 4.3, Table 3) show that exact covariance doesn't always help when the factorized-Gaussian variational posterior is the limiting factor. Worth tracking when applying to CTPC: if Test 4 passes but downstream metrics (collision-probability-bound tightness, MSE) don't improve, the bottleneck is elsewhere.
- **Pooling / batch-norm / softmax in hypothetical extensions.** Current Latent NCDE doesn't have these. Future variants might. Refactoring to linear+ReLU [Shriver 2022] is the standard solution.

## Sources

- `raw/papers/uncertainty-propagation/AnalyticUncertaintyProp.pdf` — Wright, Nakahira, Moura 2024 (AISTATS). Theorem 1, Algorithm 1, closed-form derivatives for Heaviside/ReLU/GELU/sigmoid, empirical validation. The technical machinery being applied here.
- `raw/papers/my-papers/KDD_submission.pdf` — Shahid, Jiang, Sarkar, Fleming 2026 (KDD '26). The Latent NCDE Corrector being modified. Algorithm 1 (K-sample inference loop being replaced), Appendix A (moment aggregation formula being superseded by analytic propagation).
- `raw/papers/formal-verification/` (14 papers, *not yet ingested*) — forward reference for the Year 3+ formal-verification phase.
