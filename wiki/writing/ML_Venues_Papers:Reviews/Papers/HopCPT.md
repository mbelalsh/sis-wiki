---
title: HopCPT — Conformal Prediction for Time Series with Modern Hopfield Networks (structural exemplar)
tags: [writing, exemplar, neurips-2023, conformal-prediction, time-series, structure-analysis]
sources:
  - raw/writing/exemplars/HopCPT.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# HopCPT — Structural Exemplar

**Source:** Auer, Gauch, Klotz, Hochreiter. *Conformal Prediction for Time
Series with Modern Hopfield Networks*. NeurIPS 2023 (arXiv:2303.12783v2,
Nov 2023). ELLIS Unit Linz + JKU + Google Research Zürich. 48 pages
including appendix; **10 main pages** analyzed here for structure +
reviewer-interaction patterns.

This page captures structural craft only, NOT research content.

---

## Contribution Claim (Verbatim from Abstract)

> "We propose HopCPT, a novel conformal prediction approach for time series
> that not only copes with temporal structures but leverages them."

One-sentence contribution; clean subject-verb-object form. The two
distinguishing predicates ("copes with" + "leverages") are co-ordinate
phrases — Strunk-White Rule 15 parallelism.

---

## Abstract Structure — Motivation-First, Three-Move

Five-sentence abstract follows the **need → gap → fill → evidence → impact**
pattern:

1. **Need** (sentence 1): "To quantify uncertainty, conformal prediction
   methods are gaining continuously more interest…"
2. **Gap** (sentence 2): "However, they are difficult to apply to time
   series as the autocorrelative structure of time series violates basic
   assumptions required by conformal prediction."
3. **Fill / claim** (sentence 3): "We propose HopCPT, a novel conformal
   prediction approach for time series that not only copes with temporal
   structures but leverages them."
4. **Evidence cue** (sentence 4): "We show that our approach is
   theoretically well justified…"
5. **Empirical scope** (sentence 5): "In experiments, we demonstrate that
   our new approach outperforms state-of-the-art conformal prediction
   methods on multiple real-world time series datasets from four different
   domains."

Total ~120 words. **Motivation-first** — the contribution is delayed to
sentence 3. The 2025-era practice (NeurIPS 2025 + ICML Best Practices)
allows claim-first abstracts; HopCPT chose motivation-first.

---

## Section Order and Approximate Length Ratio (10 Main Pages)

| Section | Pages | % of main body |
|---|---|---|
| 1 Introduction (incl. 1.1 Related Work, 1.2 Setting) | 1-4 | ~40% |
| 2 HopCPT (2.1 Theoretical Motivation, 2.2 Associative Soft-selection, 2.3 Encoding Network, 2.4 Training Procedure, 2.5 Synthetic Example, 2.6 Limitations) | 4-7 | ~30% |
| 3 Experiments (3.1 Setup, 3.2 Results & Discussion) | 7-9 | ~30% |
| 4 Conclusions | 10 | <10% |

**Notable structural choices:**

- **Related Work is a subsection of Introduction** (§1.1), not a standalone
  section. Sits between the contribution bullets and the formal Setting.
- **Setting is a subsection of Introduction** (§1.2), not a standalone
  preliminaries section. Definitions live early.
- **Limitations is a subsection of Methods** (§2.6), not a subsection of
  Conclusion or an appendix. Limitations come BEFORE Experiments. This is
  unusual and worth replicating.
- **Synthetic Example is a methods subsection** (§2.5), not in
  Experiments. Used as a teaching example before the real experiments.
- **Single Conclusions paragraph + Future Work paragraph + 3 future-work
  items** (a/b/c list). Total Conclusion ~half a page.

---

## How Method Is Motivated Before Introduction

The paper has **no pre-Introduction "Motivation" section**. The motivation
arc lives inside §1 Introduction:

1. **Paragraph 1** (4 sentences): domain motivation — uncertainty
   estimation matters for environmental phenomena (flood forecasting,
   hydrology). Cites Gneiting & Katzfuss 2014, Zhu & Laptev 2017,
   Krzysztofowicz 2001. **No method names yet.**
2. **Paragraph 2** (3 sentences): CP background — what CP achieves
   (finite-sample marginal coverage with almost no assumptions) + the gap
   (exchangeability). Cites Vovk et al. 1999, 2005, Vovk 2012.
3. **Paragraph 3 labeled "HopCPT."**: the method is introduced in a single
   bolded-label paragraph with a forward-pointer to Figure 1. 6 sentences
   covering soft-selection mechanism + regime concept + scalability claim.
4. **Bulleted "Our main contributions are:" list** (5 numbered items)
   ending with the hydrology-specific application claim.

**Operational pattern:** the method's *name* (HopCPT) appears in the
abstract sentence 3 AND as a bolded paragraph label in the intro. Two
explicit anchors in the first ~1.5 pages. The reader knows what HopCPT is
before page 2.

---

## Baseline Selection and Framing

**Six compared approaches** (Section 3.1, "Compared approaches"):

| Baseline | Year | Framing in HopCPT |
|---|---|---|
| EnbPI (Xu & Xie 2022a) | 2022 | "designed for scarce data… difficult to use for larger datasets" — scalability gap |
| SPCI (Xu & Xie 2022b) | 2022 | "re-calculates random forest at each step, computational burden that prohibits its application to large datasets" — efficiency gap |
| NexCP (Foygel Barber et al. 2022) | 2022 | "uses exponential decay… HopCPT can learn this strategy but does not a priori commit to it" — generality gap |
| CopulaCPTS (Sun & Yu 2022) | 2022 | multivariate-target setting; "conformalize their prediction based on a copula" |
| AdaptiveCI (Gibbs & Candes 2021) | 2021 | "Adaption-based approaches like these are orthogonal to HopCPT and can serve as an enhancement" — complement, not rival |
| Standard split CP / CF-RNN (Stankevičiūtė 2021) | 2021 | baseline floor — "no weighting of the scores is required" |

**Plus one non-CP comparison:** Klotz et al. 2022 (deep-learning UQ for
hydrology) — evaluated only on Streamflow dataset to show HopCPT beats the
domain-specific non-CP SOTA too.

**Framing pattern:** each baseline is positioned by **what it can't do
that HopCPT can** (scarce-data limitation, computational burden, fixed
weighting, multivariate-only, orthogonal-not-competing). Every dismissal
is grounded in a *specific operational shortcoming*, never "not novel" or
"old method".

**Table 1 layout:** 4 datasets × 4 prediction models × 3 metrics (Δ Cov,
PI-Width, Winkler) × 7 CP approaches. Bold = best PI-Width given
Δ Cov ≥ -0.25α. Grey = failed coverage. The bold-grey convention is a
**clean signal** — reader sees instantly which methods preserved coverage
and which were efficient.

---

## Limitation Handling — Acknowledged, Mid-Paper, Standalone Subsection

§2.6 Limitations (one full paragraph, ~half-page) appears at the END of
Methods, BEFORE Experiments. Four explicit limitations:

1. **Relies on assumptions** about identifying error regimes + exchangeability
   within regimes. "Our extensive empirical evaluation suggests that these
   assumptions hold in practice" — acknowledges + counter-evidence.
2. **Memory scales linearly** with size. "It can be the case that not all
   historical time steps may be kept in the Hopfield memory anymore" —
   states the constraint + proposed workarounds (oldest-entry removal,
   sub-sampling).
3. **Data scarcity** edge case: "for datasets with very scarce data it
   might be difficult to learn a useful embedding… in that case, it could
   be better to use kNN rather than learning the similarity with MHN."
   Honest concession + offers alternative.
4. **Social impact deferred** to Appendix I.

**Operational pattern:** limitations are stated as **operational
constraints, not as fatal flaws**. Each constraint is paired with either
empirical counter-evidence ("our evaluation suggests these hold") or a
proposed workaround. This is the [[NeurIPS-2025-Reviewer-Guidelines]] Q8
"rewarded rather than punished for being up front" template.

**Where the limitations DO NOT appear:** Conclusion section (which is
forward-looking only) and Abstract (which is positive-only). The
limitations stay in §2.6.

---

## Positioning — Two Prior Works Argued Against Most Explicitly

### 1. EnbPI (Xu & Xie 2022a) — empirical rival

**Most explicit objection:** "This is specifically geared to settings with
scarce data and difficult to use for larger datasets, which is why we do
not apply it in our experiments. EnbPI is designed around the notion that
near-term errors are often independent and identically distributed and
therefore exchangeable."

Two distinct argument modes:
- **Scalability claim:** EnbPI doesn't work on large datasets.
- **Assumption claim:** EnbPI's i.i.d. near-term-error assumption is too
  strong for HopCPT's domain.

### 2. NexCP (Foygel Barber et al. 2022) — conceptual rival

**Most explicit objection:** "NexCP uses exponential decay as the weighting
method, arguing that the recent past is more likely to be of the same
error distribution. **HopCPT can learn this strategy, but does not a
priori commit to it.**"

The contrast is **strict generalization** — "we contain you as a special
case but our parameterization is richer." This is the strongest positive
positioning move in the paper: HopCPT is presented as a strict
generalization of NexCP's exponential-decay weighting, not as a
replacement.

**Notably absent from explicit positioning:** SPCI (Xu & Xie 2022b) is
dismissed on computational grounds only, not on methodology. CopulaCPTS
and AdaptiveCI are sidestepped as "different setting" / "orthogonal."

---

## Structural Patterns Worth Replicating for [[CTPC-KDD-Submission]]

1. **Three-move motivation-first abstract** (need → gap → fill → evidence →
   impact) with one-sentence contribution claim using parallel co-ordinate
   predicates.
2. **Related Work as a subsection of Introduction**, not a separate
   section. Tightens the structural flow.
3. **Method name introduced in the abstract AND as a bolded paragraph
   label in §1**. Two explicit anchors in first 1.5 pages.
4. **"Our main contributions are:" bulleted list at end of §1**. 4-5
   numbered items, last one calling out the application-domain hook.
5. **Limitations as a Methods subsection, mid-paper**, with each limitation
   paired to either empirical counter-evidence or a proposed workaround.
   This pre-empts [[NeurIPS-2025-Reviewer-Guidelines]] Q8 rebuttal targets.
6. **Baseline framing as operational shortcomings**, not novelty claims.
   "Doesn't scale" / "computational burden" / "doesn't learn the
   weighting" — each is checkable.
7. **Bold-grey table convention** for "best given coverage held" — separates
   the efficiency dimension from the coverage dimension visually.
8. **Synthetic Example as a Methods subsection**, not in Experiments. Lets
   the reader see the mechanism work in a controlled toy setting before
   the real experiments.

---

## Connection to SiS / CTPC

Direct applicability to [[CTPC-KDD-Submission]]:

- **Three-move abstract** maps to CTPC's "need (orbital UQ) → gap
  (Gaussian/Latent-ODE break under heavy tails) → fill (Student-t-headed
  NCDE on GMAT-corrected residuals)".
- **Generalization-not-replacement framing** ("HopCPT can learn NexCP's
  strategy") maps to "CTPC contains GMAT-only and Latent-ODE as
  limiting cases" — a strong positive positioning move.
- **Limitations subsection mid-paper** maps to listing CTPC's intentional
  scope boundaries (1-satellite-class, RTN frame, NASA CDDIS regime)
  upfront, each with proposed extensions.
- **Baseline framing as operational shortcomings** maps to framing Latent
  ODE / standard GP / quantile regression each by what they *can't do*
  (heavy-tail likelihood / scalable irregular sampling / dissipation
  enforcement).

---

## Connections

- [[HopCPT-reviews]] — paired OpenReview reviews (next ingest)
- [[NeurIPS-2025-Reviewer-Guidelines]] — Q8 limitation-handling alignment
- [[ICML-2022-Reviewer-Tutorial]] — positioning matches "well-grounded
  knowledge advancement" criterion
- [[Strunk-White-Elements-Style]] — Rule 15 parallelism (abstract
  contribution sentence)
- [[Peyton-Jones-Research-Paper]] — Matryoshka reader-funnel (paper
  follows the brief→long→complete pattern)
- [[CTPC-KDD-Submission]] — direct structural-pattern adoption target

---

## Open Questions

1. **Appendix structure (pages 13-48)** not analyzed in this ingest;
   would inform supplementary-material conventions for KDD.
2. **Acknowledgements paragraph** is a long list of funded projects — 16
   funder mentions. NeurIPS-typical pattern but verbose; KDD/ICML may
   prefer shorter.
3. **Number of references** (~100+ from the visible bibliography) is on
   the high end. CTPC may want a tighter ~50-70.

---

## Sources

- `raw/writing/exemplars/HopCPT.pdf` — Auer, Gauch, Klotz, Hochreiter,
  NeurIPS 2023, arXiv:2303.12783v2. 48 pages total; pages 1-12 read in
  this ingest (covers all main sections + start of references).
