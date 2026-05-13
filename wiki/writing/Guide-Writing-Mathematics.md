---
title: Law, Boynton & Tait — Guide to Writing Mathematics (HKU, 2015)
tags: [writing, mathematical-writing, english-grammar, non-native-speakers, hku]
sources:
  - raw/writing/guides/guide_to_writing_mathematics.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Law, Boynton & Tait — Guide to Writing Mathematics (HKU)

**Authors:** K. H. Law (Department of Mathematics, principal investigator)
+ S. Boynton + C. Tait (Centre for Applied English Studies), The
University of Hong Kong. **Version:** June 2015 (first version Jan 2015).
**Cited in:** [[ICML-Best-Practices]] resource list as "Guide to Writing
Mathematics (The U. of Hong Kong). Detailed, good advice."
**Origin:** Teaching Development Grant project "Promoting Teaching and
Learning of Professional Writing in Mathematics" — explicitly targets
**students whose secondary-school math-writing habits differ from
university expectations**, with particular focus on **non-native English
speakers**.

## Structural Arc (TOC, Verbatim)

1. **Preface**
2. **Basic Principles:** Complete Sentences · Capital Letters · Commas
   between Variables
3. **The Use of English:** Grammatical Errors (articles, singular/plural,
   verb forms, verb-to-be/verb-to-do, word-form conversions) · Lexical
   Errors (word order, choice of words, other) · Other Issues
   (similar-pronunciation words, confused words, linking verbs,
   spelling)
4. **The Use of Symbols:** "=" vs "≠" · "⇒" vs "⇔" · "∀" vs "∃" ·
   "∈" vs "⊆" · Overusing Symbols
5. **The Use of Terminology:** Elementary Algebra · Geometry ·
   Mathematical Induction · Functions and Calculus · Linear Algebra
6. **Miscellaneous:** Handwriting · Presentation · Avoiding Isolated
   Equations

## Core Actionable Rules (Verbatim from Ch 2-3)

### Basic Principles (Ch 2)

1. **Write structurally in complete sentences.** Math writing should
   contain mostly **words**, supplemented by equations and symbols —
   not the reverse. Even worked-example solutions are mostly words.
   > "Hence x + 1 > 3" reads as: "Hence x plus 1 is greater than 3."
   > Wrong (sentence fragments): "Since x is positive." / "If this is
   > not true." / "When two triangles are similar."

2. **Don't start a sentence with a symbol or lower-case variable.**
   > BAD: "p is not a prime number if it is divisible by 3 and greater
   > than 3."
   > GOOD: "If the number p is divisible by 3 and greater than 3, then
   > it is not a prime number."
   >
   > BAD: "x² + x + 1 = 0 has no real root..."
   > GOOD: "The equation x² + x + 1 = 0 has no real root..."

3. **Comma between variables avoids ambiguity.**
   > BAD: "In addition to p, q is also a prime number." (reader
   > expects p AND q to share a property + a third thing)
   > GOOD: "In addition to p, the number q is also prime."

### Use of English (Ch 3, verbatim wrong/correct table extracts)

4. **Articles:**
   - "Construct a m × n table" → **"Construct an m × n table"** (m
     pronounced /em/, vowel sound).
   - "By binomial theorem" → **"By the binomial theorem"** (definite
     for specific theorem).
   - "By Pythagoras' Theorem" → **"By Pythagoras' Theorem"** (zero
     article for named-person theorem).
   - "By (a), the g is continuous" → **"By (a), the function g is
     continuous"** (noun form takes definite article; letter alone takes
     zero article).
   - "Let x be non-negative number" → "Let x be **a** non-negative
     number" or "Let x be non-negative" (adjective form).
   - "Let x be a positive" → "Let x be **positive**" (adjective, not
     noun form).

5. **Singular vs plural agreement:**
   - "Every square are rectangles" → "Every square is a rectangle" or
     "All squares are rectangles."
   - "for some positive integers k, i.e. k(k+1)" → "for some positive
     integer k" (one integer, singular).
   - "Let A be a square matrice" → "Let A be a square matrix."
   - "(0,0) is the only local maxima" → "...local maximum."

6. **Verb agreement:**
   - "There exist a real number m" → "There exists a real number m."
   - "There exists real numbers m and n" → "There exist real numbers..."

7. **Verb-to-be / verb-to-do:**
   - "It is a rational number between 1 and 2" → "There is a rational
     number between 1 and 2" (introducing new info, not referring back).
   - "is not exist" → "does not exist."
   - "is not converge" → "does not converge" OR "is not convergent."

8. **Word forms — critical examples:**
   - "Besides from completing the square" → "Besides completing the
     square."
   - "The total increasement" → "The total increase."
   - "The function y = x² concaves upward" → "...is concave upward."
   - "One plus one equals to two" → "One plus one equals two" or
     "...is equal to two."
   - "The followings are equivalent" → "The following statements are
     equivalent."
   - "The result is followed" → "The result follows."

### Symbols (Ch 4)

9. **=, ≠, ⇒, ⇔, ∀, ∃, ∈, ⊆** all have specific usage rules — the
   guide gives full chapter to disambiguating these.

10. **Overusing symbols** — the guide warns against symbol-heavy
    sentences; complete English sentences with embedded math are
    preferred over symbol chains.

### Terminology (Ch 5)

11. Specific terminology pitfalls per domain (elementary algebra,
    geometry, mathematical induction, functions and calculus, linear
    algebra) — too domain-specific to summarize, but the chapter is the
    reference when those domains appear.

### Miscellaneous (Ch 6)

12. **Avoid isolated equations** — equations should be embedded in
    grammatical sentences, not free-standing.

## Reviewer-Facing Implications

This guide is **error-density focused** — it catalogs the specific
grammatical/lexical errors that cause reviewer-level "this paper isn't
ready" reactions. The error-table format (wrong / correct / comment) is
directly applicable as a pre-submission audit checklist for non-native-
English-speaking authors.

ICML reviewers per [[ICML-2022-Review-Form]] §"Quality of
writing/presentation" — these grammatical errors trigger the
"miscellaneous minor issues" section of the review. Enough of them
crossing into the major-flaw threshold and the writing-quality score
drops materially.

## ML-Paper-Specific Advice (Distinguished from General Writing)

This guide is **mathematical-writing-specific, not ML-specific**. The
ML-applicability is on the *math sections* of ML papers — theorem
statements, derivation passages, definitions. The error patterns
catalogued (article use, singular/plural, verb-to-be) are universal
academic-writing pitfalls but specifically illustrated with math
examples.

The guide does NOT cover: experimental sections, benchmark conventions,
reproducibility — covered by [[ICML-Best-Practices]] §10.

## Connection to SiS / CTPC

Direct audit checklist applicable to:
- **Theorem statements** in [[Analytic-Sigma-CTPC-Composition]] —
  especially the Year-2 Analytic-Σ-NCDE theorem statement.
- **Definitions** in [[Port-Hamiltonian-Neural-Network]] /
  [[Port-Hamiltonian-Systems]] / [[Goldstein-Ch8-J-vs-PH-Cholesky-R]].
- **Generally** — Bilal is the author for KDD '26 submission; non-native
  English speaker errors in this catalog should be audited.

Most-likely-relevant patterns for SiS work:
- Item 4 (articles): "the function H" vs "the H" — H named via letter
  should take zero article unless preceded by "the function".
- Item 5 (singular/plural agreement): "for some integers k" vs "for
  some integer k" — particularly with index variables.
- Item 9 (∀, ∃): replace with "for all" / "for some" where possible per
  [[Ramdas-Paper-Checklist]] item 9 (mathematical writing).

## Connections

- [[ArXiv-1612-Writing-Guide]] — Krantz Ch 1.11-1.12 covers similar
  ground at higher granularity
- [[Ten-Rules-Mathematical-Writing]] — Bertsekas's small rules overlap
- [[Ramdas-Paper-Checklist]] — items 6, 9 overlap
- [[Strunk-White-Elements-Style]] — general-English companion
- [[Milne-Tips-Authors]] — math-writing tips counterpart

## Open Questions

1. **Symbol-disambiguation chapter (Ch 4)** not fully captured here —
   worth a follow-on read when SiS finalizes notation conventions.
2. **Terminology chapter (Ch 5)** is domain-specific; the linear-algebra
   §5.5 may apply to PHNN's Cholesky/Hessian/eigenvalue discussion.

## Sources

- `raw/writing/guides/guide_to_writing_mathematics.pdf` — June 2015, HKU,
  Teaching Development Grant project.
- Authors: K. H. Law (Mathematics, principal investigator), S. Boynton
  + C. Tait (Centre for Applied English Studies).
- Pages 1-10 read in this ingest (TOC + Ch 1 Preface + Ch 2 Basic
  Principles + Ch 3.1 Grammatical Errors).
- Pages 11-27 (lexical errors, similar-pronunciation, symbols,
  terminology, miscellaneous) summarized from chapter names only.
