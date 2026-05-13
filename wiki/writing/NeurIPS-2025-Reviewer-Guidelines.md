---
title: NeurIPS 2025 Reviewer Guidelines
tags: [writing, peer-review, neurips, reviewer-guidelines, openreview, 2025, scoring-rubric]
sources:
  - raw/writing/guides/2025 Reviewer Guidelines.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# NeurIPS 2025 Reviewer Guidelines

**Identified source (important correction):**

The file `raw/writing/guides/2025 Reviewer Guidelines.pdf` was filed under a generic
name and was assumed by Bilal at ingest-request time to be ICML 2025.
**The actual document is NeurIPS 2025 Reviewer Guidelines** (URL header:
`https://neurips.cc/Conferences/2025/ReviewerGuidelines`). The user's
specified target filename `ICML-2025-Reviewer-Guidelines.md` has therefore
been corrected to `NeurIPS-2025-Reviewer-Guidelines.md` in this ingest.

**Why this matters:** ICML and NeurIPS review practices have converged
closely over time, so the cross-comparison to [[ICML-2022-Reviewer-Tutorial]]
and [[ICML-2022-Review-Form]] is still operationally valuable — but the
delta now captures both **temporal** drift (2022→2025) AND **venue** drift
(ICML↔NeurIPS) entangled. Where I can confidently isolate temporal-only or
venue-only signal I will flag it; otherwise treat the delta as joint.

**Format:** 8-page HTML rendering of a website page, not a slide deck. The
content is the formal procedural + scoring guidance plus an FAQ + policies.

---

## Important Dates (Verbatim)

> "Invited Reviewers Bid on Papers: Sat, May 17 – Wed, May 21
> Check Paper Assignments: Thur, May 29
> Reviewing: Thur, May 29 – Wed, Jul 2
> [**If your reviews are late and/or are not of sufficient quality, you
> could lose access to the reviews on your own co-authored papers**, as our
> responsible reviewing initiatives indicate.]
> Author Rebuttal: Thur, Jul 24 - Wed, Jul 30
> Reviewer-Author Discussions: Thur, Jul 31 – Wed, Aug 6
> Reviewer-AC Discussions: Thur, Aug 7 – Wed, Aug 13
> Author Notification: Thur, Sept 18"

**New in 2025 (vs ICML 2022):** the **"reviewer-author rolling discussion"**
window (Jul 31 – Aug 6) is a separate phase after the one-week rebuttal,
followed by a separate reviewer-AC discussion phase. The total post-rebuttal
discussion is **two consecutive weeks** with structured handoff between
author-facing and AC-facing discussion.

---

## Review Form (Verbatim Question Set + Scoring Rubrics)

NeurIPS 2025 uses a **13-question review form**, several questions with
numerical sub-ratings. Verbatim:

### Q1. Summary

> "Briefly summarize the paper and its contributions. This is not the place
> to critique the paper; the authors should generally agree with a
> well-written summary. This is also not the place to paste the abstract —
> please provide the summary in your own understanding after reading."

### Q2. Strengths and Weaknesses — Four Dimensions

> "Please provide a thorough assessment of the strengths and weaknesses of
> the paper. A good mental framing for strengths and weaknesses is to think
> of reasons you might accept or reject the paper. Please touch on the
> following dimensions:"

**Quality (Q2.1):**
> "Is the submission technically sound? Are claims well supported (e.g., by
> theoretical analysis or experimental results)? Are the methods used
> appropriate? Is this a complete piece of work or work in progress? Are the
> authors careful and honest about evaluating both the strengths and
> weaknesses of their work?"

**Clarity (Q2.2):**
> "Is the submission clearly written? Is it well organized? (If not, please
> make constructive suggestions for improving its clarity.) Does it
> adequately inform the reader? (Note that a superbly written paper provides
> enough information for an expert reader to reproduce its results.)"

**Significance (Q2.3):**
> "Are the results impactful for the community? Are others (researchers or
> practitioners) likely to use the ideas or build on them? Does the
> submission address a difficult task in a better way than previous work?
> Does it advance our understanding/knowledge on the topic in a demonstrable
> way? Does it provide unique data, unique conclusions about existing data,
> or a unique theoretical or experimental approach?"

**Originality (Q2.4):**
> "Does the work provide new insights, deepen understanding, or highlight
> important properties of existing methods? Is it clear how this work differs
> from previous contributions, with relevant citations provided? Does the
> work introduce novel tasks or methods that advance the field? Does this
> work offer a novel combination of existing techniques, and is the reasoning
> behind this combination well-articulated? **As the questions above
> indicates, originality does not necessarily require introducing an
> entirely new method. Rather, a work that provides novel insights by
> evaluating existing methods, or demonstrates improved efficiency,
> fairness, etc. is also equally valuable.**"

### Q3-Q6. Four Numerical Ratings (each 1-4 Scale, Verbatim)

> "**Quality / Clarity / Significance / Originality:**
>   1. 4 excellent
>   2. 3 good
>   3. 2 fair
>   4. 1 poor"

Each of the four dimensions assigned a 1-4 score independently.

### Q7. Questions (Verbatim — OPERATIONALLY CRITICAL)

> "Please list up and carefully describe questions and suggestions for the
> authors, which should focus on key points (ideally **around 3–5**) that
> are actionable with clear guidance. Think of the things where a response
> from the author can change your opinion, clarify a confusion or address a
> limitation. **You are strongly encouraged to state the clear criteria
> under which your evaluation score could increase or decrease. This can be
> very important for a productive rebuttal and discussion phase with the
> authors.**"

The bolded line is the single most operationally valuable instruction in
this document for authors: reviewers are **explicitly told to state
score-flip criteria**, which makes rebuttal targeting deterministic.

### Q8. Limitations (Verbatim)

> "Have the authors adequately addressed the limitations and potential
> negative societal impact of their work? If so, simply leave 'yes'; if not,
> please include constructive suggestions for improvement. **In general,
> authors should be rewarded rather than punished for being up front about
> the limitations of their work and any potential negative societal impact.**
> You are encouraged to think through whether any critical points are
> missing and provide these as feedback for the authors."

### Q9. Overall Score (1-6 Scale, Verbatim — THE PRIMARY DECISION INPUT)

> "**6: Strong Accept:** Technically flawless paper with groundbreaking
> impact on one or more areas of AI, with exceptionally strong evaluation,
> reproducibility, and resources, and no unaddressed ethical considerations.
>
> **5: Accept:** Technically solid paper, with high impact on at least one
> sub-area of AI or moderate-to-high impact on more than one area of AI,
> with good-to-excellent evaluation, resources, reproducibility, and no
> unaddressed ethical considerations.
>
> **4: Borderline accept:** Technically solid paper where reasons to accept
> outweigh reasons to reject, e.g., limited evaluation. Please use sparingly.
>
> **3: Borderline reject:** Technically solid paper where reasons to reject,
> e.g., limited evaluation, outweigh reasons to accept, e.g., good
> evaluation. Please use sparingly.
>
> **2: Reject:** For instance, a paper with technical flaws, weak
> evaluation, inadequate reproducibility and incompletely addressed ethical
> considerations.
>
> **1: Strong Reject:** For instance, a paper with well-known results or
> unaddressed ethical considerations"

**Note:** the chairs explicitly direct "Please use sparingly" for the
borderline categories (3 and 4) — pushes reviewers toward decisive 2/5/6
recommendations rather than parking on the borderline.

### Q10. Confidence Score (1-5 Scale, Verbatim)

> "**5:** You are absolutely certain about your assessment. You are very
> familiar with the related work and checked the math/other details
> carefully.
> **4:** You are confident in your assessment, but not absolutely certain.
> It is unlikely, but not impossible, that you did not understand some parts
> of the submission or that you are unfamiliar with some pieces of related
> work.
> **3:** You are fairly confident in your assessment. It is possible that
> you did not understand some parts of the submission or that you are
> unfamiliar with some pieces of related work. Math/other details were not
> carefully checked.
> **2:** You are willing to defend your assessment, but it is quite likely
> that you did not understand the central parts of the submission or that
> you are unfamiliar with some pieces of related work. Math/other details
> were not carefully checked.
> **1:** Your assessment is an educated guess. The submission is not in your
> area or the submission was difficult to understand. Math/other details
> were not carefully checked."

The confidence rubric makes the **low-confidence-low-score combination** a
weak rejection signal — an AC can discount a Confidence-1 or -2 Reject more
than a Confidence-4 or -5 Reject.

### Q11-Q13. Declarations (Verbatim)

> "11. **Ethical concerns:** If there are ethical issues with this paper,
> please flag the paper for an ethics review.
> 12. **Code of conduct acknowledgement.** ...I have and will continue to
> abide by the NeurIPS code of conduct.
> 13. **Responsible reviewing acknowledgement:** I acknowledge I have read
> the information about the 'responsible reviewing initiatives' and will
> abide by that."

---

## Best Practices Section (Verbatim — Tone Guidance)

> "**Be thoughtful.** The paper you are reviewing may have been written by a
> first year graduate student who is submitting to a conference for the
> first time and you don't want to crush their spirits.
>
> **Be fair.** Do not let personal feelings affect your review.
>
> **Be useful.** A good review is useful to all parties involved: authors,
> other reviewers and AC/SACs. Try to keep your feedback constructive when
> possible.
>
> **Be specific.** Do not make vague statements in your review, as they are
> unfairly difficult for authors to address.
>
> **Be flexible.** The authors may address some points you raised in your
> review during the discussion period. Make an effort to update your
> understanding of the paper when new information is presented, and revise
> your review to reflect this.
>
> **Be timely.** Please respect the deadlines and respond promptly during
> the discussion. If you cannot complete your review on time, please let
> the AC know as soon as possible."

---

## Key Policy Statements (Verbatim, Operationally Important)

### On novelty alone not being grounds for rejection

(Per Q2.4 Originality verbatim above): "originality does not necessarily
require introducing an entirely new method. Rather, a work that provides
novel insights by evaluating existing methods, or demonstrates improved
efficiency, fairness, etc. is also equally valuable."

### On the paper checklist + "no" answers

> "Remember that answering 'no' to some questions is typically not grounds
> for rejection. In general, authors should be rewarded rather than punished
> for being up front about the limitations of their work and any potential
> negative societal impact."

### On uninformed reviews

> "Please make your review as informative and substantiated as possible;
> superficial, uninformed reviews without evidence are worse than no review
> as they may contribute noise to the review process. For example, if you
> argue about the lack of novelty, please provide appropriate references and
> point to existing mechanisms within. **Please ensure to thoroughly comment
> on technical aspects of work rather than focusing only on paper
> organisation or its grammar.**"

The bolded clause is operationally important — it forbids grammar-only
reviews. ML-paper authoring need not optimize prose beyond clarity.

### On format violations

> "Do not worry about minor violations of the required format (e.g., papers
> that exceed the page limit by a few lines), but immediately report any
> major violations that you notice to your AC."

### On formatting page limit

> "submissions are limited to **nine content pages**, including all figures
> and tables, in the NeurIPS 'submission' style; additional pages containing
> only references and the NeurIPS 2025 paper checklist are allowed."

(Note: KDD '26 will have its own page-limit policy — verify before
[[CTPC-KDD-Submission]] preparation.)

### On public visibility of reviews

> "When writing your review, please keep in mind that after decisions have
> been made, **reviews and meta-reviews of accepted papers as well as your
> discussion with the authors will be made public** (but reviewer and
> SAC/AC identities will remain anonymous). **This year, authors of rejected
> papers will have the option to make this information public for their
> rejected papers as well.**"

This is a major shift — see Delta section below.

### On stacks and acceptance rates

> "Q: Can I accept or reject all the papers in my stack?
> A: Please accept and reject papers based on their own merits. **You do not
> have to match the conference acceptance rate.**"

Reviewer is not implicitly quota-bound; the score is the score.

### On dual submission

> "NeurIPS does not allow submissions that are identical or substantially
> similar to papers that are in submission to, have been accepted to, or
> have been published in other archival venues. Submissions that are
> identical or substantially similar to other NeurIPS submissions fall under
> this policy as well; all NeurIPS submissions should be distinct and
> sufficiently substantial. **Slicing contributions too thinly is
> discouraged**, and may fall under the dual submission policy."

### On workshop tolerance

> "Q: What if I've seen similar work in a NeurIPS/ICML workshop?
> A: We allow work that has been submitted to non-archival workshops to be
> submitted to NeurIPS. To maintain anonymity, do not mention the workshop
> paper in your review."

### Roles: AC + SAC + Ethics Reviewers (Verbatim Definitions)

> "**Area Chairs (ACs).** ACs are the principal contact for reviewers
> during the whole reviewing process. ACs are responsible for recommending
> reviewers for submissions, ensuring that all submissions receive quality
> reviews, facilitating discussions among reviewers, writing meta-reviews,
> evaluating the quality of reviews, and making decision recommendations.
>
> **Senior Area Chairs (SACs).** Each SAC oversees the work of a small
> number of ACs, making sure that the reviewing process goes smoothly. SACs
> are also responsible for helping ACs find expert reviewers, calibrating
> decisions across ACs, discussing borderline papers, and helping the
> Program Chairs (PCs) make final decisions.
>
> **Ethics Reviewers.** You may flag submissions for additional review by
> ethics reviewers."

NeurIPS uses **AC/SAC**; ICML 2022 used **MR/SMR** (Meta-Reviewer / Senior
Meta-Reviewer). Same role, different name.

---

## DELTA: NeurIPS 2025 vs ICML 2022 (Explicit, Per User Instruction)

Per Bilal's batch instruction to compare against [[ICML-2022-Review-Form]]
and [[ICML-2022-Reviewer-Tutorial]] and "capture the delta explicitly".
Changes signal what the community has decided matters more or less.

### Δ1. Scoring scale change — coarsening + named tiers

| Aspect | ICML 2022 ([[ICML-2022-Review-Form]]) | NeurIPS 2025 |
|---|---|---|
| Overall score | Phase-1 binary recommend-for-rejection + (Phase-2 score scale) | **1-6 named tiers** (Strong Reject … Strong Accept) |
| Sub-ratings | Six numbered evaluation axes | **4 sub-ratings on 1-4 scale** (Quality/Clarity/Significance/Originality) |
| Confidence | Implicit | **Explicit 1-5 confidence score** |

**Implication:** the NeurIPS 2025 scoring is more granular per-dimension
(four sub-ratings each on a 1-4 scale = 4×4 = 16 possible quality vectors)
but the overall 1-6 collapses these. The "Please use sparingly" warning on
borderline 3/4 pushes reviewers to commit to a side.

### Δ2. Novelty is explicitly softened in the formal rubric

| ICML 2022 | NeurIPS 2025 |
|---|---|
| "Novelty fallacy" appears in the **tutorial** (slide 17) as a reviewer-error category. The Review Form does not formally rate originality on a 1-N scale — it bundles into the overall rec. | **Originality is one of four explicit sub-ratings** AND the verbatim Q2.4 text says "originality does not necessarily require introducing an entirely new method. Rather, a work that provides novel insights by evaluating existing methods, or demonstrates improved efficiency, fairness, etc. is also equally valuable." |

**Operational delta:** in 2022, "not novel" was a *reviewer error*; in 2025,
it is *formally legitimized as a rating axis* but with the floor explicitly
raised so that novel-combinations and improved-efficiency suffice. The 2025
framing is more author-friendly than the 2022 framing on this dimension —
the community has decided to weaken novelty-as-gate further.

### Δ3. Reviewer obligation to expose score-flip criteria (NEW in 2025)

| ICML 2022 | NeurIPS 2025 |
|---|---|
| Reviewers were encouraged to be constructive and suggest improvements but **not formally obligated** to expose criteria under which their score would change. | **Verbatim Q7:** "You are strongly encouraged to state the clear criteria under which your evaluation score could increase or decrease. This can be very important for a productive rebuttal and discussion phase with the authors." |

**Operational delta:** rebuttals in 2025 can ask reviewers, "What would
change your score?" with explicit institutional support. This is a
significant shift toward rebuttal-as-deterministic-process. **Critical for
SiS's [[CTPC-KDD-Submission]]** if KDD adopts similar language: pre-empt
this by explicitly listing in the paper itself which experiments would
strengthen the contribution and where the boundaries of validity lie.

### Δ4. Reviews of accepted papers become public; rejected opt-in

| ICML 2022 | NeurIPS 2025 |
|---|---|
| Reviews stayed within the review system; no public posting of accepted-paper reviews. | "**reviews and meta-reviews of accepted papers as well as your discussion with the authors will be made public** (but reviewer and SAC/AC identities will remain anonymous). This year, authors of rejected papers will have the option to make this information public for their rejected papers as well." |

**Operational delta:** reviewers now write knowing their text will be
permanently public on OpenReview (anonymized). This raises review quality
incentive AND creates a permanent searchable record of what objections were
raised against a given paper. **For SiS:** if [[CTPC-KDD-Submission]] is
rejected and Bilal opts in to public reviews, those reviews become part of
the long-tail discovery surface for future SiS work.

### Δ5. "Responsible reviewing initiatives" — accountability lever (NEW)

| ICML 2022 | NeurIPS 2025 |
|---|---|
| Late or sloppy reviews resulted in "MR will not have a good impression of you" (tutorial slide 9). | **"If your reviews are late and/or are not of sufficient quality, you could lose access to the reviews on your own co-authored papers."** |

**Operational delta:** material consequence for sloppy reviews — reviewer
loses visibility into reviews of their *own* submitted papers. This is a
direct incentive alignment toward quality.

### Δ6. Discussion phase structure — two-stage rolling vs one-shot

| ICML 2022 | NeurIPS 2025 |
|---|---|
| Phase 2: rebuttal → reviewer discussion → MR consolidation; single rebuttal slot. | **Two-stage discussion:** Jul 24-30 author rebuttal → Jul 31-Aug 6 **rolling author-reviewer discussion** (authors can keep commenting through this window) → Aug 7-13 reviewer-AC discussion. |

**Operational delta:** authors have ~2 weeks of structured engagement, not
one. The rolling discussion makes the rebuttal phase iterative. **For SiS:**
reserve calendar bandwidth across both weeks if [[CTPC-KDD-Submission]] uses
similar structure.

### Δ7. Sub-reviewers explicitly banned (NEW formal policy)

| ICML 2022 | NeurIPS 2025 |
|---|---|
| Sub-reviewers were informally discouraged; not always policy-explicit. | **"Q: Can I invite a sub-reviewer to help with my reviews? A: No, sub-reviewers are not allowed."** |

**Operational delta:** reviewer must do the work themselves; no "graduate
student doing the review under my name" loophole.

### Δ8. Anonymity preserved during review; non-archival workshops allowed

| ICML 2022 | NeurIPS 2025 |
|---|---|
| Workshop venue mentions were a common anonymity hazard. | Explicit: "We allow work that has been submitted to non-archival workshops to be submitted to NeurIPS. To maintain anonymity, do not mention the workshop paper in your review." |

**Operational delta:** SiS workshop-paper-then-conference-paper pipeline is
explicitly supported; the conference reviewer must self-police.

### Δ9. Bidding ethics policy made explicit (NEW)

| ICML 2022 | NeurIPS 2025 |
|---|---|
| Bidding was informal; no explicit policy against deceptive bidding. | "Unfortunately, in past years there have been a small number of reviewers who engage in deceptive bidding practices. If we have a reason to suspect that a reviewer is engaged in deceitful bidding to influence reviewing outcomes, we will request an ethics investigation, and malicious actors may be removed from future involvement in the program committee." |

**Operational delta:** bidding manipulation is now a named offense. For SiS,
this matters only if Bilal ever serves as a reviewer.

### Δ10. Format leniency vs strictness

| ICML 2022 | NeurIPS 2025 |
|---|---|
| Hard page-limit enforcement was the norm. | "Do not worry about minor violations of the required format (e.g., papers that exceed the page limit by a few lines), but immediately report any major violations." |

**Operational delta:** NeurIPS 2025 explicitly tolerates small overruns (a
"spillover to page 10"). Don't game it, but don't sweat a single line.

### Δ11. Role renames: MR → AC, SMR → SAC

Same hierarchy; different label. No operational impact other than vocabulary.

### Δ12. Continuity points (not changed)

The acceptance criterion language has converged but not changed in spirit:
- ICML 2022: "sufficient knowledge advancement, well grounded, sufficient
  interest to some ICML audiences."
- NeurIPS 2025: split into Quality (technically sound + claims supported) +
  Significance (impact, build-upon-able) + Originality (insights, novel
  combination) + Clarity, all four required.

Both venues converge on: **well-grounded + significant + interesting to some
audience = accept.** The 2025 form is more procedural about how to render
that judgment.

---

## What Reviewers Are Explicitly Told to Look For (Operationally)

Synthesizing the verbatim review-form questions:

1. **Technical soundness** (Q2.1 Quality) — claims supported by evidence;
   methods appropriate; honesty about strengths AND weaknesses.
2. **Reproducibility** (Q2.2 Clarity, Q10 Confidence-5) — "a superbly
   written paper provides enough information for an expert reader to
   reproduce its results."
3. **Significance / community impact** (Q2.3) — others likely to use or
   build on; advances understanding demonstrably.
4. **Originality (broadly construed)** (Q2.4) — novel insight or novel
   combination of existing techniques counts; pure novelty not required.
5. **Limitations honestly addressed** (Q8) — proactive disclosure of
   limitations is *rewarded*.
6. **Ethical considerations** (Q11) — flagged separately.
7. **Reproducibility resources** (in Overall Q9.5/Q9.6 explicit) —
   evaluation, code, data.

---

## What Constitutes a Valid Rejection Reason (NeurIPS 2025)

From the Overall rubric (Q9.1, Q9.2) verbatim:

- **Strong Reject (1):** "well-known results or unaddressed ethical
  considerations"
- **Reject (2):** "technical flaws, weak evaluation, inadequate
  reproducibility and incompletely addressed ethical considerations"

Plus the implicit "outweighs reasons to accept" for Borderline reject (3).

**Reasons NOT to use** (synthesized from the body):
- Lack of perfect novelty — "originality does not necessarily require
  introducing an entirely new method"
- Format-overrun by a few lines
- Author having said "no" to checklist items (typically not grounds)
- Workshop-version existence (must be ignored)
- Failure to match acceptance rate ("You do not have to match the conference
  acceptance rate")
- Grammar/organization complaints alone ("thoroughly comment on technical
  aspects rather than focusing only on paper organisation or its grammar")

These ICML 2022 anti-patterns (policy entrepreneurism, novelty fallacy,
intellectual laziness) are not separately enumerated in the NeurIPS 2025
guidelines body — the 2025 form effectively *bakes* them into the rubric
language (e.g., the Originality verbatim softening = embedded
novelty-fallacy correction).

---

## Examples of Good vs Bad Reviews

NeurIPS 2025 guidelines do NOT include the slide-deck-style negative-example
scripts of [[ICML-2022-Reviewer-Tutorial]] (no "Reviewer: This is not good
enough for ICML 2022 — Why?" verbatim refutations). Instead, the
prescriptions are positive-only ("Be specific", "Be useful", "Be
flexible"). The negative-pattern recognition has to be backported from the
ICML 2022 deck — they remain operationally valid since the underlying
community standards converge.

The **closest direct guidance on "what makes a review bad"**:
> "superficial, uninformed reviews without evidence are worse than no
> review as they may contribute noise to the review process."

---

## Scoring Calibration Guidance — What Each Overall Score Means (Compressed)

For pre-submission and rebuttal use:

| Score | Verbatim threshold | What it implies for rebuttal target |
|---|---|---|
| 6 Strong Accept | "Technically flawless... groundbreaking impact... exceptionally strong evaluation, reproducibility, and resources" | Not the realistic target for first-time-submission SiS work |
| 5 Accept | "Technically solid... high impact on at least one sub-area... good-to-excellent evaluation, resources, reproducibility" | **Realistic target for [[CTPC-KDD-Submission]]** if positioning as core ML methodology contribution |
| 4 Borderline accept | "Reasons to accept outweigh reasons to reject, e.g., limited evaluation" | Common starting point; rebuttal target is to flip one of 3 reviewers to 5 |
| 3 Borderline reject | "Reasons to reject outweigh reasons to accept" | Survival mode; rebuttal must explicitly address the dominant reject reason |
| 2 Reject | "Technical flaws, weak evaluation, inadequate reproducibility" | Hard to flip without new experiments; identify which of the three causes apply |
| 1 Strong Reject | "Well-known results or unaddressed ethical considerations" | Likely fatal unless reviewer's "well-known" claim can be specifically refuted with citation |

**Operational rule:** the chairs say "Please use sparingly" for borderline
3 and 4 — reviewers are nudged toward decisive 2/5/6 calls. A paper
landing at 3-3-3 is rare; more likely is 2-4-5 or 3-4-5, which means the
spread is the leverage point.

---

## Confidence Score — Implications for Rebuttal Strategy

The 1-5 confidence scale (Q10) governs how much weight an AC gives each
score. The asymmetric weights:

- **Confidence 5 + Score 2/Reject** = nearly fatal (reviewer is sure)
- **Confidence 5 + Score 5/Accept** = nearly decisive (reviewer is sure)
- **Confidence 1-2 + Reject** = an AC can discount; rebuttal can target by
  improving explanation of contribution
- **Confidence 3 + any score** = ripe for rebuttal-driven shift

For [[CTPC-KDD-Submission]]: identify each reviewer's stated confidence in
their review. Target the lowest-confidence reviewer for the strongest
rebuttal effort.

---

## Core Actionable Rules (Compressed for Pre-Submission Audit)

1. **Aim for Quality, Clarity, Significance, Originality each ≥ 3 (good)
   on 1-4 scale.** Each is independently rated.
2. **Originality includes novel combinations** — frame [[CTPC-KDD-Submission]]'s
   contribution as a novel combination (NCDE + Cholesky-PSD + RTN + Student-t)
   if that's its strongest framing.
3. **Disclose limitations honestly in the paper** — Q8 explicitly rewards
   this; up-front limitations protect against rejection-by-omission.
4. **Reproducibility is part of the Overall score 5/6 definitions** —
   release of code/data should be mentioned in the submission to clear this
   bar.
5. **Sections that signal a 5-Accept:** "high impact on at least one
   sub-area" — for SiS, the sub-area is SDA + safe ML. Frame impact on a
   specific sub-area, not generic ML.
6. **Anticipate that reviewers will name "what would flip my score"** —
   pre-empt by stating in the paper itself which limitations are addressed
   and which are out of scope.
7. **Page limit:** 9 content + unlimited refs + checklist. KDD '26 limit may
   differ; verify.
8. **Workshop overlap:** if any [[CTPC-KDD-Submission]] precursor exists at
   a non-archival workshop, ensure no identifying tells; the policy permits
   this but anonymity must hold.
9. **Code release is policy-protected** — no requirement, but it raises
   reproducibility score.
10. **Public-review awareness:** if accepted, the reviews become a permanent
    public artifact. Write the paper as if the reviews of it will be a
    permanent academic record.

---

## Reviewer-Facing Implications

The 2025 guidelines harden several anti-patterns that were tutorial-only
in 2022:

| Anti-pattern | 2022 mechanism | 2025 mechanism |
|---|---|---|
| "Not novel" rejection | Tutorial slide 17 — reviewer-error category | Q2.4 Originality verbatim — formal rubric language |
| Reviewer hostility / "comment about authors" | Tutorial slide 18 — blank assertions category | Q12 Code of conduct acknowledgement + Best Practices "Be thoughtful" |
| Sloppy/late reviews | Tutorial slide 9 — "no good impression" | **Material penalty:** lose access to own paper reviews |
| Format complaints alone | Tutorial slide 20 — intellectual laziness | Verbatim "thoroughly comment on technical aspects rather than focusing only on paper organisation or its grammar" |
| Grammar-only reviews | Implicit | Explicit prohibition (Q4 paragraph) |

The 2025 framework is **more procedurally enforceable** than the 2022
guidance, which relied on reviewer self-policing via the tutorial deck.

---

## ML-Paper-Specific Advice (Distinguished from General Writing)

This is **NeurIPS-specific** but ML-conference-universal in spirit:
- The 1-6 Overall + 1-5 Confidence pair is now the de-facto ML standard
  (ICLR, ICML, AAAI all use similar scales).
- The 4-dimension Quality/Clarity/Significance/Originality split is
  spreading; KDD may or may not adopt.
- The "originality includes novel combinations" softening is a major
  community shift — assume KDD 2026 has converged similarly even if not
  verbatim.

For [[CTPC-KDD-Submission]]: confirm KDD 2026's reviewer form before final
draft; if it diverges, use this NeurIPS form as the closest available proxy.

---

## Connection to SiS / CTPC

Direct mapping for [[CTPC-KDD-Submission]]:

| Review-form question | SiS-side preparation |
|---|---|
| Q2.1 Quality (technical soundness) | d̄² ≈ 1 + 64% MSE + held-out NASA CDDIS evidence — well grounded |
| Q2.2 Clarity (reproducibility-grade writing) | Run [[Ramdas-Paper-Checklist]] audit + [[Strunk-White-Elements-Style]] rules 9-18 + [[Guide-Writing-Mathematics]] grammar audit |
| Q2.3 Significance (community impact) | Frame for SDA + safety-critical-ML audiences explicitly |
| Q2.4 Originality (novel combination) | Frame contribution as "NCDE + Cholesky-PSD + Student-t + RTN frame" composition, not as a novel monolith |
| Q7 Questions (score-flip criteria) | Pre-empt in paper: state which limitations are intentional + what would extend the work |
| Q8 Limitations | Section explicitly listing limitations |
| Q9 Overall (target = 5 Accept) | Need: technically solid + high impact on at least one sub-area + good-to-excellent eval + reproducibility |

For Bilal's pre-submission audit pipeline, this page joins
[[ICML-2022-Reviewer-Tutorial]] (objection refutations) and
[[ICML-2022-Review-Form]] (scoring axes) as the third reviewer-side anchor.

The relationship between these three:
- [[ICML-2022-Review-Form]] = formal scoring rubric, ICML-specific
- [[ICML-2022-Reviewer-Tutorial]] = qualitative reviewer-error catalog,
  CVPR-derived
- [[NeurIPS-2025-Reviewer-Guidelines]] = formal scoring rubric + qualitative
  guidance combined, with the most recent community-consensus framing of
  novelty + reproducibility + responsible-reviewing
- All three should be consulted; this 2025 page supersedes neither but
  represents the current community position.

---

## Connections

- [[ICML-2022-Reviewer-Tutorial]] — explicit delta source; objection
  refutations remain valid
- [[ICML-2022-Review-Form]] — formal rubric companion; scoring scale delta
- [[ICML-Best-Practices]] — author-side checklist for satisfying these
  reviewer questions
- [[Ramdas-Paper-Checklist]] — pre-submission yes/no audit
- [[Lipton-Writing-Heuristics]] — voice and grounding hygiene
- [[Freeman-Writing-Papers]] — reviewer POV (slide 34+)
- [[CTPC-KDD-Submission]] — pre-submission + rebuttal target
- [[CTPC-Design-Rationale]] — Q5 formal verification supports
  reproducibility + significance dimensions

---

## Open Questions

1. **Filename correction:** this document was filed as
   `2025 Reviewer Guidelines.pdf` and the user assumed ICML 2025 at
   ingest-request time. **Actual source: NeurIPS 2025.** Wiki filename
   corrected accordingly. Bilal should be aware when referring to this in
   conversation.
2. **KDD '26 reviewer form:** is it published yet? Compare against
   NeurIPS 2025 when available — likely high similarity but worth
   confirming.
3. **ICML 2025 reviewer form:** does it exist separately? Not in current
   SiS corpus. Worth a future `raw/writing/guides/` addition if Bilal wants
   ICML-specific 2025 data.
4. **The "responsible reviewing initiatives" blog post** (URL referenced):
   `https://blog.neurips.cc/2025/05/02/responsible-reviewing-initiative-for-neurips-2025/`
   — not in corpus; full content not captured. May be worth ingesting if
   the consequences for SiS authoring are non-obvious.

---

## Sources

- `raw/writing/guides/2025 Reviewer Guidelines.pdf` — **NeurIPS 2025 Reviewer
  Guidelines** (identified via URL header `https://neurips.cc/Conferences/2025/ReviewerGuidelines`).
- 8 pages, full document read in this ingest.
- File-naming note: the SiS corpus filed this as `2025 Reviewer Guidelines.pdf`
  with no venue prefix. The actual source is NeurIPS, not ICML. Cross-venue
  delta to ICML 2022 captured in main body.
