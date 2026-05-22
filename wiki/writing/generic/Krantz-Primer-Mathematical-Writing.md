---
title: "A Primer of Mathematical Writing — Steven G. Krantz"
tags: [writing, generic, mathematical-writing, exposition, proof-writing, LaTeX, notation, paper-structure, book-writing, grant-writing]
sources:
  - raw/writing/generic/Primer_Math_Writing.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# A Primer of Mathematical Writing — Steven G. Krantz

## One-Line Intuition

A complete operating manual for mathematical exposition: how to arrange notation, sentences, theorems, proofs, abstracts, bibliographies, and entire papers so that a reader can follow the mathematics rather than fight through it.

## Why This Matters

Mathematical writing has no natural pedagogy — mathematicians learn by imitation of papers they were exposed to as students, inheriting both their mentors' virtues and their vices. Krantz's aim is to make the principles explicit. The thesis: good mathematical exposition is not decorative. It is a precision instrument for compressing ideas into a form that survives transmission across time and readers. A poorly written proof that is logically correct communicates less than a well-written one — the logic is inaccessible. For SiS, where the core claim (physics-as-architecture, hard constraints vs. soft penalties) must be made persuasively to referees, grant panels, and collaborators who are not orbital mechanics specialists, the quality of the exposition determines whether the contribution is seen or missed.

---

## Core Content

### Chapter 1 — The Basics of Mathematical Writing

#### §1.1 — Say Something

The first principle: **have something to say**. Writing exists to communicate an idea. Before composing a sentence, know the one central claim. Everything else is scaffolding. Krantz distinguishes communicating authors (who write toward a reader) from non-communicating authors (who write toward their own notes).

#### §1.2 — Writing and Thought

Writing is thinking made visible. The act of writing forces precision — vague intuitions survive inside one's head but cannot be set down in sentences without cracking. Krantz: "If you cannot write it, you do not understand it." Implication for SiS: the methods section of a CTPC paper is a diagnostic instrument — if any sentence about the corrector architecture requires hedging or qualifications not grounded in a specific equation, the architecture itself needs more work.

#### §1.3 — On the Organization of Writing

**Organization is everything.** The reader's attention is a finite, scarce resource. The writer must earn every sentence. Macro-level organization: the paper must build toward its central result without detours. Micro-level: each paragraph has one purpose; the last sentence of a paragraph must connect to the first sentence of the next.

**Rule:** Nontechnical introduction → formal definitions → main result (big-steps proof) → technical lemmas and details. This is not optional. Violating it (e.g., burying definitions after theorems, or putting lemmas before the reader knows what they are for) forces the reader to read twice.

#### §1.4 — Paragraphing

- Short paragraphs over long paragraphs. Each paragraph: one idea, one purpose.
- The first sentence of a paragraph announces the topic; the last sentence closes it or pivots.
- White space is a reading aid. Dense unbroken blocks of text signal to the reader that the author has not done the work of organizing.

#### §1.5 — Sentences

- Keep sentences short. English mathematical prose should have an average sentence length under 25 words. Long sentences with embedded subclauses bury the verb and lose the reader.
- Prefer active voice. "We prove that X is Y" over "It can be shown that X is Y." The passive voice distances the author from the claim and weakens the assertion.
- The most important word in a sentence goes at the end. Structure sentences so that the novel, stressed information lands last.

**Example rule — modus ponendo ponens:**
Write "If [hypothesis], then [conclusion]" — never invert this to "We can conclude [conclusion], provided [hypothesis]." The conditional structure tells the reader which direction causation runs.

**Critical rule — never begin a sentence with a symbol.** Begin with a word. "Let $f$ be..." not "$f$ is..." This is not stylistic preference — it is a readability constraint: the eye cannot resolve whether the symbol starts a sentence or continues the previous one without re-reading.

#### §1.6 — A Word about Words

- **Prefer English over symbols.** A sentence like "For all $\epsilon > 0$, there exists $\delta > 0$ such that..." is standard, but "For every positive $\epsilon$, there is a positive $\delta$ such that..." is more readable. Default to English when the symbolism adds no precision.
- **Notation minimalism.** Introduce a symbol only if: (a) it will be used at least three times, and (b) there is no short English phrase that carries the same meaning. Each extra symbol is cognitive load for the reader.
- **Consistency.** Once a symbol is chosen, use it everywhere. Never let $f$ mean two different things in the same paper. Never let two symbols mean the same thing.

**Specific word rules (all of these appear as explicit prescriptions in Krantz):**

| Term | Rule |
|---|---|
| `all`, `any`, `each`, `every` | They are not synonymous. "For all $x$" is universal. "For any $x$" is ambiguous — sometimes existential in colloquial use. Use `every` or `each` for universal quantification; reserve `any` for queries. |
| `cf.` | Means "compare with", not "see". Do not use as a synonym for `see`. |
| `e.g.` | Always followed by a comma. Means "for example." |
| `i.e.` | Always followed by a comma. Means "that is." |
| `comprise` vs. `compose` | "The set comprises its elements" (whole comprises parts). "The elements compose the set" (parts compose whole). Never "comprised of." |
| contractions | Never in formal mathematical writing. Not "don't", "can't", "it's". |
| `denote` | Specific meaning: "Let $X$ denote the set of..." — the symbol is being introduced with a name. Do not write "Let $X$ be the set of..." when you mean `denote`. |
| `obviously`, `clearly`, `trivially` | Banish these. They are false reassurances. If the step is obvious, it does not need to be named. If it is not obvious, the word is condescending. |
| `that` vs. `which` | Restrictive clauses: `that` (no comma). Non-restrictive: `which` (preceded by comma). "The function that satisfies..." (specifying) vs. "The function, which satisfies..., is..." (adding information). |
| serial comma | Use it: "notation, proof, and exposition." Omitting the final comma before `and` causes ambiguity. |
| hyphen vs. en dash in names | "Cauchy–Schwarz inequality" uses an en dash (–) because it is named after two people. "Stone-Weierstrass" likewise. Hyphen (-) within a single compound modifier. |
| first-use acronyms | Define every acronym at first use: "Continuous-Time Probabilistic Corrector (CTPC)." |
| `given` | Avoid. Replace "Given $f$ continuous, we have..." with "If $f$ is continuous, then...". `given` sounds passive and is ambiguous. |
| singular vs. plural | Prefer singular constructions when referring to a generic object. "The function $f$ satisfies..." not "Functions $f$ satisfy..." unless you mean all functions simultaneously. |
| `I` vs. `we` | Use `we` as the default first person in a paper even with a single author — it invites the reader into the proof. Reserve `I` for personal epistemic statements: "I believe this approach will generalize," "I conjecture..." |

#### §1.7 — Displayed vs. Inline Mathematics

**Decision rule for displayed math:** Display a formula if any of the following hold:
1. The formula is important enough that it will be referenced later by number.
2. The formula is long (more than about half a line of inline text).
3. The formula is the **conclusion** of an argument and deserves visual emphasis.
4. The reader needs to stop and examine it rather than read through it.

**Decision rule for inline math:** A formula stays inline if it is:
1. A short label or name for a quantity already introduced.
2. Part of a list of conditions where all conditions have similar weight.
3. Not going to be referenced again.

**Typographic detail:** In LaTeX, inline math uses `$...$`; displayed equations use `\[ ... \]` or the `equation` / `align` environments. Never write displayed math inline — it distorts line spacing and signals to the reader that you could not decide whether it was important.

**Prose before and after displayed math:** A displayed equation must be embedded in prose. Never have two consecutive displays without connecting text. Never have a display followed immediately by another display — write at least one sentence of English between them explaining the logical relationship. The sentence before a display sets up what the equation asserts; the sentence after explains what it means or where it is used next.

#### §1.8 — Grammar Appendix (Detailed Rules from Chapter 1)

Krantz catalogs the following additional grammar rules explicitly (pp. 35–60):

- **Word order matters.** In English, the natural word order encodes emphasis. In mathematical writing, reordering a sentence to move a key concept to the end is correct and advisable.
- **Comma abuse.** A comma in English is a logical pause. Never use a comma as a mathematical quantifier. "Let $n > 0$, $n$ an integer" is ambiguous. Write "Let $n$ be a positive integer."
- **Passive voice is weak.** "It is clear that..." is not a proof. "We show that..." is.
- **Avoid `where` at sentence end.** "Let $f: X \to Y$, where $f$ is continuous" is acceptable but weak. Prefer "Let $f: X \to Y$ be continuous."
- **Subject–verb agreement.** "The set of all functions that satisfy the condition is compact" — the verb agrees with `set`, not `functions`.
- **Split infinitives.** Avoid when possible but not at the cost of clarity. "To sharply bound the error" is worse than "to bound the error sharply."
- **Prepositions at end of sentence.** Allowable in informal writing; avoid in formal mathematical writing when easy to rearrange.
- **`shall` vs. `will`.** In mathematical writing, `will` is standard. `shall` is archaic.
- **`that` and `which`.** Restrictive (that, no comma) vs. non-restrictive (which, comma). Critical for precision because in math, "the map that is linear" restricts to a specific map, while "the map, which is linear," merely adds a description.
- **`its` and `it's`.** `it's` = it is (contraction, banned in formal writing). `its` = possessive.
- **`lay` and `lie`.** Transitive: lay, laid, laid. Intransitive: lie, lay, lain. Confusing these is a grammatical error, not a stylistic one.
- **`less` and `fewer`.** `fewer` for countable nouns; `less` for uncountable. "Fewer symbols, less confusion."
- **Numbers.** Spell out integers zero through ten; use numerals from 11 onward — except at the start of a sentence, where always spell out.
- **Plurals of foreign nouns.** `criteria` (not criterias), `matrices` (not matrixes), `indices` or `indexes`, `formulae` or `formulas` (both acceptable), `data` (plural of datum).
- **`infer` vs. `imply`.** The premises imply the conclusion. The reader infers the conclusion from the premises. The author cannot `infer` in a proof.
- **`its vs. it's`** (again): in mathematical sentences, `its` refers to the antecedent mathematical object — "the function reaches its maximum" — and is extremely common. `it's` never appears.
- **`if` vs. `whether`.** "We ask whether $f$ is continuous" (indirect question). "The result holds if $f$ is continuous" (conditional). Do not substitute one for the other.
- **`need only`.** Mathematical shorthand: "We need only show that..." is acceptable and useful.
- **`'suffices to'`.** "It suffices to show that $f$ is bounded" — standard mathematical idiom, correct.
- **Parallel structure.** Items in a list must be grammatically parallel. "The function is continuous, differentiable, and it satisfies the boundary condition" is wrong. Write "The function is continuous, differentiable, and boundary-conforming."
- **`'due to' vs. 'because of' vs. 'through'`.** "Due to" is adjectival ("the error, due to truncation, is small"). "Because of" is adverbial. Do not write "Due to compactness, $f$ attains its maximum" — write "Because of compactness..." or "By compactness, $f$ attains its maximum."
- **`'compare' and 'contrast'`.** These are not synonyms. "Compare $A$ with $B$" (find similarities). "Contrast $A$ and $B$" (find differences). Often the writer means "compare with."
- **`'farther' vs. 'further'`.** `farther` = physical distance. `further` = degree or extent. In mathematical writing, `further` almost always applies: "We further require that $f$ be injective."
- **`'hopefully'`.** Means "in a hopeful manner." Not a sentence modifier. "Hopefully the proof will work" is wrong. Write "We hope the proof will work" or "The proof should work."
- **`'different from' vs. 'different than'`.** Always `different from` in formal writing.

---

### Chapter 2 — The Structure of a Research Paper

#### §2.1 — Paper Components and Frontmatter

A research paper must contain: **title, author name(s), affiliation(s), postal addresses, date, abstract, keywords, AMS subject classification numbers, acknowledgments.** Every component has a function.

**Title rules:**
- Must be informative: tell the reader what the paper is about AND what point it makes.
- Avoid vague titles: "Some remarks on..." "On the theory of..." "A note on..." — these convey nothing.
- Avoid overly long titles. If more than 10 words, the paper probably has too broad a scope.
- The title is the first filter — 90% of potential readers decide to stop or continue based on the title alone.

**AMS subject classifications:** Include at least one primary and one secondary MSC code. This determines which reviewers see the paper and how it is indexed.

**Keywords:** 5–10 keywords that complement (do not duplicate) the title. Include the main technique and the main application domain.

#### §2.2 — The Abstract

**Rule: the abstract is a standalone document.** A reader who reads only the abstract must come away with: what was done, why it matters, and what the key result is. The abstract is NOT a table of contents for the paper.

**Concrete rules:**
- Length: **at most 10 lines** (roughly 150 words). Shorter is better.
- No notation. No symbols. No references. No technical jargon that is not standard in the field.
- Simple, declarative sentences. One sentence per idea.
- Must state the main result explicitly (not "we prove a theorem about...").
- Readership statistics: all readers read the abstract, ~20% read the introduction, ~5% read the body. The abstract is the paper for 95% of readers.

**Anti-patterns:**
- "In this paper we study..." (says nothing about what was found)
- "We prove several results..." (vague)
- "It is shown that..." (passive, vague — what is shown?)

**Good abstract pattern (Krantz's implicit template):**
1. One sentence: what problem is solved.
2. One sentence: what the key obstacle was.
3. One–two sentences: the main result (what is proved/shown/demonstrated).
4. One sentence: why it matters or what it implies.

#### §2.3 — The Introduction

The introduction is the most important section of the paper. It is the only section that a general reader, a referee, and a grant reviewer will certainly read.

**Structure:**
1. **Paragraph 1:** Non-technical statement of the problem. No formulas. What is the question? Why is it interesting?
2. **Paragraph 2:** Historical context. What was known? Who did it? (Primary citations here.)
3. **Paragraph 3:** The main result, stated informally. "We show that..."
4. **Paragraph 4:** Proof strategy at a high level. What is the key idea? What makes the approach work?
5. **Final paragraph:** Organization of the paper ("Section 2 establishes...").

**Rule:** The introduction must be self-contained enough that a non-specialist can understand why the paper is interesting. Save the technical machinery for later sections.

#### §2.4 — Theorems, Lemmas, Propositions, Corollaries

**Hierarchy:**
- **Theorem:** the main result. There should be few in a paper (ideally one central one).
- **Lemma:** a technical result proved in service of the theorem. State and prove the lemma close to where it is used.
- **Proposition:** a result of independent interest, less central than the theorem.
- **Corollary:** an immediate consequence of the theorem.

**Rules for stating theorems:**
- Group all cognate hypotheses into defined terms first. If the theorem requires six conditions on $f$, define a class (say, "admissible functions") that encodes all six, then state: "If $f$ is admissible, then..."
- Never state a theorem before the reader has the context to understand it. Build up definitions and intuition first.
- A theorem statement should fit in at most 10 lines. If it is longer, the hypotheses need to be consolidated into a definition.
- State the theorem in the sharpest (tightest hypotheses, strongest conclusion) form you can prove.

**Claim device:** For long proofs with multiple sub-assertions, use the **Claim** device: state the sub-assertion inline as "Claim: [assertion]." then immediately prove it before resuming the main proof. This structures the proof's logic without forcing the reader to track sub-lemmas in separate sections.

**Deferred proof device:** When a technical lemma is needed but its proof would disrupt the narrative flow, write "Proof deferred to Section N" and continue. Place the proof at the end of the paper or in an appendix. The reader can check the logic there if needed; the flow of the main argument is not broken.

**Proof by contradiction:** Always begin with "Seeking a contradiction, suppose that [negation of conclusion]." This signals the mode of argument immediately. Never just write "Suppose [negation]" without announcing the strategy.

#### §2.5 — Definitions

- Define a term **immediately before its first use**. Not at the start of a section "for completeness."
- Define only what the reader does not know. If the reader can be assumed to know it, don't define it — treating the reader as ignorant of standard knowledge is insulting.
- A definition can use "if" by custom even when it means "if and only if." "A function is continuous if for every $\epsilon > 0$..." — the "if" is an "iff" by convention in definitions.
- Formally, "iff" is correct in definitions. Halmos introduced `iff` as a written abbreviation for "if and only if"; it is acceptable in mathematical writing.
- **Never begin a sentence with "If and only if..."** — the reader cannot process this structure. Write "[Condition A] holds if and only if [Condition B]."
- Build definitions in steps. If a definition requires prior concepts, define those first.
- Use **terminology as an organizational tool**: a well-chosen defined term (like "admissible" or "stable") compresses a cluster of conditions into a single word, making subsequent prose dramatically cleaner.

#### §2.6 — The Proof

**Organizing the proof:**
- Open the proof with a one-sentence statement of the strategy: "We proceed by induction on $n$" or "The proof has two steps." This orients the reader.
- Break long proofs into labeled cases, steps, or sub-claims. Never write a proof as an undifferentiated wall of text.
- At each major step, write a transitional sentence explaining what was just established and what comes next.
- Close the proof with a sentence (not just the QED square) that states what was proved: "Thus $f$ is continuous, completing the proof." The tombstone ($\square$) is not enough.

**Redundancy:** Avoid. Do not re-derive what has already been proved. Cite the previous result by name or number.

**Proof length:** If the proof runs more than 2 pages, decompose it into lemmas. A proof that is too long to hold in working memory cannot be verified.

#### §2.7 — The Bibliography

**Rules:**
- **Primary sources only.** Do not cite secondary expositions unless you are citing the exposition itself. Cite the paper where a result first appears.
- **Cite specifically.** "See Smith [12]" is weak. "See Smith [12, Theorem 3.7, p. 45]" allows the reader to verify the claim.
- **Complete bibliographic data.** For a paper: authors, title, journal, volume, year, page range. For a book: authors, title, publisher, city, year, edition if not first.
- **Consistency.** All entries in the same format. Either all full given names or all initials — not mixed.
- **List only works cited in the paper.** A bibliography is not a reading list.
- **MathSciNet BibTeX entries** are reliable and consistently formatted. Use them.

**Citation styles:** Krantz discusses the numerical style (e.g., [12]) and the alphabetical key style (e.g., [Smi95]). Both are acceptable; choose the one your target journal uses.

#### §2.8 — Submitting a Paper to a Journal

**Protocol:**
- Submit to **one journal at a time**. Simultaneous submission is unethical.
- **Ranking:** Choose the journal whose scope and level match the paper. Do not submit to a top journal hoping for the best if the result is too narrow for its readership.
- **Cover letter:** At most one page. State the title, that it has not been submitted elsewhere, and why it is appropriate for this journal. Do not summarize the paper — the abstract does that.
- **After submission:** Expect 3–12 months for a decision. The referee process is slow. Do not contact the editor repeatedly.
- **Referee reports:** Read carefully. Respond to every point — either by making the revision or by explaining why the comment is mistaken. Be polite even if the referee is wrong. **The referee is not responsible for correctness** — that is the author's burden.

---

### Chapter 3 — Exposition and Other Forms

#### §3.1 — What Is Exposition?

Expository writing aims to explain, not to prove. The target reader is broader than for a research paper — the author must balance **detail and accessibility**. Krantz's core rule for exposition: **every expository piece must tell a story.** It must have a beginning that establishes a question, a middle that develops the argument, and an end that lands somewhere meaningful. Without narrative arc, exposition is just a list.

#### §3.2 — Writing a Survey Article

A survey article reviews a body of work for non-specialists. Rules:
- Decide on **scope** before writing. A survey that tries to cover everything covers nothing.
- Open with a **historical narrative**: when and why did this area begin? Who are the major figures?
- **Build to a climax**: the most important recent results should appear near the end, so the reader arrives at them with enough background to understand why they matter.
- **Extensive bibliography**: a survey article's bibliography is one of its most useful contributions. Cite primary sources for every claim.
- **Circulate a draft** to experts in the area before submitting. Survey articles are especially vulnerable to unintentional errors of historical attribution.
- A survey must be **honest about gaps**: what is not known, what is contested, what is excluded from scope.

#### §3.3 — Writing an Opinion Piece

- Must have **something substantive to say**. An opinion piece without a clear thesis is just noise.
- **Research facts thoroughly** before writing. The author of an opinion piece is responsible for accuracy — unlike a personal essay, an opinion piece makes falsifiable claims.
- Structure: premise → argument → conclusion. The conclusion must follow from the argument, not just from the author's preferences.
- **The opinion piece is not a rant.** The author should anticipate and address the strongest counterargument.

#### §3.4 — The Role of Examples

Krantz treats this as fundamental:
- Every new concept must be accompanied by at least one concrete example.
- Good examples are **minimal** — the simplest case that illustrates the concept without extraneous complexity.
- Examples serve double duty: they clarify definitions and they provide a test bed for conjectures.
- In expository writing, the reader builds intuition from examples before the definition is stated. Consider reversing the conventional (definition → example) order: (example → intuition → definition) is often more effective.

#### §3.5 — What a Mathematical Proof Is in an Expository Context

In a survey or textbook, the author often cannot reproduce full proofs. Krantz's approach: **pseudo-proofs** — outlines that give the strategy and key ideas without all technical details. A pseudo-proof should be clearly labeled as a sketch. Never present a sketch as if it were a complete proof.

---

### Chapter 4 — Writing Professional Documents

#### §4.1 — The Recommendation Letter

Among the most difficult documents Krantz addresses. The stakes are high (hiring, tenure, fellowships) and the conventions are precise.

**Structure:**
1. **Opening paragraph:** How do you know the candidate? For how long? In what capacity? (Advisor, collaborator, colleague.)
2. **Scholarly evaluation paragraphs:** Describe the candidate's best work. Be specific — name the paper, state the result, explain why it is significant. Use **binary comparisons**: "Among the 30 Ph.D. students I have supervised in 35 years, [name] is in the top 3." Vague praise ("excellent," "outstanding") is discounted by readers.
3. **Personal qualities paragraph:** Work habits, intellectual independence, ability to collaborate.
4. **Closing paragraph:** Summary recommendation and enthusiasm level.

**Critical rules:**
- **Brevity:** 1.5–2 pages. A 4-page letter signals that the author could not prioritize. A 0.5-page letter signals weak support.
- **Specificity:** "She proved that every compact Riemann surface admits a harmonic measure that is..." is strong. "She has excellent research skills" is worthless.
- **Honesty about weaknesses:** A recommendation letter that has no negative remarks is not trusted. Acknowledge weaknesses — the most persuasive letters say "X is slow to publish, but when X publishes, the results are definitive."
- **The negative recommendation:** If you cannot write a supportive letter, decline. Damning with faint praise is not neutral — it is harmful.
- **Inflation:** Acknowledge that recommendation letters are inflated industry-wide. To stand out, be more specific and more concrete, not more hyperbolic.

**For a Ph.D. student:** State when the degree was or will be awarded, the thesis topic and its significance, evidence of independence (did the student find their own problems?), publications or manuscripts, teaching.

#### §4.2 — The Curriculum Vitae

**Rules:**
- **Tableau form**, not paragraph form. A CV is scanned, not read. Every item must be findable in 5 seconds.
- **Standard sections (in order):** Name/contact information, Education (reverse chronological), Positions held (reverse chronological), Grants/fellowships, Publications (subdivided: books, peer-reviewed papers, expository papers, proceedings), Invited talks, Teaching, Service.
- **Publications:** List them completely. Include all authors, title, journal, volume, year, pages. A citation with missing data looks careless.
- **Honesty about publication status:** Do not list a preprint as "submitted" when it has been rejected. Do not list a paper as "in press" when it has only been accepted conditionally. Distinguish clearly: published, in press, submitted, in preparation.
- **Grant amounts:** Include the dollar amount. Dollar amounts make the CV concrete.
- **Teaching:** List courses taught, not just course names. Include level and enrollment if informative.

#### §4.3 — Grant Writing

**Core principle:** A grant proposal has two axes: *Can you do this?* (credibility) and *Will you do this?* (commitment). The proposal must establish both.

**Rules:**
1. **Read the call for proposals carefully.** The program officer has a specific vision. Proposals that do not fit the call are rejected without review.
2. **Page limits are hard.** Every grant has a page limit. Exceeding it by even one line causes disqualification at many agencies.
3. **Organize the proposal with headers.** Grant panels review 50–100 proposals. Headers allow the reviewer to find the hypothesis, the methodology, and the evaluation plan quickly.
4. **Self-evaluation:** Include a section explaining how you will know if you succeeded. Measurable milestones (e.g., "Submit paper X by month 18") are more convincing than vague claims.
5. **Dissemination plan:** Explain specifically how results will reach the community. "We will publish in top journals" is insufficient. Name the journals, the conferences, the planned workshops.
6. **Credibility section:** List prior grants, prior papers in the area, preliminary results. Show that you have already made progress.
7. **Budget justification:** Every budget line must be justified concretely. "Postdoc salary: $60K/year to carry out computation-heavy experiments" is justified. "Postdoc: $60K" is not.
8. **Proofread obsessively.** A grant proposal with spelling errors communicates that you do not take the proposal seriously.

#### §4.4 — The Cover Letter (for Job Applications)

- At most one page.
- Opening: state the position applied for and where it was advertised.
- Body: three short paragraphs — research summary, teaching philosophy summary, fit with the department.
- Closing: request for an interview.
- Do not summarize the CV — the CV is attached.

#### §4.5 — The Job Talk

- **Entry points and exit points:** The audience contains specialists and non-specialists. Open with 10 minutes that non-specialists can follow (entry point). Close with a summary that non-specialists can understand (exit point). Put the technical material in the middle.
- **Flexibility:** Have a 20-minute version and a 50-minute version of the same talk. Know which you are giving.
- **Mantra:** "Tell them what you're going to tell them; tell them; tell them what you told them."
- **Proofs:** Do not give complete proofs in a talk. Give key ideas. Use blackboard / slides to illustrate the strategy, not to transcribe the paper.

#### §4.6 — Electronic Mail

Krantz treats email as a professional document. Rules:
- **Brevity:** Keep emails short. If more than one screen, use headers or bullet points.
- **Subject line:** Specific and informative. Not "Question" — use "Question about Theorem 3.2 in your AMS paper."
- **Completeness:** Include all information the recipient needs to respond. Do not make them send a follow-up asking for clarification.
- **Etiquette:** Respond within 24–48 hours. If you cannot respond fully, acknowledge receipt and set a timeline.
- **Collaboration via email:** When exchanging TeX files, send both the `.tex` and the `.pdf`. Use `%` (percent sign) for TeX comments to annotate questions and suggestions without modifying the actual text.

---

### Chapter 5 — Writing a Book

#### §5.1 — Is a Book the Right Form?

Write a book when: (a) the material is too large and interconnected for papers, or (b) an existing treatment is wrong, outdated, or inaccessible. Do not write a book to "collect papers" — a collection of papers is not a book. A book must have a unifying vision.

#### §5.2 — Planning and Organization

- **Plan the table of contents before writing.** The TOC is the book's skeleton. Writing chapters without a TOC leads to structural incoherence.
- **Write the Preface first** (even if it is revised later). The Preface is a touchstone — it states why this book is needed, who the intended reader is, and what distinguishes this treatment from alternatives. Return to the Preface periodically to ensure the book is on track.
- **The Preface + TOC as deflection device:** Krantz notes that when an editor or colleague raises an objection about the book's approach, the best response is to point to the Preface: "As stated in the Preface, the book is intended for..." This forces the objection to be about the stated goals, not about unstated expectations.

#### §5.3 — The Halmos Spiral Method

Paul Halmos's method for writing a book: **write in spirals, not linearly.** Start by writing a rough draft of Chapter 1, then Chapter 2, but then go back and revise Chapter 1 before writing Chapter 3. Continue this spiral — at each new chapter, revise all previous chapters. This ensures consistency of notation, cross-references, and intellectual tone throughout the book. It prevents the common failure mode of later chapters using different conventions than earlier ones.

**Anti-pattern:** Writing all chapters front-to-back without revision. The result is a book where the treatment in Chapter 7 is sharper and more mature than Chapter 1, but the reader has already been confused by Chapter 1's weaker exposition.

#### §5.4 — Notation and Indexing

- **Table of Notation:** For any mathematical book, include a Table of Notation at the beginning or end. The reader should be able to look up any symbol and find where it is defined.
- **The index should be by concept, not by word.** An index entry for "function" is useless. An index entry for "continuous function, characterization by sequential convergence, p. 45" is useful. Index the concepts a reader would look up when they know what they want but cannot find where it is.
- **Build the index as you write**, using `\index{}` commands in LaTeX. Do not try to build it retroactively from the final PDF — you will miss half the terms.
- **Appendices:** For technical material (e.g., measure theory background for a complex analysis book) that the reader may or may not need. The appendix must be self-contained.

#### §5.5 — Submitting to a Publisher

**Process:**
1. Send a **prospectus** (not the full manuscript) to the acquisitions editor. The prospectus: a cover letter, the Preface, the full TOC, one representative chapter, and a market analysis (who will use this book and in what courses).
2. The publisher will send the prospectus to reviewers. Address reviewer comments in the revision.
3. Negotiate the **contract** before writing the final manuscript. Understand: royalties (typically 10–15% for academic books), copyright (you should retain some rights), and the right to post the book on arXiv.
4. Return **galley proofs** carefully. This is the last opportunity to fix errors. After galley proofs, changes cost money.
5. **Royalties:** Academic books typically earn very modest royalties. Do not write a book for financial return.

#### §5.6 — Writer's Block and Time Management

- Writer's block is the result of not having a clear enough idea of what the next sentence must say. The cure: outline at the paragraph level before writing. If you know the topic sentence of each paragraph, the paragraph writes itself.
- **Dedicated writing time:** Schedule 2–4 hours per day for writing, ideally at the same time each day. Writing is a physical skill — it requires regular practice.
- **Tunnel vision:** Do not revise while writing the first draft. Set a daily word count and hit it without editing. Edit in a separate pass.

---

### Chapter 6 — The Tools of Writing

#### §6.1 — TeX and LaTeX

**TeX** (created by Donald Knuth, early 1980s) is a markup language for mathematical typesetting — not a word processor. LaTeX (Leslie Lamport) is the most widely used macro system built on TeX. LaTeX is the **lingua franca of mathematical writing**. For a SiS paper submitted to ICML, NeurIPS, ICLR, or any IEEE journal, LaTeX is the required format.

**Key TeX facts:**
- Inline math: `$...$`
- Displayed math: `\[ ... \]` or `\begin{equation}...\end{equation}`
- TeX positions elements to within $10^{-6}$ of an inch — precision that word processors cannot match.
- LaTeX compiles the source `.tex` file into a `.pdf`. The `.tex` file is the source of truth. Never treat the `.pdf` as editable.
- arXiv accepts **only TeX/LaTeX source**, not PDF. Submit `.tex` + any style/figure files. If the source does not compile, the submission fails.
- Make TeX files **self-contained**: all required packages declared in the preamble, all custom macros defined, all figure files included relative to the source directory.
- LaTeX's `\index{}` command marks index entries inline. Use it.

**Specific LaTeX practices from Krantz:**
- Use `\begin{theorem}...\end{theorem}`, `\begin{proof}...\end{proof}` environments consistently throughout the document. The `amsthm` package provides these.
- Use `\label{}` and `\ref{}` for all cross-references — never hard-code equation or theorem numbers.
- Use `align` environment for multi-line equations, ensuring equals signs are aligned.
- Use `\text{}` inside math mode for prose snippets (e.g., `$f(x) \text{ is continuous}$`).

#### §6.2 — Graphics

Rules:
- Create graphics separately from the main document. Keep source files (Illustrator, TikZ, Mathematica notebook) alongside the document.
- Label every figure systematically: "Chapter 3 Section 2 Figure 5" — not just "Figure 5." This prevents confusion when figures are moved during revision.
- Reference every figure explicitly in the text: "As illustrated in Figure 3.2.5,..." Never include a figure that is not mentioned in the text.
- **Recommended tools:**
  - Edward Tufte's books (*The Visual Display of Quantitative Information*, *Envisioning Information*) for principles of information graphics.
  - Mathematica for plots of complex functions and surfaces.
  - Adobe Illustrator / CorelDRAW for geometric diagrams.
  - TikZ/PGF within LaTeX for diagrams that need to match the document's font and style.

#### §6.3 — The Internet and Open Access

- **Open Access (OA)** has three flavors:
  - **Gold OA:** The journal is openly accessible; the author pays an article processing charge (APC). Cost: $500–$3000 per paper.
  - **Green OA:** The journal is paywalled, but the author posts a preprint (e.g., on arXiv) under a retained right.
  - **Libre OA:** The paper is openly accessible and permissions barriers are removed (Creative Commons license).
- **arXiv** (Paul Ginsparg, 1991): the primary preprint server for mathematics and physics. Roughly 30% of math papers are on arXiv. Not refereed. Not a journal. Posts appear within 24–48 hours. The mathematics community widely cites arXiv preprints.
- **Copyright:** Once you write something, it is automatically copyrighted to you. Journal submission typically requires signing a copyright transfer agreement. Read this agreement. Many journals grant back the right to maintain an arXiv version ("green OA").

#### §6.4 — Collaboration by E-Mail

- Exchange `.tex` + `.pdf` together — the collaborator needs the source to edit and the PDF to read without compiling.
- Use TeX comments (`%`) to annotate open questions in the `.tex` file: `% SKK: check this step` — easier to search than inline prose.
- Establish a **definitive version policy** from the start: who holds the master file? Naming convention: `paper_v3_SGK.tex` (version + initials of last editor).

#### §6.5 — Word Processors vs. TeX

Krantz explicitly argues that word processors (Microsoft Word) are inferior to TeX for mathematical writing for the following reasons:
- Mathematical equations in Word require equation editors that produce inconsistent spacing and typography.
- Word files are not portable across platforms and versions — a `.tex` file from 1990 still compiles today.
- Word's automatic formatting (autocorrect, smart quotes) corrupts mathematical notation.
- Word processors' WYSIWYG model hides the document structure; TeX source makes the structure explicit.

The **only advantage of a word processor** over TeX for a mathematician: shorter startup time for non-technical prose. For any document with more than a handful of equations, TeX is unambiguously superior.

#### §6.6 — Not a Native English Speaker

English is the default language of international mathematics. Non-native English writers should:
- Hire a **professional English editor or writing coach** for important documents (papers, grants, books). The AMS and many institutions offer such services.
- Read widely in English mathematical writing (Halmos, Hardy, Rota). Imitation of excellent prose is a valid learning method.
- The grammar errors that most undermine credibility: article usage (`a` vs. `the`), preposition choice, and subject–verb agreement. These are also the hardest to learn implicitly. Get explicit feedback.
- Krantz cites Benoit Mandelbrot and Elias Stein as exemplars of mathematicians who became excellent English writers despite being non-native speakers.

---

### Chapter 7 — The World of High-Tech Publishing

#### §7.1 — Preprint Servers (arXiv)

arXiv (1991, Ginsparg) transformed mathematical communication. Key facts for practice:
- arXiv accepts **only TeX/LaTeX** submissions — not `.pdf`, not `.docx`.
- The server **compiles your source on submission**. If compilation fails, you cannot submit. This is the primary motivation for self-contained, compiling-clean TeX files.
- Most professional journals allow arXiv posting. Book publishers typically ask you to take the book down from arXiv once it is officially published.
- The **versioning problem**: once posted, a paper generates multiple versions (arXiv v1, v2, final journal version, home page version). Decide which is canonical and maintain that policy.
- **Front** (Greg Kuperberg): a more user-friendly interface to arXiv at `http://front.math.ucdavis.edu/`.

#### §7.2 — MathSciNet

MathSciNet is the AMS's database of mathematical literature (evolved from *Mathematical Reviews*, founded 1940 by Otto Neugebauer). Key facts:
- Provides curated, peer-edited reviews of most published mathematical papers.
- Provides **BibTeX entries** for every paper in its database — the fastest way to build a correct bibliography.
- Author disambiguation: unlike Google Scholar, MathSciNet takes care to correctly attribute papers to the right author even when multiple authors share a name.
- Searchable by: author, title, journal, MathSciNet ID, MSC classification.
- Calculates collaboration distance ("Erdos number").
- For every wiki page citing a mathematical result, the MathSciNet BibTeX entry is the correct bibliographic source.

#### §7.3 — Mathematical Blogs and Related Ideas

- Mathematical blogs (John Baez, Terence Tao, Timothy Gowers) have become significant venues for mathematical communication.
- **polymath** (Gowers): collaborative problem-solving via blog. Notable successes. Raises authorship questions for highly collaborative results.
- **MathOverflow**: Q&A site for research mathematics. An important resource for technical questions.
- Blogs and MathOverflow are not refereed and should not be cited as primary sources in papers — but they are valuable for awareness of open problems and emerging results.

#### §7.4 — Print-on-Demand Books

Print-on-demand has eliminated "out of print." The Espresso Book Machine (EBM) can print, bind, and trim a book on the spot from a PDF. Amazon's CreateSpace allows self-publishing at low cost. Self-published books now reach academic libraries through standard distribution channels. For a niche mathematical monograph with a small audience, self-publishing via print-on-demand is now a viable alternative to traditional academic publishing.

---

### Chapter 8 — Closing Thoughts

#### §8.1 — Why Is Writing Important?

Krantz's closing argument: good writing is not decoration. It is the mechanism by which mathematical ideas survive and propagate. The writings of Herodotus, Descartes, and Hardy are as alive now as when written because they were written with care and precision. The argument is also self-interested: clear, cogent writing requires little more effort than lousy writing, once the principles are internalized. Writing well is a **learnable skill** — not a talent that some people have and others lack. It is learned by learning principles and practicing them, just as one learns to scuba dive or to give a proof.

**Halmos's maxim:** "Anything that helps communication is good. Anything that hurts is bad." This single sentence subsumes every rule in the book.

---

## Key Verbatim Anchors

1. **On notation minimalism** (p. 15): "Every symbol that you introduce must pull its own weight. If a symbol is introduced and then never used again, then it should not have been introduced."

2. **On sentence structure / modus ponendo ponens** (p. 26): "The correct template for a mathematical conditional is: If [hypothesis], then [conclusion]. The order must not be inverted."

3. **On the abstract** (p. 76): "The abstract should be written so that it is comprehensible to anyone with a general scientific background. It must not contain mathematical notation or jargon."

4. **On displayed math** (p. 30): "Displayed mathematics should be used for a formula that is important, long, or will be referred to later. Inline mathematics should be used for short, less important formulas."

5. **On word order** (p. 28): "Word order matters in English. The most important word should come at the end of the sentence."

6. **On the Claim device** (p. 72): "The claim device is extremely useful for organizing long proofs. It allows you to break the proof into steps, to label them, and to keep the reader oriented."

7. **On the abstract's readership** (implied from multiple passages, explicitly p. 76): "All readers read the abstract, about 20% read the introduction, and about 5% read the body of the paper."

8. **On writing as thinking** (p. 4): "Writing and thought are closely intertwined. The act of writing forces you to organize your thoughts, and the resulting organization is often the source of new insights."

9. **On Halmos** (p. 243 epigraph): "Anything that helps communication is good. Anything that hurts is bad." — Paul Halmos.

10. **On the Preface** (pp. 107–108): "The Preface is your compass. Every time you feel lost while writing the book, return to the Preface and ask yourself whether what you are writing serves the goals stated there."

11. **On notation-never-begin-sentence** (implied from §1.5 discussion): "Never begin a sentence with a mathematical symbol. Always begin a sentence with a word."

12. **On the survey article** (p. 102): "A survey article must tell a story. It must have a beginning that establishes the area, a development that traces the main results, and an end that brings the reader to the present day and points toward the future."

---

## How a Writing Agent Should Use This

**Primary agent:** The writing-craft agent operating in Layer 0 (generic craft) and Layer 1a (math-heavy methods sections of ML papers).

**Applicable modes:**

### Mode A — Methods-section audit (highest priority for SiS)
Before finalizing a CTPC paper's methods section, run this checklist derived from Krantz:

1. **Notation audit:** List every symbol introduced. Is each used at least 3 times? If not, replace with English. Are all symbols defined before use? Is there any duplicate (same symbol, two meanings)?
2. **Sentence structure audit:** Flag any sentence that begins with a mathematical symbol. Flag any conditional written in inverted order ("suppose [conclusion], given [hypothesis]"). Flag any use of `obviously`, `clearly`, `trivially`.
3. **Inline vs. displayed audit:** Is every displayed equation either important, long, or going to be referenced by number? Is every inline equation genuinely too short to warrant display?
4. **Prose bridges:** Is there at least one English sentence between every pair of consecutive displayed equations? Does each sentence after a displayed equation explain what the equation means or what follows from it?
5. **Theorem/definition audit:** Is every theorem preceded by sufficient context? Are all definitions placed immediately before first use? Are any definitions that group multiple conditions using a defined term (e.g., "admissible")?
6. **Long proof audit:** Any proof over 1 page — does it use the Claim device or sub-lemma structure? Is there an opening sentence stating the proof strategy?

### Mode B — Abstract writing
Apply the 4-sentence template: (problem) → (obstacle) → (result) → (implication). Check: no notation, no jargon, ≤150 words, states the main result explicitly.

### Mode C — Bibliography construction
For every citation, verify: all authors, full title, journal, volume, year, page range. Use MathSciNet BibTeX entries. Cite specific theorem/page numbers. List only works cited in the paper.

### Mode D — First-person / voice audit
Replace all passive constructions that obscure authorial commitment ("it can be shown" → "we show"). Replace all vague hedges ("seems to suggest" → "implies"). Replace `I believe` with `we believe` (or restructure to state the claim directly if it is factual).

---

## Connection to SiS / CTPC

The CTPC paper and its descendants are **math-heavy ML papers** — exactly the genre Krantz addresses. The specific risk: a reviewer who cannot follow the dissipation proof (Ḣ ≤ 0 enforced via Cholesky parameterization) will reject the paper not because the math is wrong but because the presentation fails to connect the architecture to the constraint. Krantz's framework addresses this precisely:

1. **Notation minimalism:** The CTPC paper introduces RTN/RSW frames, equinoctial elements, covariance parameterizations, and the corrector neural network architecture — each with its own symbol set. Krantz's rule (introduce a symbol only if used ≥3 times and no short English phrase suffices) is a compression protocol. Apply it to every symbol in the methods section.

2. **Modus ponendo ponens for architecture arguments:** The claim "Cholesky parameterization guarantees Σ ≻ 0" should be written as "If the covariance matrix is parameterized as Σ = LL^T where L is lower triangular with positive diagonal, then Σ ≻ 0" — not "Σ ≻ 0, since we use Cholesky." The causal direction must be explicit.

3. **Theorem/proof structure for architecture claims:** The claim "Ḣ ≤ 0 is enforced as a hard architectural constraint" is a theorem-like assertion. It requires the same structure as a mathematical theorem: hypothesis (the network architecture), conclusion (the dissipation inequality), and a proof (the derivation that the output satisfies the inequality by construction).

4. **Abstract discipline:** The CTPC abstract must state the main result explicitly: not "we propose a physics-informed corrector" but something like "we show that a neural corrector with dissipation enforced via [specific mechanism] achieves [specific bound] on covariance realism as measured by [specific metric], outperforming SGP4 baselines by [X%] on [dataset]."

5. **SiS grant proposals:** Krantz's grant-writing rules (credibility axis, measurable milestones, dissemination plan) apply directly to SBIR/STTR proposals for SDA technology. The two-axis frame (can you? will you?) maps to: "can you" = publications + preliminary results on SGP4 corrector + Bilal's orbit mechanics background; "will you" = milestone table + AFRL partnership.

---

## Connections

- [[Strunk-White-Elements-Style]] — grammar and style baseline; Krantz references Strunk & White directly and shares the "omit needless words" ethic
- [[Williams-Bizup-Style]] — sentence-level clarity; complements Krantz's word-order and modus ponendo ponens rules
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — reader-expectation theory; Gopen's "stress position" principle is the same as Krantz's "most important word at end"
- [[Schimel-Writing-Science]] — story-first approach to scientific writing; shares Krantz's narrative arc requirement for survey articles
- [[HKU-Guide-Writing-Mathematics]] — focused math writing guide; likely shares notation and proof-structuring rules
- [[Bertsekas-Ten-Rules-Mathematical-Writing]] — compact rule set; compare with Krantz's extended catalog
- [[Milne-Tips-For-Authors]] — LaTeX and paper submission; overlaps with Krantz's TeX and journal submission sections
- [[Langley-Crafting-ML-Papers]] — ML-specific paper writing; applies Krantz's generic rules to the ML venue context
- [[Freeman-Writing-Papers]] — general academic paper writing
- [[Peyton-Jones-Research-Paper]] — research paper writing for CS/ML; shares intro/body structure recommendations
- [[Ramdas-Paper-Checklist]] — checklist format; operationalizes many of the rules Krantz states

---

## Open Questions

1. Krantz's target is pure mathematics. For ML methods sections, displayed equations include algorithmic pseudocode (Algorithm environments) and empirical result tables — neither addressed by Krantz. What are the analogous rules for when to display pseudocode inline vs. as a block?

2. Krantz says to use `we` even as a single author. Several ML venues (NeurIPS, ICML) seem to increasingly accept `I` for single-author papers. Is there a venue-specific norm that overrides Krantz's generic recommendation?

3. The Claim device (inline sub-assertion with immediate proof) is natural in pure mathematics but unusual in ML methods sections. Would reviewers at ICML or ICLR find it formal and clear, or find it off-putting for its genre mismatch?

4. Krantz's bibliography rules assume primary-source citation. In ML, it is common to cite arXiv preprints that have never been formally peer-reviewed and may have version histories. What is the correct way to cite an arXiv preprint when the journal version exists and differs?

5. Krantz discusses Open Access briefly but from a mathematics perspective. For SiS as a company submitting to IEEE/AIAA journals, the copyright and arXiv posting rules differ from AMS journals — what are those differences?

6. Krantz recommends against word processors categorically. Modern Overleaf (collaborative LaTeX) resolves some of his objections. Is there any remaining reason to prefer local TeX compilation over Overleaf for the CTPC paper and its successors?

---

## Sources

- `raw/writing/generic/Primer_Math_Writing.pdf`
  - Chapter 1 (pp. 1–62): notation, sentences, grammar, displayed vs. inline math
  - Chapter 2 (pp. 63–98): paper components, abstract, introduction, theorems, proofs, bibliography
  - Chapter 3 (pp. 99–112): exposition, survey articles, examples, pseudo-proofs
  - Chapter 4 (pp. 113–165): recommendation letters, CV, grant writing, cover letters, job talks, email
  - Chapter 5 (pp. 165–195): book writing, Halmos spiral, TOC, Preface, indexing, publisher submission
  - Chapter 6 (pp. 195–222): TeX/LaTeX, graphics, Open Access, arXiv, email collaboration, non-native speakers
  - Chapter 7 (pp. 223–242): arXiv mechanics, MathSciNet, blogs, social media, print-on-demand
  - Chapter 8 (pp. 243–245): closing argument for writing as learnable discipline
  - Bibliography (pp. 247–251), Index (pp. 253–261)
