---
title: Bertsekas — Ten Simple Rules for Mathematical Writing
tags: [writing, mathematical-writing, bertsekas, composition-rules, structured-style]
sources:
  - raw/writing/guides/Ten_Rules.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Bertsekas — Ten Simple Rules for Mathematical Writing

**Author:** Dimitri Bertsekas, M.I.T. **Format:** 28-slide talk,
April 2002. **Cited in:** [[ICML-Best-Practices]] resource list as
"D. Bertsekas: Ten Simple Rules for Mathematical Writing."

## Why This Talk Was Given (Verbatim)

> "Experience is something you get only after you need it..."
>
> "One current model: **The conversational style**" — Halmos's
> "mathematics should be written so that it reads like a conversation
> between two mathematicians on a walk in the woods." Bertsekas's
> critique: this is "very hard to teach to others" (Halmos: "Effective
> exposition is not a teachable art. There is no useful recipe...") and
> "controversial" (where do proofs start and end? what are the
> assumptions? etc.).
>
> **"Instead we will advocate a structured style"** — offers specific
> verifiable rules that students can follow and thesis advisors can check.

This is the operational complement to Halmos/Krantz: where they
emphasize an art of writing, Bertsekas extracts checkable rules.

## What Is Mathematical Writing (Verbatim Definition)

> "Writing where mathematics is used as a primary means for expression,
> deduction, or problem solving.
>
> **Examples that are:** Math papers and textbooks · Analysis of
> mathematical models in engineering, physics, economics, finance, etc
>
> **Examples that are not:** Novels, essays, letters · **Experimental /
> nonmathematical scientific papers and reports**"

⚠ **SiS implication:** the experimental side of ML papers is *not*
the focus here. Bertsekas applies to the theorem/proof/derivation
sections of an ML paper, not the experimental section. Pair with
[[ICML-Best-Practices]] §10 for the experimental side.

## What Is Different About Math Writing (Verbatim)

> "Math writing blends **two languages** (natural and math):
> Natural language is rich and allows for ambiguity. Math language is
> concise and must be unambiguous.
>
> Math writing requires **slow** reading: Often expresses complex ideas.
> Often must be read and pondered several times. Often is used as
> reference. **Usually must be read selectively and in pieces.**"

The "read selectively in pieces" point bridges to Bertsekas's segment
concept below: math documents are non-linearly accessed.

## Rules of the Game (Three-Tier Taxonomy)

1. **Small rules** — apply to a single sentence (sentence structure,
   mathspeak conventions, comma rules). Verifiable.
2. **Broad rules** — apply to entire document (general style + writing
   strategy). **Non-verifiable** (e.g., "organize", "be clear and
   concise").
3. **Composition rules** ← **the focus of this talk.** Relate to how
   parts of the document connect. Apply to multiple sentences.
   **Verifiable.**

## Small Rules (Examples — Verbatim Highlights)

### 2-3-4 rule (sentence/paragraph length)

> "Consider splitting **every sentence of more than 2 lines, every
> sentence with more than 3 verbs, and every paragraph with more than 4
> 'long' sentences.**"

### Mathspeak readability

> "BAD: Let k>0 be an integer. **GOOD:** Let k be a positive integer
> *or* Consider an integer k>0."
>
> "BAD: Let x ∈ Rⁿ be a vector. **GOOD:** Let x be a vector in Rⁿ *or*
> Consider a vector x ∈ Rⁿ."

### Don't start a sentence with mathspeak

> "BAD: Proposition: f is continuous. **GOOD:** Proposition: The function
> f is continuous."

### Additional small rules

- "Use active voice ('we' is better than 'one')."
- "Minimize 'strange' symbols within text."
- "Make proper use of 'very,' 'trivial,' 'easy,' 'nice,' 'fundamental,'
  etc." (overlaps with [[Lipton-Writing-Heuristics]] #15)
- "Use abbreviations correctly (e.g., cf., i.e., etc.)"
- Comma rules. "Which" and "that" rules.

## Broad Rules (Examples — Verbatim Highlights)

> "Language rules/goals to strive for: **precision, clarity, familiarity,
> forthrightness, conciseness, fluidity, rhythm**."
>
> "Organizational rules (how to structure your work, how to edit,
> rewrite, proofread, etc)"
>
> Halmos quotes: **"Down with the irrelevant and the trivial"** ·
> **"Honesty is the best policy"** · **"Defend your style"** (against
> pushy copyeditors).

## Math Writing Without Math Proofs

A distinct genre Bertsekas calls out:

> "Rigorous proofs are the essence of mathematical writing. A
> mathematician relies on proofs to gain intuition... **but many readers
> prefer no detailed proofs.**"
>
> "**Intuitive math writing:** An alternative to a proof-based development
> (works in some settings): Explain mostly in words (some) math results,
> and give refs. State precisely a few (if any) theorems... place (some)
> proofs in appendixes. **Use suggestive natural language to describe the
> intuition behind theorems/algorithms.**"
>
> "A challenge: **Intuitive math writing is trickier/more demanding than
> rigorous proof-based writing.**"
>
> "Example: Bertsekas/Tsitsiklis probability book."

### Tips for intuitive math writing (Verbatim)

- **"Don't cut corners: Better to skip a proof than to give a sloppy
  proof."**
- "Maintain rigor in the use of natural language: Without math, precise
  language becomes more important. Define terms rigorously and use them
  consistently. Don't use multiple terms with the same meaning. **Avoid
  ambiguous, undefined, or loose terms** (e.g., don't use 'random
  values' or 'random samples' instead of 'random variables';
  'likelihood' or 'chance' instead of 'probability')."
- "Provide enough explanation/intuition (perhaps in footnotes) so a
  mathematician can believe your argument and even construct a proof."
- "Use good examples to illustrate key proof idea."

## **THE TEN COMPOSITION RULES (Verbatim, Numbered)**

Grouped into three meta-categories:

### Structure rules (break it into digestible pieces)

1. **Organize in segments**
2. **Write segments linearly**
3. **Consider a hierarchical development**

### Consistency rules (be boring creatively)

4. **Use consistent notation and nomenclature**
5. **State results consistently**
6. **Don't underexplain — don't overexplain**

### Readability rules (make it easy for the reader)

7. **Tell them what you'll tell them**
8. **Use suggestive references**
9. **Consider examples and counterexamples**
10. **Use visualization when possible**

### Expansion of Rule 1 — Organize in segments (Verbatim)

> "Composition is the strongest way of seeing" (Weston)
>
> "Extended forms of composition have a fundamental unit: Novel →
> Paragraph. Film → Scene. Slide presentation → Slide. Evening news
> program → News report.
>
> **Key Question:** What is the fundamental unit of composition in math
> documents?
>
> **Answer: A segment**, i.e., an entity intended to be read comfortably
> from beginning to end. **Must be not too long to be tiring, not too
> short to lack content and unity.**"

**Examples of segments:**

- A mathematical result and its proof
- An example
- Several related results/examples with discussion
- An appendix
- A long abstract
- A conclusions section

**Segment properties:**

- "Should 'stand alone' (identifiable start and end, transition
  material)."
- "Length: **1/2 page to 2-3 pages.**"

(Detailed elaboration of Rules 2-10 in slides 16-28 not fully captured
in this ingest read; the rule names + grouping are the structural
summary.)

## Core Actionable Rules (Compressed)

**Small rules (sentence-level):**

1. **2-3-4 rule:** split sentences > 2 lines, > 3 verbs, paragraphs > 4
   long sentences.
2. **Mathspeak readability:** "Let k be a positive integer" not "Let
   k>0 be an integer."
3. **No sentence starts with mathspeak.**
4. **Active voice; "we" over "one".**
5. **Minimize strange symbols in text.**
6. **Use intensifiers ("very", "trivial", "easy", "nice", "fundamental")
   correctly.**
7. **Correct abbreviations (cf., i.e., etc.).**

**Broad rules (document-level):**

8. **Strive for: precision, clarity, familiarity, forthrightness,
   conciseness, fluidity, rhythm.**
9. **Down with the irrelevant and the trivial. Honesty is the best
   policy. Defend your style.**

**Composition rules (multi-sentence, verifiable) — The Ten:**

10. Organize in segments (1/2 to 2-3 pages, stand alone).
11. Write segments linearly.
12. Consider a hierarchical development.
13. Use consistent notation and nomenclature.
14. State results consistently.
15. Don't underexplain; don't overexplain.
16. Tell them what you'll tell them.
17. Use suggestive references.
18. Consider examples and counterexamples.
19. Use visualization when possible.

**Intuitive math writing specifics:**

20. Don't cut corners — skip a proof rather than give a sloppy one.
21. Define terms rigorously and use them consistently.
22. Avoid ambiguous/undefined/loose terms.
23. Provide intuition so a mathematician can reconstruct the proof.

## Reviewer-Facing Implications

The Ten Composition Rules are **verifiable** — they're not advisory,
they're checklist items. A thesis advisor or reviewer can ask:

- Is the paper organized in segments of 1/2 to 2-3 pages?
- Is notation consistent throughout?
- Are results stated in a consistent form?
- Are examples + counterexamples present?
- Is visualization used where possible?

The intuitive-math-writing tips bridge to ML reviewer expectations:
[[ICML-2022-Review-Form]] §"Quality of writing" "A superbly written
experimental paper" parallels Bertsekas's intuitive math writing — both
emphasize natural-language precision over formula density.

## ML-Paper-Specific Advice (Distinguished from General Writing)

Bertsekas explicitly **excludes** experimental ML papers from his scope
("Experimental/nonmathematical scientific papers and reports" are NOT
math writing). So this resource applies to:

- The theorem/proof section of an ML paper (e.g., the Year-2 Analytic-Σ-NCDE
  theorem statement in SiS).
- Method-derivation sections (e.g., the Cholesky-PSD R = LLᵀ
  derivation).
- Definitions sections (e.g., port-Hamiltonian J-R structure
  definition).

It does NOT directly apply to:

- Experimental design choices (use [[ICML-Best-Practices]] §10).
- Benchmark / dataset documentation.
- Reproducibility checklist items.

**The "intuitive math writing" mode** (Bertsekas Slides 11-12) is the
operationally relevant mode for ML papers — most ML papers don't have
proofs and use intuitive-mathematical natural language. Bertsekas's tip
that **intuitive math writing is HARDER than rigorous proof-based
writing** is the key insight: when you don't have proofs to anchor
precision, every natural-language phrase must be even more rigorous.

## Connection to SiS / CTPC

- **The "segment" frame is the unit of writing audit** for
  [[CTPC-KDD-Submission]]. Audit: is each section/subsection a coherent
  1/2-to-2-3-page segment? Or are some too long (a 4-page methods
  section needs splitting) or too short (a 1-paragraph subsection
  should be merged)?
- **Composition Rule 4 (consistent notation):** SiS notation has many
  symbols (`J`, `R`, `D`, `L`, `H`, `Ḣ`, `η`, `Σ`, `μ`, `ν`, `K`).
  Audit the paper for one symbol-per-concept consistency.
- **Composition Rule 5 (state results consistently):** all SiS
  empirical results should follow the same "metric ± uncertainty on
  [dataset] at [horizon]" template.
- **Composition Rule 6 (don't under/over-explain):** the Cholesky
  derivation in [[Goldstein-Ch8-J-vs-PH-Cholesky-R]] — the synthesis
  page — was explicit about the R-side algebra. The published version
  should compress this to the level the venue's audience needs.
- **Intuitive math writing (Tips slide 12):** SiS's "physics-as-architecture"
  framing is intuitive math writing — define "hard architectural
  constraint" rigorously and use the term consistently (don't oscillate
  with "structural constraint", "baked-in physics", etc.). [[CLAUDE.md]]
  vocabulary table is the existing version of this discipline.

## Connections

- [[ArXiv-1612-Writing-Guide]] — Krantz's primer overlaps heavily; Bertsekas
  is the "structured style" alternative to Krantz's Halmos-tradition art.
- [[Lipton-Writing-Heuristics]] — Lipton's #14-#16 (short sentences,
  jettison intensifiers, subject-verb agreement) overlap with Bertsekas
  small rules.
- [[Ramdas-Paper-Checklist]] — items 7 (theorem reservation), 8
  (interpret theorems), 9 (math writing) overlap.
- [[ICML-Best-Practices]] — explicitly cites this talk; complements with
  experimental-side reproducibility checklist.
- [[CTPC-KDD-Submission]] — pre-submission audit target.

## Open Questions

1. **Full expansion of Rules 2-10.** This summary captures Rule 1
   (segments) in depth; Rules 2-10 are named but not fully
   characterized. Worth a follow-on read of slides 16-28 when the
   SiS paper's revision phase begins.

## Sources

- `raw/writing/guides/Ten_Rules.pdf` — Dimitri Bertsekas, M.I.T., April 2002,
  "Ten Simple Rules for Mathematical Writing" talk, 28 slides.
- Identified via cross-reference in [[ICML-Best-Practices]] resource list.
- Slides 1-15 read in detail. Slides 16-28 (rule-by-rule expansion)
  summarized from rule names only.
- Bibliography (slide 6) of cited sources: Strunk-White, Fowler-Aaron,
  Venolia, Halmos, Knuth, Kleiman, Krantz, Higham, Alley, Thomson.
