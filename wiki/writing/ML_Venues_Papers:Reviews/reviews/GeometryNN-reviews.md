---
title: GINN — OpenReview Reviews (ICML 2025, accepted poster)
tags: [writing, exemplar-reviews, openreview, icml-2025, geometric-dl, reviewer-interaction]
sources:
  - raw/writing/reviews/GeometryNN-OpenReview.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# GINN — OpenReview Reviews

**Paper:** Berzins, Radler, Volkmann, Sanokowski, Hochreiter, Brandstetter.
*Geometry-Informed Neural Networks*. ICML 2025. **Submission #10617.**
Paired with [[GeometryNN]] structural exemplar. **OpenReview URL fragment:**
`forum?id=o4KpjiCdrk`. **Application-Driven Machine Learning track.**

This page captures reviewer interaction patterns only, NOT research
content.

---

## Final Decision

**Accept (poster).** ICML 2025 Application-Driven ML track.

**PC decision letter (verbatim):**

> "This is a very promising work. It presents GINN, geometry-informed
> neural networks that can be trained with constraints instead of usual
> data. This is surprising and promising — the work should generate many
> follow-up efforts.
>
> There were concerns around evaluation, ablation, and effects on other
> constraints. The authors' rebuttal addressed many of these concerns
> satisfactorily. The authors are encouraged to include the additional
> details (about explicit constraints in the various cases) as
> supplemental. Authors already agreed to provide code.
>
> **The paper received 3WAs and one WR.** Given the potential of the work
> and the quality of results/evaluation, I am happy to recommend
> acceptance (as is the consensus among the reviewers). The reviews and
> rebuttals add a lot of value to the material included in the paper
> (e.g., updated timing, more ablation), and authors are encouraged to
> incorporate them in the final version."

Three operational signals:
1. **3 WA + 1 WR is acceptable at ICML 2025 application track.** Single
   holdout reject does not block acceptance.
2. **"Promising direction" framing outweighs evaluation completeness**
   for new-problem-space papers.
3. **PC explicitly references reviews-add-value-to-paper** — this is
   pre-OpenReview-public-record-era thinking.

---

## Score Distribution

ICML 2025 uses a **1-5 Overall Recommendation scale** (different from
NeurIPS 2025's 1-6):
- **5:** Strong accept
- **4:** Accept (i.e., good paper, accept)
- **3:** Weak accept (leaning towards accept, but could also be rejected)
- **2:** Weak reject (leaning towards reject, but could also be accepted)
- **1:** Strong reject

| Reviewer | Initial | Post-rebuttal | Pre-rebuttal stance | Notable signal |
|---|---|---|---|---|
| 8unU | 3 WA | **3 WA (unchanged)** | Concerned about generality, baselines, runtime | Generally positive; satisfied by rebuttal |
| 9JbR | 3 WA | **3 WA (unchanged)** | Concerned about smoothness, qualitative scope | "willing to champion this paper if needed" |
| **dCHc** | 2 WR | **2 WR (UNCHANGED)** | Sparse ablations, only 2/6 problems on core task, fragile system | **Holdout reject** — explicitly refused to raise score |
| q1uT | 3 WA | **3 WA (unchanged)** | Most positive; concerned about "fake problems" framing | Maintained position |

**Final aggregate:** 3, 3, 2, 3. **0 of 4 reviewers changed scores
post-rebuttal.** This contrasts sharply with [[HopCPT-reviews]] where 2/4
explicitly raised scores.

**Rebuttal acknowledgement count:** 4/4 reviewers wrote follow-up
comments. **0/4 explicit score lifts.** Three of four expressed
qualified satisfaction; dCHc explicitly refused.

---

## What the Strongest Reviewers (8unU + q1uT) Praised Specifically

### 8unU (Weak Accept) — three explicit strengths

1. **Novelty of paradigm:** "GINN is a new paradigm that does not
   requiring any training data at all. The pipeline learns neural
   implicit fields using only analytical objectives and constraints
   provided by the user. This approach draws inspiration from
   physics-informed neural nets, and is novel in the field of 3D
   generation."
2. **Diversity term mechanism:** "A major contribution is introducing an
   explicit diversity term in the optimization. By penalizing similarity
   among solutions, the framework yields diverse shapes that all meet
   the design criteria. This addresses mode collapse and is particularly
   valuable for design tasks where multiple viable solutions are
   desired."
3. **Fine-grained control:** "The method allows fine-grained control of
   shape properties through constraint formulation."

Also: **"The related work section is extremely well-written and
informative."** This is structurally significant — the unusually long
~25% Related Work that [[GeometryNN]] uses was directly praised by a
reviewer.

### q1uT (Weak Accept) — most-positive holistic praise

> "The experiment design is comprehensive with lots of qualitative and
> quantitative metrics provided to support the effectiveness of GINN.
> The ablation study in the supplement is also helpful in analyzing the
> effect of different losses and components of GINN. **Overall I found
> the experiments to be complete.**"

Note the direct contradiction: 8unU/9JbR/dCHc found experiments
insufficient; q1uT found them complete. **Reviewer-disagreement on
experimental scope was the deciding axis** for this paper's acceptance.

---

## What the Weakest Reviewer (dCHc, the Holdout WR) Objected to

dCHc gave the only Weak Reject (2). Four specific objections, all
maintained post-rebuttal:

1. **(Q1) Only 2/6 problems on core task:** "Although the authors propose
   six questions, **only two of them are related to the core task**
   (shape generation, problem 5 and problem 6). Although the experiment
   result looks convincing, just working on one or two problems does not
   show the effectiveness of the proposed method on a wide range of
   problems."

2. **(Q2) No convergence theory:** "There is **no theoretical guarantee
   on the convergence** of the proposed optimization process. The task
   itself can also be ill-defined from a theoretical perspective: how
   should we evaluate whether the generative model indeed recovers the
   desired distribution, or, what is the desired distribution from the
   first place?"

3. **(Q3) Sparse ablation studies:** "The ablation studies are also
   sparse. Only Fig 8. ablates the influence of different network choice
   for only one task. (Fig. 3 cannot be regarded as ablation study most
   of the time)"

4. **(Other) System fragility:** "The proposed system seems to be
   **fragile and hard-to-tune**. The authors use different network
   architectures and different training parameters for different tasks,
   which indicates the complexity to tune the optimization process."

**Post-rebuttal statement (verbatim):**
> "I agree with the authors and other reviewers that the problem and the
> solution are both interesting. **However I keep my opinion that only
> two use cases are not enough to fully support the soundness of the
> proposed method.** Given that this paper also does not have adequate
> theoretical contribution, I do expect extensive qualitative evaluations
> for this paper to meet the bar of ICML."

The "fragile and hard-to-tune" framing also **propagated to 9JbR**, who
explicitly cited it: "I agree with reviewer dCHc on a notion that 'the
proposed system seems to be fragile and hard-to-tune' but I think that
this is something to be studied in follow-up work."

---

## What Rebuttal Responses Changed Scores

**None.** All four reviewers maintained their initial scores. But
several rebuttal moves had qualitative effects:

### Move 1 — Topology-optimization baseline added (8unU response)

Authors added a previously-absent classical TO baseline comparison
(FeniTop SIMP method), with a 4-metric comparison table including
Connectedness, Interface, Design region, Curvature, Compliance. **8unU
explicitly accepted this as "at least partial" baseline.**

### Move 2 — simJEB dataset diversity comparison

Authors compared diversity metric against a **"human-expert dataset"**
(simJEB) — argued GINN achieves **diversity 0.167 vs human-experts'
0.099** — explicitly framed as "produce diversity on the same and larger
magnitude as a dataset that required an estimated collective effort of
14 expert human years." Strong framing but didn't flip a score.

### Move 3 — Runtime correction (significant error caught)

> "We thank the reviewer for highlighting the runtimes, which made us
> realize that we have reported an **outdated runtime** in the
> submission. The up-to-date runtimes on an A100-SXM GPU are 30 min for
> 10k iterations of a single-shape model and **4.5h for 50k iterations
> for the generative model, not 72h, as mistakenly reported in the
> submission.**"

A 16× runtime correction (72h → 4.5h) in the rebuttal. The PC's "updated
timing" mention in the decision letter likely refers to this. **An
acknowledged numerical error in the submission did not block
acceptance** when caught and corrected in rebuttal.

### Move 4 — Reframing problems as ablations (dCHc Q1 response)

> "We believe the first four problems do support the central theme by
> **isolating two key aspects: shape optimization with NN representations
> (Plateau and mirror) and training a generative model through a
> diversity constraint (physics and obstacle).** We invite the reviewer
> to view these tasks as ablations of solution multiplicity and shape
> representations."

This rhetorical reframing — "view our extra problems as ablations" — was
not accepted by dCHc, who maintained the Weak Reject.

### Move 5 — Constraint ablation table (9JbR response)

A detailed ms/iteration timing table across constraint ablations (None
260, Eikonal 291, Interface 264, etc.). Quantitative + structured — but
9JbR maintained 3 anyway, while explicitly noting "I am willing to
champion this paper if needed."

---

## Recurring Objections Across Multiple Reviewers

### 1. **EVALUATION SCOPE INSUFFICIENT — 4/4 reviewers** (unanimous)

The dominant objection, in different framings:
- **8unU:** "Limited Evaluation of Generality"
- **9JbR:** "almost all qualitative examples in the paper use one
  particular set of constraints"
- **dCHc:** "only two of them are related to the core task"
- **q1uT:** "experiments do seem like they are fake problems and do not
  have any significant practical applications"

**4/4 objection unanimous, paper still accepted.** This is the
operationally significant finding: at ICML 2025, even unanimous
evaluation-scope concerns do not block acceptance when the **new problem
space** framing is strong. The PC explicitly cited "the potential of the
work" as outweighing.

### 2. **NO THEORETICAL GUARANTEES — 2/4 reviewers**

- **dCHc:** "No theoretical guarantee on the convergence."
- **q1uT:** indirectly via "I did not go into details into the
  derivations presented in the supplement."

Authors deflected: "This is the case for most adjacent methods in
machine learning, physics-informed learning, and topology optimization."
The deflection was sufficient for q1uT (kept at 3) but NOT for dCHc
(kept at 2).

### 3. **FRAGILE / HARD-TO-TUNE SYSTEM — 2/4 reviewers, cross-propagating**

- **dCHc:** originated the objection.
- **9JbR:** explicitly adopted the framing, citing dCHc by name.

**This is a notable reviewer-interaction pattern:** strongly-worded
critique by one reviewer can propagate to another reviewer's score
justification. The "fragile" framing became sticky.

### 4. **LACK OF BASELINE COMPARISONS — 1.5/4 reviewers**

- **8unU:** strong explicit critique. Author response added FeniTop +
  simJEB. 8unU accepted these as "at least partial" baselines.
- **q1uT:** indirect via "fake problems" concern.

This was the most directly fixable objection — rebuttal addition of
classical TO baseline visibly worked even without score lift.

### 5. **COMPUTATIONAL COST — 1/4 reviewers strongly**

- **8unU:** "Training GINNs appears computationally expensive and
  potentially impractical."
- Authors corrected 72h → 4.5h. **Submission had a wrong number.**

---

## Desk-Reject Signals for This Subfield (Generative Geometric Modeling at ICML)

No desk-reject occurred — paper survived Phase 1 despite 1 WR. The
review record reveals:

**Tolerated:**
- **Single Weak Reject vote among 4 reviewers.** PC accepted 3WA+1WR.
- **Unanimous evaluation-scope concerns.** "Limited tasks" objection
  from all four did NOT block.
- **No convergence guarantees.** Deflected as "standard for the field."
- **Submitted-paper numerical errors** (16× runtime mis-report) if
  caught and corrected in rebuttal.
- **Self-defined metrics** since "no established baselines exist."
- **"Fake problems" framing** in q1uT's question — paper still got that
  reviewer's WA.

**Demanded:**
- **At least one classical-baseline comparison** OR a strong
  justification for absence. Authors initially had neither; rebuttal
  added FeniTop TO comparison.
- **A diversity-or-similar metric.** simJEB comparison was the
  strongest single rebuttal addition.
- **Application-domain framing.** This paper succeeded in the
  Application-Driven ML track explicitly; without that framing the same
  paper at a methodology track might face higher novelty bars.
- **At least 3 of 4 reviewers above the borderline.** A 2-2-3-3 score
  would likely have been rejected; the 3-3-3 majority was load-bearing.

**For SiS [[CTPC-KDD-Submission]] (application-driven SDA + UQ subfield):**
- The "new problem space" framing is available — SDA × NCDE × heavy-tail
  UQ is genuinely under-explored. But CTPC has real comparators (Latent
  ODE, GP, standard CP for orbital prediction). Don't claim "unexplored
  field" when comparators exist.
- A single holdout WR is survivable at NeurIPS/KDD-class venues with
  3WA+1WR.
- Numerical errors in submission can be corrected in rebuttal without
  fatal consequences (per the 72h → 4.5h precedent).

---

## Patterns Worth Replicating in Rebuttal Strategy

1. **Add at least one classical baseline late** (8unU FeniTop comparison
   move) — even an imperfect comparison is better than none. Frame as
   "implemented at least partially comparable baselines."
2. **Self-defined diversity-vs-expert-effort framing** ("equivalent to
   14 expert human years") — quantifies novelty in human-cost terms.
3. **Acknowledge numerical errors transparently in rebuttal.** "We
   realize we have reported an outdated runtime…" defuses the
   honest-error-vs-incompetence concern.
4. **Reframe extra experiments as ablations** when reviewer says "too
   few problems." Worked partially (1 reviewer accepted, dCHc didn't).
5. **Champion-language exploits ("willing to champion this paper if
   needed").** When even one reviewer adopts this stance, it shifts the
   PC's framing.
6. **Strategic deflection of theory critiques** by appealing to field
   norms ("this is the case for most adjacent methods"). Worked on
   q1uT, failed on dCHc.
7. **Pre-empt cross-propagation of negative framing.** dCHc's "fragile
   and hard-to-tune" wording traveled to 9JbR's review. Single negative
   framings can propagate; counter them strongly early.

---

## Connection to SiS / CTPC

Direct rebuttal-strategy lessons for [[CTPC-KDD-Submission]]:

| GINN objection pattern | CTPC pre-emption strategy |
|---|---|
| "Only 2 of 6 problems on core task" (4/4 reviewers) | Pre-empt by clearly marking which experiments are core (multi-day NASA CDDIS forecasting) and which are ablations (synthetic / single-satellite) |
| "No convergence theory" (2/4) | State the Ḣ ≤ 0 structural guarantee in main paper; cite Khalil Ch 6 passivity for the dissipative-stability backbone |
| "Fragile and hard-to-tune" (cross-propagated 1→2 reviewers) | Pre-empt by stating that CTPC uses ONE hyperparameter setup across all NASA CDDIS satellites — don't tune per-satellite |
| "Lack of baselines" (1.5/4) | Always include Latent ODE + standard CP + GP baselines — never claim "unexplored field" when comparators exist |
| Reviewer-disagreement on experimental sufficiency | Aim for 4/4 above borderline; 2-2-3-3 fails, 3-3-3-3 borderline succeeds, 3-3-3-2 succeeds, 4-3-3-2 is safe |
| Numerical errors in submission | If discovered mid-review-period, correct in rebuttal with explicit acknowledgment ("we realize we have reported…") |

---

## Connections

- [[GeometryNN]] — paired structural exemplar
- [[HopCPT-reviews]] — contrast: HopCPT had 2/4 score lifts; GINN had
  0/4. Both accepted with similar final aggregates
- [[NeurIPS-2025-Reviewer-Guidelines]] — 1-6 Overall scale vs ICML
  2025's 1-5 scale; calibration differs
- [[ICML-2022-Reviewer-Tutorial]] — slide 19 "No such policy" applies
  to dCHc's "only two use cases" objection (no policy requires N use
  cases); but reviewer training preceded ICML 2025 so authors didn't
  cite it
- [[ICML-2022-Review-Form]] — soundness/presentation/contribution
  3-axis-rubric appears here too (8unU: 3/4/3 implied)
- [[CTPC-KDD-Submission]] — direct pre-empt + rebuttal template target

---

## Open Questions

1. **No score lifts at all** despite extensive rebuttal effort — what
   would have flipped dCHc? Possibly only an additional core-task
   experiment, which the paper deferred to follow-up work.
2. **Champion-language operational mechanics:** 9JbR said "willing to
   champion this paper if needed" — did the AC/SAC invoke this? Not
   visible in the OpenReview thread.
3. **Confidence scores** are not present in this ICML 2025 review form
   (only ratings and rubric axes). Different from NeurIPS 2025's
   explicit confidence axis.

---

## Sources

- `raw/writing/reviews/GeometryNN-OpenReview.pdf` — 8 pages, full
  OpenReview thread including PC decision, 4 official reviews + post-
  rebuttal comments, and 4 author rebuttals.
- Submission #10617, ICML 2025 Application-Driven ML track, accepted as
  poster.
