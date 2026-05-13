---
title: SafeDNN — Understanding and Verifying Neural Networks (NASA Ames project)
tags: [formal-verification, property-inference, explainability, nasa, safety-critical, navigational]
sources:
  - raw/papers/formal-verification/SafeDNN_NASA.pdf   # Pasareanu (NASA Ames / KBR / CMU), project overview slide deck
created: 2026-05-12
updated: 2026-05-12
sis_relevance: high
hard_constraint_possible: yes
---

# SafeDNN (NASA Ames Project — Pasareanu et al.)

## One-Line Intuition

NASA Ames' multi-year research program on making deep neural networks safe, robust, and
interpretable enough for aerospace deployment — Property Inference extracts likely
specifications from trained networks, formal verifiers (Reluplex, DeepPoly) prove them,
and proof-decomposition via "layer patterns" scales verification to networks too large
for monolithic checking.

## Why This Was Invented

By 2018 the NN-verification literature (Reluplex, AI², Crown, DeepPoly) could verify
*small* networks against *human-written* specs. Two practical problems remained for
aerospace deployment:

1. **Specification gap.** Humans rarely have crisp specs for "safe perception" or "safe
   collision-avoidance." Where do the formal properties come from?
2. **Scale.** Monolithic verification timeouts on networks beyond a few thousand neurons.

The SafeDNN project's responses, accumulated across ~10 papers 2018–2021:

- **Property Inference** — *infer* likely properties from the trained network itself
  (Gopinath et al. ASE'19).
- **Programmatic Explainability** — generate *semantic* rules over SCENIC scenarios (Kim
  et al. CVPR'20).
- **Proof Decomposition via Layer Patterns** — split `A → B` into `A → σ` ∧ `σ → B`
  through an intermediate-layer spec σ; verify each leg separately. Significant speedup;
  recovers proofs that monolithic verification times out on.

NASA driver: **ACAS-Xu** (Airborne Collision Avoidance System X-unmanned) — an NN
collision-avoidance controller for unmanned aircraft. The canonical aerospace
NN-verification benchmark.

## The Three Pieces I'll Use From This Deck

### Property Inference for DNNs (Gopinath et al. ASE'19)

**Idea.** Infer "likely" properties of the form `Pre ⟹ Post` from a trained DNN. A
property is a constraint on **ReLU on/off activation patterns**. Two flavors:

- **Layer properties**: `Pre` = conjunction of on/off constraints on an intermediate
  layer's neurons; `Post` = output property. Built via decision-tree learning over
  observed activations. *"Semantic features the network has learned."*
- **Input properties**: `Pre` = constraints over input-space neurons; `Post` = output
  label. Built via concolic execution and iterative relaxation. *"Convex regions of
  consistent labeling."*

Inferred properties can then be **proved valid** via Reluplex / similar decision
procedures, or **associated with a statistical metric of confidence** (number of
satisfying instances).

Example output (from ACAS-Xu slides):
```
range = 499 ∧ −0.314 ≤ θ ≤ −3.14 ∧ −3.14 ≤ ψ ≤ 0 ∧ 100 ≤ vown ≤ 571 ∧ 0 ≤ vint ≤ 150
    ⟹ advisory = Strong Left
```
This is exactly the form of specification a domain expert (aerospace operator) would
write — but the system inferred it from the trained network. **Same machinery generalizes
to SDA**: input = orbital state + observation history; output = trajectory advisory.

### Programmatic Semantic Explainability (Kim et al. CVPR'20)

**Idea.** For perception networks (object detection in autonomous driving), use SCENIC
programs to describe **scenarios** at a semantic level, then extract rules of the form
`scenario-feature predicates ⟹ correct-detection / failure`. Decision-tree learning over
SCENIC features + activation anchors yields rules like:
```
Failure: x_coordinate ≤ -200.76 ∧ distance ≤ 8.84 ∧ car_model = PRANGER
Success: x_coordinate ≥ -198.1
```
**Why this matters for SiS Interpretability-Agent**: this is *exactly* the structural-
invariance Barbiero requires (Symmetry 4) — the explanations live in the user's mental
model (orbital-mechanics user's scenario predicates), not in raw input space. Maps cleanly
onto [[CBM-CTPC-Composition]] where the k=9 concepts are the SCENIC-like vocabulary.

### Proof Decomposition via Layer Patterns

**Idea.** A monolithic proof of `A ⟹ B` over a large NN often times out. Insert an
intermediate-layer spec `σ`; prove `A ⟹ σ` and `σ ⟹ B` *separately*. If each leg is
within Reluplex's budget, the composition gives the original property at much lower cost.

Empirical from the deck: ACAS-Xu properties that **timed out monolithically** were verified
in reasonable time via decomposition. The layer pattern is itself an output of Property
Inference — so the same machinery that *finds* the property *also* finds the proof
decomposition.

## Other Pieces Cataloged (not yet ingested as standalone)

The deck inventories a broader project portfolio:

- **NEUROSPF** (ICSE'21, FoMLAS'21) — symbolic analysis of NNs.
- **DeepCert** (SAFECOMP'21) — contextually relevant robustness verification.
- **Fast Geometric Projections for Local Robustness Certification** (ICLR'21).
- **Probabilistic Analysis of Neural Networks** (SEAMS'20, ISSRE'20) — probabilistic NN
  verification, directly relevant to [[Latent-NCDE-Corrector]] which is probabilistic.
- **Parallelization Techniques for Verifying Neural Networks** (FMCAD'20).
- **DeepSafe** (ATVA'18) — data-driven robustness assessment.
- **NNRepair** (CAV'21) — constraint-based *repair* of misbehaving classifiers (if a
  property is violated, modify the network to satisfy it).

For SiS, the operationally relevant ones are **NEUROSPF** (symbolic analysis at the
ACAS-Xu scale), **Probabilistic Analysis** (NCDE-relevant), and **NNRepair** (operational
fix-up when verification fails). Worth tracking for future deep ingests.

## Connection to SiS / CTPC

**Combined with [[GAINS-Certified-NODE]], SafeDNN completes the formal-verification
toolkit for Q5 of [[CTPC-Design-Rationale]].** The two are complementary:

- **GAINS** verifies the *continuous-time ODE solver* of a Latent ODE — forward
  robustness, includes solver behavior. Best for the *runtime* analysis of NCDE
  inference.
- **SafeDNN / Property Inference** extracts likely specs from a *discrete NN* and verifies
  them via Reluplex. Best for analyzing the *encoder / decoder MLPs* in Latent-NCDE-
  Corrector and the *Predictor-Corrector composition* at discrete time-points.

**SiS-specific integration plan:**

1. **Property Inference on the trained CTPC Latent NCDE Corrector** to extract specs of
   the form `(orbital state regime) ⟹ (correction magnitude bound)` or
   `(near-singular orbit) ⟹ (variance head outputs σ > σ_threshold)`. These specs are
   the AFRL-reviewable form of "what the network actually does."

2. **Programmatic explainability via SCENIC-like orbital scenarios.** Define orbital
   regimes as SCENIC programs (LEO + drag-dominated + high-inclination + ascending-node
   in shadow, etc.); extract semantic rules. This is exactly the form of explanation an
   AFRL operator can reason about. **This is Barbiero's structural invariance for the
   orbital-mechanics-trained user, made operational.**

3. **Proof decomposition for multi-day CTPC verification.** A 7-day-horizon CTPC prediction
   is a composition: GMAT predictor + per-time-step corrector. Verifying the composition
   monolithically would time out. Use layer-pattern decomposition: prove "input regime
   ⟹ encoder latent regime", "encoder latent regime ⟹ NCDE trajectory regime", "NCDE
   trajectory regime ⟹ output prediction safe." Per-leg verification with Reluplex.

4. **ACAS-Xu is the precedent benchmark for an aerospace NN safety case.** Any AFRL
   safety claim for CTPC will be evaluated against the same standards as ACAS-Xu. The
   SafeDNN methodology is the most direct template for what an AFRL-deployable CTPC
   safety dossier would look like.

5. **NNRepair as a fallback if verification fails.** If Property Inference + Reluplex
   identifies a spec violation in CTPC, NNRepair's constraint-based repair (or its
   architectural counterpart in PhyArch) can fix the network rather than re-train from
   scratch.

**Hard vs soft constraint.** SafeDNN methods are *verification* (and *repair*) — they
operate after architecture choices. SiS hard constraints (PHNN Cholesky-PSD, PhyArch
parity-split) make the verification *easier*: properties like `Ḣ ≤ 0` are algebraic
identities that Property Inference would detect immediately and Reluplex would prove
trivially. The combination is the AFRL-deployable end-state.

## Connections

- [[GAINS-Certified-NODE]] — complementary verification for the continuous-time ODE solver.
- [[CTPC-Design-Rationale]] — Q5 (formal verification + safety) operationally addressed
  by SafeDNN + GAINS combination.
- [[CBM-CTPC-Composition]] — the k=9 concept layer is the SCENIC-style vocabulary that
  Programmatic Explainability would use to extract rules.
- [[Actionable-Interpretability-Symmetries]] — Barbiero's structural-invariance symmetry
  is the formal name for what Programmatic Explainability achieves.
- [[Port-Hamiltonian-Neural-Network]] — PHNN's Cholesky-PSD parameterization gives
  algebraic identities that Property Inference detects + Reluplex proves trivially.

## Open Questions

1. **NCDE-aware Property Inference.** The published Property Inference is for static
   feed-forward MLPs. Extending to NCDE-driven dynamics (where the property might be
   about the trajectory class rather than the output point) is non-trivial. Same gap as
   for [[GAINS-Certified-NODE]] — NCDE extension is unpublished.

2. **Right scenario vocabulary for orbital mechanics.** SCENIC was designed for
   autonomous-driving scenarios. The orbital analog needs its own DSL — orbital regimes,
   sensor-coverage geometries, conjunction scenarios. Worth designing as a SiS deliverable.

3. **AFRL safety dossier template.** What does the SafeDNN-style safety case look like
   for a CTPC deployment? Spec list + per-spec verification result + decomposition
   rationale + counterexample analysis + repair history. Concrete deliverable shape for
   Year 3+ of the SiS arc.

4. **Probabilistic Property Inference for the Latent NCDE Corrector.** The
   "Probabilistic Analysis of Neural Networks" (SEAMS'20, ISSRE'20) piece in this deck is
   directly relevant — CTPC's predictive distribution is probabilistic, so probabilistic
   specs ("probability of violation ≤ ε") are the right form. Priority follow-on ingest.

## Sources

- Pasareanu, C. S. (NASA Ames / KBR / CMU). *SafeDNN: Understanding and Verifying Neural
  Networks.* Project overview slide deck. Pages 1–15 read for this ingest (project
  overview, motivating challenges, Property Inference detail with ACAS-Xu case study,
  Programmatic Explainability detail). Remaining 22 slides (further sub-projects,
  Distillation, Probabilistic Analysis, NNRepair examples) referenced via the recent-
  advances catalog but not deeply summarized — topic-driven follow-on ingests when a
  specific SiS milestone requires the depth.
- Constituent papers cataloged (for future ingest): Gopinath, Converse, Pasareanu, Taly
  (ASE'19, Property Inference); Kim, Gopinath, Pasareanu, Seshia (CVPR'20, Programmatic
  Explainability); plus NEUROSPF (ICSE'21), DeepCert (SAFECOMP'21), Probabilistic
  Analysis (SEAMS'20, ISSRE'20), Fast Geometric Projections (ICLR'21), Parallelization
  (FMCAD'20), DeepSafe (ATVA'18), NNRepair (CAV'21).
