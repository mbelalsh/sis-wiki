---
title: Goldreich — How to Write a Paper
tags: [writing, goldreich, reader-needs, theoretical-cs, weizmann]
sources:
  - raw/writing/guides/re-writing.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Goldreich — How to Write a Paper

**Identified title and authors:**

**Oded Goldreich**, Department of Computer Science and Applied
Mathematics, Weizmann Institute of Science, Rehovot, Israel.
oded.goldreich@weizmann.ac.il. **Title:** "How to write a paper."
**Date:** March 2004, last revised November 8, 2015.

**Cited in:** [[ICML-Best-Practices]] resource list as "Oded Goldreich:
How to write a paper?" **Note:** the file in the SiS corpus is named
`re-writing.pdf` but the actual title is *How to write a paper*; this
is a revised + augmented version of Goldreich's earlier essay "How NOT
to write a paper" (Spring 1991, revised Winter 1996).

## Self-Critique Opening (Verbatim — Sets Stance)

> "It is strange that I should write an essay about how to write a
> paper, because I consider myself quite a bad writer. Still, it seems
> that nobody else is going to write such an essay, which I feel is in
> great need in light of the writing quality that is common in our
> community."

(Footnote: "My obsessive use of footnotes is merely one of the bad
aspects of my bad writing style.")

## Structure (TOC, Verbatim)

1. Introduction
2. Why do we write papers or on the scientific process
3. **How to serve the reader's needs** — 3.1 Focusing on readers' needs
   rather than writer's desires · 3.2 Awareness to the knowledge level
   of the reader · 3.3 Balancing between contradictory requirements ·
   3.4 Making reading a non-painful experience
4. Some concrete suggestions — 4.1 Title and authors · 4.2 Abstract ·
   4.3 Introduction · 4.4 Technical part · 4.5 Conclusions / further
   work not a "must" · 4.6 References and acknowledgments
5. Benefiting from readers' comments

## §2 — Why Do We Write Papers (Verbatim Core)

> "The purpose of writing scientific papers is to communicate an idea
> (or a set of ideas) to people who have the ability to either carry
> the idea even further or make other good use of it."

> "An idea can be a new way of looking at objects (e.g., a 'model'), a
> new way of manipulating objects (i.e., a 'technique'), or new facts
> concerning objects (i.e., 'results'). **If no such idea can be
> identified, then one should reconsider whether to write the paper at
> all.**"

**Who is the relevant community?**

> "We believe that the relevant community includes not only the experts
> working in the area, but also their current and future graduate
> students as well as current and future researchers that do not have
> a direct access to one of the experts. **We believe that it is best
> to write the paper taking one of these less fortunate people as a
> model of the potential reader.**"

The "graduate-student-at-the-beginning-of-graduate-studies" reader is
Goldreich's recommended model — anchors clarity at the right level.

## §3.1 — Reader's Needs vs Writer's Desires (Five Symptoms, Verbatim)

Five symptoms of writer-centric (rather than reader-centric) writing:

1. **The "Checklist" Phenomenon:** "The writer wishes to put in the
   paper everything he knows about the subject matter. [...] In extreme
   cases, the writer has a list of things he wants to say and his only
   concern is that they are all said *somewhere* in the paper. Clearly,
   such a writer has forgotten the reader."

2. **Obscure Generality:** "The writer chooses to present his ideas in
   the most general form instead of in the most natural (or easy to
   understand) one. Utmost generality is indeed a virtue in some cases,
   but even in these cases one should consider whether it is not
   preferable to present a meaningful special case first."

3. **Idiosyncrasies:** "Some writers tend to use terms, phrases and
   notations that only have a personal appeal (e.g., some Israelis use
   notations which are shorthand for Hebrew terms...). **The
   justification to using a particular term, phrase or notation should
   be its appeal to the intuition or the associations of the reader.**"

4. **Lack of Hierarchy/Structure:** "Some people can maintain and
   manipulate their own ideas without keeping them within a
   hierarchy/structure. But it is very rare to find a person who will
   not benefit from having new ideas presented to him in a
   structured/hierarchical manner. Specifically, the write-up should
   make **clear distinctions between the more important
   ideas/statements and the less important ones.**"

5. **"Talmud-ism":** "The writer explores all the subtleties and
   refinements of his ideas when first introducing them and before
   clarifying the basic ideas. Furthermore, the writer discusses all
   possible criticisms (and answers them), before providing a clear
   presentation of the basic ideas."

## §3.2 — Awareness to Reader's Knowledge Level (Verbatim Highlights)

- "Whenever presenting a complex concept/definition, **beware that the
  reader cannot be assumed to fully grasp the new concept and all its
  implications immediately.**"
- "Whenever presenting proofs be sure to **elaborate on the conceptual
  steps rather than on the standard technical analysis.** [...]
  conceptual steps are typically the most important ideas in the paper
  and the ones with which the readers have most difficulties."
- "One should try to avoid treating the general case with all its
  complications in one shot. Thus, one may first present a special
  case that captures the main ideas..."
- "Don't hide a fundamental difficulty by using a definition that
  ignores it without first discussing the issue."
- "**Try to minimize the amount of new concepts and definitions you
  present. The reader's capacity of absorbing concepts and definitions
  is bounded.**"

## §3.4 — Making Reading Non-Painful (Four Anti-Patterns)

1. **Labyrinth of implicit pointers:** "The words 'it' and 'this' are
   commonly used as implicit pointers to entities mentioned in previous
   sentences, but the reader can find it difficult to figure out to
   which entities the writer was referring." Solution: explicit
   references to objects by name.

2. **Sentences with complex logical structure:** conditional sentences
   with nested conditions ("if X and Y or Z then P or Q"). Split.

3. **Mixture of mathematical symbols and text:** "On input x, y, A runs
   B^y on f(x)" — too dense. Better: "on input x and y, algorithm A
   runs the oracle machine B on input f(x) placing y on B's oracle
   tape."

4. **Cumbersome notations and terms:** examples like M_{i,j,k_t}^{O_b^c}
   or "(a,b,c,d,e,f,g,h,i,j)-system" or "kuku-muku popo-toto system."
   ("Yes, we have seen such things!")

## §4.1-4.2 — Title + Abstract (Verbatim Highlights)

**Title:** "informative as possible and yet not too cumbersome or too
long [...] one should also bear in mind that the paper's title should
fit into a sequence of past and future work."

**Author list:** alphabetical order is the convention in theoretical
CS; non-alphabetical only when contribution is highly unequal, but
even then "consider finding alternative ways to compensate."

**Abstract:**

> "**The abstract should be self-contained.** On the other hand, the
> abstract should not be long (because then it stops being an
> abstract). Typically, the abstract should not exceed 200 words."

Three things the abstract need NOT contain:
- Need not motivate the model (intro does this).
- Need not list/recall prior work.
- Need not provide accurate description of results (may describe in
  "loosely speaking" terms).

(Pages 6-8 covering §4.3 Introduction, §4.4 Technical Part, §4.5-4.6
not captured in this ingest read.)

## Core Actionable Rules (Compressed)

1. **Identify a single idea** before deciding to write the paper.
2. **Write for the graduate-student model reader** — intelligent +
   basic background but not expert.
3. **Reader-centric, not writer-centric** — beware the five symptoms:
   checklist phenomenon, obscure generality, idiosyncratic notation,
   lack of hierarchy, Talmud-ism.
4. **Hierarchy: distinguish important from less-important ideas
   conspicuously.**
5. **Present special case before general case.** "Postpone the more
   general statement, and prove it by a modification of the basic
   ideas."
6. **Elaborate proof's conceptual steps**, not the standard technical
   analysis.
7. **Minimize new concepts/definitions.** Reader capacity is bounded.
8. **No implicit pointers** (avoid "it" and "this" as cross-sentence
   references).
9. **Split sentences with complex logical structure.**
10. **Translate math-symbol-density into prose** ("on input x, y" →
    "on input x and y, algorithm A...").
11. **No cumbersome notations** (M_{i,j,k_t}^{O_b^c} is bad).
12. **Title fits a sequence** of past + future work; not too long, not
    too short.
13. **Alphabetical author order** is the TCS convention.
14. **Abstract ≤ 200 words, self-contained at high level only.**
15. **Future work / conclusions are NOT a must.**

## Reviewer-Facing Implications

Goldreich's framing is **reader-centric, not reviewer-centric** — but
the reader-centric model produces a paper that satisfies reviewers as
a side effect.

The "graduate student model reader" matches the kind of reader an ML
reviewer typically is: smart, broad background, but not a deep expert
in your specific subfield. Writing for this model directly raises
review scores.

The five symptoms (§3.1) are a checklist of writing pathologies a
reviewer can call out by name. **Lack of hierarchy** in particular is
a common reviewer complaint phrased as "the paper does not clearly
distinguish its main contribution from secondary observations."

## ML-Paper-Specific Advice (Distinguished from General Writing)

Goldreich writes from theoretical CS background; the advice
**transfers to ML papers** with minor calibration. ML-specific notes:

- The **alphabetical-author-order** convention (§4.1) is THEORETICAL-
  CS-specific. ML follows contribution-ordered authorship.
- The **§3.2 "present special case before general case"** is highly
  applicable to ML — present a single-task / single-dataset result
  first, then generalize.
- **Abstract ≤ 200 words** matches NeurIPS / ICML / KDD norms.

## Connection to SiS / CTPC

- **§3.1 Checklist phenomenon:** SiS has many results (PHNN + Cholesky
  + RTN + Student-t + d̄² ≈ 1 + 64% MSE reduction); the
  [[CTPC-KDD-Submission]] must avoid listing all of these and focus on
  the single contribution claim.
- **§3.2 special-case-before-general:** Year-1 CTPC on NASA CDDIS is
  the special case; the general framework (potentially extending to
  other satellites, other regimes) is the generalization. Present
  special case first.
- **§3.4 implicit pointers:** SiS architecture diagrams contain many
  components; cross-sentence references should be explicit ("the
  Latent NCDE Corrector" not "it" / "this").
- **§4.2 abstract ≤ 200 words:** KDD requires this; verify current
  draft.
- **§4.5 future work NOT a must:** matches [[Freeman-Writing-Papers]]
  slide 13's "I can't stand future work sections."

## Connections

- [[ICML-Best-Practices]] — cites Goldreich
- [[Freeman-Writing-Papers]] — agrees on future-work-not-required
- [[Peyton-Jones-Research-Paper]] — overlaps on reader-first framing
- [[How-To-Write-1]] — Igor Pak also recommends Goldreich
- [[CTPC-KDD-Submission]] — pre-submission audit target

## Open Questions

1. **§4.3-4.6 not captured** — Introduction, Technical part,
   Conclusions, References sections of Goldreich's essay are on pages
   6-8 not read in this ingest. Worth follow-on read during KDD '26
   revision phase.

## Sources

- `raw/writing/guides/re-writing.pdf` — Oded Goldreich, Weizmann Institute of
  Science. March 2004, last revised November 8, 2015.
- Identified via title page line 1: "How to write a paper. Oded
  Goldreich. March 2004 (last revised November 8, 2015)."
- File-naming note: the user filed this as `re-writing.pdf` but the
  actual title is "How to write a paper." It is the revised version of
  Goldreich's earlier essay "How NOT to write a paper" (Spring 1991,
  revised Winter 1996).
- Pages 1-6 read in this ingest.
