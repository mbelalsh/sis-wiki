---
title: Disparate Benefits of Deep Ensembles (structural exemplar)
tags: [writing, exemplar, icml-2025, fairness, deep-ensembles, characterization-paper, structure-analysis]
sources:
  - raw/writing/exemplars/Disparate-DeepEns.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Disparate Benefits of Deep Ensembles — Structural Exemplar

**Source:** Schweighofer, Arnaiz-Rodriguez, Hochreiter, Oliver. *The
Disparate Benefits of Deep Ensembles*. ICML 2025 (PMLR 267). ELLIS Unit
Linz + LIT AI Lab JKU + ELLIS Alicante + NXAI. **~9 main pages** + appendix
analyzed here for structure + reviewer-interaction patterns.

This page captures structural craft only, NOT research content.

**Type:** *Characterization paper* — names and explains a phenomenon
rather than proposing a new method. Different rhetorical category from
[[HopCPT]] (methods paper) and [[GeometryNN]] (new-problem-space paper).

---

## Contribution Claim (Verbatim from Abstract)

> "Our analysis reveals that they unevenly favor different groups, a
> phenomenon that we term the *disparate benefits* effect."

**Finding-as-contribution** rather than method-as-contribution. The
single sentence does three things at once: (1) names the
phenomenon ("disparate benefits"), (2) credits the naming explicitly
("a phenomenon that we term"), (3) asserts the empirical finding.

Compare to [[HopCPT]]'s "We propose HopCPT…" (method-as-contribution)
and [[GeometryNN]]'s "we introduce geometry-informed neural networks…"
(framework-as-contribution). Three different contribution-claim
templates.

---

## Abstract Structure — Motivation-First, Five-Move (Extended)

Eight-sentence abstract following **need → gap → fill → finding →
evidence → mechanism → mitigation → claim**:

1. **Need:** "Ensembles of Deep Neural Networks, Deep Ensembles, are
   widely used as a simple way to boost predictive performance."
2. **Gap:** "However, their impact on algorithmic fairness is not well
   understood yet."
3. **Definition cue:** "Algorithmic fairness examines how a model's
   performance varies across socially relevant groups…"
4. **Activity:** "In this work, we explore the interplay between the
   performance gains from Deep Ensembles and fairness."
5. **Finding (the contribution sentence):** "Our analysis reveals that
   they unevenly favor different groups, a phenomenon that we term the
   *disparate benefits* effect."
6. **Evidence:** "We empirically investigate this effect using popular
   facial analysis and medical imaging datasets with protected group
   attributes…"
7. **Mechanism:** "Furthermore, we identify that the per-group
   differences in predictive diversity of ensemble members can explain
   this effect."
8. **Mitigation:** "Finally, we demonstrate that the classical Hardt
   post-processing method is particularly effective at mitigating the
   disparate benefits effect of Deep Ensembles by leveraging their
   better-calibrated predictive distributions."

Total ~200 words — **longer than HopCPT (~120) or GeometryNN (~130)**.
The eight-move structure mirrors the paper's three-part body
(characterize → explain → fix). The abstract IS a three-act structure
in miniature.

---

## Section Order and Approximate Length Ratio (~9 Main Pages)

| Section | Pages | % of main body |
|---|---|---|
| 1 Introduction | 1-2 | ~12% |
| 2 Related Work (Algorithmic Fairness / Deep Ensembles / Ensemble Fairness) | 2 | ~10% |
| 3 Background (binary classification setup, Deep Ensembles formal def, Group Fairness desiderata) | 3 | ~12% |
| 4 Experimental Setup (Datasets, Models and training, Performance Metrics, Group Fairness Metrics) | 4 | ~10% |
| **5 The Disparate Benefits of Deep Ensembles** (the phenomenon) | 4-6 | ~22% |
| **6 What is the Reason for Disparate Benefits?** (the mechanism) | 6-7 | ~13% |
| **7 Mitigating the Unfairness caused by the Disparate Benefits Effect** (the fix) | 7-9 | ~17% |
| 8 Conclusion | 9 | ~4% |

**Notable structural choices:**

- **Three-part body structure: characterize → explain → fix.** §5/§6/§7
  each get a dedicated section. This is the canonical structure for
  characterization papers (different from methods papers which usually
  have a single Methods section).
- **Section names are full question-form clauses** ("What is the Reason
  for Disparate Benefits?" / "Mitigating the Unfairness caused by the
  Disparate Benefits Effect"). Conversational; long. **Different style
  from HopCPT's terse "2 HopCPT / 3 Experiments" labels.**
- **Background is a standalone section §3**, separate from Related Work
  §2. Two-tier preliminaries.
- **Limitations are at the END of Conclusion** as the final paragraph,
  not a subsection.
- **Impact Statement** is the ICML template paragraph after Conclusion.

---

## How Method Is Motivated Before Introduction

The introduction's motivation structure is **classic gap-driven**:

1. **Paragraph 1:** "Deep Ensembles… have demonstrated their efficacy
   as a simple and robust method…"
2. **Paragraph 2:** "In such applications, it is crucial to examine how
   these models perform across different groups that are defined by a
   protected attribute…"
3. **Paragraph 3 (gap-naming):** "Although the differences in
   performance across protected groups (group fairness violations) of
   individual DNNs has been thoroughly studied (Zhang et al., 2018;
   Sagawa et al., 2020; Zhang et al., 2022; Arnaiz-Rodriguez & Oliver,
   2024), **the impact on fairness due to ensembling these networks
   remains underexplored.**"
4. **Paragraph 4 (activity statement):** "In this paper, our aim is to
   fill this gap by conducting an extensive empirical study of the
   fairness implications of Deep Ensembles…"

Then findings preview paragraph + **numbered contributions list (3
items)**.

**Operational pattern:** the "thoroughly studied X, but Y remains
underexplored" template is the same as [[HopCPT]] and [[GeometryNN]].
This is the **canonical gap-naming sentence structure** across ML
papers. The phrase "remains underexplored" or "remains an open question"
or "has not been investigated" is doing 90% of the rhetorical work.

---

## Baseline Selection and Framing

**Baselines aren't competitor methods** — they're internal contrasts:

| Comparison axis | What's compared | Why |
|---|---|---|
| Ensemble vs individual members | average individual ensemble member | the central effect — does ensembling change fairness? |
| Architecture | ResNet18/34/50, RegNet-Y, EfficientNetV2-S (5 architectures) | robustness across models |
| Dataset | FairFace, UTKFace, CheXpert (3 datasets, 2 domains) | robustness across applications |
| Fairness metric | SPD, EOD, AOD (3 metrics) | robustness across operationalizations |
| Mitigation strategy | Hardt Post-Processing (HPP) on individual vs ensemble | the fix demonstration |

**Total experimental cells:** 5 architectures × 5 seeds × 4 target
variables × 3 datasets = 1,000 individual models (per paper §4).

**Framing:** "We evaluate a total of fifteen tasks across five different
DNN model architectures and three standard group fairness metrics."
**Quantitative comprehensiveness as a positioning lever** — explicitly
counts the experimental scope.

**Disagreement-of-conclusion positioning vs Ko et al. (2023):**

> "**The most closely related previous work to ours is that by Ko et
> al. (2023)**, which investigates the effect of Deep Ensembles on
> subgroup performance and served as an inspiration for our work.
> However, their focus and methodology are different from ours. … **Ko
> et al. conclude that Deep Ensembles have exclusively positive impact,
> while we show that they can negatively affect group fairness.**"

This is **the strongest possible single positioning move** —
explicit-conclusion-disagreement with the most-closely-related prior
work. Three sub-differences are enumerated (group definition, metric
choice, conclusion direction). Rebuttal-safe because the disagreement
is sourced and decomposable.

---

## Limitation Handling — Acknowledged in Conclusion Paragraph + Impact Statement

**§8 Conclusion final paragraph (verbatim):**

> "The main limitations of our study are that **we focus on vision
> tasks**, and hence on ensembles of convolutional DNNs. Furthermore,
> **the three considered group fairness metrics, while widely used, are
> not sufficient to guarantee fair outcomes**, as fairness can't be
> reduced to satisfying any single metric alone. In future work, we
> thus plan to explore other notions of fairness, such as individual
> fairness, and extend our analysis to other types of models and
> datasets, for instance, in the language domain. Furthermore, we
> intend to investigate the disparate benefits effect of Deep Ensembles
> where pre- or in-processing fairness interventions have been applied
> to individual ensemble members."

Two methodological limitations + two future-work bullets in one
paragraph. **Briefer than HopCPT's §2.6 limitations subsection or
GeometryNN's §5 "Limitations and future work" section.**

**Impact Statement (separate paragraph, ICML template requirement):**

> "Our study unveils the disparate benefits effect of Deep Ensembles,
> which potentially causes socially harmful predictions. Although we
> investigate its origin and explore a way to mitigate it, **this
> intervention alone can not guarantee fair outcomes.** Researchers and
> practitioners should keep in mind that the fairness of predictions of
> any ML model applied in the real world can't be reduced to any single
> metric and must be carefully assessed depending on the application."

Three notable moves:

1. **Limitations are split across two locations:** methodological in
   Conclusion, ethical/societal in Impact Statement.
2. **Most damaging admission ("intervention alone can not guarantee")**
   appears in Impact Statement, not Conclusion. Rhetorically lighter
   placement.
3. **No mid-paper limitation subsection** — different from HopCPT, more
   like GeometryNN.

---

## Positioning — Prior Works Argued Against

### 1. Ko et al. (2023) — disagreement-of-conclusion (most explicit)

Detailed above. Three enumerated sub-differences + opposite-conclusion
framing. **The strongest single positioning move I have seen in this
corpus so far.**

### 2. Hardt et al. (2016) — repositioned as complement, not rival

The classic Hardt post-processing fairness method is the central
method this paper deploys (§7). Not "argued against" but explicitly
adopted. Position: "Notably, although HPP is a classical method to
improve the fairness of ML models, it hasn't been investigated for
Deep Ensembles. … HPP is thus very effective in mitigating unfairness
for Deep Ensembles while preserving their superior performance."

**Application-novelty claim:** "first to investigate HPP for Deep
Ensembles specifically." Rebuttal-safe.

### 3. Lakshminarayanan et al. (2017) — examined-consequences-of

The original Deep Ensembles paper. Cited as the method whose
unintended fairness consequences this paper documents. **No
disagreement** — only consequence-investigation.

---

## Structural Patterns Worth Replicating for [[CTPC-KDD-Submission]]

1. **Three-act body structure** (characterize → explain → fix) for
   papers that diagnose AND resolve a problem. Each act gets a
   standalone section.
2. **Section names as full question-form clauses** ("What is the
   Reason for X?") — conversational, signal-rich, easier-to-skim than
   terse labels. Reader can find the mechanism section from the TOC
   alone.
3. **Quantitative-comprehensiveness statement in §1** — "We evaluate
   a total of N tasks across M models and K metrics" — explicit count
   pre-empts "evaluation scope" objections (cf. [[GeometryNN-reviews]]
   where this objection was unanimous).
4. **Disagreement-of-conclusion positioning vs single
   most-closely-related work.** "X concludes Y; we show ¬Y." Strong
   rhetorical move when sourced and decomposed.
5. **Naming the phenomenon** — "we term the *disparate benefits*
   effect." Citation-friendly, memorable. Like GINN's "we introduce
   geometry-informed neural networks." Both papers create a noun phrase
   for future citation.
6. **Limitations split into methodological (Conclusion) + ethical
   (Impact Statement)** — separates the two failure-mode classes.
7. **Numbered contributions list (3 items)** at end of intro — same
   pattern as HopCPT (5 items) and GeometryNN (2 items, longer).

---

## Connection to SiS / CTPC

Direct applicability to [[CTPC-KDD-Submission]]:

- **Naming the phenomenon:** SiS could name the calibration phenomenon
  observed ("d̄² → 1 only with Student-t + physics-grounded predictor")
  — give it a citation-friendly noun phrase.
- **Question-form section names:** "What is the Reason for CTPC's
  Calibration?" / "Why does Latent ODE Fail?" — easier to find in TOC
  than "Discussion of Calibration."
- **Quantitative-comprehensiveness pre-emption:** count CTPC's
  experimental scope upfront (N satellites × M horizons × K metrics) in
  §1, not in §4.
- **Disagreement-of-conclusion vs immediately-closest work** — if there
  is a specific paper that reaches a different conclusion on orbital UQ
  (e.g., a Latent ODE paper claiming calibration), use the Ko et al.
  template: "X concludes Y; we show ¬Y."
- **Naming-as-positioning:** rather than "CTPC method," reframe as "CTPC
  framework" or "the CTPC composition" to create a citation-friendly
  noun phrase.

---

## Connections

- [[Disparate-DeepEns-reviews]] — paired OpenReview reviews (next ingest)
- [[HopCPT]] — contrast: methods paper vs characterization paper;
  numbered contributions list pattern shared
- [[GeometryNN]] — contrast: new-problem-space paper. All three papers
  use gap-driven motivation but different contribution types
- [[NeurIPS-2025-Reviewer-Guidelines]] — Q2.3 Significance verbatim
  rewards "unique conclusions about existing data" — this paper
  exemplifies
- [[ICML-2022-Reviewer-Tutorial]] — slide 10 acceptance criterion
  "sufficient knowledge advancement" satisfied by phenomenon-naming
- [[How-To-Write-1]] — Pak §3.2 memorable naming
  (disparate-benefits effect is a citation-friendly term)
- [[CTPC-KDD-Submission]] — direct structural-pattern adoption target

---

## Open Questions

1. **Appendix structure** (Appendices D, E, F referenced) not analyzed
   in this ingest. Likely substantial.
2. **1000 individual models** mentioned (5 architectures × 4 target
   variables × 5 seeds × subset of datasets) — interestingly large
   ablation scope. Worth verifying for SiS's scope-counting purposes.
3. **Hardt et al. 2016** is positioned as a "classical method" — does
   ICML 2025 still treat 9-year-old papers as classical? Worth tracking
   the temporal language pattern.

---

## Sources

- `raw/writing/exemplars/Disparate-DeepEns.pdf` — Schweighofer et al.,
  ICML 2025 (PMLR 267). Pages 1-10 read in this ingest (covers full
  main body + start of references).
