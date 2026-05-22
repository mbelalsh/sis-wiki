---
title: HopCPT — OpenReview Reviews (NeurIPS 2023, accepted poster)
tags: [writing, exemplar-reviews, openreview, neurips-2023, conformal-prediction, reviewer-interaction]
sources:
  - raw/writing/reviews/HopCPT-OpenReview.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# HopCPT — OpenReview Reviews

**Paper:** Auer, Gauch, Klotz, Hochreiter. *Conformal Prediction for Time
Series with Modern Hopfield Networks*. NeurIPS 2023. **Submission #6410.**
Paired with [[HopCPT]] structural exemplar. **OpenReview URL fragment:**
`forum?id=KTRwpWCMsC`.

This page captures reviewer interaction patterns only, NOT research
content.

---

## Final Decision

**Accept (poster).** NeurIPS 2023 main track.

**PC meta-review (verbatim):**

> "By and large, this paper has been well received by the reviewers. In
> particular, they all agree that the paper is well-structured and clearly
> presented, and that the experimental results are convincing. **In the
> beginning, two of the reviewers were nevertheless quite critical and
> raised a couple of concerns. These could mostly be clarified in the
> rebuttal and discussion phase. Eventually, all reviewers were on the
> positive side**, although the novelty of the paper still appears to be
> somewhat limited, and the empirical findings are not backed up by
> theoretical results. It could also be seen a bit critical that the
> authors made quite substantial changes to the manuscript. Admittedly,
> these were requested by the reviewers, but actually couldn't be checked.
> So in the end, there are pros and cons, but the consensus is that the
> former outweigh the latter."

Two operational signals from this meta-review:
1. **Rebuttal converted critical reviewers.** Score increases were the
   deciding factor.
2. **PC accepted "substantial revisions" without verification.** Authors
   making large reviewer-requested edits did not block acceptance.

---

## Score Distribution

| Reviewer | Rating (1-10 scale) | Confidence (1-5) | Soundness | Presentation | Contribution | Score change |
|---|---|---|---|---|---|---|
| tZC3 | **7 (Accept)** | 4 | 3 good | **4 excellent** | 3 good | — (high anchor) |
| YCj6 | 5 → **6** (Borderline → Weak Accept) | 4 | 3 good | 3 good | 2 fair | **+1 after rebuttal** |
| yfWn | 6 (Weak Accept) | 3 | 3 good | 3 good | 3 good | — |
| Fgat | 6 (Weak Accept) | 3 | 2 fair | 3 good | 2 fair | **Raised OVERALL after rebuttal** (final magnitude unstated but explicit lift) |

**Final aggregate:** 7, 6, 6, 6 (post-rebuttal). Two of four reviewers
explicitly raised their score in response to the rebuttal. Reviewer tZC3
served as the high anchor and never wavered.

**Rebuttal acknowledgement count:** 4/4 reviewers wrote follow-up
comments. 2/4 explicit score lifts. 2/4 expressed satisfaction without
score change ("Thanks for clarifying all my questions." / "Thank you for
the thorough response").

---

## What the Strongest Reviewer (tZC3, Rating 7) Praised Specifically

Verbatim strengths:

1. **"The manuscript is extremely well structured and very easy to follow.
   The authors provided an excellent overview of the conformal prediction
   task and relevant work."**
2. **"The experiments provide a thorough insight into how the proposed
   method functions in a synthetic and more realistic setting. Figure 2
   contrasts the proposed with competing methods in a comprehensible and
   straightforward way."**
3. **"In addition to a solid theoretical motivation, the proposed method
   performs outstandingly and convincingly on a wide range of prediction
   models and several data sets."**

**Operational patterns:** tZC3's praise targeted structure (paragraph
1) + figure clarity (paragraph 2) + empirical breadth (paragraph 3). The
"4 excellent" presentation score correlates with explicit structural
praise — clarity is a high-presentation-score lever.

---

## What the Weakest Reviewers (Fgat / YCj6) Objected to

### Fgat (Soundness 2, Contribution 2) — four explicit weaknesses

1. **"This paper consists only of a method proposal and an experimental
   evaluation, and does not include theoretical effects. In this
   structure, is an evaluation on four data sets sufficient?"** —
   theory-light objection.
2. **GP/Bayesian optimization not discussed.** "While it may not be
   directly comparable, would it be appropriate to make no mention of
   it?" — coverage-of-related-work objection.
3. **MHN choice not justified.** "Experiments did not provide
   justification for introducing MHNs to solve the problem."
4. **Novelty.** "It is a combination of existing methods, and I feel it
   is weak in terms of novelty."

### YCj6 (Contribution 2) — three explicit weaknesses

1. **"Contribution… seems to be relatively marginal."** Combination on
   top of NexCP + Xu & Xie 2022a.
2. **Computational complexity** added vs. baselines.
3. **Encoding network choice (Section 2.3)** not justified.
4. **Unclear role of W_q, W_k.** Notation question.

### Both shared the **novelty/contribution anxiety** axis.

---

## What Rebuttal Responses Changed Scores

### YCj6: **+1 explicit raise** triggered by

The rebuttal acknowledged the inheritance from Foygel Barber 2022 + Xu &
Xie 2022a but asserted specific delta:

> "To the best of our knowledge, **we are the first to use a learned,
> similarity-based retrieval mechanism for conformal prediction**. Our
> approach incorporates the covariates of the time series, while still
> being able to (1) consider the temporal proximity and (2) work in an
> online fashion. Hence, HopCPT provides much broader capabilities than
> Foygel Barber et al. (2022) and Xu & Xie (2022a). Further… **our paper
> contains the largest evaluation conducted for time series CP yet.**"

Three specific levers in one paragraph: (a) name the unique mechanism
(learned similarity), (b) enumerate operational capabilities competitors
lack (covariates + temporal + online), (c) quantify empirical scope
(largest evaluation). YCj6 explicitly: **"regarding my point about the
contribution, I decided to raise my score by one."**

### Fgat: **OVERALL score raise** triggered by

Two rebuttal moves:

**Move 1 — Three reasons for MHN specifically (numbered, structured):**
> "(1) MHN allow to go beyond simply considering temporal proximity (as
> in EnbPI and NexCP). Instead, MHN also consider covariates…
> (2) MHN provide a flexible memory mechanism, which we can update for
> each new observation…
> (3) MHN learn a measure of similarity based on covariates. The ablation
> in Appendix D… shows that a learned representation is needed…"

**Move 2 — Decomposing "novelty" into idea vs. implementation:**
After Fgat's follow-up clarifying their fourth point, the authors wrote:
> "Conceptually, our idea is to leverage the concept of regimes to enable
> CP for time series. The introduction of MHN is already a concrete
> implementation of this idea. … We will therefore **split the second
> point of our contributions (Section 1) into two parts. The first will
> describe the idea and the second the implementation.**"

Fgat then: **"As a discussion within the framework of the CP field, the
novelty was also clarified by the authors and judged to be highly
effective in practical use. I raise the OVERALL score and make it my
final response."**

### Key meta-observation

Both score-flipping rebuttals worked by **decomposing a vague
"novelty/contribution" objection into specific operational sub-claims**
that could be individually checked. The verbatim language used by both
reviewers post-rebuttal: "clarified."

---

## Recurring Objections Across Multiple Reviewers

### 1. **LIMITATIONS SECTION MISSING — 4/4 reviewers** (unanimous)

- **tZC3:** "I am missing a section on the limitations of the proposed
  method. For instance, how does the method scale to very long time
  series?"
- **YCj6:** "There is still room for elaboration on other limitations."
- **Fgat:** "The authors do not explicitly address Limitation."
- **yfWn:** "It might be good to stress that the guarantee is marginal
  and asymptotic…"

**Author response:** Added §2.6 Limitations as a dedicated section.
"Other reviewers have raised similar points regarding the limitations.
So far, the limitations of our method were somewhat dispersed throughout
the manuscript and supplemental material. We added a dedicated limitations
section to the revised paper."

**This is the single most important reviewer-interaction signal in this
exemplar:** a paper that mentions limitations in scattered places but
lacks a dedicated section will get the same objection from every
reviewer. The reviewer training material ([[NeurIPS-2025-Reviewer-Guidelines]]
Q8) explicitly rewards limitations disclosure.

### 2. **THEORY-IN-MAIN-PAPER GAP — 2/4 reviewers**

- **yfWn:** "From the paper alone, it's difficult to tell what exactly
  are the theoretical guarantees of your algorithm. While the authors did
  refer to Appendix B, I think it is important to briefly describe the
  assumptions and state the main bound (B.8 and B.9) of your theoretical
  analysis in the paper."
- **Fgat:** "Does not include theoretical effects."

**Author response:** Added a discussion paragraph elaborating Appendix B
theoretical results to the main paper's theory section.

### 3. **MHN VS. ATTENTION CLARITY — 2/4 reviewers**

- **tZC3:** Explicit weakness — "It would be helpful to lay out the
  similarities and differences between MHNs and the attention mechanism."
- **YCj6:** Questions about W_q, W_k roles.

**Author response:** Expanded MHN-attention connection in §2.4 and
preceding MHN introduction.

### 4. **NOVELTY/CONTRIBUTION ANXIETY — 2/4 reviewers**

- YCj6 + Fgat both used the exact phrase pattern "combination of existing
  methods" or "relatively marginal." This is the textbook **Novelty
  Fallacy** ([[ICML-2022-Reviewer-Tutorial]] slide 17).

**Author response:** Per the score-flipping rebuttal moves above —
decompose vague novelty into named operational deltas + split idea vs.
implementation contributions.

---

## Desk-Reject Signals for This Subfield (Conformal Prediction for Time Series at NeurIPS)

No actual desk-reject occurred — paper survived Phase 1. But the
review record reveals **what the subfield tolerates and what it
demands:**

**Tolerated:**
- Building directly on 2-3 prior weighted-CP frameworks (Foygel Barber
  2022, Xu & Xie 2022) — the PC's meta-review noted "novelty… somewhat
  limited" but accepted.
- "Combination of existing methods" framing — survived because the
  combination produced clearly better empirical results.
- Theoretical results being asymptotic and confined to appendix.
- Substantial post-rebuttal manuscript revisions.

**Demanded:**
- **A dedicated Limitations section.** 4/4 reviewers asked for this;
  authors fixed it. Without this, the paper would have lost ≥1 score.
- **At least one theoretical guarantee mentioned in the main paper.**
  yfWn's specific request was "briefly describe the assumptions and
  state the main bound" — Appendix-only theory triggered objection.
- **Clear demarcation from immediate predecessors.** Both NexCP and
  Xu & Xie 2022 are cited as prior work; the rebuttal had to spell
  out what HopCPT does that they don't.
- **Operational justification for architectural choices.** "Why MHN
  specifically?" was Fgat's question; the three-reason rebuttal worked.
- **Evidence-of-evaluation-breadth language.** "Largest evaluation
  conducted for time series CP yet" was a deployed framing.

**For [[CTPC-KDD-Submission]] in the related-application subfield
(probabilistic forecasting for SDA):** the same demands apply. A
limitations section, a main-paper theorem reference (even if proof
deferred), explicit demarcation from Latent ODE / standard CP / GP
baselines, and operational justification for each architectural choice
(Cholesky-PSD, Student-t, RTN frame, NCDE) are non-optional.

---

## Patterns Worth Replicating in Rebuttal Strategy

1. **Acknowledge inheritance, name the delta.** "Our framework is
   indeed motivated by and built upon X and Y. However, our work adds
   significant additional contributions: [specific deltas]." This
   defuses the novelty-fallacy objection without antagonism.

2. **Numbered-reason structure for architecture choices.** When
   "why X specifically?" is asked, answer with (1) X enables A,
   (2) X enables B, (3) X enables C — each with a paper-section pointer
   for verification.

3. **Decompose contribution into idea + implementation.** If
   reviewers find "novelty marginal," propose splitting the contribution
   bullet into "conceptual idea" + "concrete implementation" parts —
   each defensible separately.

4. **Quantify empirical scope as a contribution.** "Largest evaluation
   conducted for X yet" is a defensible empirical claim if true.

5. **For each new request, point to the section that addresses it.**
   The HopCPT rebuttal always paired "we added X" with a specific
   appendix or section number.

6. **Volunteer the limitations section even before reviewers demand
   it.** Per [[HopCPT]] structural analysis: limitations as a
   mid-paper Methods subsection (§2.6), not buried in conclusions.

---

## Connection to SiS / CTPC

Direct rebuttal-strategy lessons for [[CTPC-KDD-Submission]]:

| HopCPT objection pattern | CTPC pre-emption strategy |
|---|---|
| "Limitations section missing" (4/4 reviewers) | Add explicit Limitations subsection in Methods. List ν_min=4.5 inheritance, RTN-frame scope, single-spacecraft-class evaluation, NASA CDDIS regime |
| "Theory only in appendix" (2/4) | State the d̄² → 1 calibration result and the structural Ḣ ≤ 0 claim in the main paper, with appendix pointer for proofs |
| "Why MHN specifically?" template question | Pre-write three-reason justifications for Cholesky-PSD, Student-t, RTN frame, NCDE substrate — each with operational deltas |
| "Combination of existing methods" novelty fallacy | Split contribution into "idea: CTPC framework" + "implementation: specific NCDE + Cholesky + Student-t composition" — defensible separately |
| Score-flipping rebuttal mechanics | Acknowledge inheritance from GMAT + Latent ODE + standard CP, then enumerate operational capabilities each lacks |

---

## Connections

- [[HopCPT]] — paired structural exemplar
- [[NeurIPS-2025-Reviewer-Guidelines]] — Q7 score-flip-criteria
  obligation is the formal version of what HopCPT rebuttal exploited
- [[ICML-2022-Reviewer-Tutorial]] — slide 17 novelty fallacy +
  slide 18 blank assertions verbatim apply to YCj6 + Fgat objections
- [[ICML-2022-Review-Form]] — soundness/presentation/contribution
  rubric is verbatim the one used here
- [[CTPC-KDD-Submission]] — direct pre-empt + rebuttal template target

---

## Open Questions

1. **Final rating after Fgat's score raise** is not numerically stated.
   "I raise the OVERALL score and make it my final response" — magnitude
   unspecified.
2. **Whether YCj6's 5→6 was the threshold for poster acceptance** —
   PC meta-review suggests yes; without it the score distribution would
   have been 7-5-6-6 with a borderline-reject anchor.

---

## Sources

- `raw/writing/reviews/HopCPT-OpenReview.pdf` — 7 pages, full OpenReview
  thread including PC decision, 4 official reviews, author rebuttal +
  response threads, and final reviewer comments.
- Submission #6410, NeurIPS 2023, accepted as poster.
