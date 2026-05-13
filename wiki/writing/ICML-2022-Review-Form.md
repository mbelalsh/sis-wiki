---
title: ICML 2022 Review Form — Ground Truth for ML Reviewer Evaluation
tags: [writing, reviewer-facing, icml, rubric, ground-truth, evaluation-axes]
sources:
  - raw/writing/guides/Review Form 2022.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# ICML 2022 Review Form — Ground Truth for ML Reviewer Evaluation

## One-Line Intuition

This is the **exact form ICML 2022 reviewers fill out** — every section, every
question, every binary recommendation. It is the operational ground truth for
what a writing-craft agent should optimize the paper toward.

## Why This Was Made the First Ingest

Per Mihir Bellare's epigraph at the top of the form: "Review the papers of
others as you would wish your own to be reviewed." The review form is
*simultaneously* the rubric your reviewers use AND the rubric you should
self-apply pre-submission. Every other writing guide in the corpus optimizes
toward subsets of these axes; this form is the union.

## The Six Evaluation Axes (Verbatim Headers + Verbatim Excerpts)

Each axis is evaluated **on its own merit**. Major-flaw shortcut allowed:
"if a paper has a major flaw, it is unnecessary to list all the issues (e.g.,
issues with writing)." But the review must explicitly tag issues as major
vs. minor.

### 1. Summary of contributions

> "Summarize the contributions made in the paper with your own words. Aim for
> precision and conciseness. This part of the review serves the purpose of
> showing to the MR and the authors how much you understood of the paper and
> what you think the paper is about. This is not the point to evaluate the
> contributions for their strengths or correctness. Merely provide a summary
> of what the contributions are."

**ML-paper-specific implication:** if reviewers can't summarize your
contributions in their own words, your contribution statement isn't clear
enough. The abstract + intro must make the summary trivial to write.

### 2. Novelty, relevance, significance

> "Assuming the contributions of the paper stand, are they relevant for our
> community? Are they new? A precise justification is needed if the answer is
> no (or partially no, e.g., citations of precise results in earlier papers),
> so that the authors know how to fix the paper if it is fixable. Are the
> results sufficiently interesting to make this a sufficiently 'complete'
> (or, one may say, significant) paper? Note that **significance does not
> necessarily mean solving a major open problem. Small, interesting results
> can also be significant. Science is incremental. But there has to be a
> detectable, positive increment of sufficient interest.** Regarding
> relevance, keep in mind that the machine learning community has
> traditionally been quite open-minded to new ideas from different areas. So,
> think whether this result could benefit some sub-area of ML, or a part of
> the community, down the line."

**Actionable rule:** state the "detectable, positive increment" explicitly.
The reviewer must be able to point to a single sentence answering "what does
this paper add to what was known before?"

### 3. Soundness

> "A paper ideally makes claims, which should be well supported, either by
> theoretical arguments, or by experimental results. Either say, the paper is
> sound, or list the problems. Only list major problems. Any problem listed
> needs a justification: do not just say that a result is incorrect, include
> an explanation of why you think it is incorrect. For example, a proof may
> have some gaps, an experiment may fail to support a claim because of its
> design or outcome (or the lack of its outcome). For experimental papers,
> the paper may fail to use a sound experimental design (e.g., the data
> collection may have problems)."

**ML-paper-specific implication:** claim → support is the unit. For every
claim, ask "is the support theoretical, experimental, or both?" and "does the
support actually substantiate the claim or merely correlate with it?"

### 4. Quality of writing/presentation

> "Is the paper well organized and clearly written? Does it do a good job at
> explaining the novelty and the results? Does the paper include enough
> information needed to support the claims it makes? [...]
> **A superbly written experimental paper explains:** (1) why were the
> experimental conditions selected the way they were selected; (2) the
> subsequent choices of what to measure and what to plot (including perhaps
> what is not shown); (3) how the results obtained substantiate the claims
> made. The paper should follow standard, best practices (e.g., includes
> error bars, better yet, uses box plots or something similar unless you
> suspect near Gaussian variables, etc.)"

**Actionable rule (the three-question test for experimental writing):**

1. Why these conditions? (justification of experimental design choices)
2. Why these measurements / plots? (including what is *not* shown)
3. How do the results substantiate the claims? (closing the loop)

**Crossover with soundness:** "Sometimes soundness cannot be decided if the
claims in the paper are not well supported." Writing failure can mask as
soundness failure — the form explicitly invites the reviewer to flag this
ambiguity.

### 5. Literature

> "Is the paper appropriately placed into contemporary literature? If not, be
> specific about what is missing. [...] **The must-mention results are
> directly relevant to the topic of the paper.** [...] It is not reasonable
> to expect a paper to refer to unpublished works that appeared within one
> month before the submission deadline. **Concurrent works should be referred
> to, but cannot be held against the paper in terms of novelty.**"

**Actionable rules:**

- Must-mention bar: *directly* relevant to the topic. If a reviewer requests
  a citation that's not directly relevant, that's a defective review.
- Concurrent-work shield: referring to a concurrent work cannot reduce your
  novelty score; only directly-prior work can.
- 1-month grace period: anything posted within 1 month of submission cannot
  be required to cite.

### 6. Basis of review

> "Declare how much of the paper you read. E.g., 'I read the full paper,
> including all the proofs.'."

**Reviewer-facing implication for authors:** reviewers self-declare reading
depth. A scoring-without-reading review is detectable from this field. If
your proofs / appendices are critical, make their relevance to the main
contribution explicit so the reviewer is incentivized to read them.

## Summary Section (Strong/Weak Points)

> "List the strong and weak points of the paper, but also provide further
> input to whether (and why) you think the strengths or the weaknesses are
> dominating. For each point, **indicate the importance of the point at
> hand: is this a major (important, critical) strength/weakness, or a minor
> one?**"
>
> "When evaluating these points take into account that **some things are
> easy, while others are harder to fix.** Include a justification. Remember,
> that the goal is to publish innovative, interesting, correct, good papers.
> Could this paper be one of those worthy to be published at ICML?"

**Phase 1 binary recommendation (the only accept/reject call reviewers make):**

> "Should progress to phase 2: Yes/No"
>
> "**Only recommend no if you believe that the paper is not acceptable AND it
> cannot be fixed in simple ways.** Easy to fix issues are: Few missing
> references, a few trivial improvements to the presentation, some fixes in
> proofs that are within reach, some extra experiments that are nice to have
> but not essential to publish the paper. On the latter point, **if a paper
> would need substantially more experimental support, doing this is beyond
> the scope of the reviewing process.** We do not expect authors to run
> extensive new experiments during the rebuttal process: Simply, there is no
> time for this, nor is there time to properly reevaluate the outcome of
> these experiments."
>
> "Note that this is a recommendation and the MR has the right to overwrite
> it. **By default, papers with two negative recommendations are heading for
> rejection for Phase 1.** MRs are asked to check the reviews and the papers
> that receive two negative recommendations and they can reverse this default
> decision."

**Authorial-implication:** two negative Phase 1 reviews = default rejection
before rebuttal even happens. Optimize the abstract + intro + experimental
section to short-circuit a negative gut reaction in the first reviewer pass.

## The Two Yes/No Declarations

> "**In my review, I recommended a paper co-authored by myself to be cited
> in a revision of the paper.** [Yes/No]"
> (Visible to MR/SMR/PC only, not to other reviewers or authors.)
>
> "**Do you have concerns regarding the ethics of the research presented in
> the paper?** [Yes/No]"
> (Flag → triggers ethics-board review.)

## Reviewer-Facing Implications for Authors (Synthesized)

1. **Optimize for the six-axis on-its-own-merit evaluation.** A great paper
   on all six is the goal; a major flaw on one shortcuts evaluation of the
   others.
2. **Make "is it fixable in simple ways?" the implicit question of your
   abstract + intro.** A reviewer leaning toward Phase 1 No must be willing
   to assert non-fixability, which is a higher bar than just "could be
   better."
3. **Experimental papers face a three-question test** (why these conditions,
   why these measurements, how do results substantiate claims). Plan the
   experimental section to answer all three explicitly.
4. **Error bars / box plots are *expected*, not optional.** Single-point
   numbers without uncertainty estimates are a presentation deficiency by
   default.
5. **The "detectable, positive increment" bar is below "solving a major open
   problem."** Small + interesting is acceptable; vague + grandiose is not.
6. **Reviewer-recommended citations have a precise standard: directly
   relevant.** Pushback against off-topic citation demands is legitimate.
7. **Concurrent work doesn't reduce your novelty.** State concurrent work
   relationships explicitly; don't omit them out of fear.

## ML-Paper-Specific Notes (Distinguished from General Writing)

- **Error bars / box plots / Gaussian-or-not** is an ML-specific
  presentation expectation. General writing guides don't enforce this.
- **Soundness encompasses experimental design soundness**, not just
  logical soundness. "The data collection may have problems" is a
  soundness call.
- **The Phase 1 / Phase 2 split** is ML-conference-specific (NeurIPS, ICML,
  ICLR variants). General writing guides assume a single-round review.
- **Rebuttal cannot include substantial new experiments.** Plan the
  experimental section so all critical experiments are in the submission,
  not delayed to rebuttal.

## Connection to SiS / CTPC

The CTPC-KDD-Submission ([[CTPC-KDD-Submission]]) is the SiS work currently
in review-eligible state. This rubric applies directly. Every claim in
[[CTPC-Design-Rationale]] D1-D7 + Q1-Q8 should be re-evaluated against the
six-axis rubric before submission. Specific bridges:

- **Six-axis novelty/relevance/significance** → CTPC's "detectable, positive
  increment" is the d̄² ≈ 1 vs. d̄² > 20,000 baseline gap, the 64% MSE
  reduction at 4-day horizon, and the analytic-Σ-CTPC + CBM-CTPC + PHNN
  triad as compositional contributions.
- **Soundness for experimental papers** → the NASA CDDIS data choice
  justification, RTN-frame error decomposition, K-sample MC for
  uncertainty propagation must each answer "why this design".
- **Quality of writing** → the three-question test (why conditions / why
  measurements / how results substantiate claims) is a direct
  pre-submission audit.

## Connections

- [[Ramdas-Paper-Checklist]] — author-side pre-submission checklist
  complementing this reviewer-side rubric
- [[ICML-Best-Practices]] — ICML PC's prescriptive complement
- [[CTPC-KDD-Submission]] — the SiS submission this rubric directly applies to

## Open Questions

1. **KDD review form** — is this corpus the right rubric for the KDD '26
   target, or should an analogous KDD review form be ingested? The ICML
   form is widely-adopted-as-template across ML venues but KDD has its own
   reproducibility-checklist tradition.
2. **What is the most-recent ICML review form?** This is the 2022 version;
   the form has likely been revised. The 2022 capture is sufficient for
   structural understanding but specific binary fields (Phase 1/Phase 2
   logic) may have changed.

## Sources

- `raw/writing/guides/Review Form 2022.pdf` — ICML 2022 official review form,
  3 pages, full text captured verbatim above.
- Original URL: https://icml.cc/Conferences/2022/ReviewForm
- Epigraph: Mihir Bellare, https://youtu.be/SPVWSG7-i_E?t=1742
