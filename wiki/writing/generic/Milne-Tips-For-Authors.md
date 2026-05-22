---
title: Milne — Tips for Authors (Satirical Anti-Pattern Checklist)
tags: [writing, generic, anti-patterns, mathematical-writing, clarity, precision, citations, quantifiers, notation]
sources:
  - raw/writing/generic/Tips_For_Authors.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# Milne — Tips for Authors (Satirical Anti-Pattern Checklist)

## One-Line Intuition

J.S. Milne lists, in straight-faced satirical form, eleven things you should do if you want your math paper to be unreadable — read every item as its exact opposite, and you have a precise checklist for clear mathematical and scientific writing.

## Why This Matters

Milne's conceit is that clarity is dangerous: "If you write clearly, then your readers may understand your mathematics and conclude that it isn't profound. Worse, a referee may find your errors." The irony is the point. Every anti-pattern he names is drawn from real published papers — some from Fields-medal-level mathematicians — which makes the list diagnostic rather than hypothetical. For a PhD student and startup CTO writing papers, proposals, and technical reports, the eleven items cover the most common failure modes in mathematical exposition: undefined notation, hidden assumptions, vague references, ambiguous quantifiers, and deliberate obscurity. The Notes section amplifies each item with real quotations from mathematical culture (Galbraith, Nietzsche, Lindenholm, Orwell) that make the underlying stakes explicit.

## Core Content

Every anti-pattern below is stated as Milne writes it (ironic imperative), followed by the inverted real advice. The "Notes" elaboration column captures what Milne's own footnotes add.

| # | Anti-Pattern (Milne's ironic advice) | Inverted Real Advice | Milne's Notes / Examples |
|---|--------------------------------------|----------------------|--------------------------|
| 1 | Never explain why you need all those weird conditions, or what they mean. Begin with two pages of notations and conditions without saying what they accomplish. Never explain what you are doing or why. The best-written paper is one where the reader won't discover what you proved until after reading the whole paper. | **Motivate every condition at first use.** State what your main result is — and why it matters — near the beginning. A reader should know what you proved before finishing the introduction, not after. | Littlewood's Miscellany, p.57: a paper opened with "The object of this paper is to prove (something very important)" and the object turned out to be unachieved. Vague "vogue words" like "we address Hilbert's nth issue" let you claim credit without actually claiming anything. |
| 2 | Refer to another obscure paper for all basic nonstandard definitions, or never explain them at all. Never explain your sign conventions — if you do, someone may prove your signs are wrong. | **Define every nonstandard term in-paper.** State sign conventions explicitly at first use. Do not outsource basic definitions to an obscure reference the reader cannot access. | A paper Milne reviewed used a term throughout — including in the statement of the main theorem — for which even the author (when asked) knew no definition. |
| 3 | When having difficulty proving a theorem, use "variation of definition" — implicitly use more than one definition for a term within a single proof. | **Use one definition per term throughout a proof (and throughout the paper).** If a definition must be generalized mid-proof, state this explicitly and justify it. | (No separate note; the anti-pattern itself is the warning — it is a form of equivocation.) |
| 4 | Use *c*, *a*, *b* to denote elements of sets *A*, *B*, *C* respectively. | **Match notation to the objects it names.** Use systematic, mnemonic naming: elements of set *A* should be *a*, *a'*, *a₁*, etc. Inconsistent naming forces the reader to maintain a lookup table in working memory. | Littlewood's Miscellany, p.60: Jordan, if he had four things on the same footing (*a,b,c,d*), would write them as *a*, *M₃'*, *ε₂*, *Π"₁,₂*. |
| 5 | When using a result in a proof, don't state the result or give a reference. Conceal that you are even making use of a nontrivial result. | **Name every nontrivial result you invoke, and give a precise reference.** Saying "by a standard argument" or silently applying a theorem is neither pleasing nor helpful (Hirschfeld, Bull. AMS 27, 1992, p.331). "Well-known" is operationally defined as "known to more than a dozen people for more than two years" (Roger Howe). | Grothendieck called a statement "well-known" in *The Hodge conjecture is false for trivial reasons* that was not in fact known at the time; in the published version he added a footnote that it was now known to Deligne. |
| 6 | If you do refer to a paper for a result, never say where in the paper the result can be found. Alternatively, refer to an earlier paper containing only a weaker result. | **Give the specific theorem number or page number for every cited result.** Citing a 2500-page work (e.g., Dunford & Schwartz) without a page number is equivalent to not giving a reference. Never substitute a reference to a weaker result for a reference to the actual result you use. | Krantz's *Primer of Mathematical Writing*, p.76: "do not give in-text bibliographic references that have the form 'see Dunford and Schwartz'." Milne quotes a reviewer (Langton) catching Krantz himself violating this rule in another of his books. |
| 7 | In long articles, number theorems, propositions, corollaries, definitions, and remarks separately. That way no reader will have the patience to track down your internal references. | **Use a single, unified numbering scheme for all labeled items** (theorems, lemmas, definitions, remarks, equations). Sequential numbering like "Theorem 3.1, Definition 3.2, Remark 3.3" is far easier to navigate than parallel independent counters. | Barsotti (MR 23#A2419): independent numbering of lemmas, theorems, propositions, corollaries, plus separate numbering for formulae, creates "a partial ordering obvious to no one" and produces strain on readers who then take revenge on other readers when they become authors. |
| 8 | Write A==>B==>C==>D when you mean (A==>B)==>(C==>D). Muddle your quantifiers. Write "If A, B, C" when you mean "If A, then B and C" or "If A and B, then C." | **Parenthesize logical implications explicitly to remove ambiguity about binding.** Write quantifiers in unambiguous English ("for all x there exists y such that…") rather than relying on symbol order. | The "quantifier negation syndrome" (QNS, Notices AMS, October 2000, p.1041): "Not all boys like mathematics" is NOT equivalent to "All boys do not like mathematics." Browder's congressional testimony suffered from this publicly. |
| 9 | Begin and end sentences with symbols wherever possible. Since periods are almost invisible (and may be mistaken for a mathematical symbol), most readers won't notice that you've started a new sentence. Attach superscripts signalling footnotes to mathematical symbols rather than words. | **Never begin or end a sentence with a mathematical symbol.** Put a word before the symbol at the start of a sentence (e.g., "Thus *f* is continuous" not "*f* is continuous"). Attach footnote superscripts to words, never to symbols, to avoid visual confusion. | Standard mathematical typesetting guidance; the invisibility of periods near symbols like period-subscript-period is a genuine typographic hazard in dense notation. |
| 10 | Write "so that" when you mean "such that" and "which" when you mean "that." Always prefer the ambiguous expression to the unambiguous and the imprecise to the precise. It is the reader's task to determine what you mean. | **Use "such that" for defining clauses after quantifiers.** Use "that" for restrictive relative clauses and "which" only for non-restrictive (parenthetical) ones. Prefer precise words: if you mean "for all," write "for all," not "for any." | Knapp (Notices AMS 47, no.11, December 2000, p.1356): careless use of "that"/"which" blurs the distinction between hypotheses and remarks; sloppy negatives and quantifiers leave the reader "totally confused." Milne also flags the paper in MR2001k:11041, which a reviewer found more confused on each re-reading. |
| 11 | If all else fails, write in German, Russian, or Turkish (or another language most mathematicians can't read). | **Write in the language of your primary audience.** If writing in English, write in clear English. Deliberate obscurity — whether linguistic or stylistic — is not a substitute for thought; it merely signals that the thought was not completed. | Robert Langlands, whose native language is English, follows this precept (writes clearly in English). Milne closes with the Orwell Ecclesiastes example: the King James version is vivid and clear; the "modern" paraphrase is bureaucratic mush. |

### Additional diagnostic patterns from the Notes section

| Pattern | Source | Real Advice |
|---------|---------|-------------|
| Using vague "vogue words" ("we address," "we consider") that let you claim to solve a problem without actually claiming to solve it | Milne, p.1 | State your results precisely: "We prove X" or "We disprove X." Do not hide behind hedged verbs. |
| Calling a result "well-known" without a reference | Roger Howe / Grothendieck anecdote, p.2 | "Well-known" is defined operationally: known to more than a dozen people for more than two years. If it qualifies, give the reference anyway. |
| Writing imprecisely because you believe obscurity signals depth | Galbraith / Nietzsche / Lindenholm quotes, p.1 | "Those who know that they are profound strive for clarity. Those who would like to seem profound strive for obscurity." (Nietzsche) |
| Symbol-heavy, word-light definitions that force the reader to reconstruct meaning | "r-regular graph" example, p.3 | Compare: "A graph is *r*-regular if *r* edges *e* have origin(*e*)=*v* for all vertices *v*" vs. "A graph is *r*-regular if each vertex is the origin of exactly *r* edges." The second is unambiguous; the first requires the reader to work out what "have origin = v" means. |

## Key Verbatim Anchors

All page references are to the 3-page PDF (raw/writing/generic/Tips_For_Authors.pdf).

1. **p.1** — "If you write clearly, then your readers may understand your mathematics and conclude that it isn't profound. Worse, a referee may find your errors. Here are some tips for avoiding these awful possibilities." [The entire ironic premise of the document.]

2. **p.1** — "The best-written paper is one in which the reader will not discover what you have proved until he has read the whole paper, if then." [Anti-pattern 1; inverted: state your result clearly near the top.]

3. **p.1** — "It is the readers task to determine what you mean; it is not yours to express it." [Anti-pattern 10; inverted: precision is the author's obligation, not the reader's puzzle to solve.]

4. **p.1–2** — "More serious was the repeatedly expressed longing of so many of the graduate students to possess a writing style that, far from being lucid and clear, was dense, knotted, oblique, difficult, ambiguous and verbally complex. This would make them impressive academics, worthy to be taken seriously..." [Galbraith, TLS, April 2, 2021 — the cultural pressure toward obscurity that Milne is pushing back on.]

5. **p.2** — "Those who know that they are profound strive for clarity. Those who would like to seem profound strive for obscurity." — Nietzsche.

6. **p.2** — "Mathematicians always strive to confuse their audiences; where there is no confusion there is no prestige. Mathematics is prestigiditation." — Carl Lindenholm, *Mathematics Made Difficult*, p.10.

7. **p.2** — "well-known: known to more than a dozen people for more than two years (MR 50:2128, Roger Howe)." [Operational definition of "well-known" — anything short of this needs a reference.]

8. **p.3** — Orwell's Ecclesiastes vs. modern-English paraphrase. Original: "I returned and saw under the sun, that the race is not to the swift..." Paraphrase: "Objective considerations of contemporary phenomena compels the conclusion that success or failure in competitive activities exhibits no tendency to be commensurate with innate capacity..." [The most concrete illustration of what obscurity costs: everything vivid and specific is replaced by nothing.]

9. **p.2** — "The QNS (quantifier negation syndrome) strikes again (Notices AMS, October 2000, p1041)." [On anti-pattern 8; quantifier confusion is endemic and named.]

10. **p.3** — "A graph is *r*-regular if *r* edges *e* have origin(*e*)=*v* for all vertices *v*" (bad) vs. "A graph is *r*-regular if each vertex is the origin of exactly *r* edges" (good). [Concrete minimal pair showing imprecise vs. precise mathematical English.]

## How a Writing Agent Should Use This

**Primary agent: Layer 0 — Generic Writing Craft Agent.**
**Secondary agent: Layer 1a — Paper-Writing Agent** (for the mathematics-specific items 3, 4, 5, 6, 7, 8, 9).

### Modes of use

**Pre-draft checklist.** Before writing any section, the agent should silently scan for susceptibility to each of Milne's 11 anti-patterns. Items 1, 5, and 6 are the most common failure modes in technical paper introductions.

**Post-draft audit.** Run each anti-pattern as a binary check against the draft. The table above is directly usable as a structured prompt:
- Anti-pattern 1: Does the introduction state what was proved, and why the conditions are needed?
- Anti-pattern 2: Is every nonstandard term defined in-paper? Are sign conventions stated?
- Anti-pattern 3: Is each defined term used with exactly one meaning throughout each proof?
- Anti-pattern 4: Is notation consistent? Do element names match their set names?
- Anti-pattern 5: Is every nontrivial result named and cited with a specific theorem/page number?
- Anti-pattern 6: Does every citation give a specific theorem number or page number?
- Anti-pattern 7: Is there a single unified numbering scheme for all labeled items?
- Anti-pattern 8: Are implications parenthesized? Are quantifiers explicit ("for all," "there exists")?
- Anti-pattern 9: Does any sentence begin or end with a bare mathematical symbol?
- Anti-pattern 10: Is "such that" used for defining clauses? Is "that/which" used correctly?
- Anti-pattern 11: Is every sentence written for the intended reader, not for the appearance of depth?

**Irony awareness.** Because the source is ironic, the agent must never quote Milne's anti-patterns as positive advice. Always invert before citing. The verbatim anchors section above provides safe quotes that are clearly in the "real advice" register (Nietzsche, Galbraith, the Orwell example).

**Citation format.** When the agent uses Milne in a writing critique, cite as: Milne, J.S. "Tips for Authors." www.jmilne.org/math/tips.html. (Pagination by PDF page 1–3.)

## Connection to SiS / CTPC

Bilal writes across three registers: ML conference papers (NeurIPS, ICML, ICLR), aerospace/SDA technical reports (AFRL, AAS), and grant proposals (NSF, DoD). Milne's checklist is register-agnostic — every item applies to all three. The highest-risk anti-patterns for Bilal's work specifically:

- **Anti-pattern 1 (hide the result):** CTPC papers must state the core claim — "we enforce Ḣ ≤ 0 as a hard architectural constraint, not a loss penalty" — in the abstract and introduction, not buried in Section 4. Reviewers who cannot find the claim in two minutes reject the paper.
- **Anti-pattern 2 (undefined nonstandard terms):** SDA has domain-specific notation (TLE, SGP4, CDM, RTN, equinoctial elements) that ML reviewers do not know. Every SDA term must be defined in-paper, not delegated to a cited standard.
- **Anti-pattern 5 (invoke results without naming them):** CTPC relies on Port-Hamiltonian theory, Riemannian geometry on SPD matrices, and Cholesky parameterization. Each nontrivial mathematical claim must cite the specific theorem it rests on (e.g., "by the Schur complement argument, Theorem 3.2 in [van der Schaft 2006]").
- **Anti-pattern 6 (vague citations):** Citing "Marsden & Ratiu" without a theorem and page number for a geometric mechanics claim is equivalent to no citation. AFRL reviewers and SBIR program managers expect precision.
- **Anti-pattern 8 (muddle quantifiers):** The dissipation condition Ḣ(x) ≤ 0 for all x is not the same as "Ḣ ≤ 0 for some x" or "¬(Ḣ > 0 for all x)." Quantifier precision is not pedantry — it is the difference between a correct claim and a false one.
- **Anti-pattern 10 (imprecise connectives):** In a proposal, "the corrector models residuals such that the Hamiltonian decreases" is substantively different from "the corrector models residuals, which decrease the Hamiltonian." The first is a constraint; the second is a description.

The Nietzsche quote is a direct restatement of the SiS core philosophy applied to writing: precision is not an obstacle to appearing serious — it is the only path to being taken seriously.

## Connections

- [[Strunk-White-Elements-Style]] — the grammar and economy foundation; Milne's anti-patterns 9 and 10 are direct applications of Strunk-White rules
- [[Krantz-Primer-Mathematical-Writing]] — Milne explicitly cites Krantz p.76 on citation precision (anti-pattern 6); the two sources are complementary
- [[Bertsekas-Ten-Rules-Mathematical-Writing]] — Bertsekas's rules on notation consistency and theorem attribution overlap with Milne anti-patterns 4, 5, 7
- [[Williams-Bizup-Style]] — Williams's clarity principle ("reader's task is comprehension, not decryption") is the positive complement to Milne anti-pattern 10
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — stress-position principle is the positive form of Milne anti-pattern 1 (result belongs in the stress position)
- [[Schimel-Writing-Science]] — narrative arc principle reinforces anti-pattern 1: state your result, then prove it
- [[HKU-Guide-Writing-Mathematics]] — mathematical writing pedagogy; shares the quantifier and notation precision concerns (anti-patterns 4, 8, 10)
- [[Pak-Clear-Math-Paper]] — another mathematical writing checklist; compare with Milne's list item by item for redundancy and gaps
- [[Lipton-Writing-Heuristics]] — ML-specific; Lipton's "be precise" heuristic is the direct ML instantiation of Milne anti-patterns 8 and 10
- [[Goldreich-How-To-Write-Paper]] — Goldreich's emphasis on stating results clearly in the introduction is the positive form of anti-pattern 1
- [[Ramdas-Paper-Checklist]] — a checklist format like Milne's but in positive rather than ironic register; the two are complementary as pre-submission audit tools

## Open Questions

1. **Proposal register:** Milne's eleven items are calibrated for mathematical proofs. Do they transfer verbatim to SBIR/STTR proposals, where "results" are often claims about future work? Which items need adaptation for the proposal register?
2. **Anti-pattern coverage gaps:** Milne does not address figure/table captioning, abstract structure, or related-work framing. Which other sources in the wiki fill these gaps? Candidates: [[Peyton-Jones-Research-Paper]] (abstract structure), [[Langley-Crafting-ML-Papers]] (related work).
3. **Irony as pedagogy:** Is the ironic register more effective than the direct positive-checklist format? Compare retention of Milne's list vs. [[Ramdas-Paper-Checklist]]. The Notes section suggests Milne believes the irony is itself a rhetorical device — making each anti-pattern more memorable by attaching it to a named real failure.
4. **Anti-pattern 3 (variation of definition):** This is the subtlest item — it describes a proof-level equivocation that is hard to catch in review. Is there a systematic detection method (e.g., term-consistency checking in a writing agent)?
5. **Orwell's essay:** Milne recommends Orwell's "Politics and the English Language" as required reading. That essay is not yet in the wiki. Should it be ingested as a separate generic writing source?

## Sources

- Milne, J.S. "Tips for Authors." Available at www.jmilne.org/math/tips.html. Local copy: `raw/writing/generic/Tips_For_Authors.pdf`, pp.1–3.
  - p.1: 11 anti-patterns (main list)
  - p.1: opening framing + Galbraith quote (TLS, April 2, 2021) + Nietzsche + Lindenholm
  - p.2: Notes on anti-patterns 1–8; Littlewood's Miscellany pp.57,60; Hirschfeld Bull.AMS 27 (1992) p.331; Roger Howe well-known definition; Grothendieck / Deligne anecdote; Langton review of Krantz; Barsotti MR 23#A2419; Browder / QNS (Notices AMS Oct 2000, p.1041)
  - p.3: Notes on anti-patterns 8–11 cont.; Knapp (Notices AMS 47, no.11, Dec 2000, p.1356); *r*-regular graph minimal pair; Orwell Ecclesiastes example; Reuben Hersh further reading pointer
