---
title: LiePointSymmetryPINN — OpenReview Reviews (NeurIPS 2023, accepted poster)
tags: [writing, exemplar-reviews, openreview, neurips-2023, pinn, lie-symmetry, reviewer-interaction, pc-intervention]
sources:
  - raw/writing/reviews/LiePointSymmetryPINN-OpenReview.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# LiePointSymmetryPINN — OpenReview Reviews

**Paper:** Akhound-Sadegh, Perreault-Levasseur, Brandstetter, Welling,
Ravanbakhsh. *Lie Point Symmetry and Physics-Informed Networks*. NeurIPS
2023. **Submission #14815.** Paired with [[LiePointSymmetryPINN]]
structural exemplar. **OpenReview URL fragment:** `forum?id=ba4boN3W1n`.

This page captures reviewer interaction patterns only, NOT research
content.

**Operationally most important pattern:** **Program Chair intervention
explicitly defending novelty** against the strongest reviewer's
novelty-fallacy critique. The PC decision letter contains a verbatim
counter-argument to one reviewer's objection.

---

## Final Decision

**Accept (poster).** NeurIPS 2023 main track.

**PC decision letter (verbatim):**

> "During the initial phase of reviewing, the submission received
> mixed scores. Some of the concerns raised related to the clarity of
> presentation, which have been addressed by the rebuttal. **One of the
> referees comments on the novelty of the work as the symmetries are
> already known. However, the incorporation of the symmetries in the
> training of PINNs is a sufficiently novel contribution.** There still
> remain concerns about limited experimental validation. The submission
> would indeed be strengthened significantly by taking into account the
> suggestions of the referees. However, there is sufficient merit in the
> submission to be accepted."

Three operational signals from this meta-review:

1. **PC explicitly defended novelty in writing** against the Reviewer
   dief novelty-fallacy critique. This is a rare and operationally
   significant move — the PC took a side in a substantive review
   disagreement rather than just summarizing.
2. **5-reviewer thread accepted despite distribution including a
   Reject (3).** Even with a strongly negative reviewer, paper
   accepted.
3. **Limited experimental validation explicitly acknowledged** in the
   decision letter but did not block.

---

## Score Distribution (5 Reviewers — Larger Than Typical)

NeurIPS 2023 uses the **1-10 Overall Rating scale**:
- 10: Award quality
- 9: Strong Accept
- 8: Top 50% of accepted papers
- 7: Accept
- 6: Weak Accept
- 5: Marginally below
- 4: Borderline Reject
- 3: Reject
- 2: Strong Reject
- 1: Trivial / wrong

| Reviewer | Initial | Post-rebuttal | Direction | Soundness/Presentation/Contribution |
|---|---|---|---|---|
| vAL8 | 6 Weak Accept | 6 (unchanged) | — | 3/2/3 |
| qo6e | 4 Borderline Reject | 4 (unchanged) | — | 3/2/2 |
| **dief** | **3 Reject** | **3 (unchanged)** | — | 3/**1 poor**/2 |
| 5AKh | 6 Weak Accept | **~7 (raised)** | **+1 UP** | 3/3/2 |
| KJvK | 6 Weak Accept | 6 (unchanged) | — | 3/2/3 |

**Final aggregate:** 6, 4, 3, 7, 6. Median ~6. One score raise, one
strong reject that didn't move.

**Rebuttal acknowledgement count:** 5/5 reviewers wrote follow-up
comments. 1/5 explicit score lift (5AKh: "Although not completely, to
some extent, my concerns have been addressed and I have increased the
score").

---

## What Reviewers Praised Specifically

### vAL8 (6 Weak Accept) — four explicit strengths

1. **"The idea of adding symmetry constraints to neural networks PDE
   solvers is a very interesting contribution to the field."**
2. **"The execution is novel and makes clever use of Lie algebra
   theory on PDEs."**
3. **"The exact form of the loss function is clearly motivated."**
4. **"The experimental results are compelling and convincing."**

### 5AKh (6 → 7) — uniqueness-of-mechanism praise

> "As far as I know, the use of the symmetry of the model as an
> infinitesimal generator, **rather than in the form of conservation
> laws, is certainly new.** This enables the use of symmetry for
> equations that are not derived from the variational principle."

This is operationally significant: **5AKh credited the paper for
mechanism-novelty, not just application-novelty.** "Infinitesimal-
generator-based" vs "conservation-law-based" is a specific technical
claim defending the contribution against the "Olver 1986 already did
this" objection.

### KJvK (6) — concise insight

> "PINN loss comes from the equation, but the proposed loss comes from
> **the property of the equation. The idea is insightful.**"

A one-sentence positive framing — the contrast between "equation" vs
"property of the equation" captures the contribution in 16 words.
Citation-friendly framing.

---

## What the Weakest Reviewer (dief, Reject) Objected to

**dief's Reject (3) is the operationally most interesting negative
review in this corpus.** It hits the textbook **novelty fallacy +
intellectual laziness** pattern from [[ICML-2022-Reviewer-Tutorial]]
slides 17 + 20.

### Five enumerated weaknesses

1. **Novelty fallacy (paradigmatic):** "**The method is a
   straightforward implementation of Olver (1986) into PINN**, and
   empirical results showed no significance or only marginal
   improvement."
2. **Effect not unique to method:** "the positive effect of the
   symmetries is well-known to the community (e.g., Wang et al.
   2021a)."
3. **Missing baselines:** "the authors include data augmentation
   methods (e.g., Brandstetter et al. 2022a) and equivariant models
   (e.g., Wang et al. 2021a)."
4. **Confusion about simulation data:** "It makes the reviewer
   confused about whether the method uses simulation data as
   supervisory signals."
5. **Mathematical correctness:** "The mathematical presentation lacks
   correctness and clarity. The reviewer found no clear definition of
   'Lie point symmetry.'"

### Post-rebuttal stance (verbatim, score unchanged)

> "The reviewer appreciates the responses made by the authors. Now it
> is clear that the method uses no training data and incorporates
> symmetries behind PDE, which is new. **However, the contribution of
> the paper does not seem sufficient to be accepted at the conference
> in terms of methodology and experimental significance. The method is
> a straightforward implementation of Olver (1986) into PINN, and
> empirical results showed no significance or only marginal
> improvement.** Therefore, the reviewer keeps the score unchanged."

**dief acknowledged the rebuttal's clarifications but did not change
the score.** This is the canonical pattern of an unconvincable
novelty-fallacy review — the reviewer treats "X already exists in
field Y" as sufficient grounds for rejection regardless of the
domain-specific contribution.

---

## What Rebuttal Responses Changed Scores

### 5AKh: **+1 score raise** triggered by

**A new experiment showing performance scales with number of
symmetries used in the symmetry loss.** Verbatim authors' rebuttal:

> "Our hypothesis is that symmetry loss will lead to more improvement
> for a system with many symmetries. We have added an experiment (see
> the attached PDF) where we track the improvement due to symmetry loss
> as we use more symmetries for the heat equation. **This experiment
> shows how increasing the number of infinitesimal generators used in
> calculating the symmetry loss leads to improved performance.**"

5AKh response:
> "Thank you for the experimental results on the relationship between
> the number of symmetries and the performance of the method. Based on
> these results, it seems that the proposed method is **very effective
> for equations with a large number of symmetries and conservation
> laws, such as integrable PDEs.**"

5AKh then asked for one more experiment (saturation behavior with more
symmetries). Authors deflected with PINN-limitation explanation. 5AKh:

> "Although not completely, to some extent, my concerns have been
> addressed and **I have increased the score.**"

**Lesson:** new experiments generated mid-rebuttal can flip a score
when targeted at a specific reviewer's hypothesis. The author's
hypothesis-test framing ("Our hypothesis is that…") put 5AKh's
intellectual curiosity in alignment with the result.

### dief, qo6e, vAL8, KJvK: no score changes despite engagement

- **dief** was unconvincable on novelty grounds.
- **qo6e** said "due to the limited numerical experiments, I will keep
  my rating." Substantive engagement but unmoved by clarifications.
- **vAL8** engaged in a detailed follow-up about ablations,
  presentation, notation. Authors responded with specific
  hyperparameter values and acknowledgments. **Score unchanged.**
- **KJvK** asked for computational-cost-vs-performance curves AND a
  demonstration of automatic symmetry derivation. Authors did not
  provide either. **KJvK: "if you could demonstrate your method with
  such a procedure, I would give it a better score."** Score unchanged.

---

## Recurring Objections Across Multiple Reviewers

### 1. **LIMITED EXPERIMENTAL VALIDATION (1D PDEs only) — 4/5 reviewers**

- **qo6e:** "experiments are only showcased on 1D PDEs"
- **dief:** "results from the symmetry model are not good enough"
- **KJvK:** "only evaluated with the heat equation and Burgers'
  equation, which are very simple PDEs"
- **vAL8:** "I am unable to fully appreciate the performance to the
  method"

Authors deflected with PINN-limitation framing: "current PINN models
that can take as input different initial conditions fail for higher
dimensions… Given these limitations, it isn't feasible to showcase the
performance of the proposed algorithm for high-dimensional or complex
systems."

**4/5 reviewers raised this; PC accepted anyway.** Same pattern as
[[GeometryNN-reviews]] — unanimous evaluation-scope concern can be
overridden by "promising direction" framing.

### 2. **NOVELTY FALLACY (Olver 1986 already did this) — primarily dief, with PC-level pushback**

dief's strongest objection. **The PC explicitly defended the paper
against this in writing** — see PC decision letter. This is the
operationally important pattern: when one reviewer hits novelty
fallacy hard, the PC may intervene if the contribution is genuinely
domain-specific.

### 3. **"DENSER SAMPLING ACHIEVES THE SAME EFFECT" — qo6e + KJvK + own admission**

- **qo6e:** "better performance obtained with the additional loss
  terms could have also been achieved by denser sampling"
- **KJvK:** "with the same computational budget, one can increase the
  number of evaluation points instead of introducing the proposed
  loss. **The comparison might not be fair.**"
- **Paper's own §5 Limitations #3:** "while symmetries can
  significantly improve performance… one could achieve a similar
  effect with PINN by increasing the sample size."

**The voluntary concession in §5 did NOT defuse the objection.**
qo6e and KJvK kept the concern in their reviews. Lesson: voluntary
admission of substitutability is not sufficient — authors need to also
provide a computational-cost-vs-performance comparison to make the
"with same compute budget" case.

### 4. **MATHEMATICAL CLARITY / NOTATION — 3/5 reviewers**

- **dief:** "mathematical presentation lacks correctness and clarity"
  (Presentation: 1 poor)
- **vAL8:** "mathematical notations of the paper are not clear"
  (Presentation: 2 fair)
- **KJvK:** "symbols are not unified" (Presentation: 2 fair)

Authors acknowledged and committed to revision. **Presentation score
of 1 (poor) from dief did NOT block acceptance** — soundness/contribution
weights more than presentation in NeurIPS aggregation.

### 5. **AUTOMATING SYMMETRY DERIVATION — 3/5 reviewers**

- qo6e, 5AKh, KJvK all asked about automatic symmetry computation.
- Authors pointed to MACSYMA, REDUCE, MAPLE, Mathematica packages.
- **KJvK was not satisfied:** "It is not easy to agree on this point
  without a demonstration. **If you could demonstrate your method with
  such a procedure, I would give it a better score.**"

**Operational lesson:** when reviewers ask for a feature demonstration,
they mean a demonstration. Pointing to existing third-party software
doesn't satisfy them; the rebuttal needs to actually run the demo.

### 6. **DATA-AUGMENTATION BASELINE MISSING — dief + vAL8**

Authors deflected: "Data augmentation is not possible for PINN models
as they are trained directly with the PDE equation — i.e., there is no
ground truth training data in PINN to augment."

**The deflection appears principled** but didn't satisfy dief. The
underlying issue: when a paper claims "no comparable baseline exists,"
some reviewers interpret that as "no rigorous comparison performed."

---

## The Program Chair Intervention — A Rare and Important Pattern

The PC decision letter contains:

> "**One of the referees comments on the novelty of the work as the
> symmetries are already known. However, the incorporation of the
> symmetries in the training of PINNs is a sufficiently novel
> contribution.**"

This is a **PC-level rebuttal to a specific reviewer's specific
objection**, included in the formal decision letter. Three observations:

1. **The PC took a side on novelty.** Most PC decision letters are
   neutral aggregations of reviewer positions. This one explicitly
   countered dief's novelty-fallacy framing.
2. **The PC's argument is rebuttal-shaped.** "Symmetries are known
   (X), but applying them to PINN training (Y) is novel (Z)." Same
   pattern as the authors' rebuttal moves — decompose the contribution
   to defend each piece.
3. **The PC's intervention legitimizes the application-novelty claim.**
   "Use of known math in a specific new ML domain counts as novel
   contribution at NeurIPS 2023." This precedent is operationally
   important for SiS-style application papers.

**For SiS [[CTPC-KDD-Submission]]:** if reviewers attack on novelty
("NCDE + Cholesky-PSD + Student-t already exist"), the application to
real-spacecraft orbital prediction is the load-bearing novelty claim.
Frame it this way in §1 so a PC-level reader can defend it.

---

## Desk-Reject Signals for This Subfield (Physics-Informed ML at NeurIPS)

Paper accepted despite 5-reviewer thread with a Reject (3) and a
Borderline Reject (4). The review record reveals:

**Tolerated:**
- **One outright Reject (3)** with unmovable novelty-fallacy stance.
- **A Borderline Reject (4)** with unmovable evaluation-scope stance.
- **Presentation score of 1 (poor)** from one reviewer.
- **Notation inconsistency** flagged by 3/5 reviewers.
- **No data-augmentation baseline.**
- **No higher-dimensional PDE experiments.**
- **No demonstrated automatic symmetry derivation** despite reviewer
  request.

**Demanded:**
- **At least one Weak Accept that becomes more positive post-rebuttal**
  (5AKh's 6 → 7 was load-bearing).
- **Hypothesis-driven mid-rebuttal experiments** to flip the
  reviewer-on-the-fence.
- **PC-level recognition of application-novelty** when reviewers
  attack on methodology-novelty.
- **Voluntary acknowledgment of substitutability** in Limitations
  section — even though it didn't defuse the objection, it prevented
  worse reviews.

**For SiS [[CTPC-KDD-Submission]]** (SDA + UQ + NCDE + dissipation
application-domain paper):
- A 6-4-3 distribution with one strong reject is survivable at
  NeurIPS-scale venues if 1-2 reviewers genuinely engage and find the
  contribution valuable.
- Aim for at least one reviewer to commit to "this is application-
  novel even if the components are known."

---

## Patterns Worth Replicating in Rebuttal Strategy

### Patterns to replicate

1. **Hypothesis-driven new experiments mid-rebuttal.** 5AKh's score
   raise was directly triggered by a new "performance vs number of
   symmetries" experiment phrased as "Our hypothesis is X."
2. **Direct response to PC-level novelty critiques.** If the PC takes
   a side, the authors should embrace that framing in revision.
3. **Pre-emptive acknowledgment in Limitations** of "alternative
   methods could achieve similar effect" — better to volunteer than be
   accused.
4. **Decompose contribution into idea + implementation + experimental
   demonstration** (the authors' final response to dief did exactly
   this).

### Patterns to AVOID

1. **Pointing to third-party software when reviewers ask for a
   demonstration.** KJvK explicitly said this would have flipped their
   score; authors didn't deliver.
2. **Deflecting comparison requests with "no comparable method
   exists."** dief interpreted this as "no comparison performed."
   Better to provide imperfect comparisons than no comparisons.
3. **Math-heavy interdisciplinary contribution without a stand-alone
   formal definition section.** dief: "no clear definition of 'Lie
   point symmetry'." Reviewers from adjacent fields need explicit
   definitions even if the math is well-known in another field.
4. **Notation inconsistency.** 3/5 reviewers flagged. Even a low
   Presentation score can be the difference between accept and reject
   for a borderline paper.

---

## Connection to SiS / CTPC

Direct rebuttal-strategy lessons for [[CTPC-KDD-Submission]]:

| LiePointSymmetryPINN objection pattern | CTPC pre-emption strategy |
|---|---|
| Novelty fallacy ("Olver 1986 already did this") | Pre-empt by framing CTPC as **application-novel** in §1: "first application of structurally-dissipative NCDE correctors to real-spacecraft orbital prediction" — the PC defended this exact framing for LiePointSymmetryPINN |
| "Limited to 1D PDEs" / scope concern (4/5 reviewers) | Pre-empt by stating evaluation scope upfront with rationale ("we evaluate on one orbital regime to isolate the calibration effect; extension to other regimes is future work") |
| "Could be achieved by more sampling" (qo6e + KJvK) | Add a compute-budget-fair comparison (CTPC with smaller dataset vs Latent ODE with larger dataset) — voluntary concession alone is not enough |
| "No clear definition of [niche math concept]" (dief) | Define every non-standard term formally in §3 Background, not just in passing |
| "Automating X" question — pointed to 3rd party tool, lost score | If reviewers ask for a demonstration, deliver one in rebuttal |
| Hypothesis-driven new experiment flipped a score (5AKh) | Generate a new experiment in rebuttal phrased as "Our hypothesis is X; here is the test" |
| PC intervention defended application-novelty | Frame CTPC contribution claim as application-novel + composition-novel, allowing PC-level support |

---

## Connections

- [[LiePointSymmetryPINN]] — paired structural exemplar
- [[HopCPT-reviews]] — contrast: +2 score lifts via novelty
  decomposition. Same author group as this paper (Brandstetter
  co-author on both)
- [[GeometryNN-reviews]] — contrast: 0 score lifts despite extensive
  rebuttal. Cross-reviewer-influence in negative direction
- [[Disparate-DeepEns-reviews]] — contrast: 0 lifts, 1 negative
  propagation
- [[ICML-2022-Reviewer-Tutorial]] — slide 17 novelty fallacy +
  slide 18 blank assertions describe dief's review pattern exactly
- [[NeurIPS-2025-Reviewer-Guidelines]] — Q2.4 originality verbatim
  ("originality does not necessarily require introducing an entirely
  new method") is the formal version of what the PC manually defended
  here
- [[CTPC-KDD-Submission]] — direct pre-empt + rebuttal template target

---

## Open Questions

1. **What triggered the PC intervention?** Was it the chair reading
   dief's review specifically, or aggregating across the 4/5 reviewers
   with substantive engagement? Unknown from the OpenReview record.
2. **5AKh's exact final score** — said "increased the score" without
   stating new number. Approximately 7 inferred.
3. **PC-level novelty defense as a precedent** — how often does this
   happen at NeurIPS? Worth tracking across the exemplar corpus.

---

## Sources

- `raw/writing/reviews/LiePointSymmetryPINN-OpenReview.pdf` — 8 pages,
  full OpenReview thread including PC decision, 5 official reviews +
  post-rebuttal comments, 5 author rebuttals + secondary responses.
- Filename note: file was renamed from
  `LiePointSymmetryNPDE-OpenReview.pdf` to
  `LiePointSymmetryPINN-OpenReview.pdf` per the corpus normalization
  pass.
- Submission #14815, NeurIPS 2023, accepted as poster.
