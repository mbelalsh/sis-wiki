---
title: Ramdas — Checklist for Effectively Writing Papers in Stat-ML
tags: [writing, checklist, pre-submission, stat-ml, latex, mathematical-writing]
sources:
  - raw/writing/guides/https:www.stat.cmu.edu:~aramdas:checklists:aadi-paper-checklist.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Ramdas — Checklist for Effectively Writing Papers in Stat-ML

**Author:** Aaditya Ramdas, Carnegie Mellon University (aramdas@cmu.edu).
**Format:** Single-page checklist, 14 numbered items + closing sign-off.
**Role in this corpus:** **the writing agent's pre-submission standing
questions.** When a SiS paper is being readied for submission, every item
below must be answerable yes / handled.

## Closing Statement (Verbatim — sets the bar)

> "When you sign off on a paper, you should be proud of it, and ready to
> defend its details. Integrity is paramount."

## The 14 Checklist Items (Verbatim)

### 1. Naming, planning

> "Everything should be in one file, except possibly macros. Name the file
> intelligently, not `paper.tex`. **Plan papers in a top-down fashion: what
> is the paper's purpose, the point of each section, etc.**"

### 2. Use macros

> "Use macros for all symbols, in case they need to be changed. Name macros
> or labels so that you recognize them a year later. Use a `~` when referring
> to equations/sections/figures: write `Section~\ref{sec:power}`. Familiarize
> yourself with commands like `bmatrix`, `align`, `cases`, `subequations`,
> `mbox`, `hspace`, `vspace`, etc."

### 3. Abstracts

> "Abstracts need not detail every single advance made: summarize the setup
> and main results succinctly. **Avoid citations unless necessary. Avoid
> future tense in abstract and paper** (we will provide → we provide)."

### 4. Divide related work into two parts

> "(a) those who have studied aspects of the same problem and those whose
> work your paper directly builds on or uses insights from, (b) other
> somewhat orthogonal but complementary papers. I prefer to include (a) in
> the introduction, and possibly (b) either in the discussion or in an
> 'other related work' section at the end of the paper. **Be concise and to
> the point, and give fair credit when it is due.**"

### 5. Figures

> "Any text inside a figure, such as in a legend, must have a readable font
> size, almost matching but not exceeding the size of the other text. Any
> lines in a figure must have **multiple complementary sources of
> identifiable information**: for example, use color, texture and symbols
> (red vs. blue, dashed vs. dotted, triangles vs. circles). **Figures should
> have error bars; if negligible, explain why explicitly in the caption.**"

### 6. Equations are part of a sentence

> "Equations are often part of a sentence, so follow them with the
> appropriate punctuation (commas or full-stops). Don't number every
> equation in a proof, **only those that a reviewer/reader may want to
> refer to**."

### 7. Theorems versus rest

> "**Leave the annotation 'theorem' for a handful of powerful central
> results: trivial theorems reduce trust in your judgment.** Some may be
> propositions (interesting results of independent interest), or lemmas (a
> packaged technical result within a longer proof), or facts (known theorems
> from classical papers) or corollaries (interesting implications of
> theorems), or remarks. Number each type separately. Statements must be
> crisp and yet self-contained. Consider defining technical assumptions and
> necessary notation before the theorem and discussing what they mean and
> why they are needed. Even so, **refer to all assumptions, by number or
> phrase, in the theorem statement so that it is self-contained.**"

### 8. Interpret formal results

> "Interpret any theorem, proposition, corollary or lemma for the reader to
> provide intuition, and link to its proof. **Avoid superlatives like
> 'extremely general' or 'very powerful' or 'huge improvement'.** Organize
> proofs modularly, give proof outlines, and don't end them abruptly."

### 9. Mathematical writing

> "If possible, **don't use equations/references as nouns.** Change '(12)
> suggests' or '[12] claimed' to 'Definition/condition (12) suggests' or
> 'Maryam [12] claimed'. **Don't say 'This implies...' or 'So, ...';**
> instead try 'Invoking the constraint (4), we see that inequality (8)
> implies...'. Avoid ∀, ∃ and say 'for all, some' if possible. Change
> phrases like 'we get' to 'we derive/conclude/infer/...'. **Distinguish
> between a function f and its value f(x) at point x. A function f can be
> monotone, but f(x) cannot.** Similarly, distinguish between random
> variables Y and a particular instantiation y."

### 10. Flow

> "Sections should not start or end abruptly. **The last line of a section
> should serve as a natural connector to the next section.** Don't have
> sections with just one subsection. If you have one important point in a
> section, don't make a subsection, if you have two important and separate
> points, have two subsections."

### 11. Check bibliography

> "Check bibliography for capitalization, authors, spurious 'et al.', see
> if arXiv papers have been published."

### 12. Appendices

> "Appendices must be organized logically, preferably in order of their
> first reference."

### 13. Spelling and grammar

> "Spelling and grammar should be checked by software (eg: Copy text into
> Google Docs or Microsoft Word)."

### 14. Sign-off

> "When you sign off on a paper, you should be proud of it, and ready to
> defend its details. **Integrity is paramount.**"

## Core Actionable Rules (Numbered, for Standing-Question Use)

These compress the 14 items into pre-submission yes/no questions:

1. **Is the .tex filename meaningful** (not `paper.tex`)?
2. **Is the paper planned top-down** — does each section have a stated point?
3. **Are all symbols macroized**, with readable-a-year-later macro names?
4. **Does the abstract avoid future tense** ("we will provide" → "we provide")?
5. **Does the abstract avoid citations** unless strictly necessary?
6. **Is related work split into (a) direct-prior in intro and (b) orthogonal
   in discussion or end-section?**
7. **Do figures have ≥2 complementary identification channels** (color +
   texture + symbol)? **Error bars present, or absence justified in
   caption?**
8. **Are equations punctuated as sentence elements** (commas/full-stops
   after)? **Are only reference-worthy equations numbered?**
9. **Is "theorem" reserved for handful of central results**, with
   proposition/lemma/corollary/remark used appropriately for the rest?
10. **Are all theorem assumptions referenced explicitly in the theorem
    statement** so it stands alone?
11. **Is every formal result interpreted for the reader** with intuition +
    proof-link? **Superlatives avoided** ("extremely general", "very
    powerful", "huge improvement")?
12. **Are equations/references NOT used as nouns** ("(12) suggests" →
    "Inequality (12) suggests")?
13. **Are casual connectives ("This implies...", "So, ...") replaced** with
    explicit-substantive transitions?
14. **Is ∀/∃ replaced with "for all/some"** where possible?
15. **Are function vs. value distinguished** (f monotone vs. f(x))? **RV vs.
    realization distinguished** (Y vs. y)?
16. **Do section transitions connect** rather than start/end abruptly?
17. **Is "one subsection inside a section" pattern eliminated**?
18. **Are arXiv-only citations checked for journal publication?**
19. **Are appendices ordered by first reference**?
20. **Is software grammar-check run** (Google Docs / Word paste)?
21. **Sign-off test:** are you proud of this paper and ready to defend
    every detail?

## Reviewer-Facing Implications

This is a **first-person-author** checklist, not a reviewer rubric — but
every item maps to a reviewer-facing weakness that triggers under-the-axis
score reductions:

- Items 7-15 (theorems, proofs, math writing) → ICML soundness + writing
  quality.
- Items 4, 11 (related work, no superlatives) → ICML literature +
  novelty/relevance/significance.
- Items 5, 6 (figures, equations) → ICML writing quality.
- Items 1-3, 10, 12-13 (top-down planning, flow, sign-off) → ICML writing
  quality + the reviewer's implicit "is this paper internally coherent" test.

## ML-Paper-Specific Advice (Distinguished from General Writing)

This is a **stat-ML-specific** checklist. ML-specific items:

- **Item 2:** LaTeX-specific macros (`bmatrix`, `align`, `subequations`,
  `cases`). These come from typesetting matrix/equation-heavy math papers,
  not general prose.
- **Item 7:** the theorem/proposition/lemma/corollary/remark stratification
  is mathematical-writing convention — general writing doesn't have it.
- **Item 9:** function-vs-value, RV-vs-realization distinctions are
  stat-ML-specific.
- **Item 11:** arXiv-publication check is ML-community-specific (papers
  flow between arXiv preprint and journal versions in ways absent from
  most disciplines).

General-writing-adjacent items (8, 10, 12, 13, 20) overlap with Strunk &
White / Milne advice but are restated in stat-ML voice.

## Connection to SiS / CTPC

Pre-submission audit for [[CTPC-KDD-Submission]] should run through every
one of the 21 compressed checklist items. Several SiS-specific
contact-points:

- **Item 4 (related work split):** SiS-specific direct-prior is
  [[NODE]] / [[Dissecting-NODE]] / [[Dissipative-SymODEN]] / [[PINN-Challenges]];
  orthogonal-complementary is the bulk of the geometric-DL paper corpus.
  Year-1 paper should split accordingly.
- **Item 5 (figures with multiple identification channels + error bars):**
  CTPC's d̄² ≈ 1 vs. baselines' d̄² > 20,000 result needs error-bar
  treatment; the K-sample MC uncertainty already gives a natural
  uncertainty source.
- **Item 7 (theorem reservation):** SiS's main theorem candidate is the
  Year-2 Analytic-Σ-NCDE theorem from [[Analytic-Sigma-CTPC-Composition]];
  Year-1 likely has propositions / lemmas only.
- **Item 9 (function vs. value):** Cholesky parameterization `R = LLᵀ`
  carefully distinguishes the *parameterization map* from a *specific
  instantiation* — this is the SiS-PHNN-precision the agent must enforce
  in writing.
- **Item 14 (sign-off, integrity paramount):** the Barbiero audit-trail
  pattern in [[CTPC-Design-Rationale]] Part III (four substantive
  errors caught via canonical-source ingest) is the SiS implementation of
  the integrity bar — corrections are visible, not hidden.

## Connections

- [[ICML-2022-Review-Form]] — reviewer-side companion to this author-side
  checklist
- [[ICML-Best-Practices]] — ICML PC's prescriptive complement
- [[Milne-Tips-Authors]] — mathematical-writing tips (Milne overlaps with
  items 6, 7, 8, 9, 10)
- [[Strunk-White-Elements-Style]] — general-writing companion (items 12,
  13, 20)

## Open Questions

1. **Order of priority across the 21 items?** The checklist is flat — but
   in practice some items (theorem reservation, related work split,
   abstract tense) are higher-leverage than others (arXiv-publication
   recheck). A weighted version would be more operationally useful.
2. **Pre-submission timing?** Items 1-4 (planning, naming, macros,
   abstract) belong at draft-start; items 11-13, 18-20 belong at final
   polish. The flat checklist hides this temporal structure.

## Sources

- `raw/writing/guides/https:www.stat.cmu.edu:~aramdas:checklists:aadi-paper-checklist.pdf`
  — single page, 14 bullets + closing line. Author: Aaditya Ramdas
  (Carnegie Mellon University, Department of Statistics & Data Science +
  Machine Learning Department).
- Original location:
  https://www.stat.cmu.edu/~aramdas/checklists/aadi-paper-checklist.pdf
