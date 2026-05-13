---
title: Krantz — A Primer of Mathematical Writing (2nd ed, arXiv:1612.04888)
tags: [writing, mathematical-writing, book, krantz, halmos-tradition, exposition]
sources:
  - raw/writing/guides/1612.04888v1.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Krantz — A Primer of Mathematical Writing (2nd ed)

**Identified title and authors (per user's special instruction):**

**Steven G. Krantz**, *A Primer of Mathematical Writing: Being a
Disquisition on Having Your Ideas Recorded, Typeset, Published, Read &
Appreciated*, **Second Edition**, December 16, 2016. Posted on arXiv as
**arXiv:1612.04888v1 [math.HO]** (15 Dec 2016). Dedicated to Paul Halmos
("for the example he has set for us all").

This is a **book**, ~200+ pages, also referenced in [[ICML-Best-Practices]]
resource list as "Steven G. Krantz: A Primer of Mathematical Writing (a
whole book)."

## Why This Resource Matters

Krantz is the standard reference for advanced mathematical writing in
the Halmos tradition. Unlike the ICML / Lipton / Peyton-Jones materials
that focus on conference papers, Krantz covers the full spectrum: papers,
books, exposition, opinion pieces, recommendations, referee reports,
talks, grants, CVs, even email. The cross-genre coverage makes it the
most comprehensive single resource in the writing corpus.

## Table of Contents (6 chapters, verbatim section list)

### Ch 1 — The Basics

1.1 What It Is All About · 1.2 Who Is My Audience? · 1.3 Writing and
Thought · 1.4 Say What You Mean; Mean What You Say · 1.5 Proofreading,
Reading for Sound, Reading for Sense · 1.6 Compound Sentences, Passive
Voice · 1.7 Technical Aspects of Writing a Paper · 1.8 More Specifics of
Mathematical Writing · 1.9 Pretension and Lack of Pretension · 1.10 We
vs. I vs. One · 1.11 Essential Rules of Grammar, Syntax, and Usage ·
1.12 More Rules of Grammar, Syntax, and Usage

### Ch 2 — Topics Specific to the Writing of Mathematics

2.1 How to Organize a Paper · 2.2 How to State a Theorem · 2.3 How to
Prove a Theorem · 2.4 How to State a Definition · 2.5 How to Write an
Abstract · 2.6 How to Write a Bibliography · 2.7 What to Do with the
Paper Once It Is Written · 2.8 A Coda on Collaborative Work

### Ch 3 — Exposition

3.1 What Is Exposition? · 3.2 How to Write an Expository Article · 3.3
How to Write an Opinion Piece · 3.4 The Spirit of the Preface · 3.5 How
Important Is Exposition?

### Ch 4 — Other Types of Writing

4.1 The Letter of Recommendation · 4.2 The Book Review · 4.3 The
Referee's Report · 4.4 The Talk · 4.5 Your Vita, Your Grant, Your Job,
Your Life · 4.6 Electronic Mail

### Ch 5 — Books

5.1 What Constitutes a Good Book? · 5.2 How to Plan a Book · 5.3 The
Importance of the Preface · 5.4 The Table of Contents · 5.5 Technical
Aspects: Bibliography/Index/Appendices · 5.6 How to Manage Your Time
When Writing a Book · 5.7 What to Do with the Book Once It Is Written

### Ch 6 — Writing with a Computer

6.1 Writing on a Computer · 6.2 Word Processors · 6.3 Using a Text
Editor · 6.4 Spell-Checkers, Grammar Checkers, and the Like · 6.5 What
Is TeX and Why Should You Use It?

## Core Actionable Rules (Structural, From TOC + Standard Krantz)

(Detailed contents of Ch 1-6 not fully captured in this ingest read; the
TOC structure itself is the structural-arc summary per user's
slide-deck-style instruction for §10. Krantz is a *reference* book —
read specific sections when their topic arises in SiS work.)

### Ch 1 (The Basics) themes

1. **Know your audience** (§1.2): write for a specific reader population.
2. **Writing IS thought** (§1.3): aligns with Peyton-Jones's
   "writing develops the idea."
3. **Say what you mean; mean what you say** (§1.4): the Halmos-tradition
   precision standard.
4. **Read for sound AND for sense** (§1.5): proofread by reading aloud +
   reading for logical sense separately.
5. **Avoid compound sentences and passive voice where possible** (§1.6):
   overlaps with [[Lipton-Writing-Heuristics]] #14, #16.
6. **Avoid pretension** (§1.9): overlaps with Lipton #15 (jettison
   intensifiers).
7. **Voice choice "We vs. I vs. One"** (§1.10): the standard mathematical-
   writing convention discussion.

### Ch 2 (Mathematical specifics) themes

8. **How to state a theorem** (§2.2): self-contained, assumptions
   referenced, no implicit context.
9. **How to prove a theorem** (§2.3): proof outline, modular
   organization, no abrupt endings — overlaps with [[Ramdas-Paper-Checklist]]
   item 8.
10. **How to state a definition** (§2.4): definitions must be crisp +
    contextualized.
11. **How to write an abstract** (§2.5): summarize, don't tease.
12. **What to do with the paper once written** (§2.7): submission +
    response-to-referees.

### Ch 3 (Exposition) themes

13. **Expository writing is different from technical writing** (§3.1):
    the audience is broader; precision is contextual.
14. **Prefaces have their own genre conventions** (§3.4): set
    expectations, not the same as introductions.

### Ch 4 (Other writing types) — particularly relevant

15. **Referee's report** (§4.3): operationally bridges to
    [[ICML-2022-Review-Form]] — the reverse-direction skill.
16. **The talk** (§4.4): conference-presentation conventions.
17. **Grant writing** (§4.5.2): proposal-specific conventions.

### Ch 6 (Computer + TeX)

18. **TeX is the mathematical-writing standard** (§6.5): operational
    given for any math/ML paper.

## Reviewer-Facing Implications

Krantz's Ch 4.3 "The Referee's Report" is uniquely valuable: it teaches
how to write a referee report, which inverts to what the author should
expect a reviewer to evaluate. This complements [[ICML-2022-Review-Form]]
by showing how a thoughtful referee thinks rather than just what fields
they fill in.

Ch 2.2-2.5 (theorem / proof / definition / abstract) defines the
mathematical-writing precision bar that ML reviewers — when the paper
has theoretical contributions — apply.

## ML-Paper-Specific Advice (Distinguished from General Writing)

Krantz is **mathematical-writing-specific**, not ML-specific. He
predates the modern ML conference-paper culture (Krantz 1st edition was
1997; 2nd edition 2016). The crossover with ML is:

- Ch 2.2-2.5 theorem/proof/definition apply to theoretical ML papers.
- Ch 1.11-1.12 grammar/syntax/usage apply universally.
- Ch 6.5 TeX/LaTeX is the operational ML-paper standard.

Krantz does NOT cover: experimental sections, reproducibility, code
release, benchmark datasets, dropout/hyperparameter conventions — those
gaps are filled by [[ICML-Best-Practices]] and [[Lipton-Writing-Heuristics]].

## Connection to SiS / CTPC

- **Ch 2.2 (How to State a Theorem) + Ch 2.3 (How to Prove a Theorem):**
  the Year-2 Analytic-Σ-NCDE theorem in [[Analytic-Sigma-CTPC-Composition]]
  is the canonical SiS theorem candidate. Krantz Ch 2 is the canonical
  reference for stating it.
- **Ch 4.3 (The Referee's Report):** Bilal serves as a reviewer for
  ICML/KDD; Krantz §4.3 is the reverse-direction craft companion to
  [[ICML-2022-Review-Form]].
- **Ch 4.5.2 (The Grant):** for AFRL / NASA grant proposals, Krantz §4.5
  is more applicable than the conference-paper guides.

## Connections

- [[ICML-Best-Practices]] — cites Krantz explicitly as resource
- [[Ramdas-Paper-Checklist]] — overlaps on theorem/proof conventions
- [[Crafting-ML-Papers]] — Langley's §4.6 (terminology/notation)
  overlaps with Krantz §1.4 / §1.8
- [[Strunk-White-Elements-Style]] — Krantz Ch 1.11-1.12 is the
  mathematical-writing analog
- [[Milne-Tips-Authors]] — Milne's playful style is a direct
  counterpoint to Krantz's systematic style

## Open Questions

1. **Specific section ingest priority.** Krantz Ch 2.2-2.5 (theorem,
   proof, definition, abstract) should be a follow-on detailed read
   when SiS finalizes the Year-2 Analytic-Σ-NCDE theorem statement.
2. **Krantz §4.3 referee's report** — should this be a separate
   wiki/writing/ page for Bilal's reviewing work, distinct from this
   primer summary?

## Sources

- `raw/writing/guides/1612.04888v1.pdf` — arXiv:1612.04888v1 [math.HO], posted
  15 Dec 2016. Steven G. Krantz, *A Primer of Mathematical Writing*,
  2nd Edition, December 16, 2016.
- Identified via title page + arXiv ID. Cross-confirmed by
  [[ICML-Best-Practices]] resource list entry "Steven G. Krantz: A
  Primer of Mathematical Writing (a whole book)."
- Dedication: "to Paul Halmos. For the example he has set for us all."
- Pages 1-5 read in this ingest (title page, dedication, TOC). Full body
  not captured — book-length resource intended for reference use, not
  cover-to-cover ingest.
