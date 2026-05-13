---
title: Pak — How to Write a Clear Math Paper (21st Century Tips)
tags: [writing, mathematical-writing, pak, clarity, modern-tex, arxiv-era]
sources:
  - raw/writing/guides/how-to-write1.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Pak — How to Write a Clear Math Paper: Some 21st Century Tips

**Identified title and authors (per user's special instruction):**

**Igor Pak**, *How to Write a Clear Math Paper: Some 21st Century Tips*.
Department of Mathematics, UCLA, Los Angeles. Email: pak@math.ucla.edu.

**Cited in:** [[ICML-Best-Practices]] resource list as "How to write a
clear math paper by Igor Pak. Long, a bit scary start, but some good
advice."

**Length:** Multi-section essay (read pages 1-5 in this ingest). Modern
in stance — explicitly contrasts old-style mathematical-writing guides
with the arXiv era + diverse-readership reality.

## Origin Statement (Verbatim)

> "In this note we explain the importance of clarity and give other
> tips for mathematical writing. Some of it is mildly opinionated, but
> most is just common sense and experience."

## Section 1 — Be Clear!

Pak's framing: **clarity is THE golden rule.**

### 1.1 What does it mean to be clear?

> "Abelian groups have trivial center" rather than "It was discovered
> by Galois, and later proved formally by Jordan in 1870 (see [Struik]),
> that having the identity being the only fixed element commuting with
> any other element is implied by the abeliannness of a given group."

(Footnote: "Mathematically, this statement is completely false. But
that's part of my point — **how would anyone even know that in the
second version? When you are unclear, all claims look reasonably
true.**")

### 1.2 Being clear — how hard can that be?

> "Making your paper clearer takes time and a lot of effort. [...] I
> once asked Noga Alon how did he get to be so good (and so fast!) at
> writing. He said **'it gets easier after the first 300 papers'**."

The h-as-variable-vs-function example: invest the 30 minutes to fix the
notation inconsistency rather than disclaimer your way out.

### 1.3 Why be clear?

> "Being clear is not about you! **You must think of the reader and
> how they will read your paper.**"

Three reader scenarios:
1. **Graduate student at small university with poor English skills** —
   confused on page 3, gives up, uses your competitor's older
   better-written paper.
2. **Postdoc at major research university** — has 20 papers to
   evaluate, your unclear notation makes her decide your paper is
   irrelevant.
3. **Senior mathematicians** — have an [actual checklist](#) for what
   they read. Clear writing = easy pass.
4. **Competitive advantage:** "If your paper is clear and your
   competitors' are not, you will get the credit." Like recording the
   same symphony with different orchestras — presentation matters.

### 1.4 Can't journals help?

> "In a word, NO."
>
> "You are likely going to be posting your paper on the arXiv anyway,
> where most people will find it (or on your web page, either way). So
> the journals are cut out of the process, and you yourself should
> strive to make your paper as clear as you possibly can."

### 1.5 For the sake of clarity, ignore all rules!

> "This is motivated by the 'Ignore All Rules' guideline page for
> Wikipedia editors. Roughly, I am saying that when the rules of style
> and grammar make math unclear, **you should simply ignore these
> rules.** Try rewording the sentence first, of course, but if nothing
> works, go for it, no matter how fundamental the rule is."

The pragmatic counter-rule: clarity trumps grammar, sometimes.

## Section 2 — Where to Start

### 2.1 Not with this article, but with other literature

Recommended foundations: **Halmos** essays, **Higham** book, **Knuth**,
**Krantz** ([[ArXiv-1612-Writing-Guide]]), Berndt, Goldreich, S.P. Jones
([[Peyton-Jones-Research-Paper]]), Terry Tao's blog.

### 2.2 Read a good guide on writing nonfiction

> "I strongly recommend **Zinsser's book** in part because I don't know
> any other, but in part because it's so well written I can't imagine a
> better guide."

Zinsser excerpt (Pak quotes Zinsser on paragraphs):

> "Keep your paragraphs short. Writing is visual — it catches the eye
> before it has a chance to catch the brain. **Newspaper paragraphs
> should be only two or three sentences long.** ... But don't go
> berserk. A succession of tiny paragraphs is as annoying as a
> paragraph that's too long."

**For non-native English speakers:** "read Halmos and other short
pieces first. Come back to Zinsser when you gain more experience."

### 2.3 So why do we need this new guide then?

> "The world is changing too fast. With the ever increasing competition
> for jobs, publishing in top journals, etc., some of the old advice
> needs to be calibrated and adjusted for modern times. This is
> particularly true about typesetting in LaTeX..."
>
> "One no longer expects their papers to be all that interesting to
> survive decades. It's the short term goals that became all too
> important. **Thus the emphasis should be on a modest goal of clear
> rather than perfect writing.**"

**Diverse readership styles:**
- Read titles on arXiv, only occasionally abstracts
- Quickly skim most papers in their areas, read none carefully
- Just read introductions
- Read whatever Google Scholar suggests
- Skip everything and go to main results; back if interested
- Read nothing, learn at seminars

Implication: write for ALL these reading patterns simultaneously.

## Section 3 — Macro Tips

### 3.1 Structure of the paper

> "Every newspaper writing guide ... will advise to write an article in
> a Matryoshka doll manner — start with a super brief summary, then
> make a longer summary, and only then, once the reader is hooked and
> interested in details, proceed to give a complete set of facts."

Math article structure: title → abstract → introduction → main part →
final remarks → references.

### 3.2 Title

> "Read about how to write a good title everywhere. Think about it a
> long time. Try different versions on your colleagues. Then think again.
> Your title shouldn't be too long, too short, too vague or generic..."

**Trickery (recommended):** if you introduce a class of objects, give
them a memorable name (Pak example: "Gayley polytopes" named after a
street he lived on, that rhymed with "Cayley" and were generalizations
to all Graphs).

Self-quoted MathOverflow advice on title naming:
- "All tennis balls are white" — proving they are
- "On white tennis balls" / "New examples of white tennis balls" — for
  proving some are
- "Short proof that all tennis balls are white" — emphasizing new proof
- "Not all tennis balls are white" — counterexample
- "A remark on white tennis balls" — minor study
- "A survey on white tennis balls" — survey

### 3.3 Abstract

> "This is the easiest section to write. Just think of a short MathSciNet
> summary (not a longer more careful review they have sometimes). The
> abstract should have nothing personal, just dry facts about the results."
>
> "As a rule of thumb, **the number of lines in the abstract should be
> 0.3-0.5 times the number of pages.** An abstract with 10 lines for a
> paper of 10 pages looks way too excessive."

(Pages 6+ not captured in this ingest read; section 3.3 cut at abstract
guidance.)

## Core Actionable Rules (Compressed)

1. **Clarity is the golden rule.** Invest 30 minutes to fix notation,
   not 1 minute to disclaimer it.
2. **Write for diverse readers** (small-university grad student to
   senior with checklist).
3. **Write clear, not perfect.** Modern stance: short-term goals
   matter; survival-for-decades is a fading concern.
4. **Ignore grammar rules when they impede clarity** (Wikipedia-IAR
   principle).
5. **arXiv is the distribution channel; journals don't fix unclear
   writing.**
6. **Read Halmos / Higham / Knuth / Krantz / Berndt / Goldreich /
   Peyton-Jones / Terry Tao blog.**
7. **Read Zinsser on writing nonfiction.**
8. **Matryoshka-doll structure:** brief summary → longer summary →
   complete facts.
9. **Title:** thought through, memorable, neither vague nor generic;
   names of new objects matter for citation longevity.
10. **Abstract is dry facts, 0.3-0.5x lines per page** of the paper.
11. **Write for the multi-modal reader** (title-only, abstract-only,
    skim, intro-only, results-first, none-at-all).

## Reviewer-Facing Implications

Pak's framing: **clarity affects citation, not just acceptance.** A
clear paper gets cited even when the underlying result is duplicated
in a less-clear competitor's paper. So the reviewer/reader audit is
about long-term impact, not just publication.

The "senior mathematician with an actual checklist" reference (§1.3)
maps to [[Ramdas-Paper-Checklist]] — senior-reviewer-style checklist
audit is operationally real.

## ML-Paper-Specific Advice (Distinguished from General Writing)

Pak is **mathematical-writing-specific** but the **arXiv-era +
diverse-readership** framing applies directly to ML papers (most ML
papers go on arXiv first; ML readers exhibit the diverse reading
patterns Pak describes).

The "clear not perfect" stance (§2.3) is particularly ML-applicable —
ML papers' short half-lives (~2-3 years before obsolescence) make the
"survive decades" framing inappropriate.

The §3.2 title-naming trickery (memorable names like "Munro
permutations", "Gayley polytopes") matches ML's convention of naming
architectures memorably (LSTM, Transformer, GAN). Pre-naming aids
citation.

## Connection to SiS / CTPC

- **§1.1 unclear-makes-false-claims-look-true:** SiS's audit-trail
  pattern in [[CTPC-Design-Rationale]] Part III explicitly catches
  unclear claims that masquerade as true (4 substantive Barbiero
  errors). This is the operational implementation of Pak's clarity
  principle.
- **§1.3 multi-reader audit:** [[CTPC-KDD-Submission]] readership
  includes SDA practitioners (who care about the orbital-prediction
  pipeline) + ML methodologists (who care about NCDE/calibration) +
  potentially future agents/students. Write for all.
- **§3.2 memorable naming:** "CTPC" (Continuous-Time Probabilistic
  Corrector) is a citation-friendly name; "PhyArch" is similar; "ν_min
  = 4.5" is a memorable specific number. Pak would approve.
- **§3.3 abstract length:** for a 9-page KDD paper, abstract should be
  ~3-4.5 lines of text per Pak's rule. Audit current draft accordingly.

## Connections

- [[ArXiv-1612-Writing-Guide]] — Krantz is one of Pak's explicit
  recommendations
- [[Peyton-Jones-Research-Paper]] — S.P. Jones is one of Pak's
  recommendations
- [[ICML-Best-Practices]] — cites Pak in resource list
- [[Ramdas-Paper-Checklist]] — Pak references "senior-reviewer
  checklists"
- [[CTPC-KDD-Submission]] — pre-submission audit target

## Open Questions

1. **Pages 6+ of Pak's essay** — sections 3.4+ (likely Introduction,
   Main Part, Final Remarks, References, Notation tips) not captured
   in this ingest read. Worth follow-on read closer to KDD '26
   submission.
2. **Zinsser's *On Writing Well*** — Pak's primary recommendation for
   nonfiction writing. Not in SiS corpus. Worth adding to a future
   `raw/writing/guides/` expansion?

## Sources

- `raw/writing/guides/how-to-write1.pdf` — Igor Pak (UCLA Mathematics).
  Identified via title page line 1: "HOW TO WRITE A CLEAR MATH PAPER:
  SOME 21ST CENTURY TIPS, IGOR PAK".
- arXiv-or-personal-website hosted (no arXiv ID visible on title page).
- Cross-confirmed by [[ICML-Best-Practices]] resource list entry "How
  to write a clear math paper by Igor Pak."
- Pages 1-5 read in this ingest. Pages 6+ summarized only.
