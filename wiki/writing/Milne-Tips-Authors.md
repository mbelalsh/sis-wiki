---
title: Milne — Tips for Authors (Satirical How-To-Write-Badly)
tags: [writing, satire, milne, mathematical-writing, anti-patterns]
sources:
  - raw/writing/guides/Tips for Authors -- J.S. Milne.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Milne — Tips for Authors

**Author:** J. S. Milne. **Hosting:** https://www.jmilne.org/math/tips.html.
**Format:** Single-page (3 PDF pages) satirical essay listing 11 tips on
how to write *badly* — the reverse-framing teaches what to avoid by
exaggerating the failure modes. **Cited in:** [[ICML-Best-Practices]]
resource list as "JS Milne's Tips for Authors (funny, teaching by
showing what not to do)."

## Origin Framing (Verbatim)

> "If you write clearly, then your readers may understand your
> mathematics and conclude that it isn't profound. Worse, a referee may
> find your errors. Here are some tips for avoiding these awful
> possibilities."

The entire essay operates in this reverse-irony register. The "tips"
are anti-patterns; the underlying advice is to do the opposite.

## The Eleven Anti-Patterns (Verbatim, Invert for Real Advice)

### 1. "Never explain why you need all those weird conditions"

> "Simply begin your paper with two pages of notations and conditions
> without explaining that they mean that the varieties you are
> considering have zero-dimensional boundary. In fact, never explain
> what you are doing, or why you are doing it. **The best-written paper
> is one in which the reader will not discover what you have proved
> until he has read the whole paper, if then.**"

**Inverted real advice:** explain *why* the conditions are needed and
*what* they mean. State the goal upfront. (Echoes
[[Peyton-Jones-Research-Paper]] suggestion 4 — nail contributions to
the mast.)

**Supporting Littlewood quote:** "A recent (published) paper had near
the beginning the passage 'The object of this paper is to prove
(something very important).' It transpired with great difficulty, and
not till near the end, that the 'object' was an unachieved one."

### 2. "Refer to another obscure paper for all basic (nonstandard) definitions"

> "This almost guarantees that no one will understand what you are
> talking about. **In particular, never explain your sign conventions
> — if you do, someone may be able to prove that your signs are wrong.**"

**Inverted real advice:** define your nonstandard terms in the paper;
state sign conventions explicitly. (Echoes [[ICML-Best-Practices]]
item 9.8 — nonstandard definitions are included.)

### 3. Variation of definition

> "When having difficulties proving a theorem, try the method of
> '**variation of definition**' — this involves implicitly using more
> than one definition for a term in the course of a single proof."

**Inverted real advice:** use ONE definition per term, consistently
throughout the proof. (Overlaps with [[Ten-Rules-Mathematical-Writing]]
Rule 4 — consistent notation/nomenclature.)

### 4. Use c, a, b respectively to denote elements of sets A, B, C

The notation-mismatch trick. Inverted: name elements according to the
set they belong to.

**Supporting Jordan quote:** "If he had 4 things on the same footing
(as a,b,c,d) they would appear as a, M₃', ε₂, ∏''_{1,2}."

### 5. "When using a result in a proof, don't state the result or give a reference"

> "In fact, try to conceal that you are even making use of a nontrivial
> result."

**Inverted real advice:** every nontrivial result invoked should be
stated or cited.

**"Well-known" definition (from MR 50:2128, Roger Howe):** "known to
more than a dozen people for more than two years."

### 6. Vague references

> "If, in a moment of weakness, you do refer to a paper or book for a
> result, **never say where in the paper or book the result can be
> found.** In addition to making it difficult for the reader to find
> the result, this makes it almost impossible for anyone to prove that
> the result isn't actually there."

**Inverted real advice:** cite specific theorem numbers or page
numbers. (Overlaps with [[Ramdas-Paper-Checklist]] item 7 — refer to
all assumptions by number; [[ICML-Best-Practices]] item 9.9 — "do not
cite a book or a long paper for a theorem, give the reader precise
information where to find the result.")

### 7. Separate numbering of theorems/propositions/etc.

> "Especially in long articles or books, **number your theorems,
> propositions, corollaries, definitions, remarks, etc. separately.**
> That way, no reader will have the patience to track down your
> internal references."

**Inverted real advice:** unified numbering (Theorem 3.1, Proposition
3.2, Corollary 3.3, etc.) so internal references are sequential and
findable. (Note: [[Ramdas-Paper-Checklist]] item 7 says "Number each
type separately." — direct contradiction with Milne. Mathematical
community is divided on this; the Ramdas convention is more common in
stat-ML, Milne's complaint is more applicable to long books.)

### 8. Muddle logical structure + quantifiers

> "Write A==>B==>C==>D when you mean (A==>B)==>(C==>D), or
> (A==>(B==>C))==>D, or .... Similarly, write 'If A, B, C' when you
> mean 'If A, then B and C' or 'If A and B, then C', or .... **Also,
> always muddle your quantifiers.**"

**Inverted real advice:** parenthesize implications; use full
"if...then..." structure; place quantifiers carefully.

**QNS — Quantifier Negation Syndrome** (AMS Notices Oct 2000, p1041):
"all data sets do not have similar characteristics" (means: not all
data sets have similar characteristics, NOT: no data sets have similar
characteristics).

### 9. Begin and end sentences with symbols

> "Begin and end sentences with symbols wherever possible. Since
> periods are almost invisible (and may be mistaken for a mathematical
> symbol), most readers won't even notice that you've started a new
> sentence. Also, where possible, attach superscripts signalling
> footnotes to mathematical symbols rather than words."

**Inverted real advice:** no sentence starts/ends with a symbol. (Echoes
[[Guide-Writing-Mathematics]] §2.2 and [[Ten-Rules-Mathematical-Writing]]
small rule "Don't start a sentence with mathspeak.")

### 10. Write "so that" when you mean "such that"

> "Write 'so that' when you mean 'such that' and 'which' when you mean
> 'that'. **Always prefer the ambiguous expression to the unambiguous
> and the imprecise to the precise. It is the readers task to determine
> what you mean; it is not yours to express it.**"

**Inverted real advice:** prefer the unambiguous, the precise. Use
"that" for restrictive clauses, "which" for non-restrictive.

### 11. "If all else fails, write in German, Russian, or Turkish"

> "(or other language that most mathematicians can't read)"

**Inverted real advice:** write in English (the lingua franca of modern
mathematics + ML).

## Closing Quotes (Verbatim)

> "Those who know that they are profound strive for clarity. Those who
> would like to seem profound strive for obscurity."
> — Nietzsche.

> "Mathematicians always strive to confuse their audiences; where
> there is no confusion there is no prestige. Mathematics is
> prestidigitation."
> — Carl Linderholm, *Mathematics Made Difficult*, p10.

> "...she writes on deeply technical matters in clear English without
> jargon. This does not inspire confidence. Obscurity, besides
> obscuring incomplete thought, often suggests that the thought was
> quite deep."
> — J.K. Galbraith.

And the closing recommendation:

> "**And everyone should read George Orwell's essay Politics and the
> English Language** (and try to write like Orwell)."

With the famous Orwell example — Ecclesiastes ("the race is not to
the swift...") vs. modern bureaucratic English ("Objective
considerations of contemporary phenomena compels the conclusion that
success or failure in competitive activities exhibits no tendency to
be commensurate with innate capacity...").

## Core Actionable Rules (Inverted)

1. **Explain WHY conditions are needed.** State goals upfront.
2. **Define nonstandard terms in the paper.** State sign conventions.
3. **One definition per term.** Don't variation-of-define.
4. **Name elements per set** (use a ∈ A, not c ∈ A).
5. **State or cite every nontrivial result** used.
6. **Cite by specific page or theorem number**, not whole book.
7. **Number internal references** in a way easy to find.
8. **Parenthesize implications + careful quantifier placement.**
9. **Sentences don't start/end with symbols.**
10. **Prefer precision over ambiguity.** "Such that" not "so that";
    "that" not "which" for restrictive clauses.
11. **Write in English.**

## Reviewer-Facing Implications

Each anti-pattern is something a reviewer can call out by name. The
satirical framing makes the failures memorable — when a reviewer says
"the paper's sign conventions are unclear" they're invoking Milne's
Tip 2.

Particularly insidious: "well-known" without citation (Tip 5). MR's
ironic definition ("known to more than a dozen people for more than
two years") is a reviewer-side litmus test.

## ML-Paper-Specific Advice (Distinguished from General Writing)

Milne is **mathematical-writing satire**. The patterns apply to ML
papers' math sections:

- Tip 1 (no explanation of conditions) — applies to ML
  hyperparameter-choice justification.
- Tip 2 (referring to obscure paper for definitions) — applies to ML
  notation imported from a single prior paper without restatement.
- Tip 5-6 (vague citations) — applies to ML "well-known" overuse.
- Tip 8 (muddle quantifiers) — applies to ML claims like "our method
  outperforms baselines on most datasets" (cf.
  [[Lipton-Writing-Heuristics]] #11 hostage to fortune).

The Milne satirical register is **complementary** to Lipton's
positive-voice heuristics: same content, different rhetorical mode.

## Connection to SiS / CTPC

Each anti-pattern is a check for [[CTPC-KDD-Submission]]:

1. Are CTPC's conditions (e.g., `ν_min = 4.5`, RTN frame, NASA CDDIS
   regime) explained, not just stated?
2. Are nonstandard SiS terms (CTPC, PhyArch, Latent NCDE Corrector)
   defined in the paper or only cited via [[CTPC-Design-Rationale]]?
3. Is one notation used consistently (does `D` mean dissipation
   matrix throughout, or does it shift to data anywhere)?
4. Are mathematical objects named per their set/type
   (`L` = lower-triangular factor, `R` = dissipation matrix, `H` =
   Hamiltonian — Milne-compliant)?
5. Every "well-known" result in CTPC — is it cited?
6. Citations to van der Schaft & Jeltsema — specific chapter / page,
   not just the book?
7. Theorem/proposition numbering — unified or separate (Year-2
   Analytic-Σ-NCDE theorem)?
8. Quantifiers — "for all orbital regimes", "for some perturbation
   types" — placed precisely?
9. Sentence starts — does any sentence start with a symbol like
   `Ḣ ≤ 0`?
10. "Such that" vs "so that" audit.

## Connections

- [[ICML-Best-Practices]] — cites Milne in resource list
- [[Ten-Rules-Mathematical-Writing]] — Bertsekas overlaps on small
  rules (Tip 9, Tip 10)
- [[Guide-Writing-Mathematics]] — HKU guide overlaps on Tip 9
- [[Ramdas-Paper-Checklist]] — item 7 directly contradicts Milne Tip
  7 (Ramdas says "Number each type separately"); the SiS convention
  should follow [[Ramdas-Paper-Checklist]] for stat-ML alignment, not
  Milne's longer-book-oriented warning
- [[Strunk-White-Elements-Style]] — Tip 10 (such-that / which-that)
  overlaps
- [[CTPC-KDD-Submission]] — pre-submission audit target

## Open Questions

1. **Theorem-numbering convention conflict** between
   [[Ramdas-Paper-Checklist]] item 7 (separate numbering recommended)
   and Milne Tip 7 (separate numbering = anti-pattern). The Ramdas
   recommendation is for short papers; Milne is for long books. For
   KDD '26 (~9 pages), Ramdas applies — use separate numbering.

## Sources

- `raw/writing/guides/Tips for Authors -- J.S. Milne.pdf` — J.S. Milne,
  hosted at https://www.jmilne.org/math/tips.html.
- 3-page PDF, full essay captured.
- Citations within: Littlewood's *Miscellany*; Hirschfeld in *Bull.
  Amer. Math. Soc.*; Roger Howe in MR 50:2128; Stacy Langton in MAA
  Online; I. Barsotti in MR 23#A2419; Anthony Knapp in *Notices AMS*
  Dec 2000; AMS *Notices* Oct 2000; Spike Milligan via Andrew Bremner
  in MR2001k:11041; Reuben Hersh in *Math. Intelligencer* 1997;
  George Orwell's *Politics and the English Language*; Nietzsche;
  J.K. Galbraith; Carl Linderholm.
