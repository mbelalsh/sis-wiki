---
title: ICML 2022 Reviewer Tutorial (How to Be a Good Reviewer)
tags: [writing, peer-review, icml, reviewer-training, objection-mapping, no-such-policy]
sources:
  - raw/writing/guides/ICML 2022 How to be a good reviewer-tutorials for ICML2022 reviewers.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# ICML 2022 Reviewer Tutorial — How to Be a Good Reviewer

**Source:** Official ICML 2022 reviewer training deck (22 slides), authored by
the **ICML 2022 Program Chairs**. Acknowledgement: "We are grateful for the
CVPR 2022 chairs who shared their slides on the same topic. These slides are
largely based on their slides, with some modifications to better fit the needs
of ICML." Sections marked `[Developed from: Reviewer Slides for CVPR'21]`
indicate CVPR lineage.

**Why this is the writing-agent's single most important document:** This deck
is the **training material for the people who decide whether
[[CTPC-KDD-Submission]] gets accepted.** Every reviewer at ICML 2022 was
expected to internalize it. Everything in it is a direct mapping from
"reviewer objection" → "what the chairs told the reviewer to do instead."
The deck is the **objection-rebuttal Rosetta Stone.**

**Note on scoring calibration:** This tutorial does NOT contain the score
rubric (1-10 scale and what each value means). The rubric lives in
[[ICML-2022-Review-Form]]. This deck covers the *qualitative* expectations;
the Review Form covers the *quantitative* rubric. Treat the two as
complementary.

---

## Decision Process (Verbatim from Slides 3-4)

### Phase 1 (Pre-rebuttal)

> Abstract submissions → Paper submissions → Program chairs (create paper
> assignments, **at least two reviewers, one "experienced"**) → Reviewers
> (**At least 2 reviewers, at least one experienced, both provide full
> reviews. Paper can be flagged for rejection**) → Meta-Reviewers (**MR reads
> reviews for papers marked by both reviewer for rejection; conservatively
> recommendation rejections**) → Senior Meta-Reviewers (SMRs review proposals,
> recommend for PCs) → Program chairs (finalize) → **Phase 1 reject decisions**

### Phase 2 (Post-rebuttal)

> "Surviving" papers → Meta-Reviewers add an extra reviewer per paper →
> Program chairs ensure all papers have **at least 3 reviewers** → Reviewers
> + MRs (the extra reviewer provides a full review **not seeing previous
> reviews**; MRs check for quality, reviewers adjust) → **Rebuttals** (authors
> get all reviews, write rebuttal if needed) → Reviewers (read and confirm
> reading rebuttal, update reviews if needed, sort out differences) →
> Meta-Reviewers (read reviews + rebuttal + discussions; prepare
> recommendation; write meta-review) → Senior Meta-Reviewers (control/oversee
> decision process) → Program chairs → **Paper Decisions**

**Implications for rebuttal strategy:** the rebuttal is read by every
reviewer + MR; reviewers are explicitly instructed to "read and confirm
reading rebuttal" and "update reviews if needed." A rebuttal that addresses
the specific objections raised has a structural channel to flip a score.

---

## What Paper Should Be Accepted? (Slide 10, Verbatim — THE ACCEPTANCE CRITERION)

> "**Any paper that, in accordance with ICML community standards,**
>   - **presents sufficient knowledge advancement that is well grounded;**
>   - **is of sufficient interest to some ICML audiences who could benefit
>     from it**
>
> **Note: ICML is very inclusive**
>   - **Historically rejection solely for out-of-scope is rather rare**"

Three operational criteria, each independently necessary:

1. **Sufficient knowledge advancement** — there exists a "knowledge
   advancement" identifiable in the paper. (Slide 7: "**Key**: What is the
   knowledge advancement in the paper, or that the paper leads to?")
2. **Well grounded** — the advancement is empirically/theoretically supported.
3. **Sufficient interest to some ICML audience** — note "some", not "all".

The chairs explicitly preempt the "out-of-scope" rejection by noting it is
"rather rare." This is operationally important for SiS — applying ML to SDA
is sometimes out-of-mainstream-ICML but not out-of-scope.

---

## What Reviewers Are Explicitly Told to Look For

**A concise summary of the paper** (Slide 11):
- What problem is addressed in the paper?
- Is it a new problem? If so, why does it matter? If not, why does it still
  matter?
- What is the key to the solution? What is the main contribution?
- Do the experiments sufficiently support the claims?

**A clear statement of strengths and weaknesses:**
- What are the key contributions and why do they matter?
- What aspects of the paper most need improvement?

**A comprehensive check of potential fundamental flaws:**
- Are the assumptions and theories (mathematically) sound?
- Are the experiments scientifically sound and valid?
- Is the problem addressed trivial?
- Did the paper miss important prior work? Has it been done before? If yes,
  where?

---

## What Reviewers Are Told to Ignore or NOT Penalize — "No Such Policy!" (Slide 19)

The **Policy Entrepreneurism** slide is the single most operationally
valuable in the deck for rebuttal authors. Four reviewer scripts and their
verbatim refutations:

| Reviewer demands | Chairs' verbatim response |
|---|---|
| "You must publish your dataset!" | **No such policy!** |
| "You must beat SOTA!" | **No such policy!** |
| "You must have a theorem!" | **No such policy!** |
| "You must beat arXiv papers!" | **No such policy!** |

**Verbatim error description:**
> "You imposed your own policies which are 1) not part of the official review
> policy and 2) against scientific review principles."

**Verbatim safe-behavior prescription:**
> "Make sure you follow common principles in scientific review.
> Most importantly, focus on whether the paper produced significant knowledge
> advancement."

These four "No such policy!" lines are the **canonical refutations for
rebuttal use.** When a reviewer demands an arXiv comparison, dataset release,
SOTA beat, or theorem inclusion, the rebuttal can cite ICML 2022 Reviewer
Tutorial slide 19 directly.

### Additional non-acceptable reviewer demands (Slide 20 — Intellectual Laziness)

| Reviewer script | Chairs' verbatim counter-question |
|---|---|
| "Does not beat SOTA so it must be rejected!" | "Does the paper present sufficient knowledge advancement?" |
| "Beat SOTA so it must be accepted!" | "Does the paper present sufficient knowledge advancement?" |
| "Theorem V looks wrong" | "It is either wrong or correct. You can not be unsure." |
| "There is this error hence it should be rejected" | "Is the error making the main knowledge advancement invalid?" |

**Verbatim error:** "Overemphasize certain factors instead of giving a
comprehensive assessment."

### Novelty fallacy (Slide 17)

**Verbatim error:**
> "Many important things are not that novel
>   - Small but clever adjustment to SOTA
> Many novel things are not that important
>   - AND most really silly things are novel"

**Verbatim safe-behavior:**
> "Focus on whether or not the paper presented well-grounded knowledge
> advancement."

This is the **canonical refutation for "not novel enough" rejections.**
"Small but clever adjustment to SOTA" is explicitly endorsed as acceptable —
which is exactly the [[CTPC-KDD-Submission]] template (NCDE + Cholesky-PSD +
RTN frame + Student-t are small clever adjustments to existing components).

---

## What Constitutes a Valid Rejection Reason

Combining the acceptance criterion (slide 10) with the comprehensive-flaw
check (slide 11), valid rejection reasons reduce to:

1. **Insufficient knowledge advancement** (no identifiable advancement in
   the paper, or the advancement claimed isn't actually advanced).
2. **Not well grounded** (assumptions/theories mathematically unsound;
   experiments scientifically invalid; the claimed advancement isn't
   supported by what's shown).
3. **Trivial problem.**
4. **Missed important prior work** — the paper has been done before, or
   misses a critical citation that invalidates the contribution claim.
5. **Out-of-scope for ICML audiences** — but per slide 10, "Historically
   rejection solely for out-of-scope is rather rare" → this is the weakest
   rejection ground.

Reviewers are explicitly NOT to use as rejection reasons: lack of SOTA beat,
lack of theorem, no dataset release, no arXiv-beating, lack of novelty (per
the novelty-fallacy slide), or "this error exists" without showing the error
invalidates the main contribution.

---

## Six Categories of Reviewer Errors (Slides 12, 14-20 — Verbatim)

The deck dedicates 7 slides to reviewer-error categories. These are
**name-and-shame examples** the chairs explicitly want reviewers to avoid.
For the writing-agent's objection-mapping table, each is a named refutation
target.

### 1. Arrogance, Ignorance, Inaccuracy (Slide 15)

- **Arrogance:** "Authors: We did A by means of B. Reviewer: The only way to
  do A is through C (i.e., my way or highway). Error: you should know or
  check."
- **Ignorance:** "Authors: All A are B. Reviewer: I do not think all A are B.
  Error: you should know or check."
- **Inaccuracy:** "Authors: A is a ring, not a field. Reviewer: All rings
  are fields. Error: They are NOT…"

**Safe behavior:** "do not provide an opinion on things you do not know
about."

### 2. Pure Opinions (Slide 16)

- "Reviewer: This is not good enough for ICML 2022 — Why?"
- "Reviewer: CNN is not that interesting — Why?"
- "Reviewer: Adversarial losses guarantee distribution matches — No
  theoretical proof indeed!"

**Verbatim error:** "These remarks are pure opinions and not grounded."

**Safe behavior:** "Check if you grounded your statement with a 'because
… …'"

### 3. Novelty Fallacy (Slide 17)

(Quoted in full above. Verbatim error: novelty is not sufficient and not
necessary; the test is "well-grounded knowledge advancement.")

### 4. Blank Assertions (Slide 18)

- "Reviewer: This has been done before. By whom? Where? Why?"
- "Reviewer: Intrinsic images are not longer important. Really? To whom?
  Why?"
- "Reviewer: Experiments on unpublished datasets are not scientific. Really?
  Why?"
- "Reviewer: Authors are ignorant/careless/incompetent…"
- "Reviewer: If the authors were smart enough, they would…."

**Verbatim error:**
> "Making ungrounded statements; Comment about authors instead of focusing
> on the paper content."

**Verbatim safe-behavior:**
> "Provide evidence to support your assertions; Confine the discussion on the
> technical content of the paper, not on the authors."

### 5. Policy Entrepreneurism (Slide 19)

(Quoted in full above — the four "No such policy!" lines.)

### 6. Intellectual Laziness (Slide 20)

(Quoted in full above — the SOTA-must-beat / SOTA-must-accept / theorem-
looks-wrong / error-hence-reject overemphasis pattern.)

---

## Examples of Good vs Bad Reviews (Verbatim Scripts)

The deck does NOT contain side-by-side "good vs bad review" examples per se,
but every error-category slide includes **negative-example scripts**:

| Bad-review verbatim | Why it fails | What should the reviewer do |
|---|---|---|
| "This is not good enough for ICML 2022" | Pure opinion; ungrounded | "Check if you grounded your statement with a 'because…'" |
| "CNN is not that interesting" | Pure opinion | Same |
| "This has been done before" | Blank assertion | "By whom? Where? Why?" — must cite |
| "Authors are ignorant/careless/incompetent…" | Comment about authors | "Be humble, nobody is perfect" |
| "You must publish your dataset!" | Policy entrepreneurism | No such policy |
| "Does not beat SOTA so it must be rejected!" | Intellectual laziness | "Does the paper present sufficient knowledge advancement?" |
| "Theorem V looks wrong" | Intellectual laziness | "It is either wrong or correct. You can not be unsure." |

For positive examples (slides 11), the chairs prescribe the **review
structure** (concise summary + clear strengths-weaknesses + comprehensive
flaw check) rather than a model good-review text. A "good review" by this
deck's standard has these three sections, each grounded with "because…"
justifications, never policy-entrepreneurial, never blank-assertional.

---

## Scoring Calibration Guidance (NOT in This Deck — Cross-Reference)

This tutorial does NOT cover the 1-10 score rubric. For score → meaning
mapping, see [[ICML-2022-Review-Form]] §"Phase 1 / Phase 2 binary
recommendation logic." The tutorial covers:

- **Reviewers can "flag for rejection" at Phase 1** (slide 3) — this is a
  binary signal, not a graded score.
- **MRs read papers marked by both reviewers for rejection** and "conservatively
  recommend rejections" — implies bias toward keeping borderline papers in
  Phase 2.
- **Phase 1 rejection requires both initial reviewers flag it** — a single
  enthusiastic reviewer prevents Phase 1 rejection.

This is operationally significant: **for SiS's KDD '26 submission, having
at least one reviewer not flag for rejection in Phase 1 is the survival
threshold.** Phase 2 then operates under the standard score rubric.

---

## Expectations to Reviewers (Slide 8, Verbatim)

> "Be constructive to the authors
>   - It is necessary to be critical, but avoid offending the authors
>   - Instead, suggest how they could make the paper better
>   - Think of the reviewing process as a collaborative effort to make the
>     paper better
>   - Use professional language, be courteous, respectful
>
> Be friendly to your buddy reviewers and MRs
>   - Again, this should be a collaborative process with the aim of making
>     the papers better if possible
>   - People could take diverse views on the same paper
>   - **Agree to disagree** – the discussions do not force consensus, but aim
>     for sorting out hard contradictions
>   - Focus the discussions on the technical side and do not take it
>     personally
>
> Be professional: on time, responsible, and responsive"

The "agree to disagree" line implies a rebuttal that converts one reviewer
can shift the balance without needing to convert all.

---

## Identity & Double-Blind (Slide 21, Verbatim)

> "reviews are double-blind: reviewers do not know author identities, authors
> do not know reviewer / MR / SMR identities
> meta reviewer does not know author identity
> program chairs know author identity
> meta reviewer knows reviewer identity, and reviewers know MR identity"

**Implication for SiS:** [[CTPC-KDD-Submission]] must be anonymized — no
self-citation that reveals authorship, no acknowledgments of SiS funding/
institution, no GitHub repos with author names visible. The KDD anonymization
convention applies the same way ICML does.

---

## Core Actionable Rules (Compressed for Pre-Submission Audit)

For [[CTPC-KDD-Submission]] authoring + rebuttal strategy:

1. **State the knowledge advancement explicitly and early** — slide 7's
   "Key" question is what reviewers are trained to answer.
2. **Ground the advancement** — show experiments support the claim.
3. **Specify the ICML/KDD audience who benefits** — even one is sufficient
   per slide 10.
4. **Don't worry about out-of-scope** — historically rare per slide 10.
5. **Don't need to beat SOTA** — slide 19 + slide 20 explicit.
6. **Don't need a theorem** — slide 19 explicit.
7. **Don't need to publish dataset** — slide 19 explicit.
8. **Don't need to be novel-as-such** — slide 17: small-but-clever
   adjustments to SOTA are acceptable.
9. **Address the comprehensive flaw check proactively** — mathematical
   soundness, experimental validity, problem non-triviality, prior-work
   coverage (slide 11).
10. **In rebuttal: cite the deck by slide number** — reviewers know this
    deck (it was their training material).

---

## Reviewer-Facing Implications

This deck is the **direct source of truth** for what reviewers were trained
on. Every objection class has a named-and-shamed entry. The 6 error
categories give a complete refutation taxonomy:

- **Arrogance/ignorance/inaccuracy:** rebuttal cites the source the reviewer
  missed.
- **Pure opinions:** rebuttal asks for the "because…" — the reviewer is
  obligated to provide it.
- **Novelty fallacy:** rebuttal points to slide 17 — "novel is neither
  necessary nor sufficient."
- **Blank assertions:** rebuttal demands the citation.
- **Policy entrepreneurism:** rebuttal cites slide 19 — "No such policy."
- **Intellectual laziness:** rebuttal points to the comprehensive-flaw
  check criterion.

The deck operationalizes a single yardstick: **"well-grounded knowledge
advancement"** — every objection that isn't this is policy entrepreneurism
or one of the other 5 errors.

---

## ML-Paper-Specific Advice (Distinguished from General Writing)

This is **ML-specific by construction** — it is the ICML 2022 reviewer
tutorial. Cross-applicability:

- **KDD '26 reviewers were not trained on this deck**, but KDD follows the
  same broad ML-conference review conventions. The deck's categories are
  field-universal; specific slide-number citations work only for ICML.
- **For KDD, cite the principle, not the slide.** "Recent ML community
  reviewer guidance (ICML 2022, NeurIPS) discourages SOTA-must-beat as a
  rejection criterion" is the right cite form for KDD rebuttal.
- **The deck explicitly mentions arXiv:** slide 19 says "no such policy" to
  "must beat arXiv papers" — this is more important for ICML (which has
  no anonymization period) than KDD; KDD anonymization is more strict.

---

## Connection to SiS / CTPC

The deck is operationally upstream of [[CTPC-KDD-Submission]]'s rebuttal
strategy. Predicted reviewer objections and pre-mapped refutations:

| Predicted objection | Refutation source from this deck |
|---|---|
| "Why not compare against Latent ODE / NeurODE?" | Already in paper; deck slide 11 (comprehensive flaw check covered) |
| "The MSE improvement (64%) is incremental" | Slide 17 novelty fallacy + slide 20 intellectual laziness |
| "No theoretical proof of d̄² → 1" | Slide 19 — "You must have a theorem! → No such policy!" |
| "Not novel — just NCDE + Cholesky" | Slide 17 — "Small but clever adjustment to SOTA" is explicitly acceptable |
| "Out-of-scope for KDD — too aerospace" | Slide 10 — historically rare; "sufficient interest to some ICML audience" → SDA + SafetyML audiences exist at KDD |
| "Dataset not released" | Slide 19 — "No such policy!" (note KDD norms may differ from ICML — verify) |
| "Should benchmark on more satellites" | Slide 11 + slide 20 — is this making the main knowledge advancement invalid? |
| "Method X also does this" | Slide 18 — must cite specific X with reference; blank "this has been done" is anti-pattern |

For the four-symmetry framing of [[CTPC-Design-Rationale]] Part III: the
deck's "knowledge advancement" criterion maps to the contribution claim;
the "well grounded" criterion maps to the empirical verification protocol
(d̄² ≈ 1, frame-equivariance, concept-closure tests).

---

## Connections

- [[ICML-2022-Review-Form]] — the formal scoring rubric this tutorial
  complements; treat as joint operational gate
- [[NeurIPS-2025-Reviewer-Guidelines]] — successor document; delta captured
  there (note: file originally filed as "2025 Reviewer Guidelines" with no
  venue prefix; identified at ingest as NeurIPS not ICML)
- [[ICML-Best-Practices]] — author-side checklist that anticipates these
  reviewer behaviors
- [[Ramdas-Paper-Checklist]] — pre-submission yes/no audit that prevents
  most error categories
- [[Lipton-Writing-Heuristics]] — anti-hostage-to-fortune + grounding
  voice (helps the "be grounded" mandate)
- [[Freeman-Writing-Papers]] — reviewer-POV is Freeman's slide 34+ arc
- [[CTPC-KDD-Submission]] — direct pre-submission + rebuttal target
- [[CTPC-Design-Rationale]] — Q5 formal verification helps the
  "well-grounded" criterion empirically

---

## Open Questions

1. **KDD-specific reviewer tutorial:** does KDD 2026 have its own equivalent
   training deck? If yes, ingest it; if no, this ICML deck is the closest
   proxy and SiS should treat it as the operational standard.
2. **Score → meaning mapping:** the 1-10 score rubric lives in
   [[ICML-2022-Review-Form]] (already ingested) — confirm cross-reference is
   complete during pre-submission audit.
3. **Slide-number citation strategy in rebuttal:** is it tactical to cite
   "ICML 2022 Reviewer Tutorial slide 19" in a KDD rebuttal? Probably not —
   cite the principle. Open for Bilal to decide closer to submission.

---

## Sources

- `raw/writing/guides/ICML 2022 How to be a good reviewer-tutorials for ICML2022 reviewers.pdf`
  — 22-slide deck, ICML 2022 Program Chairs, based on CVPR 2022 reviewer
  slides.
- All 22 slides read in this ingest.
- Slide-number citations throughout this page refer to PDF page numbers (=
  slide numbers in the source).
- Cross-confirmed against [[ICML-2022-Review-Form]] for scoring rubric (not
  duplicated here).
