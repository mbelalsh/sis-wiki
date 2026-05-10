---
title: CBM-CTPC Composition (Year 1 Design)
tags: [cbm, ctpc, phyarch, sis-design, year-1-milestone, concept-closure, intervention, orbital-mechanics]
sources: [raw/papers/interpretability/ConceptBottleneckModels.pdf, raw/papers/my-papers/KDD_submission.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: critical
hard_constraint_possible: yes
---

# CBM-CTPC Composition (Year 1 Design)

> **Forward-looking design document.** This page lays out the architecture, training protocol, verification protocol, and operational counterfactuals for inserting a [[Concept-Bottleneck-Models|Concept Bottleneck Model]] layer into the [[Latent-NCDE-Corrector]] with named orbital-physics concepts. Closes the concept-closure-invariance gap identified in [[CTPC-Design-Rationale]] Part III. **Year 1 milestone of the two-year PhyArch-CTPC research arc.**

## One-Line Intuition

Insert a `k=9` concept bottleneck between the Latent NCDE decoder hidden state and the Student-t prediction head, where each concept corresponds to a named orbital perturbation (J2, drag, SRP, third-body) or a frame-decomposition component (RTN). The bottleneck forces the corrector's residual prediction to flow *through* named physics concepts, making counterfactual queries like "what if drag were zero?" or "what if J2 were 30% larger?" well-defined operations on the model — enabling the AFRL-relevant operational use case that motivates this entire research thread.

## Key Architectural Insight — One Mechanism, Two Barbiero Symmetries

> **CBM-CTPC achieves two of the four [[Actionable-Interpretability-Symmetries|Barbiero]] symmetries with one architectural mechanism: the concept bottleneck.**
>
> The k=9 named-concept bottleneck simultaneously:
>
> 1. **Closes concept-closure invariance** (Barbiero Symmetry 3) — the model uses concepts from the user's vocabulary; sound translations like `drag ↔ atmospheric_dissipation` preserve the underlying physical mechanism. Paper §2.3.1 explicitly cites CBMs as the mechanism.
> 2. **Closes information invariance** (Barbiero Symmetry 2) — the bottleneck IS the compressed sufficient statistic Z required by the symmetry definition, with `H(Z) ≪ H(X)` (k=9 concepts vs. full hidden state) and `I(Y;X|ĉ) ≈ 0` (verified by Test 1 of the verification protocol below).
>
> This is the most efficient point in the architectural design space — one structural commitment, two symmetry guarantees. The original Part III framing of [[CTPC-Design-Rationale]] (pre-2026-05-10) incorrectly attributed information invariance to analytic Σ propagation (Year 2). After the Barbiero canonical ingest, the corrected reading is: **CBM-CTPC closes information invariance in Year 1; analytic Σ propagation refines inference equivariance to the full distribution in Year 2.**

## Why This Was Done

[[CTPC-Design-Rationale]] Part III identifies four [[Actionable-Interpretability-Symmetries|Barbiero]] symmetries for actionable interpretability. **Reframed 2026-05-10** after canonical Barbiero ingest: PhyArch + CTPC currently satisfies inference equivariance (for the mean) and structural invariance (for orbital-mechanics-trained users) — but lacks both **concept-closure invariance** and **information invariance**, because PhyArch alone has no compressed bottleneck with named-concept semantics.

CBM is the architectural mechanism for *making this property verifiable*. The standard CBM result (Koh et al. 2020): inserting a concept bottleneck `f ∘ g` where `f` takes only `ĉ` as input means concept interventions are guaranteed to propagate through `f` to the prediction. The composition with CTPC inherits this guarantee for orbital residual modeling.

The ultimate operational stakes: in Space Domain Awareness, an operator needs to ask "*why* did the corrector adjust this trajectory by 200 m radial at hour 36?" and receive an answer in physical concepts ("the drag concept fired strongly because the trajectory passed through high atmospheric density at altitude X") rather than "the latent state had this value at that timestep." CBM-CTPC is the architectural commitment that makes such queries first-class.

## The Architecture

Current [[Latent-NCDE-Corrector]] (Bilal's [[CTPC-KDD-Submission]]):

```
encoder NCDE (past errors)  →  latent (μ_L, Σ_L)  →  K samples
                                                      ↓
                              decoder NCDE (driven by GMAT forecasts)
                                                      ↓
                                                  z_d(t)
                                                      ↓
                                          prediction head → (μ_t, L_t)
```

CBM-CTPC inserts a concept layer between `z_d(t)` and the prediction head:

```
... → z_d(t) → CONCEPT NET ĝ → ĉ(t) ∈ ℝ⁹ → PREDICTION HEAD f → (μ_t, L_t)
```

**Critical architectural commitment:** the prediction head `f` takes *only* `ĉ(t)`, not `z_d(t)` or any other side channel. This is what makes concept-closure invariance hold by construction — the same property that makes [[Concept-Bottleneck-Models]] work on OAI/CUB.

## The Concept Set (`k = 9`)

Six **cause concepts** + three **frame-decomposition concepts**:

| Concept | Type | What it represents | Priority | Ground-truth label source |
|---|---|---|---|---|
| `c_drag(t)` | cause | atmospheric drag acceleration magnitude | **HIGH** — where the real residuals live | NRLMSISE-00 atmosphere model + spacecraft area-to-mass ratio + relative velocity |
| `c_SRP(t)` | cause | solar radiation pressure magnitude | **HIGH** — significant in GEO + cislunar | Sun position + cannonball SRP model + spacecraft surface properties |
| `c_J2(t)` | cause | J2 zonal harmonic perturbation magnitude | **MEDIUM** — GMAT models J2 well already | analytic from `(R_⊕/r)²` and spacecraft state |
| `c_3body_lunar(t)` | cause | lunar third-body perturbation magnitude | MEDIUM | analytic from Moon ephemeris (DE440 or similar) |
| `c_3body_solar(t)` | cause | solar third-body perturbation magnitude | MEDIUM | analytic from Sun ephemeris |
| `c_J3(t)` | cause | J3 zonal harmonic | **LOW** — even smaller than J2 residuals | analytic |
| `c_R(t)` | frame | residual error component along radial direction | HIGH | direct from training data + RTN transform |
| `c_T(t)` | frame | residual error along along-track | HIGH | direct |
| `c_N(t)` | frame | residual error along orbit-normal | HIGH | direct |

### Cause vs Frame-Decomposition — Why Both

These two groups answer different operator questions:

- **Cause concepts** answer: "*Which physical process is driving this residual?*" If `ĉ_drag(t)` fires strongly, the corrector is attributing the residual to under-modeled drag. Operator can validate against the atmosphere model independently.
- **Frame-decomposition concepts** answer: "*In which direction does the residual point?*" Radial errors propagate differently into collision probability than along-track errors do. Decomposing the residual into RTN components is what conjunction-analysis tooling expects.

Both are concept-closure–enforceable. Both are useful. They serve different downstream queries.

### Priority Rationale

- **Drag and SRP first** — these are where the real residuals live. GMAT's atmospheric drag depends on the atmosphere model (NRLMSISE-00 is itself approximate, with errors that scale with solar activity). SRP depends on spacecraft surface properties that are often poorly known.
- **J2 lower priority** — GMAT already models J2 well; the residual contribution from J2 is small relative to drag/SRP. Including J2 as a concept is more about completeness than expected residual magnitude.
- **J3 lowest priority** — even smaller. Initial implementation may omit J3 entirely; add later if ablations show concept-set incompleteness causes the bottleneck to "absorb" J3 into other concepts (a sign of concept-set incompleteness per CBM paper Section 8).
- **Third-body (lunar, solar)** — important for high-altitude / GEO orbits, less so for LEO. Include from the start.

## Training Protocol

### Design Decision: Independent Training Scheme

**Decision.** Use the **independent** training scheme from [[Concept-Bottleneck-Models]] (Sec. 3): train `g` (concept net) on `(z_d(t), c(t))` pairs, then train `f` (prediction head) on `(c_true(t), e_ECI(t))` pairs *using true concept values, not predicted ones*.

**Rationale — the operational use case IS intervention.** The whole point of CBM-CTPC for AFRL deployment is counterfactual reasoning: an operator queries "what if drag were 30% higher?" and the model produces a physically-meaningful adjusted forecast + uncertainty ellipsoid. This means at deployment time, `f` will receive *modified* concept values that may differ substantially from what `g` would have predicted. **Independent training matches this exactly**: `f` is trained on the distribution of *true* concept values, so when the operator substitutes counterfactual concept values close to physically-plausible truth, the model is in-distribution.

The CBM paper validates this empirically (Sec. 6, Figure 4-Left): independent CBM achieves substantially better task accuracy under full-concept intervention than sequential or joint variants, despite (slightly) worse pre-intervention accuracy. **The operational use case dominates the choice.**

This is *not* just a preference. It's a load-bearing design decision: choosing sequential or joint would optimize the wrong metric for the deployment scenario.

### Concept Label Generation

For each training timestep `t`, compute the 9-dim concept ground truth:

- **Cause concepts** — invoke analytic perturbation models on the *true* spacecraft state at time `t`:
  - J2, J3: analytic from spacecraft position relative to Earth
  - Drag: NRLMSISE-00 atmosphere model + drag coefficient (use spacecraft-specific value; default `C_d = 2.2`)
  - SRP: Sun position from JPL ephemeris + cannonball model
  - Third-body: lunar/solar position from JPL ephemeris
- **Frame concepts** — direct: rotate the *true* error vector `e_ECI(t)` into the RTN frame using the GMAT-forecast position+velocity for that timestep.

Critical: use the *true* state (from observations), not GMAT's predicted state, when computing cause-concept labels. Otherwise the concept labels are biased toward GMAT's model and learned concepts won't generalize.

### Loss Function

For independent training, two separate optimization problems:

```
g* = argmin_g  Σ_{i,t}  ||g(z_d(t)) − c_true(t)||²        (concept regression)

f* = argmin_f  Σ_{i,t}  L_NLL(f(c_true(t)), e_ECI(t))
              + L_CRPS(f(c_true(t)), e_ECI(t))
              + L_KL(...)                                  (Student-t head)
```

Both networks separately. At test time: `(μ_t, L_t) = f*(g*(z_d(t)))`.

The encoder NCDE + latent posterior + decoder NCDE training stays as in [[CTPC-KDD-Submission]] — only the bottleneck is added between `z_d(t)` and the prediction head.

## Open Design Decisions

### Prediction Head Structural Commitment

[[Actionable-Interpretability-Symmetries|Barbiero §2.4.1]] explicitly disqualifies CBMs with free-form MLP task predictors: *"concept-based models whose mappings between concepts and tasks (or between concepts themselves) are arbitrary, such as employing DNNs, have a hypothesis space that lacks any well-defined structure and therefore do not satisfy this [structural invariance] symmetry."*

**Implication for CBM-CTPC:** the prediction head `f: ĉ → ŷ` cannot be a free-form MLP if structural invariance is to hold. An explicit structural commitment is required.

**Three candidate forms** (the choice depends on what mental model the AFRL operator user actually reasons in — open question; needs domain-expert input):

| Head form | Description | Pros | Cons |
|---|---|---|---|
| **Linear** | `μ_t = W ĉ(t) + b`, with `Σ_t` head structurally simple (e.g., diagonal Cholesky times scale) | Maximally interpretable; user can read off coefficients | Likely too constrained — orbital-residual dynamics are nonlinear in concept values |
| **Monotonic** | `μ_t = m(ĉ(t))` with `m` monotonic in each concept (e.g., monotonic neural network, isotonic regression head) | Captures nonlinearities; intervention semantics remain meaningful (increasing drag should increase residual magnitude predictably) | Off-the-shelf monotonic networks often have lower expressiveness; might trade accuracy |
| **Manipulator-skeleton** | `μ_t = Σⱼ aⱼ(ĉ_invariant) × ĉ_basis_j` where `ĉ_invariant` are scalar-invariant concepts and `ĉ_basis` are RTN-typed concepts | Structurally aligned with PhyArch's manipulator-equation form; matches orbital-mechanics-trained user's mental model | Requires concept set partitioned into invariant + basis; tighter coupling to PhyArch |

**Decision deferred to Year 1 implementation.** The right choice depends on:
- The empirical performance trade-off (linear may be too constrained; monotonic and manipulator-skeleton may be expressive enough)
- The target user's actual mental model (what does an AFRL operator reason in — linear coefficients, monotonic effects, or manipulator-form coupling?)
- The verification protocol's pass/fail thresholds when each head is tried

**Flag for Year 1 work:** include all three head forms as ablation conditions in the verification protocol. The structurally-committed head choice is a load-bearing contribution to satisfying Barbiero's Symmetry 4 — not a free hyperparameter.

## Verification Protocol — How Concept Closure Becomes Empirically Verified

This is the protocol that promotes concept-closure invariance from **asserted** to **verified** in [[CTPC-Design-Rationale]] Part III.

### Test 1: Concept Accuracy

Necessary precondition. On a held-out test trajectory:

```
RMSE(ĉⱼ, c_true_j)  for each concept j ∈ {1, ..., 9}
Pearson(ĉⱼ, c_true_j)  for each j
```

If concept accuracy is low, the model's `ĉ` doesn't align with named physics — interventions on `ĉ` won't have meaningful physical interpretation. CBM paper threshold: Pearson ≥ 0.87 was achieved on OAI; analogous target for CTPC.

### Test 2: Concept Intervention Accuracy

The core test for concept closure. Pick a held-out trajectory with a known dominant perturbation (e.g., low-altitude orbit dominated by drag):

1. Run CBM-CTPC normally → corrected forecast `μ_t` and ellipsoid `Σ_t` over the forecast horizon
2. Counterfactually set `ĉ_drag(t) ← 0` for all `t` in the forecast horizon. Propagate through `f`. → `μ_t', Σ_t'`
3. Independently: re-run GMAT *without drag* (or with a 0-drag-coefficient setting). Subtract from the true trajectory to get the analytic "no-drag residual." This is the ground-truth counterfactual.
4. Compare: `μ_t' − μ_t` (model's response to drag intervention) vs. analytic counterfactual residual difference.

**Pass criterion:** correlation between the model's intervention response and the analytic counterfactual exceeds a threshold (e.g., ≥ 0.85 across multiple test trajectories). High correlation means concept-closure invariance is empirically *verified* — when the operator intervenes on `ĉ_drag`, the model produces a physically-meaningful counterfactual.

### Test 3: Composability of Interventions

Test that concept interventions compose:

- Intervene on `ĉ_drag` alone → response_drag
- Intervene on `ĉ_SRP` alone → response_SRP
- Intervene on both jointly → response_both
- Verify: response_both ≈ response_drag + response_SRP (to leading order, modulo coupling)

If interventions don't compose, the bottleneck has hidden coupling that breaks the concept-closure interpretation.

### Test 4: Intervention with Wrong Values

Sanity check. Substitute physically-implausible concept values (e.g., `ĉ_drag ← −5σ`, drag-magnitude can't be negative). The model should produce a degraded forecast, but the *direction* of degradation should still match physics intuition. This tests whether the model truly learned the concept semantics or just the joint distribution.

## Operational Counterfactuals — What AFRL Operators Can Ask

Once concept-closure invariance is verified, the model supports first-class counterfactual queries. Examples:

| Operator query | Mechanism | Operational use |
|---|---|---|
| "What if the atmosphere model is wrong by 30%?" | `ĉ_drag(t) ← 1.3 · ĉ_drag(t)` ∀t | Sensitivity to atmosphere-model uncertainty during high solar activity |
| "What if J2 were perfectly modeled?" | `ĉ_J2(t) ← 0` ∀t | Estimate the unmodeled-J2 contribution to total error |
| "What if there's no SRP for the next 6 hours?" (eclipse window) | `ĉ_SRP(t) ← 0` for `t ∈ eclipse` | Pre-compute uncertainty bounds over eclipse passage |
| "What's the ellipsoid attributable to the radial component alone?" | `ĉ_T ← 0`, `ĉ_N ← 0`, propagate | Decompose collision-probability contributors |
| "Lunar perturbation 10% larger than ephemeris?" | `ĉ_3body_lunar ← 1.1 · ĉ_3body_lunar` | Robustness to ephemeris uncertainty for cislunar |

These are not post-hoc explanations. They are *operations on the model* — the answers come from the model itself, not from a separate explanation tool. The intervention's response is computed by the same `f` that produces the nominal prediction.

This is the difference between "*the model thinks drag is large here*" (post-hoc, fragile) and "*if drag were zero according to the model, here is the corrected forecast and uncertainty ellipsoid*" (CBM, well-defined).

## Connection to SiS / CTPC

CBM-CTPC closes the **Year 1 milestone** of the two-year research arc in [[CTPC-Design-Rationale]] Part III. As elevated in the *Key Architectural Insight* section above, **the bottleneck closes TWO Barbiero symmetries simultaneously** (concept-closure + information invariance), making Year 1 disproportionately high-leverage.

- **Year 1 (this design):** CBM corrector layer — closes concept-closure AND information invariance in one mechanism. **Plus:** decide structural commitment of prediction head (see Open Design Decisions above) — required for structural invariance per [[Actionable-Interpretability-Symmetries|Barbiero §2.4.1]] disqualification of free-form MLP heads.
- **Year 2:** [[Analytic-Sigma-CTPC-Composition]] — replaces K-sample MC with analytic moment propagation; **refines inference equivariance from "mean exact" to "full distribution exact" under input transformations**. (Originally framed as closing information invariance per Barbiero — corrected 2026-05-10; analytic Σ propagation does NOT close information invariance, which is Year 1's job. See [[CTPC-Design-Rationale]] § Corrections from Barbiero Ingest for the audit trail.)
- **End-state:** All four Barbiero symmetries empirically verified for orbital-mechanics-trained user + formal verification (links to Q5 of Part II)

**Year 1 → Year 2 dependency.** Year 2 analytic propagation extends Year 1 CBM with full-distribution inference equivariance: variance from the concept bottleneck propagates through the prediction head analytically; intervention experiments (Year 1) and frame-equivariance tests (Year 2) compose. The combined verification — intervening on `ĉ_drag` AND changing reference frame should commute up to machine precision — is Test 5 in [[Analytic-Sigma-CTPC-Composition]] § verification protocol.

The composition is *additive* with respect to existing [[PhyArch]] hardwiring:
- PhyArch hardwires inference equivariance + structural invariance (parity-split coefficient assembly)
- CBM bottleneck adds concept-closure verifiability to the residual modeling
- Both can coexist: PhyArch's parity-split structure can be used *inside* `g(z_d(t))` if the concept extraction itself benefits from symmetry hardwiring

## Forward Reference: Formal Verification Stub

> *Stub for Year 3+ work — not developed here.*

Once concept-closure invariance is empirically verified via the protocol above, the next progression is *formally* verifying CBM-CTPC's safety properties. The composition of (a) PhyArch's algebraic equivariance guarantees, (b) CBM's bottleneck propagation, (c) CTPC's strict-properness theorems should be amenable to formal verification in a system like Lean, Coq, or dReal.

Concrete verification targets:

- **Bottleneck invariant:** prove that the CBM-CTPC architecture has no `z_d → (μ_t, L_t)` path that bypasses `ĉ(t)` (no side channel)
- **Intervention soundness:** prove that for any concept perturbation `Δĉ`, the prediction response satisfies operator-specified bounds (Lipschitz, monotonicity, etc.)
- **Composition with strict-properness:** prove that the calibration consistency theorem (Theorem 5.2 of [[CTPC-KDD-Submission]]) carries through the bottleneck

Source materials in `raw/papers/formal-verification/` (already in the wiki, not yet ingested as separate pages): 14 papers covering proof assistants, neural network robustness verification, autoformalization. Particularly relevant:
- `FVerify_NODE.pdf` — formal verification of Neural ODEs (closest existing analog to verifying NCDEs)
- `SafeDNN_NASA.pdf` — NASA's DNN safety verification methodology
- `17_efficient_robustness_verificat.pdf` — robustness verification of NNs

These will be ingested when the formal-verification phase begins. Until then this section is a forward-reference placeholder.

## Connections

- [[Concept-Bottleneck-Models]] — the foundational CBM paper (Koh et al. 2020) being applied here
- [[CTPC-KDD-Submission]] — the deployed framework into which CBM gets composed
- [[CTPC-Design-Rationale]] — the synthesis page identifying concept-closure invariance as the weakest Barbiero symmetry; this page closes the gap
- [[Latent-NCDE-Corrector]] — the architectural backbone; CBM bottleneck inserted between decoder hidden state and prediction head
- [[PhyArch]] — orthogonal hardwiring; can coexist with CBM bottleneck
- [[PhyArch-Double-Pendulum-Benchmark]] — deterministic counterpart; analogous to inserting a CBM into the PaA architecture (but DP doesn't operationally need it since the toy doesn't need counterfactual queries)
- [[Pi-Block-Polynomial-Approximator]] — alternative interpretability mechanism (post-hoc symbolic readout from polynomial-form network); CBM does up-front concept enforcement instead

## Open Questions

- **Time-varying concept layer details.** CBM was designed for static `c` (one image → one concept vector). Orbital concepts vary continuously: `c(t)`. Architecturally simplest: have `g(z_d(t))` operate per-timestep to produce `ĉ(t)`. But: should there be temporal smoothness constraints on `ĉ(t)`? Concepts like `c_drag(t)` should be smooth functions of altitude/density. Adding a smoothness regularizer is open.
- **Probabilistic concepts.** Should each `ĉⱼ(t)` have its own uncertainty distribution? Two paths: (a) deterministic `ĉⱼ` outputs with all uncertainty in `f`; (b) probabilistic `ĉⱼ` outputs feeding a hierarchical-uncertainty `f`. Path (a) is simpler and matches the CBM paper. Path (b) would enable *concept-level* uncertainty queries ("how confident is the model that this is a drag-dominated residual?") at the cost of architectural complexity.
- **Concept ground-truth fidelity.** Training requires `c_true` from analytic perturbation models. NRLMSISE-00 / Brouwer-Lyddane / etc. are themselves approximate. If concept labels are biased, the bottleneck learns biased concepts. Mitigation: ablation against held-out high-fidelity reference trajectories where ground-truth perturbations are computed at higher accuracy (e.g., from precise orbit determination products).
- **Side-channel question for the latent state.** The Latent NCDE encoder output `(μ_L, Σ_L)` derived from past errors is upstream of the bottleneck. Strict CBM would say this is fine — the bottleneck only needs to gate `f`'s input. But the latent state could in principle carry information beyond what gets summarized in `ĉ(t)`. Empirical check: train two variants, one with the bottleneck strictly between `z_d(t)` and `f`, one with a "context vector" side channel from the latent posterior into `f`. Compare intervention performance.
- **Concept whitening alternative.** Chen, Bei, Rudin 2020 (cited by CBM paper) propose decorrelating concept axes via a whitening transform instead of a hard bottleneck. Different trade-off: less aggressive constraint on the architecture, may preserve more task-relevant information at the cost of cleaner intervention semantics. Worth ablating.
- **Information loss at `k=9`.** A 9-dim bottleneck may lose nuance that the unconstrained Latent NCDE captures. If pre-intervention task accuracy degrades unacceptably, options include: (a) increase concept set (e.g., add atmospheric density at altitude as a separate concept); (b) use concept whitening; (c) add a small unconstrained "auxiliary" channel and verify intervention-soundness empirically.
- **Joint training with PhyArch's symmetry hardwiring.** If the underlying decoder uses PhyArch-style parity-split assembly (the next-generation architecture proposed in [[CTPC-Design-Rationale]] Part II Q1), the CBM concept extraction `g` should also respect the symmetry. Concrete: `g` should be parity-typed so that `ĉⱼ` transforms correctly under symmetry actions on the input. Untested.

## Sources

- `raw/papers/interpretability/ConceptBottleneckModels.pdf` — Koh, Nguyen, Tang, Mussmann, Pierson, Kim, Liang 2020 (ICML). The architectural pattern + training schemes + intervention mechanism + theoretical bound being applied here.
- `raw/papers/my-papers/KDD_submission.pdf` — Shahid, Jiang, Sarkar, Fleming 2026 (KDD '26). The Latent NCDE Corrector being modified.
- `raw/papers/formal-verification/` (14 papers, *not yet ingested*) — forward reference for the formal-verification phase that follows empirical verification.
