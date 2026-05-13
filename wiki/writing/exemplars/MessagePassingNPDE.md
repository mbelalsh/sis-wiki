---
title: MP-PDE — Message Passing Neural PDE Solvers (structural exemplar)
tags: [writing, exemplar, iclr-2022, neural-pde-solver, message-passing, structure-analysis, containment-framing]
sources:
  - raw/writing/exemplars/MessagePassingNPDE.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# MP-PDE — Structural Exemplar

**Source:** Brandstetter, Worrall, Welling. *Message Passing Neural PDE
Solvers*. ICLR 2022 (arXiv:2202.03376v3, March 2023). Amsterdam + JKU
Linz + Qualcomm AI Research. **~9 main pages** + appendix analyzed here
for structure + reviewer-interaction patterns.

This page captures structural craft only, NOT research content.

**Type:** **End-to-end-neural-vs-hybrid methods paper.** Uses
**containment framing** ("our method representationally contains the
classical schemes") — strongest single positioning move in this
exemplar corpus.

---

## Contribution Claim (Verbatim from Abstract)

> "In this work, we build a solver, satisfying these properties, where
> all the components are based on neural message passing, **replacing
> all heuristically designed components in the computation graph with
> backprop-optimized neural function approximators.**"

Method-as-contribution with **explicit-replacement claim**. The phrase
"replacing all heuristically designed components" positions the
contribution as **maximalist** — replaces everything, not just one
component.

**Plus three numbered contributions in §1:**

1. End-to-end fully neural PDE solver based on neural message passing
2. Temporal bundling + pushforward trick (training techniques)
3. Generalization across multiple PDEs within a given class

Same numbered-contributions-list pattern as [[Disparate-DeepEns]] (3
items) and [[HopCPT]] (5 items).

---

## Abstract Structure — Motivation-First with Strong Containment Claim

Seven-sentence abstract following **need → recent progress → gap →
claim → containment-positioning → training-trick → empirical**:

1. **Need:** "The numerical solution of partial differential equations
   (PDEs) is difficult, having led to a century of research so far."
2. **Recent progress:** "Recently, there have been pushes to build
   neural-numerical hybrid solvers, which piggy-backs the modern trend
   towards fully end-to-end learned systems."
3. **Gap:** "Most works so far can only generalize over a subset of
   properties to which a generic solver would be faced, including:
   resolution, topology, geometry, boundary conditions, domain
   discretization regularity, dimensionality, etc."
4. **Fill / claim:** "In this work, we build a solver, satisfying these
   properties…"
5. **Containment positioning:** "**We show that neural message passing
   solvers representationally contain some classical methods, such as
   finite differences, finite volumes, and WENO schemes.**"
6. **Training-trick sub-claim:** "In order to encourage stability in
   training autoregressive models, we put forward a method that is
   based on the principle of zero-stability, posing stability as a
   domain adaptation problem."
7. **Empirical:** "We validate our method on various fluid-like flow
   problems, demonstrating fast, stable, and accurate performance
   across different domain topologies, equation parameters,
   discretizations, etc., in 1D and 2D."

Total ~180 words.

**Distinctive pattern: containment claim in sentence 5.** "Our method
representationally contains classical schemes" is one of the strongest
possible positioning moves — claims structural superiority via
generalization rather than experimental comparison.

---

## Section Order and Approximate Length Ratio (~9 Main Pages)

| Section | Pages | % of main body |
|---|---|---|
| 1 Introduction | 1-2 | ~12% |
| 2 Background AND Related Work | 2-3 | ~20% |
| 3 Method (3.1 Training framework, 3.2 Architecture) | 3-5 | ~25% |
| 4 Experiments (4.1 Interpolating between PDEs, 4.2 Validating training tricks, 4.3 Irregular grids/boundary conditions, 4.4 2D) | 6-9 | ~33% |
| 5 Conclusion | 9 | <10% |
| 6 Reproducibility Statement | 10 | small |
| 7 Ethical Statement | 10 | small |

**Notable structural choices:**

- **"Background AND Related Work" combined into §2.** Different from
  [[GeometryNN]] (standalone Related Work) and [[Disparate-DeepEns]]
  (separate Background + Related Work). Combination is more compact.
- **§2.3 Neural Solvers does explicit field taxonomy** — two
  categories (autoregressive, neural operator) with Figure 1a
  visualizing both. **Landscape-mapping positioning.**
- **§3.1 Training Framework + §3.2 Architecture** as separate
  subsections — clear "what we train" + "what we build" separation.
- **Reproducibility + Ethical statements as separate sections** (§6,
  §7) — ICLR 2022 template requirement.
- **Conclusion is ~half a page** with limitations folded in
  (no standalone Limitations section).

---

## How Method Is Motivated Before Introduction

The introduction uses **"splitter vs lumper" framing** — a memorable
rhetorical device:

1. **Para 1 (context):** "In the sciences, years of work have yielded
   extremely detailed mathematical models of physical phenomena."
2. **Para 2 (problem framing with enumerated requirements):** "The
   design of 'good' PDE solvers is no mean feat. The perfect solver
   should satisfy an almost endless list of conditions. There are
   *user requirements*… *structural requirements*… *implementational
   requirements*."
3. **Para 3 (the rhetorical lever):** "**It is precisely because of
   this considerable list of requirements that the field of numerical
   methods is a *splitter* field, tending to build handcrafted solvers
   for each sub-problem, rather than a *lumper* field, where a
   mentality of 'one method to rule them all' reigns.** This tendency
   is commonly justified with reference to *no free lunch theorems*."
4. **Activity statement:** "We propose to numerically solve PDEs with
   an end-to-end, neural solver."
5. **Numbered contributions list (3 items).**

**Operational pattern:** the "splitter vs lumper" frame is a
**conceptual device that situates the entire contribution** in one
sentence. "We're proposing the lumper alternative to a splitter field."
The reader has a mental model for the whole paper after paragraph 3.

Same template as [[LiePointSymmetryPINN]]'s "for the first time" framing
and [[Disparate-DeepEns]]'s phenomenon-naming — **distinctive
short-noun-phrase rhetorical lever** that captures the contribution
crisply.

---

## Baseline Selection and Framing

**Multi-baseline comparison across multiple experiment families:**

| Baseline | Type | Framing |
|---|---|---|
| WENO5 | Classical 5th-order finite difference | Ground truth for E1/E2/E3; SOTA classical |
| FNO-RNN | Neural Operator (autoregressive variant of Li et al. 2020a) | SOTA neural operator |
| FNO-PF | FNO + our pushforward trick | Cross-pollination experiment: does our trick help their method? |
| MP-PDE-θ_PDE | Our method ablation: no equation parameters input | Internal ablation |
| MP-PDE | Our method | The proposed |
| Pseudospectral (PS) solver | Classical for irregular grids/BCs | For WE1/WE2/WE3 experiments |

**Notable framings:**

1. **"We compare against downsampled groundtruth (WENO5)"** — frames
   classical numerical methods as **groundtruth, not competitor**.
   Subtle move that positions the paper as offering **speed/flexibility
   trade-offs** rather than accuracy improvements.
2. **Cross-pollination experiment:** apply own pushforward trick to a
   competitor (FNO). Shows the trick is general, not method-specific.
   **Generosity move** — gives the competitor an upgrade.
3. **Transparent about losses:** "**FNO-PF outperforms MP-PDE-θ_PDE in
   some cases**" — explicitly acknowledged where the method does not
   win. Not all wins reported; reviewer-trust building.

---

## Limitation Handling — Folded into Conclusion Paragraph

§5 Conclusion contains limitations interleaved with the summary:

> "**A limitation of our model is that we require high quality
> groundtruth data to train.** Indeed generating this data in the first
> place was actually the toughest part of the whole project. However,
> **this is a limitation of most neural PDE solvers in the literature.**
> **Another limitation is the lack of accuracy guarantees typical
> solvers have been designed to output.** This is a common criticism of
> such learned numerical methods. A potential fix would be to fuse this
> work with that in probabilistic numerics (Hennig et al., 2015), as
> has been done for RK4 solvers (Schober et al., 2014). Another
> promising follow-up direction is to research alternative
> adversarial-style losses as introduced in Equation 7. Finally, we
> remark that leveraging symmetries and thus fostering generalization
> is a very active field of research, which is especially appealing for
> building neural PDE solvers since every PDE is defined via a unique
> set of symmetries (Olver, 1986)."

**Two limitations + three future-work pointers**, all in a single
prose paragraph.

**Distinctive moves:**

1. **"This is a limitation of most neural PDE solvers in the
   literature"** — **field-shared-limitation framing**. Acknowledges
   the limitation but contextualizes it as subfield-wide, not specific
   to the paper.
2. **"Indeed generating this data in the first place was actually the
   toughest part of the whole project"** — **unusually personal
   disclosure**. Shows the practical pain authentically. Builds reviewer
   trust.
3. **No numbered list** (unlike [[LiePointSymmetryPINN]]) — prose form,
   integrated into Conclusion.
4. **Forward-pointer to symmetry-based extension** (Olver 1986) —
   precisely the direction [[LiePointSymmetryPINN]] took (Brandstetter
   is co-author on both papers).

---

## Positioning — Prior Works Argued Against

### 1. Bar-Sinai 2019 / Greenfeld 2019 / Hsieh 2019 — partial-neural vs fully-neural

> "Three important works in this area are Bar-Sinai et al. (2019),
> Greenfeld et al. (2019), and Hsieh et al. (2019). Each paper focuses
> on a distinct class of PDE solver: finite volumes, multigrid, and
> iterative finite elements, respectively. **Crucially, they all use a
> hybrid approach (Garcia Satorras et al., 2019), where the solver
> computational graph is preserved and heuristically-chosen parameters
> are predicted with a neural network.**"

**Positioning move:** "hybrid (partial-neural)" vs "fully-neural" —
strict generalization claim. Same template as [[HopCPT]] vs NexCP and
[[GeometryNN]] vs PINNs (we contain you as a limit case).

The containment-claim is then made formal in §3 "Connections":
> "It is through this connection, that we see that message-passing
> neural networks **representationally contain these classical
> schemes**, and are thus a well-motivated architecture."

### 2. Sanchez-Gonzalez 2020 — adversarial training comparison

> "Adversarial losses were also introduced in Sanchez-Gonzalez et al.
> (2020) and later used in Mayr et al. (2021), where Brownian motion
> noise is used for ε…"

Empirical positioning in §4.2: "we compare the efficacy of the
pushforward trick. … **applying the pushforward trick leads to far
higher survival times** … **Interestingly, injecting Gaussian
perturbations appears worse than using none**."

This is the strongest empirical positioning move — **directly compares
own method to Sanchez-Gonzalez 2020 mechanism and shows superiority**.
Not just better in metrics, but the alternative actively harms.

### 3. FNO / Li 2020a — paradigm-level positioning

> "Neural operator methods treat the mapping from initial conditions to
> solutions at time t as an input–output mapping learnable via
> supervised learning."

Positioned as **paradigm-level alternative** (autoregressive vs.
operator), not direct competitor. Both paradigms validated as
legitimate; MP-PDE chosen for specific reasons. **No claim of
superiority over FNO as a paradigm** — only that MP-PDE wins under
specific operating conditions (low resolution, generalization across
PDE coefficients).

---

## Structural Patterns Worth Replicating for [[CTPC-KDD-Submission]]

1. **Containment-framing positioning move.** "Our method
   representationally contains classical schemes." If true and
   defensible, this is the strongest possible positioning move.
   CTPC could frame: "CTPC representationally contains standard split
   CP (zero corrector) and standard Latent ODE (zero physics
   predictor) as limit cases."
2. **Splitter-vs-lumper rhetorical lever** in §1. A short noun-phrase
   that captures the entire contribution. CTPC could frame as
   "post-processing vs end-to-end" or "Gaussian-tail vs heavy-tail."
3. **Numbered enumerated requirements** (user / structural /
   implementational) in §1 as motivation. Three categories of
   requirements that the contribution must satisfy.
4. **Combined "Background AND Related Work" §2** — more compact than
   separate sections. Reasonable for ~9-page submissions.
5. **Field-taxonomy figure (Figure 1a)** distinguishing paradigms.
   For SiS: autoregressive correctors (Latent ODE, NCDE) vs
   one-shot predictors (FNO for orbital regressors) vs hybrid
   (CTPC, GMAT + corrector).
6. **Cross-pollination experiment:** apply our training trick to a
   competitor's method to show generality.
7. **Transparent loss disclosure** — "Method X outperforms ours in
   case Y." Pre-empts reviewer accusations of cherry-picking.
8. **Field-shared-limitation framing** — "this is a limitation of
   most methods in the subfield" — acknowledges but contextualizes.
9. **Authentic personal disclosure in Conclusion** ("generating data
   was the toughest part of the whole project") — builds trust.

---

## Connection to SiS / CTPC

Direct applicability to [[CTPC-KDD-Submission]]:

- **Containment claim:** "CTPC representationally contains [GMAT-only,
  Latent-ODE, standard split CP] as limit cases." If formally
  defensible, this is the strongest positioning move available.
- **Splitter-vs-lumper framing for SiS:** the orbital-prediction
  subfield is a "splitter" field (different tools for SGP4, GMAT,
  Cowell, etc.). CTPC proposes the "lumper" alternative — one
  corrector framework, multiple physics predictors.
- **Three-category requirements list in §1:** user requirements
  (latency, calibration), structural requirements (frame equivariance,
  dissipation), implementational requirements (continuous-time,
  irregular sampling).
- **Cross-pollination:** apply Student-t prediction head to a Latent
  ODE baseline to show heavy-tail likelihood helps even outside CTPC.
- **Field-shared-limitation framing:** "This is a limitation of all
  ML-based orbital-prediction methods" (e.g., training-data quality).

---

## Connections

- [[MessagePassingNPDE-reviews]] — paired OpenReview reviews (next
  ingest)
- [[LiePointSymmetryPINN]] — same author (Brandstetter) on both;
  papers are conceptually sequential. MP-PDE's Conclusion-paragraph
  pointer to "leveraging symmetries" is precisely
  [[LiePointSymmetryPINN]]'s contribution
- [[HopCPT]] — same generalization-as-positioning template ("HopCPT
  contains NexCP as limit case")
- [[GeometryNN]] — different positioning style (new-problem-space
  carving rather than containment)
- [[Disparate-DeepEns]] — same numbered-contributions template
- [[NeurIPS-2025-Reviewer-Guidelines]] — Q2.4 originality verbatim
  "novel combination" + Q2.3 significance "advance our
  understanding/knowledge demonstrably" both well-satisfied here
- [[PeRCNN]] — the SiS physics-as-architecture canonical comparator;
  conceptually adjacent to MP-PDE's containment-of-classical-schemes
  claim
- [[CTPC-KDD-Submission]] — direct structural-pattern adoption target

---

## Open Questions

1. **Multi-author paper (Brandstetter on multiple exemplars)** — is
   the style consistent across his papers, or shaped by venue/coauthor?
   Worth tracking.
2. **Appendix is referenced ~10 times** — substantial supporting
   material. Worth examining for CTPC's appendix structure.
3. **The "splitter vs lumper" framing is distinctive** — does it work
   only for fields that explicitly have this characterization? Or can
   any paper find such a binary?
4. **"This is a limitation of most neural PDE solvers in the
   literature"** — does this work? Or does it telegraph that the
   limitation is acceptable because the subfield accepts it? Reviewer
   reactions worth checking in [[MessagePassingNPDE-reviews]].

---

## Sources

- `raw/writing/exemplars/MessagePassingNPDE.pdf` — Brandstetter,
  Worrall, Welling, ICLR 2022 (arXiv:2202.03376v3). Pages 1-10 read in
  this ingest (covers full main body + start of references).
