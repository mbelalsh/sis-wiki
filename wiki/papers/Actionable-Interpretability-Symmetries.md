---
title: Actionable Interpretability Must Be Defined in Terms of Symmetries (Barbiero et al. 2026)
tags: [interpretability, position-paper, symmetries, markov-categories, actionable-interpretability, four-symmetries]
sources: [raw/papers/interpretability/Interpretability&Symmetry.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: critical
hard_constraint_possible: yes
---

# Actionable Interpretability Must Be Defined in Terms of Symmetries (Barbiero et al. 2026)

> Barbiero, Espinosa Zarlenga, Giannini, Termine, Bonchi, Jamnik, Marra. *Position: Actionable Interpretability Must Be Defined in Terms of Symmetries*. arXiv:2601.12913, 29 Jan 2026.

> **Position-paper status.** The title says it: this is a *position paper* arguing that interpretability research SHOULD be defined via four specific symmetries — it is not a survey of established consensus. The framework is *posited* (Section 2: "we hypothesise that four symmetries... suffice") with the call to action being adoption by the community (Section 6). Claims that depend on the framework being authoritative ("X is the first practical instantiation under Barbiero") inherit this positional status.

> **Critical wiki audit context.** This page exists because [[CTPC-Design-Rationale]] Part III was originally drafted from discussion context (Bilal's verbal description of the framework), not from the paper itself. After this canonical ingest (2026-05-09), four substantive errors in the original Part III were identified and corrected. See [[CTPC-Design-Rationale]] § "Corrections from Barbiero Ingest (2026-05-09)" for the audit trail. The canonical content here is what those corrections were checked against.

## One-Line Intuition

The paper argues that current interpretability research is *ill-posed* because existing definitions (Lipton 2018, Doshi-Velez & Kim 2017, Miller 2019, etc.) are informal and fail to provide formal verification frameworks or design principles. Inspired by Klein's Erlangen Program (1893), the authors posit that interpretability must be defined in terms of **symmetries** — structure-preserving transformations — and identify four symmetries (inference equivariance, information invariance, concept-closure invariance, structural invariance) that together characterize "interpretable models" as a Markov subcategory.

## Why This Was Written

Existing interpretability foundations (Kim et al. 2016, Biran & Cotton 2017, Doshi-Velez & Kim 2017, Lipton 2018, Miller 2019, Watson & Floridi 2021, Facchini & Termine 2021, Giannini et al. 2024, Tull et al. 2024) all fail to provide:

1. **A formal framework for verifying interpretability** — most definitions are descriptive ("a method is interpretable if a user can predict its outputs"), leaving no mathematical handle for testing whether a given model satisfies the property.
2. **A set of design principles** — without formal grounding, model designers can't be told *what* to build to be interpretable.

The paper's response is the Erlangen-style move: characterize the field by the symmetries (transformations preserving relevant structure) that the desired objects (interpretable models) satisfy. This makes interpretability **(i) testable** (verify each symmetry), **(ii) actionable** (identify which symmetry is violated and design to satisfy it), and **(iii) compositional** (subsume existing properties as consequences of the symmetries).

## The Four Symmetries

The setup uses Markov categories ([Fritz 2020]); specifically `BorelStoch`, the category of standard Borel spaces and Markov kernels. A *human user* `h` has a category `Hm` of "mental models" (Johnson-Laird 1983, Kim et al. 2018) — characterized by user-dependent structural properties (linearity, sparsity, monotonicity, etc.). The category of *interpretable models for h* is `Im[h]`. Both embed faithfully into `BorelStoch`.

A *probabilistic model* is a stochastic morphism `X → Y` written `P_{Y|X}` — a conditional distribution.

### Symmetry 1: Inference Equivariance (Section 2.1)

**Informal:** "A model is interpretable if a user can correctly predict the model outputs."

**Formal (Symmetry 1).** Inference `P_{Y|X}(Y | X = x)` is equivariant w.r.t. a reference `Hm` under translations `τ_X : X → X[h]` and `τ_Y : Y → Y[h]` if and only if there exists a model `P_{Y|X}^{[h]} : X[h] → P(Y)` in `Hm` such that the following diagram commutes:

```
       P_{Y|X}
   X ─────────→ Y
   │            │
τ_X│            │τ_Y
   ↓            ↓
   X[h] ─────→ Y[h]
        P^[h]_{Y|X}
```

**Reading:** translate-then-predict equals predict-then-translate. The user's mental model `P^[h]_{Y|X}` produces the same outputs (after translation) as the model `P_{Y|X}` does on the original inputs.

**Three challenges (paper enumerates as C1, C2, C3):**

- **C1: Verification is intractable.** For binary `10×10` images, exhaustive verification needs `2^100` evaluations — more than atoms in the universe.
- **C2: Translations exist but some are unsound.** Need a way to characterize sound translations.
- **C3: Many models satisfy this; some don't satisfy desirable user-specific properties.** Inference equivariance alone isn't enough.

These three challenges motivate the other three symmetries.

### Symmetry 2: Information Invariance (Section 2.2)

**Informal:** "An interpretable model should retain only the input information that is sufficient for the task, discarding irrelevant details."

**Formal (Symmetry 2).** Given `P_{Y|X}`, the mutual information `I(Y;X)` is invariant under marginalization `P_{Y|Z} ∘ P_{Z|X}` with `H(Z) ≪ H(X)` if the diagram commutes:

```
   X ─────P_{Z|X}─────→ Z
    \                  │
     \                 │P_{Y|Z}
   P_{Y|X}             ↓
       ↘──────────────→ Y
```

**Reading:** `Z` is a *compressed sufficient statistic* — `H(Z) ≪ H(X)` (compression) and `I(Y;X|Z) = 0` (sufficiency). The model factors as `P_{Y|X} = P_{Y|Z} ∘ P_{Z|X}`.

**Why this matters operationally:** verifying inference equivariance for `P_{Y|Z}` is exponentially cheaper than for `P_{Y|X}` because `Z` is much smaller than `X`. So information invariance addresses challenge C1.

**Derivable properties (Section 2.2.1):** sparsity (few features/parameters), compactness (exclusion of irrelevant info), completeness (sufficient-statistic property), modularity (decomposability) — all become *consequences* of information invariance, not first principles.

**Methods that satisfy:** feature selection (sparse decision trees), manifold-hypothesis-leveraging models (Tishby & Zaslavsky 2015 information bottleneck for DNNs).

**Methods that fail (paper explicitly disqualifies):** post-hoc feature attribution (SHAP, LIME, integrated gradients, etc.) — they "operate in the original input space and do not guarantee the existence of a lower-dimensional representation `Z` that captures all and only the information relevant for `Y`. Therefore, by failing to satisfy information invariance, verifying inference equivariance using post-hoc feature attribution remains intractable."

### Symmetry 3: Concept-Closure Invariance (Section 2.3)

**Informal:** "The variables used by an interpretable model should have the same semantics as those used by humans (e.g., a concept 'red' in the model matches 'red' for a human)."

**Formal setup.** A *concept* is a fixed point of `(β, γ)` operators (formal concept analysis, Goguen 2005, Ganter & Wille 1996):
- `β : P(S) → P(U)` maps a sentence set `T ⊆ S` to objects `M ⊆ U` satisfying *all* sentences in `T`
- `γ : P(U) → P(S)` maps an object set `M ⊆ U` to sentences satisfied by *all* objects in `M`
- A concept is a tuple `(T, M)` with `T = γ(M)` and `M = β(T)`

**Formal (Symmetry 3).** Concept closure is invariant under sentence translation `τ_C : T → T'` if for any concepts `C = (T, M)` and `C' = (T', M)` (same underlying object set!), the closure diagram commutes for all `ω ∈ M`.

**Reading:** a translation `τ_C` is *sound* if it preserves the underlying object set. Example: `τ_C = {black → noir}` is sound because both pick out the same set of black objects. `τ = {black → un}` is *not* sound because "un" picks out different objects.

**This is about translation between vocabularies, NOT about intervention compositionality.** Interventions appear in Section 4 as a *consequence* (Bayesian inversion on aligned concept-based models), not as the symmetry definition.

**Methods that satisfy (paper explicitly cites):** Concept Bottleneck Models (Koh et al. 2020) — "enforce concept-closure by construction by forcing the model to use concepts from the user's conceptual space."

**Methods that fail:** decision trees and additive models operating on non-concept spaces (e.g., individual pixels) — paper says explicitly such models "are uninterpretable when they operate on non-concept spaces."

### Symmetry 4: Structural Invariance (Section 2.4)

**Informal:** "A model is interpretable if it is drawn from a hypothesis class that the target user can reason about."

**Formal (Symmetry 4).** Structural properties of models in `Im` are invariant under the functor `F : Im → Hm` if and only if there exist injective functors `E_1 : Im ↪ BorelStoch` and `E_2 : Hm ↪ BorelStoch` such that the diagram commutes:

```
    Im ──E_1──→ BorelStoch
    │           ↗
   F│         /
    ↓        / E_2
    Hm  ────/
```

**Reading:** the model's hypothesis class belongs to (or is functorially mapped to) the user's mental-model hypothesis class.

**This is user-relative — the most important framing in the paper.** From Section 2.4.1: "Structural invariance makes interpretability explicitly user-centric and task-specific... linearity, monotonicity, or sparsity are not first principles but instantiations of structural properties chosen to match a given user."

**Worked example (paper Example 3):** A probabilistic XOR model. For a *student user* whose mental model is linear-logistic kernels — fails (XOR can't be expressed as a linear function). For a *researcher user* whose mental model includes piecewise-linear kernels — succeeds. **Same model, different interpretability status, depending on user.**

**Methods that satisfy:** linear models (Rudin 2019), monotone functions (Debot et al. 2024) — for users whose mental model includes those classes.

**Methods that fail (paper explicitly disqualifies):** "Sparse autoencoders and concept-based models whose mappings between concepts and tasks (or between concepts themselves) are arbitrary, such as employing DNNs, have a hypothesis space that lacks any well-defined structure and therefore do not satisfy this symmetry. More broadly, models that violate domain-required structures must be excluded: for example, in medical risk scoring, where monotonicity is a critical structural requirement, highly expressive models such as unconstrained DNNs are not interpretable for that domain, regardless of their predictive accuracy."

**This disqualification matters for CBMs:** a CBM with a free-form MLP task predictor `f : ĉ → ŷ` fails structural invariance even if it satisfies concept-closure. The CBM-style architecture needs a structurally-committed head (linear, monotonic, manipulator-skeleton, etc.) to satisfy all four symmetries simultaneously.

## Section 3: Probabilistic Interpretable Models

**Definition 2:** Interpretable models are those satisfying *all* four interpretability symmetries.

**Definition 3 (Category of Interpretable Models):** `Im ↪ BorelStoch` has:
- **Objects:** concept spaces `C_1, ..., C_k`
- **Processes:** concept-based conditional distributions `P_{C_i|C_j}`, copy maps `copy_{C_i}`, discard maps `discard_{C_i}`
- **Composition rules:** sequential and parallel composition (string-diagram notation)

The category is rendered with string diagrams (Joyal & Street 1991, Selinger 2010) — the same formalism used for quantum circuits and Markov processes. This makes interpretable models a *general recipe*: anything built from the listed objects + processes + composition rules is potentially interpretable, with concrete interpretability requiring the four symmetries to hold for the target user `h`.

## Section 4: Inference on Probabilistic Interpretable Models

**Three forms of inference unified as Bayesian inversion:**

### Concept Alignment (Definition 4)

Compute the posterior `P(Θ | C = c, pa(C))` over parameters `Θ` of a parametric concept map, given ground-truth concept observations `c`. Standard Bayesian inversion.

### Intervention (Definition 5)

Compute the posterior `P_{C | pa(C), pa'(C)}(c | pa(c), pa'(c))` given a concept-based prior `P_{C | pa(C)}` and a likelihood `P_{pa'(C) | C}` representing the intervention.

**Special cases:**
- **Ground-truth interventions** (Koh et al. 2020): set `ĉ_j ← c*_j` (true value). The likelihood is the identity; the evidence is a delta function on `c*`.
- **Do-interventions** (Pearl 1988): set `ĉ_j ← k` (constant). Same form with evidence `k`.

### Counterfactual (Definition 6)

Three-step Bayesian-inversion sequence:
1. **Abduction:** observe target value `Y = y`, compute posterior over exogenous variables `U`
2. **Action:** given the posterior on `U`, duplicate the model and intervene on the new copy
3. **Prediction:** infer `Y` in the duplicated intervened model

**Conclusion 5:** alignment, interventional, and counterfactual inference are all forms of Bayesian inversion — meaning a single posterior-inference algorithm (e.g., belief propagation) can perform all interpretable-inference queries.

## Section 5: Alternative Views (Rebuttals)

**View #1:** "There is no universal mathematical definition of interpretability" (Murphy 2023).
**Rebuttal:** the four symmetries provide formal grounding while preserving user-centrism (Symmetries I, III, IV all reference the user `h`).

**View #2:** "Symmetries are not enough."
**Rebuttal:** the framework is extensible; new properties can be analyzed to determine whether they correspond to additional symmetries or refinements of existing ones.

**View #3:** "Symmetries are too unfamiliar to the interpretability community."
**Rebuttal:** trade-off between rigor and accessibility; each symmetry admits intuitive example-level interpretations without full categorical machinery.

## Section 6: Call to Action

**Concrete diagnostic** (the most important prescriptive part for SiS):

> "The concept-based interpretability community has largely focused on concept-closure invariance, but has paid comparatively little attention to structural invariance. As a result, concept-based models often exhibit arbitrary behaviour (e.g., when using DNNs as task predictors) or are weak classifiers (e.g., when using linear models as task predictors). Conversely, the mechanistic interpretability community has been less attentive to concept-closure invariance and information invariance. This typically leads to concepts that are not aligned with human semantics and to information overload."

**Three-part research call:**
1. Map existing interpretability methods to the proposed symmetries.
2. Identify which symmetries are implicitly assumed or violated.
3. Develop new methods that are explicitly symmetry-complete w.r.t. their intended tasks and users.

## Connection to SiS / CTPC

The framework provides the formal grounding for design decisions in [[CTPC-Design-Rationale]] Part III. Key implications:

- **PhyArch + CBM-CTPC + Analytic-Σ-CTPC** is a *gap-filling* contribution to the diagnostic in Section 6: it brings *structural invariance* (PhyArch's manipulator-skeleton) to the *concept-based interpretability* community (CBM's bottleneck), in the orbital domain. Section 6 explicitly identifies this as a deficit in the literature.
- **All claims of "first practical instantiation" must acknowledge position-paper status.** The framework is *posited* by Barbiero et al., not yet established as field consensus. Wiki claims build on a positional argument.
- **All structural-invariance claims must specify the target user class.** Per Section 2.4: structural invariance is user-relative. PhyArch's manipulator-skeleton is structurally invariant *for an orbital-mechanics-trained user* (the AFRL operator). For a generic ML practitioner, the relevant structural form is different.
- **Information invariance is closed by *compression*, not by frame invariance.** The `k=9` concept bottleneck in [[CBM-CTPC-Composition]] IS the compressed sufficient statistic `Z`. Analytic-Σ propagation does NOT close information invariance per Barbiero — it refines *inference equivariance* to the full predictive distribution.
- **Concept closure is about translation, not intervention.** Interventions are a consequence (Section 4), enabled by the framework but not equivalent to the symmetry definition.

## Connections

- [[CTPC-Design-Rationale]] Part III — the page that operationalizes this framework for SiS; corrected after this ingest
- [[CBM-CTPC-Composition]] — Year 1 milestone closing concept-closure AND information invariance via the concept bottleneck (one architectural mechanism, two symmetries)
- [[Analytic-Sigma-CTPC-Composition]] — Year 2 milestone refining inference equivariance to full predictive distribution
- [[Concept-Bottleneck-Models]] — paper Section 2.3.1 explicitly cites CBMs as concept-closure mechanism
- [[PhyArch]] — manipulator-equation skeleton provides structural invariance for physics-trained users (Section 2.4 framing)

## Open Questions

- **Symmetry-completeness of the four.** Paper hypothesizes (Section 2: "we hypothesise that four symmetries... suffice") but doesn't prove uniqueness or completeness. Future work may identify additional symmetries.
- **Verification at scale.** Naive verification of inference equivariance is intractable (challenge C1, paper). Information invariance reduces this exponentially via compression, but actual verification protocols for real models are open work — the paper sketches the framework but doesn't operationalize verification algorithms.
- **Translation soundness automation.** Concept-closure invariance requires *sound* translations between vocabularies. Paper's example (`black ↔ noir`) is hand-checked. Automating this for large concept vocabularies is open.
- **User mental-model elicitation.** Structural invariance requires knowing the user's hypothesis class `Hm`. In practice, this is rarely formalized — domain experts have implicit mental models that need to be made explicit. How to elicit `Hm` operationally is open.
- **Compositional symmetry guarantees.** When composing interpretable models (e.g., CBM + structurally-committed head + analytic propagation), do the four symmetries compose? The category-theoretic framework suggests yes, but explicit composition theorems are not proven in the paper.
- **Position-paper adoption.** The framework's primacy depends on community uptake. As of 2026-05, the framework is recent (arXiv Jan 2026) and unratified. Claims that lean on the framework being authoritative inherit this dependency.

## Sources

- `raw/papers/interpretability/Interpretability&Symmetry.pdf` — Barbiero, Espinosa Zarlenga, Giannini, Termine, Bonchi, Jamnik, Marra 2026 (arXiv:2601.12913v3, 29 Jan 2026). Position paper. Section 1 (introduction + framework overview), Section 2 (four symmetries with formal definitions: Symmetry 1 inference equivariance, Symmetry 2 information invariance, Symmetry 3 concept-closure invariance, Symmetry 4 structural invariance), Section 3 (Markov-category formalization), Section 4 (alignment / intervention / counterfactual as Bayesian inversion), Section 5 (alternative views + rebuttals), Section 6 (call to action including the concept-based-vs-mechanistic-interpretability diagnostic).
