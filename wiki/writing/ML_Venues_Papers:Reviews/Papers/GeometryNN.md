---
title: GINN — Geometry-Informed Neural Networks (structural exemplar)
tags: [writing, exemplar, icml-2025, neural-fields, data-free-generation, structure-analysis]
sources:
  - raw/writing/exemplars/GeometryNN.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# GINN — Structural Exemplar

**Source:** Berzins, Radler, Volkmann, Sanokowski, Hochreiter, Brandstetter.
*Geometry-Informed Neural Networks*. ICML 2025 (PMLR 267). LIT AI Lab JKU
Linz + SINTEF/Oslo + Emmi AI Linz. **~9 main pages** + appendix analyzed
here for structure + reviewer-interaction patterns.

This page captures structural craft only, NOT research content.

---

## Contribution Claim (Verbatim from Abstract)

> "To this end, we introduce geometry-informed neural networks (GINNs) –
> a framework for training shape-generative neural fields *without data*
> by leveraging user-specified design requirements in the form of
> objectives and constraints."

One-sentence contribution. The italicized "*without data*" is the single
load-bearing differentiator — it visually anchors the gap-fill. The verb
"introduce" rather than "propose" or "present" — slightly more
declarative.

---

## Abstract Structure — Motivation-First, Four-Move

Five-sentence abstract following **need → gap → fill → differentiator →
impact**:

1. **Need:** "Geometry is a ubiquitous tool in computer graphics, design,
   and engineering."
2. **Gap:** "However, the lack of large shape datasets limits the
   application of state-of-the-art supervised learning methods and
   motivates the exploration of alternative learning strategies."
3. **Fill:** "To this end, we introduce geometry-informed neural networks
   (GINNs)…"
4. **Differentiator:** "By adding *diversity* as an explicit constraint,
   GINNs avoid mode-collapse and can generate multiple diverse solutions,
   often required in geometry tasks."
5. **Impact + evidence cue:** "Experimentally, we apply GINNs to several
   problems spanning physics, geometry, and engineering design… These
   results demonstrate the potential of training shape-generative models
   without data, paving the way for new generative design approaches
   without large datasets."

Total ~130 words. **Motivation-first** with explicit differentiator
sentence — the "by adding diversity as an explicit constraint" framing is
unusually rhetorical (most ML abstracts list features; this one names a
*property*).

---

## Section Order and Approximate Length Ratio (~9 Main Pages)

| Section | Pages | % of main body |
|---|---|---|
| 1 Introduction | 1-2 | ~12% |
| 2 Related Work (2.1 Theory-Informed Learning, 2.2 Neural Fields, 2.3 (Data-Free) Generative Modeling) | 2-4 | **~25% — unusually large** |
| 3 Method (3.1 Finding a Solution, 3.2 Generating Diverse Solutions) | 4-5 | ~22% |
| 4 Experiments (4.1 Experimental Details, 4.2 Problems, 4.3 Discussion) | 6-8 | ~33% |
| 5 Conclusion (incl. "Limitations and future work" subsection) | 9 | <10% |

**Notable structural choices (contrasts with [[HopCPT]]):**

- **Standalone Related Work section** (§2), not folded into Introduction.
  This contrasts directly with HopCPT's §1.1 nesting.
- **Related Work is unusually long (~25%)** with 3 subsections AND a
  Venn-diagram figure (Figure 2) positioning GINNs at the intersection
  of three fields. This is structurally rare — Related Work usually gets
  ~10-15% of a main paper.
- **Limitations live INSIDE Conclusion** as a subsection ("Limitations
  and future work"), not as a mid-paper Methods subsection like HopCPT.
- **Discussion is part of Experiments §4.3**, not a separate section.
- **Impact Statement** is a labeled paragraph after Conclusion (ICML
  template requirement) — not the same as Limitations.

---

## How Method Is Motivated Before Introduction

The introduction's motivation arc uses an **explicit research-question
device**:

1. **Paragraph 1:** "Recent advances in deep learning have revolutionized
   fields with abundant data… However, the scarcity of large datasets in
   many other domains, including 3D computer graphics, design,
   engineering, and physics, restricts the use of advanced supervised
   learning techniques…"
2. **Paragraph 2:** "Fortunately, these disciplines are often equipped
   with formal problem descriptions, such as objectives and constraints.
   Previous works for PDEs (Raissi et al., 2019), molecular science
   (Noé et al., 2019), and combinatorial optimization (Bengio et al.,
   2021) demonstrate these can suffice to train models even in the
   absence of any data. The success of these data-free approaches
   motivates an analogous attempt in geometry, **raising the question:**
   *Is it possible to train a shape-generative model on objectives and
   constraints alone, without relying on any data?*"

The italicized question is a strong rhetorical move — frames the
contribution as an answer to a specific binary question rather than as
a methods proposal. **The next paragraph opens "We address this question
by introducing geometry-informed neural networks or GINNs."** — direct
answer with method-name introduction.

**Operational pattern:** the method-name (GINN) is introduced via a
**question-answer rhetorical pairing**, not via a paragraph label like
HopCPT. This is a more literary opening; it commits the reader to the
question before introducing the answer.

---

## Baseline Selection and Framing

**Pre-emptive baseline-absence declaration:**

> "To the best of our knowledge, **data-free shape-generative modeling
> is an unexplored field with no established baselines, problems, and
> metrics.** Thus, in addition to the problems defined and solved in
> Section 4.2, we define metrics for each constraint as detailed in
> Appendix C.1."

This is a **claim of new-territory positioning** — the paper carves out
a problem space rather than competing on an existing one. **Different
positioning move than [[HopCPT]]** (which beat established CP-for-time-
series baselines on existing metrics).

Comparisons performed:

| Comparison | Type | Framing |
|---|---|---|
| Softplus-MLP / SIREN / WIRE | NF model ablation | "highlighting the superiority of WIRE… due to their spectral behavior" |
| Adaptive ALM vs naive weighted loss | optimization-method ablation | "balances feasibility and optimality… avoiding ill-conditioning" |
| Diversity constraint on/off | mechanism ablation | mode-collapse demonstration |
| GINN solutions vs. known closed-form (Plateau's, parabolic mirror) | sanity-check | "approximates the known solution" |
| Boltzmann generators / PINNs / topology optimization | conceptual sibling | discussed in Related Work + Method, NOT in Experiments table |

**Framing pattern:** absent direct competitors, the paper compares
*internal variants* (mechanism on/off, alternative architectures,
optimizer choices) + *closed-form ground truths* on toy problems. This
is the standard playbook for **carving-out-new-territory papers**.

---

## Limitation Handling — Acknowledged, in Conclusion, Paired with Follow-Up

**§5 Conclusion / Limitations and future work** subsection contains four
explicit admissions, each paired with a follow-up pointer:

1. **Component-level theoretical depth:** "GINNs combine several known
   and novel components, each of which warrants an in-depth study of
   theoretical and practical aspects, including alternative shape
   distances, their aggregation into diversity, conditioning mechanisms,
   constraints, and optimization." — admits breadth-over-depth scope.
2. **No comparison to established topology-optimization methods:** "In
   this work, we focused on building the conceptual framework of GINNs
   and validating it experimentally. This included a realistic
   engineering design task. **However, we considered a modified version
   of the original task and did not compare to established
   topology-optimization methods as this required the integration of a
   PDE solver – a task we address in a follow-up work.**" — explicit
   admission of the most damaging absence + concrete plan.
3. **ALM as suboptimal:** "Even though ALM is a significant improvement
   over the naive approach of manually weighted loss terms, the recent
   literature on multi-objective and second-order optimizers suggests
   further possible improvements."
4. **Data-free limit:** "Finally, we investigated GINNs in the limit of
   no data. However, GINNs can integrate partial observations of a
   single or multiple shapes."

**Operational pattern:** the most damaging admission (no comparison to
established baselines) is stated with explanation (PDE solver
requirement) and forward pointer (follow-up work). This is the
**[[ICML-2022-Reviewer-Tutorial]] slide 19 / [[NeurIPS-2025-Reviewer-Guidelines]]
Q8** template — be up front, get rewarded not punished.

**Where limitations DO NOT appear:** abstract, introduction, or as a
mid-paper standalone subsection (unlike HopCPT §2.6).

---

## Positioning — Two Prior Works Argued Against Most Explicitly

### 1. PINNs (Raissi et al. 2019) — shared inspiration, different numerical behavior

Two-paragraph discussion in §2.1:

> "Same as PINNs, GINNs use neural fields to represent the solution.
> Consequentially, we also observe that some best practices of training
> PINNs (Wang et al., 2023) transfer to training GINNs. **However, PINNs
> may suffer from ill numerical properties due to minimizing the
> squared residual of the strong-form different to classical PDE
> solvers (Rathore et al., 2024; Ryck et al., 2024). In contrast, GINNs
> share the same underlying formulation and numerical properties as
> classical topology optimization methods.**"

Positioning move: **claim of structural-numerical superiority** over
PINNs. Cites two recent papers (Rathore 2024, Ryck 2024) for the
PINN-criticism — borrows the criticism rather than originating it.

### 2. Boltzmann generators (Noé et al. 2019) — shared data-free inspiration, different mechanism

> "Boltzmann generators avoid mode-collapse using an entropy-regularizing
> term, which presupposes invertibility, making them not directly
> applicable to function spaces. **Instead, GINNs use a more general
> diversity term to hinder mode-collapse over the function space of
> shapes.**"

Positioning move: **claim of generality** — Boltzmann generators are a
restricted special case (require invertibility); GINNs operate on the
broader function space. Same "we contain you as a limit case" template
[[HopCPT]] used against NexCP.

### Tertiary explicit positioning

- **vs. Implicit Neural Shapes (INSs) for surface reconstruction:** §2.2
  distinguishes "generative" vs "surface reconstruction" training
  regimes — GINNs in the former.
- **vs. classical topology optimization:** "While classical methods
  optimize a single shape directly, **we optimize a model that generates
  diverse feasible shapes.**" (§2.3) — generality claim via the
  generative leap.

---

## Structural Patterns Worth Replicating for [[CTPC-KDD-Submission]]

1. **Italicized research-question rhetorical device** before introducing
   the method name. "Is it possible to X without Y?" → "We address this
   question by introducing Z." Strong literary opening, commits the
   reader.
2. **Venn-diagram positioning figure** (Figure 2) when the contribution
   sits at the intersection of multiple existing fields. SiS's
   CTPC sits at SDA ∩ NCDE ∩ UQ ∩ port-Hamiltonian — a Venn-diagram
   figure could anchor that.
3. **"Unexplored field with no established baselines" framing** when
   carving out a new problem space — different framing than "we beat
   existing methods" but equally valid per
   [[NeurIPS-2025-Reviewer-Guidelines]] Q2.4 novelty.
4. **Limitations paired with follow-up-work pointers**. Each admission
   has a "this is addressed in follow-up work" or "this leaves room for
   future investigation" clause. Defuses the "why didn't you do X?"
   rebuttal target.
5. **"In contrast" / "Instead" demarcation language** for prior-work
   positioning. Each named competitor gets one sentence stating the
   structural difference, not paragraphs of criticism.
6. **Cited-criticism technique:** instead of originating the criticism
   of PINNs, the paper cites Rathore 2024 + Ryck 2024 *for* the
   criticism. This is rebuttal-safe — the critique is sourced.

---

## Connection to SiS / CTPC

Direct applicability to [[CTPC-KDD-Submission]]:

- **Research-question opening** maps to "Is it possible to achieve
  calibrated probabilistic orbital prediction with d̄² ≈ 1 using
  physics-grounded ML correctors on real spacecraft data?"
- **Venn-diagram positioning** maps to CTPC = SDA ∩ probabilistic
  forecasting ∩ continuous-time ML ∩ port-Hamiltonian dissipation.
- **"In contrast" demarcation language** for Latent ODE, standard CP,
  GP, Kalman filters — one-sentence structural-difference statements.
- **Limitations-with-follow-up pointers** for: single-satellite-class,
  RTN frame only, NASA CDDIS regime, Mashiku ν_min inheritance — each
  paired with what would extend the work.
- **Cited-criticism technique** for Gaussian-tail-inappropriate-for-
  orbital-errors: cite Mashiku-Garrison-Carpenter 2012 rather than
  originate the criticism.

---

## Connections

- [[GeometryNN-reviews]] — paired OpenReview reviews (next ingest)
- [[HopCPT]] — contrast: nested vs standalone Related Work; mid-paper
  vs conclusion Limitations
- [[NeurIPS-2025-Reviewer-Guidelines]] — Q2.4 originality verbatim
  "novel combination of existing techniques" is the rubric this
  paper exemplifies
- [[Peyton-Jones-Research-Paper]] — Adelson 4-point structure matches
  the explicit-question opening
- [[How-To-Write-1]] — Pak §3.2 memorable-naming (GINN is a
  citation-friendly acronym)
- [[CTPC-KDD-Submission]] — direct structural-pattern adoption target

---

## Open Questions

1. **Appendix structure** (pages 11+) not analyzed; likely substantial
   given the breadth of experiments.
2. **Acknowledgement length** is moderate (~10 funder mentions) —
   shorter than HopCPT's ~16, longer than minimal.
3. **Standalone Related Work as 25%** of main body — is this
   geometry/ICML-typical or paper-specific? Worth comparing against
   the other 4 exemplars in this corpus.

---

## Sources

- `raw/writing/exemplars/GeometryNN.pdf` — Berzins et al., ICML 2025
  (PMLR 267). Pages 1-10 read in this ingest (covers full main body +
  start of references).
