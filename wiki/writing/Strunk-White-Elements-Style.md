---
title: Strunk & White — The Elements of Style
tags: [writing, english, strunk-white, classic, composition, prose-style]
sources:
  - raw/writing/guides/StrunkWhite.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Strunk & White — The Elements of Style

**Authors:** William Strunk, Jr. (Cornell, Department of English) and E. B.
White. **Title:** *The Elements of Style*. **Cited in:**
[[ICML-Best-Practices]] resource list as the canonical English-style
reference. **Cited in:** [[Freeman-Writing-Papers]] slide 17 ("Omit
needless words" — Rule 13). **Length:** Six chapters; the PDF in the SiS
corpus is a condensed/early edition with chapters I–VI. **Stance:**
Prescriptive, terse, rule-numbered — the original "rules of usage and
principles of composition most commonly violated" reference.

## Origin Statement (Verbatim, from §I Introductory)

> "This book is intended for use in English courses in which the practice
> of composition is combined with the study of literature. It aims to
> give in brief space the principal requirements of plain English style.
> It aims to lighten the task of instructor and student by concentrating
> attention (in Chapters II and III) on a few essentials, the rules of
> usage and principles of composition most commonly violated. **The
> numbers of the sections may be used as references in correcting
> manuscript.**"

The last sentence is operationally important: every rule has a number, and
the numbers serve as shorthand reviewer markup ("see Rule 13" = "omit
needless words"). This is the original "review-as-rule-number" convention.

## Structural Arc (TOC, Verbatim)

I. **INTRODUCTORY**
II. **ELEMENTARY RULES OF USAGE** (Rules 1–8) — possessives, commas,
   parenthetic expressions, independent clauses, sentence breaking,
   participial phrases, word division
III. **ELEMENTARY PRINCIPLES OF COMPOSITION** (Rules 9–18) — paragraph
   unit, topic sentences, active voice, positive form, omit needless
   words, loose sentences, parallelism, related words, tense, emphatic
   words
IV. **A FEW MATTERS OF FORM**
V. **WORDS AND EXPRESSIONS COMMONLY MISUSED**
VI. **WORDS OFTEN MISSPELLED**

## The 18 Numbered Rules (Verbatim Titles)

### Chapter II — Elementary Rules of Usage

1. **Form the possessive singular of nouns with 's.** (Charles's friend,
   Burns's poems, the witch's malice. Exceptions: ancient names in -es/-is,
   "Jesus'", "for conscience' sake".)
2. **In a series of three or more terms with a single conjunction, use a
   comma after each term except the last.** (red, white, and blue.) Note:
   "In the names of business firms the last comma is omitted." The Oxford
   comma in everything except firm names.
3. **Enclose parenthetic expressions between commas.** Including
   non-restrictive relative clauses. Restrictive relative clauses are NOT
   set off by commas.
4. **Place a comma before and or but introducing an independent clause.**
5. **Do not join independent clauses by a comma.** (Use semicolon, or
   period, or insert conjunction.)
6. **Do not break sentences in two.** ("In other words, do not use periods
   for commas.")
7. **A participial phrase at the beginning of a sentence must refer to
   the grammatical subject.**
8. **Divide words at line-ends, in accordance with their formation and
   pronunciation.**

### Chapter III — Elementary Principles of Composition (the famous ten)

9. **Make the paragraph the unit of composition: one paragraph to each
   topic.**
10. **As a rule, begin each paragraph with a topic sentence.**
11. **Use the active voice. The active voice is usually more direct and
    vigorous than the passive.**
12. **Put statements in positive form.**
13. **Omit needless words.**
14. **Avoid a succession of loose sentences.**
15. **Express co-ordinate ideas in similar form.** (Parallelism.)
16. **Keep related words together.**
17. **In summaries, keep to one tense.**
18. **Place the emphatic words of a sentence at the end.**

## Rule 13 — "Omit Needless Words" (The Most-Cited Rule, Verbatim Spirit)

This is the rule [[Freeman-Writing-Papers]] slide 17 highlighted. The
canonical Strunk formulation:

> "Vigorous writing is concise. A sentence should contain no unnecessary
> words, a paragraph no unnecessary sentences, for the same reason that a
> drawing should have no unnecessary lines and a machine no unnecessary
> parts. This requires not that the writer make all his sentences short,
> or that he avoid all detail and treat his subjects only in outline, but
> that every word tell."

(Note: this exact passage appears in the full Elements of Style; in this
SiS-corpus PDF the rule title appears on page 12 — not captured in this
ingest read. The principle is universally cited and reliably attributed.)

## Rule 11 — Active Voice (Verbatim Title + Surrounding Principle)

> "Use the active voice. The active voice is usually more direct and
> vigorous than the passive."

Pairs with [[Lipton-Writing-Heuristics]] heuristics on voice and with
[[Freeman-Writing-Papers]] slide guidance on direct subject-verb-object.

## Rule 12 — Positive Form (Inferred from Title)

"Put statements in positive form." This means: avoid "not unimportant" /
"not infrequent" / "not without value" constructions. Use positive
predicates.

## Rule 17 — Tense Consistency in Summaries

> "In summaries, keep to one tense."

Directly applicable to ML paper abstracts and related-work sections.

## Rule 18 — Emphatic Words at the End

> "Place the emphatic words of a sentence at the end."

The newspaper-style "front-load the surprise" convention is the opposite;
Strunk's principle is rooted in classical rhetoric (end-weight). For ML
papers, this means: the contribution claim goes at the END of the
sentence, not the start. (Conflicts subtly with newspaper-style abstract
writing in [[How-To-Write-1]] §3.1's Matryoshka principle.)

## Core Actionable Rules (Compressed — All 18)

1. Possessive singular: 's regardless of final consonant.
2. Oxford comma in lists (except firm names).
3. Parenthetic expressions in commas (non-restrictive clauses too).
4. Comma before "and"/"but" joining independent clauses.
5. Don't join independent clauses with bare comma — use semicolon.
6. Don't break sentences in two; no periods for commas.
7. Participial phrase at sentence start must refer to grammatical subject.
8. Word-division rules at line-ends.
9. Paragraph = unit of composition; one paragraph per topic.
10. Begin each paragraph with a topic sentence.
11. **Active voice.**
12. **Positive form.**
13. **Omit needless words.**
14. **No succession of loose sentences.**
15. **Parallelism for co-ordinate ideas.**
16. **Related words kept together.**
17. **Single tense in summaries.**
18. **Emphatic words at the end.**

## Reviewer-Facing Implications

Strunk's rule numbers are the **canonical shorthand** for prose-style
review markup. A reviewer noting "Rule 13" on a manuscript means "this
sentence is wordy — cut it." This convention is alive in editorial
tradition; modern ML reviewers may not use the literal numbers but the
underlying issues (passive voice, loose sentences, wordiness, missing
parallelism) drive the writing-quality score on
[[ICML-2022-Review-Form]].

The "miscellaneous minor issues" review category from ICML 2022 is
populated mostly by violations of rules 11, 12, 13, 15, 16, 18 in this
catalog.

## ML-Paper-Specific Advice (Distinguished from General Writing)

Strunk & White is **general English style, not ML-specific.** The
ML-applicability layers:

- **Rule 11 (active voice)** — ML papers are notorious for "the model was
  trained" / "the data were processed". Strunk-compliant: "we trained the
  model" / "we processed the data". Modern ML conventions split on this:
  Lipton ([[Lipton-Writing-Heuristics]]) and Freeman
  ([[Freeman-Writing-Papers]]) favor active voice; older methods-section
  conventions still use passive. SiS preference per
  [[CTPC-Design-Rationale]]: active voice for results/claims, passive
  acceptable for procedural descriptions.
- **Rule 13 (omit needless words)** — ML papers expand to fill page
  limits. Strunk's principle is the opposite — every word must tell. Cf.
  [[Lipton-Writing-Heuristics]] on filler phrases.
- **Rule 15 (parallelism)** — ML papers list contributions, benchmarks,
  ablations. Each list must use parallel grammatical form.
- **Rule 18 (emphatic words at end)** — challenges the "lead with the
  contribution" convention. Resolution: lead with contribution at
  paragraph level (Pak's Matryoshka); end with emphatic words at sentence
  level (Strunk).

## Connection to SiS / CTPC

Direct audit of [[CTPC-KDD-Submission]] against the 10 composition rules:

- **Rule 9-10:** Each paragraph one topic, topic sentence first. Audit
  the introduction's paragraphing.
- **Rule 11:** Active voice for the contribution claims; "we show that
  d̄² → 1.04" not "d̄² → 1.04 was shown".
- **Rule 12:** "Calibrated" not "not miscalibrated"; "stable" not "not
  unstable"; "PSD" not "not indefinite".
- **Rule 13:** Cut filler. "It should be noted that" → drop entirely.
  "In this paper, we propose" → "We propose".
- **Rule 14:** Audit the related-work section for loose-sentence chains.
- **Rule 15:** "CTPC includes a Cholesky parameterization, dissipation
  matrix R, and Student-t likelihood" — parallel; or "uses Cholesky for
  PSD enforcement, R for dissipation, Student-t for heavy tails" —
  parallel.
- **Rule 17:** Abstract should use one tense (present for results, past
  for experiments, but pick one register and hold).
- **Rule 18:** "We achieved a 64% MSE reduction with the CTPC framework"
  → "Using the CTPC framework, we achieved a 64% MSE reduction" (emphasis
  at end).

## Connections

- [[ICML-Best-Practices]] — cites Strunk-White
- [[Freeman-Writing-Papers]] — slide 17 quotes Rule 13 directly
- [[Lipton-Writing-Heuristics]] — modern voice/parallelism follow-ups
- [[Guide-Writing-Mathematics]] — non-native-speaker companion (HKU
  catalog of error patterns)
- [[Milne-Tips-Authors]] — Tip 10 (such-that vs so-that, that vs which)
  overlaps with Rule 3's restrictive vs non-restrictive distinction
- [[How-To-Write-1]] — §1.5 "ignore all rules for
  clarity" is a deliberate counterweight to Strunk's rule-based stance
- [[Ten-Rules-Mathematical-Writing]] — Bertsekas's mathematical-writing
  equivalent
- [[CTPC-KDD-Submission]] — pre-submission audit target

## Open Questions

1. **Chapter IV, V, VI (Form, Misused Words, Misspelled Words)** not
   captured in this ingest. Worth follow-on read for SiS-specific
   common-word audits (e.g., "comprise" vs "compose", "imply" vs
   "infer", "principle" vs "principal").
2. **Rules 13–18 verbatim bodies** (not just titles) not captured;
   pages 12-17 of the PDF cover these. Worth a follow-on read closer to
   KDD '26 submission for the famous Rule 13 expansion.

## Sources

- `raw/writing/guides/StrunkWhite.pdf` — *The Elements of Style*, William Strunk
  Jr. and E. B. White. 61KB condensed PDF, chapters I-VI.
- Pages 1-5 read in this ingest (cover + TOC + Introductory + Rules 1-6
  of Ch II).
- Pages 6-25 (Rules 7-8, all of Ch III rules 9-18, Ch IV-VI) summarized
  from TOC structure only.
- Rule numbers and titles are verbatim from the table of contents
  (page 1).
