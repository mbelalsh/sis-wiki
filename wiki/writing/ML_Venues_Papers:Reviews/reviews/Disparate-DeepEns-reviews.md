---
title: Disparate-DeepEns — OpenReview Reviews (ICML 2025, accepted poster)
tags: [writing, exemplar-reviews, openreview, icml-2025, fairness, reviewer-interaction, cross-reviewer-influence]
sources:
  - raw/writing/reviews/Disparate-DeepEns-OpenReview.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Disparate Benefits of Deep Ensembles — OpenReview Reviews

**Paper:** Schweighofer, Arnaiz-Rodriguez, Hochreiter, Oliver. *The
Disparate Benefits of Deep Ensembles*. ICML 2025. **Submission #15371.**
Paired with [[Disparate-DeepEns]] structural exemplar. **OpenReview URL
fragment:** `forum?id=tjPxZiqeHB`. **Social Aspects → Fairness track.**

This page captures reviewer interaction patterns only, NOT research
content.

**Operationally most important pattern in this review thread:**
**cross-reviewer influence in the NEGATIVE direction** — Reviewer rDWq
explicitly lowered their score after reading another reviewer's critical
review. This is a failure mode authors must pre-empt.

---

## Final Decision

**Accept (poster).** ICML 2025 Social Aspects (Fairness) track.

**PC decision letter (verbatim):**

> "The paper presents an empirical study about the fairness impacts of
> deep ensembles. Experimental results show that deep ensembles unevenly
> helps boost performance of groups, which furthers the parity in
> prediction accuracy between groups. The authors explain this may be
> linked to predictive diversity for each group. **The paper received
> mixed ratings (1,2,3,4).** While the reviewers appreciated the
> empirical findings and potential underlying causes, **they also had
> concerns about the validity of the claim and questioned whether the
> provided evidence can be tightly connected to the main argument.**
> While the concerns exist, the paper's findings can provide a new
> insight about the property of deep ensembles."

Three operational signals from this meta-review:

1. **A 1-2-3-4 score distribution accepted at ICML 2025.** Mean ~2.5.
   Phenomenon-naming + empirical novelty overrode the validity concerns.
2. **Validity-of-claim concerns explicitly acknowledged** in the decision
   letter as outstanding — but did not block.
3. **"New insight" framing** carried the paper. The named phenomenon
   ("disparate benefits effect") was the load-bearing rhetorical device.

---

## Score Distribution

ICML 2025 uses a **1-5 Overall Recommendation scale**.

| Reviewer | Initial | Post-rebuttal | Direction | Key signal |
|---|---|---|---|---|
| iGve | **4 Accept** | 4 (unchanged) | — | Most positive; "Excellent" related work; deep engagement |
| zbgH | 3 Weak Accept | 3 (unchanged) | — | Vision-only concern; HPP not novel |
| **rDWq** | 3 Weak Accept | **2 Weak Reject** | **DOWN ↓1** | **Lowered explicitly after reading mL9u's review** |
| mL9u | **1 Reject** | 1 (unchanged) | — | Causality gap; "could be standard fairness/accuracy tradeoff" |

**Final aggregate:** 4, 3, 2, 1. **One reviewer changed score — in the
negative direction.** This is the **inverse** of [[HopCPT-reviews]]
(where 2 reviewers RAISED scores) and unique among this exemplar
corpus.

**Rebuttal acknowledgement count:** 4/4 reviewers wrote rebuttals.
**0/4 explicit score raises.** 1/4 explicit score lowering (rDWq).

---

## What the Strongest Reviewer (iGve, Accept) Praised Specifically

> "This is an overall **well-written research paper** that focuses on
> the behaviour of fairness notions in the context of fairness
> violations. The results are **abundantly communicated through the
> figures and also discussed in the main body of the paper.**"

> "The overall **experimental design is sound** with many variables and
> several random seeds to account for variance in the results. The
> analyses are elaborate enough when also accounting for the results
> provided in the appendix. … The **controlled experiment is a nice
> touch** to validate the authors' suspicion with regard to the role
> that predictive diversity plays."

Three structural patterns iGve explicitly praised:
1. **Abundant figure communication.**
2. **Many variables + multiple seeds** (the 1,000-model experimental
   scope).
3. **The controlled synthetic experiment** as validation device for
   the hypothesis.

The **single weakest objection from iGve:** "The paper obscures quite
some information with regards to fairness by only considering binary
sensitive attributes (numerical and categorical attributes are mapped
to be binary). This limits the scope of the analysis significantly and
**this choice could be communicated at an earlier stage of the paper.**"

This is a **clarity-of-scope** objection, not a substantive one. iGve
maintained the 4 Accept.

---

## What the Weakest Reviewer (mL9u, Reject) Objected to

mL9u's review is the **most operationally instructive in this batch**
because it produced cross-reviewer score propagation. Verbatim core
critique:

> "The central thesis of this paper is that deep ensembles 'unevenly
> favor different groups', which the authors call the disparate
> benefits effect. The key evidence relies on demonstrating that as
> models are added to an ensemble, we see accuracy rise and fairness
> get worse.
>
> **I am not sure I am convinced of this thesis by the arguments in
> this paper. In particular, I don't think the authors dispel the
> possibility that this is simply a property of improving model (read:
> ensemble) accuracy. We frequently observe such a fairness/accuracy
> tradeoff across many types of models**, in particular if group base
> rates differ (in this case, we must observe such a tradeoff for a
> metric like SPD)."

Four enumerated sub-objections:

1. **Average-predictive-diversity argument doesn't connect to SPD:**
   "the argument here is that if diversity differs between groups then
   the benefits of ensembling would mostly accrue to that group.
   However, this seems to lead more naturally to an argument about
   subgroup accuracy than equalized odds type metrics."
2. **Synthetic example doesn't isolate the effect:** "Fig 4: this
   synthetic example does not necessarily demonstrate the desired
   pattern - it just constructs a case where there is differing DIV
   and disparate benefits. To show the pattern, one would need to
   also construct the counterfactual faces where there is equal DIV
   and equal benefits."
3. **Single model per pattern figure:** "Fig 3 does demonstrate the
   desired pattern, however it is just observed for a single model
   and I would want to see this pattern shown more systematically."
4. **Hardt post-processing is not ensemble-specific:** "The fact
   that Hardt post-processing is successful is also concordant with
   the view that ensembles are just like any other classifier."

**This is a causality-gap + claim-uniqueness critique** — the
strongest possible substantive objection to a characterization paper.
Methodology objections (sample size, etc.) are fixable; the "is this
effect actually unique?" objection requires demonstrating the
isolation of the named phenomenon from confounders.

---

## Cross-Reviewer Influence — The Most Important Pattern in This Thread

**Reviewer rDWq's verbatim score-change statement:**

> "Update after rebuttal
>
> **I found the arguments risen by reviewer mL9u very reasonable and
> compelling. I hadn't seen it from that viewpoint.** I still think
> that the paper makes interesting points, **but will adjust my scoring
> below the acceptance threshold.**"

rDWq moved from **3 Weak Accept → 2 Weak Reject** after reading
mL9u's review. The mechanism:
- rDWq's initial review had similar concerns (sample size, correlation-
  not-causation).
- mL9u articulated the causality-gap critique more forcefully.
- rDWq found the framing compelling and adopted it post-hoc.

**This is the inverse of the pattern in [[GeometryNN-reviews]]:** there,
dCHc's "fragile and hard-to-tune" framing propagated to 9JbR who
endorsed it without changing score. Here, mL9u's "could be standard
fairness/accuracy tradeoff" framing propagated to rDWq AND triggered a
score lowering.

**Implication for authors:** if any single reviewer raises a
fundamental conceptual objection that other reviewers might adopt
post-hoc, the rebuttal must address it forcefully and EARLY before the
other reviewers re-read their own positions. The rebuttal to mL9u was
substantial but did not arrive in time to prevent rDWq from being
influenced.

---

## What Rebuttal Responses Changed Scores

**None positively. One negatively (rDWq).**

But the rebuttal moves themselves were structurally interesting:

### Move 1 — Synthetic-experiment with controlled base rates (mL9u response)

Authors pointed to Appendix F.1 — a synthetic experiment where:
- Base rates set equal (0.25 / 0.25 / 0.25 / 0.25) across
  Y={0,1} × A={0,1}
- Predictive diversity systematically varied via parameter α
- Disparate benefits effect emerges as α increases; vanishes at α=0

**This is the strongest causality-isolation move possible.** The
synthetic experiment SHOULD have settled mL9u's objection. mL9u's
response: "I do find the calibration threshold finding to be
interesting. I think that my asks are fairly substantial and so this
still won't be an accept from me - however, I'll consider changing
my score in discussion with other reviewers." **No score change.**

### Move 2 — Additional base-rate-variation table (post-rebuttal-comment response)

After mL9u's initial response, authors generated an ENTIRELY NEW data
table showing the disparate-benefits effect across 4 different base-
rate configurations × 4 α values (16 cells total). Table verbatim
showed predictive diversity drives the effect, NOT base-rate imbalance.

**Verbatim authors:** "We will include this analysis in the final
version of the paper to further reinforce our argument."

This was a HEROIC rebuttal effort — generating new experiments
mid-review-period. Did not flip mL9u but may have contributed to PC
acceptance decision.

### Move 3 — Shapiro-Wilk test for t-test assumption (rDWq response)

In response to rDWq's t-test-validity concern, authors performed a
Shapiro-Wilk normality test on each of 16 task/metric combinations.
**Found one case rejecting normality (CX SPD age, p<0.05).** Offered
to make that entry non-bold in Table 1.

**Authors explicitly acknowledged a statistical-significance issue
discovered during rebuttal** — same pattern as [[GeometryNN-reviews]]
72h → 4.5h runtime correction. Honest-error-acknowledgment did not
prevent rDWq's score drop, however.

### Move 4 — Promise to move appendix experiment to main paper

> "We fully agree that the experiment in Appendix F.1 is central to
> the paper. It was the last section we moved to the appendix to fit
> within the 8-page limit for the submission. **Upon acceptance, we
> will use the extra page to bring this experiment back into the main
> paper**, as we believe it strengthens the core argument."

Conditional-promise rebuttal move. Did not change scores but
demonstrated good-faith engagement.

---

## Recurring Objections Across Multiple Reviewers

### 1. **CAUSALITY GAP / "COULD BE STANDARD TRADEOFF" — 2/4 reviewers (mL9u + post-rebuttal rDWq)**

The propagated critique. mL9u originated, rDWq adopted, with explicit
attribution. **This is the operationally most important pattern in
this review thread.**

### 2. **VISION-ONLY DATASETS / NARROW GENERALIZABILITY — 2/4 reviewers (zbgH + iGve)**

Both reviewers flagged restriction to image data. zbgH framed it as
limited applicability; iGve framed it as scope-clarity. Both kept
their scores.

### 3. **NO NOVEL FAIRNESS ALGORITHM — 1/4 reviewers (zbgH)**

zbgH: "the paper does not introduce a novel algorithm for this
purpose. Comparing more diverse fairness algorithms would enhance the
paper." This objection was deflected by authors as "not our objective"
— characterization-paper framing.

### 4. **STATISTICAL POWER (5 SEEDS, t-TEST) — 1/4 reviewers (rDWq)**

rDWq: "you have only five ensembles (seeds) for each setup; I doubt
that the t-test is a proper way of measuring statistical significance
here." Authors addressed with Shapiro-Wilk test.

### 5. **EFFECT-SIZE TRIVIALITY — 1/4 reviewers (rDWq)**

rDWq: "the highest difference is only 0.022, not particularly high."
Authors deflected by reframing as relative-effect-size (10% relative
SPD increase).

### 6. **DISPARATE-BENEFITS DEFINITION NOT FORMALIZED — 1/4 reviewers (rDWq)**

rDWq: "The paper coins the term 'disparate benefits effect' but fails
to properly introduce and maybe formalize it. I searched with Ctrl + F
and found that **curiously the abstract (not the introduction) gives
the best intuition.**" Authors agreed and committed to revision.

**This is a subtle but operationally significant critique:** the
phenomenon-naming exemplified by [[Disparate-DeepEns]] (a new term in
the abstract) requires a formal definition in the body — not just an
intuitive description spread across §5.

---

## Desk-Reject Signals for This Subfield (Algorithmic Fairness at ICML)

Paper accepted despite **the worst score distribution in this exemplar
corpus** (4-3-2-1 with a 1 Reject and a converted-to-Weak-Reject). The
review record reveals:

**Tolerated:**
- **A single Reject (1) vote.** PC accepted despite outright reject.
- **Cross-reviewer-influence-driven score lowering** (rDWq's drop).
- **Causality gap not fully resolved** despite extensive rebuttal.
- **Vision-only experimental scope** (matches [[GeometryNN-reviews]]).
- **Sample size of 5 ensembles per task.**
- **Late-discovered statistical violations** (Shapiro-Wilk failed for
  one task).
- **Definition of phenomenon found mainly in abstract, not body.**

**Demanded:**
- **Phenomenon-naming.** "Disparate benefits effect" carried the paper
  per PC: "the paper's findings can provide a new insight about the
  property of deep ensembles."
- **At least one strongly-positive reviewer.** iGve's 4 Accept was
  load-bearing. Without iGve, the paper likely fails (3-2-1 mean).
- **Substantive rebuttal effort.** Authors generated new tables
  mid-review-period (the 16-cell base-rate × α table) — visible work
  signals quality to PC even when reviewers don't update.
- **Honest acknowledgment of statistical issues** when caught.

**For SiS [[CTPC-KDD-Submission]]:** the 4-3-2-1 ICML 2025 acceptance
is encouraging — a single strong reject does not necessarily block.
But the cross-reviewer-influence pattern is a clear failure mode to
pre-empt.

---

## Patterns Worth Replicating (and Avoiding) in Rebuttal Strategy

### Patterns to replicate

1. **Generate new tables in rebuttal** (the 16-cell base-rate × α
   table). Demonstrates engagement; PC notices even when reviewers
   don't update.
2. **Promise conditional revisions** ("upon acceptance, we will use
   the extra page to bring this experiment back into the main paper").
3. **Acknowledge statistical-test violations transparently**
   (Shapiro-Wilk result with non-bold proposal).
4. **Deflect "no novel algorithm" objections** by reframing as
   characterization-paper scope ("this was not our objective").
5. **Pre-empt the formalization objection** — if naming a new
   phenomenon, define it formally in §1 or §3, not just intuitively
   in the abstract.

### Patterns to AVOID (failure modes)

1. **Letting strong skeptical reviews go un-addressed early.** mL9u's
   critique propagated to rDWq before the rebuttal could land.
2. **Hiding the load-bearing experiment in the appendix to meet page
   limits.** Authors admitted Appendix F.1 was "the last section we
   moved to the appendix to fit within the 8-page limit" — and it was
   the experiment that should have settled mL9u's central objection.
   **Reviewer-load-bearing content should be in the main paper.**
3. **Defining a new phenomenon only in the abstract.** rDWq's
   "Ctrl+F" complaint is precisely the failure mode.
4. **Effect sizes that look trivial in absolute terms.** Frame
   relatively (10% relative increase) BEFORE the reviewer raises the
   triviality objection.

---

## Connection to SiS / CTPC

Direct rebuttal-strategy lessons for [[CTPC-KDD-Submission]]:

| Disparate-DeepEns objection pattern | CTPC pre-emption strategy |
|---|---|
| "Could be standard fairness/accuracy tradeoff" (mL9u → propagated to rDWq) | Pre-empt by isolating CTPC's unique mechanism (Cholesky-PSD + Student-t + dissipation) from generic UQ improvements |
| Cross-reviewer-influence in negative direction | Pre-empt strongest possible objection in introduction, not in rebuttal. "Why isn't CTPC just a fancier Latent ODE?" — answer in §1, not §5 |
| "Definition mainly in abstract, body lacks formal definition" | Define the CTPC framework formally in §3 Background, not just intuitively in §1 |
| "5 seeds, sample-size concerns" | Pre-empt by running 10+ seeds OR explicitly justifying the sample-size choice with a power analysis |
| "Effect size only 0.022, not particularly high" | Frame as relative effect (e.g., "64% MSE reduction" vs absolute MSE delta) |
| "Load-bearing experiment in appendix" | Keep CTPC's calibration-verification experiment (d̄² ≈ 1) in main paper, never appendix |
| 1-Reject doesn't block acceptance at ICML 2025 | Encouraging baseline but not a strategy — aim for 3-3-3-3 minimum |

---

## Connections

- [[Disparate-DeepEns]] — paired structural exemplar
- [[HopCPT-reviews]] — contrast: HopCPT had +2 score lifts; this had
  -1. Cross-reviewer-influence in opposite direction
- [[GeometryNN-reviews]] — contrast: similar cross-reviewer-influence
  pattern (dCHc's "fragile" → 9JbR), but Disparate-DeepEns version
  triggered explicit score change
- [[NeurIPS-2025-Reviewer-Guidelines]] — Q2.1 Quality "claims well
  supported by theoretical analysis or experimental results" is
  precisely the axis mL9u disputed
- [[ICML-2022-Reviewer-Tutorial]] — slide 20 Intellectual Laziness
  applies to rDWq's effect-size critique (overemphasis on one factor)
- [[CTPC-KDD-Submission]] — direct pre-empt + rebuttal template target

---

## Open Questions

1. **Why did the PC accept despite 1-2-3-4?** The "new insight"
   framing is doing heavy lifting in the decision letter. Worth
   tracking whether other phenomenon-naming papers have similar
   threshold tolerance at ICML.
2. **rDWq's pre-mL9u 3-Weak-Accept** — what would have prevented the
   conversion? Possibly: a §1 explicit pre-emption of the
   fairness-accuracy-tradeoff alternative explanation.
3. **No score raises despite extensive rebuttal effort** —
   characterization papers may have a structural ceiling on how much
   rebuttals can raise scores once the central thesis is doubted.

---

## Sources

- `raw/writing/reviews/Disparate-DeepEns-OpenReview.pdf` — 8 pages,
  full OpenReview thread including PC decision, 4 official reviews +
  post-rebuttal comments, 4 author rebuttals, 1 author reply-rebuttal.
- Submission #15371, ICML 2025 Social Aspects (Fairness) track,
  accepted as poster.
