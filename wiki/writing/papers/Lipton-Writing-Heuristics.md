---
title: Heuristics for Scientific Writing (a Machine Learning Perspective)
tags: [writing, paper-writing, ml-writing, rhetoric, style, structure, citations, voice]
sources:
  - raw/writing/papers/guides/Heuristic_Scientific_Writing.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# Heuristics for Scientific Writing (a Machine Learning Perspective)

## One-Line Intuition

A compact, opinionated checklist of named heuristics — each a one-liner maxim with a short explanation — that tells ML researchers exactly how to write a paper that survives peer review without embarrassing itself.

## Why This Matters

Zachary C. Lipton (CMU, "Approximately Correct" blog, January 29 2018) wrote this piece because thousands of ML papers hit arXiv every conference cycle and many are unreadable — not because the science is bad, but because the writing is careless. Poor writing causes rejections, limits impact, and exposes authors to later criticism for sloppy scholarship. The heuristics were distilled from Lipton's PhD training under Charles Elkan and from his own advising practice: he found himself repeating the same one-liners to students and eventually wrote them down as a living document. The document is structured as easy-to-memorize dictates, each with a short explanation addressing language, positioning, or aesthetics. Lipton is explicit: these are heuristics, not laws — violating one is fine if you have a good reason.

---

## Core Content

The document organizes heuristics into four sections: **The Introduction**, **Organization**, **Style**, and **Language**, with a final section on **Bibliography**. Every heuristic has a name (in small-caps in the original) that functions as a mnemonic.

---

### Section 1: The Introduction

#### Heuristic 1: KEEP YOUR ABSTRACT SHORT

**Maxim:** You can't get it all out in the abstract. Don't even try.

**Explanation:** Think of the abstract as the 2-minute spotlight talk advertising your paper. Points should feel like bullets. Lipton offers one tried-and-true formula:

1. Contextualize the problem in one sentence or one phrase.
2. Identify what is wrong with existing approaches.
3. Go big: clearly state your major contribution (can also lead with this).
4. Two or three sentences to sell the details — the major quantitative result, etc.

**Exemplar:** Sanjoy Dasgupta's abstract for "Learning Mixtures of Gaussians" is cited as the first brilliant abstract Lipton ever read: it opens with "Mixtures of Gaussians are among the most fundamental and widely used statistical models," immediately identifies the problem with existing techniques ("local search heuristics with weak performance guarantees"), then leads with the contribution ("We present the first provably correct algorithm…"), and closes with quantitative specifics ("runs in time only linear in the dimension"). Lipton notes that Dasgupta could have merged the first two sentences for compactness — but leading with the key phrase "Mixtures of Gaussians" serves discoverability.

**Takeaway:** The abstract is an advertisement, not a miniature paper. Every sentence must earn its place.

---

#### Heuristic 2: DON'T TEASE THE READER

**Maxim:** If you have a great quantitative result, stick the number right in the abstract and the introduction.

**Explanation:** This is a direct follow-up to keeping the abstract short. If the paper yields a single equation that can be operationalized, put it in the introduction. People should read on because they are interested in the work, not because you are withholding information to create artificial suspense. Teasing is hostile to the reader.

---

#### Heuristic 3: DELETE GENERIC OPENINGS

**Maxim:** If the first sentence of your paper can be prepended to any paper in all of ML/big data, delete it.

**Explanation:** Examples of generic openings: "The last 10 years have witnessed tremendous growth in data and computers." Or: "Deep learning has had many successes at many things." These sentences convey nothing specific to your contribution and squander the most precious real estate in your paper — the first sentence of the introduction. First impressions matter. Don't squander them.

**Test:** Could your opening sentence appear, without modification, at the top of a random ML paper? If yes, delete it and write something specific.

---

#### Heuristic 4: Q BEFORE A

**Maxim:** Lead with a compelling real-world example, formalize it as an abstract problem, then close the loop with experiments that address the motivating case.

**Explanation:** It is difficult to get excited about a solution if you don't believe there is a problem. If your paper is completely abstract and has no bearing on the real world, it should be evaluated as a work of pure mathematics — and it probably won't fare well in that theater. The structure should be: problem (Q) → formalization → solution (A) → experiments that close the loop on the original motivating example.

---

#### Heuristic 5: FOCUS ON WHAT YOUR METHOD DOES, NOT WHAT IT DOESN'T DO

**Maxim:** When all else is equal semantically, ditch the indirection and say precisely what something is — not what it isn't.

**Explanation:** Sometimes you need to set up a contrast, and describing the negative is necessary. But don't get bogged down describing ideas in the negative, especially your own. The positive formulation is more readable. This is especially important for your own methods — describe what they do, not what they avoid doing.

---

### Section 2: Organization

#### Heuristic 6: WORDS ARE NOT SENTENCES. SENTENCES ARE NOT PARAGRAPHS. PARAGRAPHS ARE NOT SUBSECTIONS. SECTIONS CONTAIN MORE THAN ONE (OR ZERO) SUBSECTIONS. PAPERS CONTAIN MORE THAN ONE SECTION.

**Maxim:** Structural hierarchy must be respected at every level. A lousy writer's paper looks wrong before you read a single word.

**Explanation:** This is a multi-layered organizational rule:

- **Sections** must be balanced — if you list the section titles, they should feel like they belong to the same scope.
- **Paragraphs** have a minimum of 3 sentences. Occasionally 2 sentences is fine, but the safe heuristic is 3 minimum.
- **Subsections** must be plural if they exist; a section with a single subsection is a structural error.
- **Papers** must have more than one section.

The overall point: the visual structure of a paper communicates before the content does. A well-organized paper looks like an organized paper at a glance.

---

#### Heuristic 7: A READER SHOULD UNDERSTAND YOUR PAPER JUST FROM LOOKING AT THE FIGURES, OR WITHOUT LOOKING AT THE FIGURES

**Maxim:** The paper must be self-contained in the text AND the figures must tell a coherent standalone story.

**Explanation:** This is a dual requirement:

- **Text-complete:** A blind reader (no figures) should understand precisely what you do. Every critical observation or technical detail must appear in the main text. Figures are for visual corroboration.
- **Figure-complete:** A reader who skips to the figures (reviewers will) should be able to see roughly what's going on and understand the significance of the findings. If it's not obvious whether higher or lower scores on the y-axis are better, the caption must say this.

**Captions:** Should be 1 to 3 lines. Not giant paragraphs. Lipton notes that the computer vision community has a different relationship with figures — sometimes a single figure takes over a page with 100s of words — but he does not like this style and defers to community norms.

---

#### Heuristic 8: QUICKLY ARRIVE AT THE PAPER'S CONTRIBUTION

**Maxim:** Most of your abstract (by sentences), your intro (by paragraphs), and your paper (by pages) should articulate what YOU do.

**Explanation:** Lipton confesses that as a young PhD student, being an outsider to ML, he tried to make each paper fully understandable to an outsider. This won him readers in the general public but likely cost him conference rejections. Longwinded front-matter in conference papers is bad for two reasons:

1. Reviewers read 5–10 papers per conference and 50–100 papers per year in very similar areas. The basics will bore them.
2. If your contribution starts on page 5 of an 8-page paper, you have very little excuse for having failed to do anything the reviewer asks for.

**Two issues:** knowing your audience and positioning intelligently. The paper should get to what YOU do, fast.

---

#### Heuristic 9: ANTICIPATE THE READER'S QUESTIONS AND ANSWER THEM IN THE PAPER

**Maxim:** If you can anticipate the question and know the answer, write it. If you do not know the answer, run an experiment to find out.

**Explanation:** A good reviewer will try to come up with critical questions to challenge the proposed work: "Is it possible that this method only works because X?" If the answer is "I don't know" and "no" would be damning, the paper might rightly be rejected. This heuristic makes explicit that strong research and clear writing are tightly linked — writing forces you to confront the hard questions. If answering the question requires new experiments, that is not a writing problem, it is a research problem surfaced by the discipline of writing.

---

### Section 3: Style

#### Heuristic 10: THE SCIENTIFIC "WE"

**Maxim:** In scientific writing, narrate with the pronoun "we." This "we" refers to "you (the reader) and I/we (the authors) together."

**Explanation:** The scientific "we" has a didactic purpose — it guides the reader through the reasoning as a shared journey. Sometimes you may need to express an opinion. Those cases should be made clear from context (and see Heuristic 12 below). Lipton later clarifies in comments: "We the authors" means "We the authors of *this paper*" — not "we, the real people who authored it." This is important because it gives you the power to disagree later with your present arguments and find flaws in your present methods.

---

#### Heuristic 11: AVOID HOSTAGES TO FORTUNE

**Maxim:** Any qualified reader, even if they do not share your opinions, should be unable to disagree with any sentence in your paper in isolation.

**Explanation:** This is one of the most important heuristics in the document. A "hostage to fortune" is a claim that is technically defensible in your narrow interpretation but can be falsified by a hostile reader who takes it literally. The canonical example:

- **Bad:** "Our method X outperforms Y on *most* datasets." — Does it? Most out of what collection? Could the reviewer choose a dataset repository and find the statement false?
- **Better:** "Our method X outperforms Y on *many* datasets." — More precisely defined and much harder to disagree with.

The fix is simple: replace vague superlatives with quantified or hedged language. "Most" is an assertion that you probably cannot defend; "many" is a characterization you can.

---

#### Heuristic 12: A SIN OF OMISSION IS BETTER THAN A SIN OF COMMISSION

**Maxim:** If you are not 100% sure about a claim, do not make it.

**Explanation:** Related to avoiding hostages to fortune. It is hard to imagine reviewers rejecting a paper because you omitted a one-line boast. It is easy to imagine one line inspiring a rejection. When in doubt, cut the claim. The asymmetry of risk is stark: boasts can only hurt you; silence cannot.

---

#### Heuristic 13: WHEN YOU MUST EXPRESS AN OPINION, IDENTIFY IT AS SUCH

**Maxim:** If a factual assertion is actually your opinion, mark it as your opinion: "in our opinion, GANs…"

**Explanation:** You can include opinions in a paper — e.g., "the great promise of GANs for anomaly detection." But the factual assertion should be that it is your opinion. Reviewers can disagree with your opinions; they cannot reject a paper for opinions clearly labeled as such. Lipton adds in comments: "Very large" expresses an opinion he wouldn't allow in a technical paper body. In the introduction and discussion, some opinions are inevitable. "Given their success on X, we believe DNNs will prove important for Y" — this is an opinion, and it is scientifically relevant because it explains why you did the work or what you think are good ideas for future work. Label it clearly.

---

### Section 4: Language

#### Heuristic 14: BREAK UP LONG SENTENCES

**Maxim:** If you find yourself struggling to pack an idea in one sentence, it probably requires more than one.

**Explanation:** Young writers believe, mistakenly, that long sentences reflect language skill. Great scientific writers write mostly in short sentences. Technical writing should be as clear as possible. The contribution of your paper should be sophisticated ideas, not sophisticated sentence structure. Simplicity in form lets complexity in content shine.

---

#### Heuristic 15: JETTISON INTENSIFIERS AND VACUOUS ADVERBS

**Maxim:** Delete: Extremely, Very, Incredibly, Completely, Barely, Essentially, Rather, Quite, Definitely, …

**Explanation:** Intensifiers are bad for two reasons:

1. **They undermine their own purpose.** "Algorithm X provides a tight approximation" sounds confident. "Algorithm X provides a *very* tight approximation" drips with insecurity.
2. **They express opinions and thus create hostages to fortune.** "Is the algorithm better? Yes. Is it *much* better? That's an opinion" — and therefore a hostage.

Tom Dietterich (in comments) adds: "real" and "truly" are particularly toxic — "real intelligence" or "true learning" leave undefined terms because we don't know how to define them. Greg Ver Steeg adds: "complex" is a vacuous word; nobody thinks what they are doing is simple, so the qualifier rarely specifies anything distinctive.

---

#### Heuristic 16: SUBJECTS, VERBS AND MODIFIERS SHOULD ALL AGREE

**Maxim:** Attribute actions and desires to the correct subject — usually "we" (the authors), not the algorithm.

**Explanation:** A common mistake is attributing verbs and modifiers to the wrong subjects. Examples:

- **Wrong:** "the algorithm *tries* to X" — algorithms don't try.
- **Wrong:** "the data is *biased*" — in the sense of intent, data doesn't have desires.

If we are speaking of desires or intentions, they belong to "we," the modelers, not the algorithm. Lipton notes this sounds like common sense but errors of disagreement plague academic writing across all disciplines. In fields like interpretability and fairness in ML, sloppy subject-verb attribution can hold back the entire field.

**Corollary:** Every action should be attributed. Verbs with no subject often emerge in passive constructions (where the main verb is "to be"). "LSTMs are claimed to X, Y, Z." — Who is doing the claiming? This information must appear somewhere. Solutions:
  - Append a parenthetical citation.
  - Better: clearly put the claim in the mouth of its authors.

---

### Section 5: Bibliography

#### Heuristic 17: CITE GENEROUSLY

**Maxim:** If the works are relevant, you have nothing to lose and much to gain by citing them.

**Explanation:** The papers you ought to cite are likely written by the people who will be reviewing your paper. One common lame review: an anonymous reviewer asking why you didn't cite works A, B, and C (all by the same author). If the works are not relevant, do not cite them. If they are relevant, cite them. Good karma from citing generously: (1) you are less likely to get a hostile review, and (2) the cited authors may notice the citation and read your paper.

---

#### Heuristic 18: CITE THROUGHOUT

**Maxim:** Do not confine your citations to the related work section — cite whenever you invoke methods that precede your own.

**Explanation:** Reviewers are lazy and do not have photographic memories. If your work builds on others' contributions, do not confine citations to the related work section, which is just to summarize your work's context in the literature. Cite throughout the text whenever you invoke methods. This is especially true for recent work (last 5–10 years), which may not yet be common knowledge and should not be relegated to a citation-dense paragraph in the related work section.

---

#### Heuristic 19: EXHAUST THE REFERENCES LIMIT

**Maxim:** If you are squatting on a blank bibliography page, don't expect sympathy from reviewers.

**Explanation:** A pragmatic positioning point for conference publications that limit reference pages (often 1 or 2 pages). If you omit the most related work, reviewers will reject you regardless. If you omit some borderline related work and they call you on it, having no room left in the references section is a good excuse. Having blank space is not excusable — it signals you didn't engage with the literature.

---

## Key Verbatim Anchors

1. **p. 2** — "Think of the abstract as the 2-minute spotlight talk advertising your paper."
2. **p. 3** — "The first sentence is the most precious real estate in your introduction. Don't squander it."
3. **p. 6** — "If you can anticipate the question and know the answer, write it. If you do not know the answer, then run an experiment to find out. I hope this point hits home that doing strong research and writing clearly are tightly linked."
4. **p. 7** — "Any qualified reader, who goes through your entire draft, even if they do not share your opinions, preference for methods, or values in life, should be unable to disagree with any sentence in isolation."
5. **p. 7** — "A sin of omission is better than a sin of commission. […] It's hard to imagine the reviewers rejecting a paper because you omitted a one-line boast. It's easy to imagine one line inspiring a rejection."
6. **p. 8** — "The contribution of your paper should be sophisticated ideas, not sophisticated sentence structure."
7. **p. 8** — "'algorithm X provides a tight approximation' sounds confident, while 'algorithm X provides a *very* tight approximation' drips with insecurity."
8. **p. 9** — "If you are squatting on a blank bibliography page, don't expect sympathy from reviewers."
9. **p. 9** — "Cite throughout the text whenever you invoke methods that precede your own. This is especially true for recent work (last 5-10 years), which may not yet be common knowledge."
10. **p. 5** — "Longwinded front-matter in conference papers is bad… If your contribution starts on page 5/8, you have very little excuse for having failed to do anything the reviewer asks for."

---

## How a Writing Agent Should Use This

**Mode:** Pre-draft checklist + post-draft audit.

The paper-writing agent (Layer 1a) should apply Lipton's heuristics in two passes:

### Pass 1: Pre-draft structural decisions

Before a sentence is written, use these heuristics to set up the paper's architecture:

- **Abstract template:** Apply Heuristic 1's 4-point formula (contextualize → problem with existing → contribution → quantitative sell). Set a hard budget of ~4 sentences.
- **Introduction structure:** Q before A (Heuristic 4) — open with a real-world motivating example, not a generic "ML has grown" sentence (Heuristic 3). Get to the contribution fast (Heuristic 8).
- **Figure planning:** Commit to the dual-channel constraint (Heuristic 7) — the paper must be readable without figures AND the figures must tell a standalone story.
- **Anticipate reviewer questions:** Before writing experiments, list the 3–5 most likely reviewer objections and ensure the paper addresses each (Heuristic 9).

### Pass 2: Post-draft sentence-level audit (apply as a checklist)

Walk through every paragraph with these checks:

| Check | Heuristic |
|---|---|
| Abstract < 5 sentences? | H1 |
| Lead quantitative result in abstract/intro? | H2 |
| First sentence specific to this paper only? | H3 |
| Every claim falsifiable by a hostile reader? | H11 |
| All uncertain claims marked as opinions? | H12 |
| All intensifiers (very, extremely, most) removed? | H15 |
| All actions attributed to correct subjects? | H16 |
| Citations scattered through body, not just related work? | H18 |
| References page filled? | H19 |
| All paragraphs have ≥ 3 sentences? | H6 |
| Long sentences broken into ≤ 2 clauses? | H14 |

**Highest-priority heuristics for a first ML paper:** H11 (avoid hostages to fortune), H12 (sin of omission), H3 (delete generic openings), H8 (arrive at contribution quickly), H15 (no intensifiers). These are the most common failure modes and the ones most likely to cause a rejection.

---

## Connection to SiS / CTPC

Bilal writes ML-for-SDA papers where the contribution is a physics-constrained architecture (CTPC, Port-Hamiltonian correctors). Lipton's heuristics apply with particular force in this setting:

1. **Abstract formula for a CTPC paper:** (1) "Orbital propagation errors accumulate rapidly for low Earth orbit objects" [context]. (2) "Current ML correctors treat dynamics as unconstrained, violating conservation laws" [problem]. (3) "We present CTPC, a continuous-time probabilistic corrector that enforces dissipation as a hard architectural constraint" [contribution]. (4) "On N TLE tracks, CTPC reduces 72-hour position error by X% vs. SGP4 while producing calibrated uncertainty" [quantitative sell].

2. **Hostages to fortune are a critical risk** for physics-ML papers. Claims like "our method better respects physics than neural ODEs" are hostages — someone will find a counterexample. Better: "our method enforces Ḣ ≤ 0 as a hard architectural constraint via [specific block], which neural ODEs do not."

3. **Subjects and verbs:** The architecture enforces dissipation. The corrector models residuals. Bilal (the modeler) chose to enforce these constraints. The algorithm does not "try" to satisfy them — it provably does by construction. Lipton's Heuristic 16 is directly relevant to physics-as-architecture claims where the distinction between what the architecture guarantees vs. what the authors intend is load-bearing.

4. **Q before A:** The motivating example (a conjunction event where SGP4 covariance is unrealistic, leading to a missed collision warning) should precede the formalism. Reviewers need to believe the problem is real before they care about the solution.

5. **Don't tease:** If CTPC achieves a specific reduction in Pc estimation error on the CARA benchmark, put that number in the abstract and introduction. Reviewers should read on out of interest, not suspense.

---

## Connections

- [[Langley-Crafting-ML-Papers]] — complementary ML-specific writing guide; both emphasize structure and positioning
- [[Peyton-Jones-Research-Paper]] — covers how to write papers across all of CS; pairs with Lipton on abstract/intro structure
- [[Whitesides-Writing-A-Paper]] — Whitesides (chemistry perspective) has similar advice on outlines and iterative writing; contrast with Lipton's conference-focused stance
- [[Strunk-White-Elements-Style]] — the generic source for brevity and active voice; Lipton's heuristics are ML-instantiated versions of Strunk-White principles
- [[Williams-Bizup-Style]] — deeper treatment of subjects, verbs, and sentence rhythm; backs Heuristics 14 and 16
- [[Ramdas-Paper-Checklist]] — paper checklist format that operationalizes similar concerns; natural companion to Lipton's checklist heuristics
- [[Freeman-Writing-Papers]] — writing craft from a CS systems perspective; compare positioning advice
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — the authoritative source for reader-expectation theory; provides the why behind Lipton's subject-verb heuristic (H16)
- [[Goldreich-How-To-Write-Paper]] — theoretical CS perspective; compare treatment of contribution clarity

---

## Open Questions

1. Lipton's advice ("sin of omission > sin of commission") is maximally conservative for a conference setting. For journal papers with unlimited space, does this tradeoff shift? The related work sections of NeurIPS papers now routinely make strong comparative claims — has norms changed since 2018?

2. The "scientific we" (Heuristic 10) is a contested convention. Some venues (ICML style guide) prefer active first-person singular. Does Lipton's justification — that "we" refers to authors + reader journeying together — hold up when the paper has a single author?

3. Heuristic 7 (dual-channel: readable with and without figures) appears to conflict with the computer vision community's figure-first culture that Lipton acknowledges. For SDA papers that must include orbital trajectory visualizations, how do we apply this heuristic without making captions into paragraphs?

4. Lipton says to anticipate reviewer questions and answer them (H9) — but he does not address the case where answering the question would require experiments that are out of scope or computationally infeasible. What is the correct rhetorical move when you can anticipate the question but cannot run the experiment?

5. The "exhaust the references limit" heuristic (H19) is pragmatically sound but potentially at odds with conciseness values. What is the right balance for a 9-page ICML submission where references are on page 10?

---

## Sources

- `raw/writing/papers/guides/Heuristic_Scientific_Writing.pdf`
  - pp. 1–2: Introduction and motivation; abstract heuristics (H1, H2)
  - pp. 3–4: Delete generic openings (H3); Q before A (H4); positive framing (H5); organization rules (H6)
  - pp. 4–5: Figures dual-channel (H7); quickly arrive at contribution (H8)
  - pp. 5–6: Anticipate reviewer questions (H9); scientific "we" (H10)
  - pp. 6–7: Avoid hostages to fortune (H11); sin of omission (H12); label opinions (H13)
  - pp. 7–8: Break up long sentences (H14); jettison intensifiers (H15); subject-verb agreement (H16)
  - pp. 8–9: Bibliography — cite generously (H17); cite throughout (H18); exhaust references (H19)
  - pp. 10–17: Author bio and reader comments (Tom Dietterich on intensifiers; Greg Ver Steeg on "complex"; Lipton clarifications on "we" and opinions)
