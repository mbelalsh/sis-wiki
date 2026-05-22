---
title: "Ten Simple Rules for Mathematical Writing (Bertsekas, MIT 2002)"
tags: [writing, generic, mathematical-writing, composition, structure, notation, readability, consistency, segmentation]
sources:
  - raw/writing/generic/Ten_Rules_Math_Writing.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# Ten Simple Rules for Mathematical Writing (Bertsekas, MIT 2002)

## One-Line Intuition

Math writing is a two-language discipline — natural language and math language — and good composition means choosing verifiable, structural rules (not just "be clear") that govern how those two languages connect across multiple sentences and segments.

## Why This Matters

Bertsekas opens by citing Hawthorne ("Easy reading is damn hard writing"), Knuth ("Word-smithing is a much greater percentage of what I am supposed to be doing in life than I would ever have thought"), and Halmos ("I think I can tell someone how to write but I can't think who would want to listen"). The thesis: the dominant model for mathematical writing is the **conversational style** (Halmos: "mathematics should be written so that it reads like a conversation between two mathematicians on a walk in the woods"), but that style is non-teachable, non-verifiable, and controversial (readers complain: "where do proofs start and end?", "I can't find what I need"). Bertsekas explicitly rejects it in favor of a **structured style** with specific, verifiable rules that a student can follow and a thesis advisor can check. The structured style also allows room to develop and improve over time. This document is a slide-format lecture distilling those rules for a technical audience writing papers, theses, and textbooks in applied mathematics and engineering.

## Core Content

### The Three-Level Classification of Rules (Slide 7)

Before the ten rules, Bertsekas establishes a taxonomy of rule types that governs which rules matter most:

1. **Small rules** — apply to a single sentence. Examples: sentence structure rules, mathspeak rules, comma rules. Useful but local in impact.
2. **Broad rules** — apply to the entire document. General style and writing strategy goals (precision, clarity, familiarity, forthrightness, conciseness, fluidity, rhythm). Non-verifiable: you cannot check them mechanically.
3. **Composition rules** — the focus of this talk. Relate to how **parts** of the document connect. Apply to multiple sentences. Crucially, they are **verifiable**. These are the ten rules.

This taxonomy is the meta-framework. The reason to focus on composition rules is not that small or broad rules don't matter — it's that composition rules are the most teachable and the most impactful for math writing specifically.

---

### Selected Small Rules (Slides 8–9, for reference)

These are not the ten main rules but are listed explicitly as important:

- **2-3-4 Rule**: Consider splitting every sentence of more than 2 lines, every sentence with more than 3 verbs, and every paragraph with more than 4 "long" sentences.
- **Mathspeak should be readable**: BAD: "Let k>0 be an integer." GOOD: "Let k be a positive integer" or "Consider an integer k>0." BAD: "Let x ∈ R^n be a vector." GOOD: "Let x be a vector in R^n."
- **Don't start a sentence with mathspeak**: BAD: "Proposition: f is continuous." GOOD: "Proposition: The function f is continuous."
- Use active voice ("we" is better than "one").
- Minimize "strange" symbols within text prose.
- Make proper use of "very," "trivial," "easy," "nice," "fundamental," etc. — do not overuse them.
- Use abbreviations correctly (e.g., cf., i.e., etc.).
- Apply comma rules and "which/that" rules correctly.

---

### Selected Broad Rules (Slide 10, for reference)

Also not the ten main rules but listed explicitly:

- Language goals to strive for: precision, clarity, familiarity, forthrightness, conciseness, fluidity, rhythm.
- Organizational rules (how to structure your work, how to edit, rewrite, proofread, etc.).
- "Down with the irrelevant and the trivial" (Halmos).
- "Honesty is the best policy" (Halmos).
- "Defend your style" against pushy copyeditors (Halmos).

---

### Intuitive Math Writing: A Special Case (Slides 11–12)

Bertsekas distinguishes **rigorous proof-based writing** from **intuitive math writing** (his example: the Bertsekas/Tsitsiklis probability textbook). In intuitive math writing:

- Explain math results mostly in words; give references for proofs.
- State precisely a few theorems; place some proofs in appendices.
- Use suggestive natural language to describe the intuition behind theorems and algorithms.
- Challenge: intuitive math writing is *trickier and more demanding* than rigorous proof-based writing — because without math as a precision anchor, natural language must carry that precision load.

Tips for intuitive writing:
- Don't cut corners: **better to skip a proof than to give a sloppy proof**.
- Maintain rigor in the use of natural language: without math, precise language becomes more important. Define terms rigorously and use them consistently. Don't use multiple terms with the same meaning. Avoid ambiguous, undefined, or loose terms (e.g., don't say "random values" or "random samples" when you mean "random variables"; don't say "likelihood" or "chance" when you mean "probability").
- Provide enough explanation/intuition (perhaps in footnotes) so a mathematician can believe your argument and even construct a proof.
- Use good examples to illustrate the key proof idea.

---

### The Ten Composition Rules (Slide 13 overview; Slides 14–27 detail)

Organized under three groups:

**Group A — Structure Rules (break it into digestible pieces):**
1. Organize in segments
2. Write segments linearly
3. Consider a hierarchical development

**Group B — Consistency Rules (be boring creatively):**
4. Use consistent notation and nomenclature
5. State results consistently
6. Don't overexplain — don't underexplain

**Group C — Readability Rules (make it easy for the reader):**
7. Tell them what you'll tell them
8. Use suggestive references
9. Consider examples and counterexamples
10. Use visualization when possible

---

#### Rule 1: Organize in Segments (Slide 14–17)

The fundamental insight: every extended composition has a natural unit. Novels use paragraphs; films use scenes; slide presentations use slides; evening news uses news reports. The question is: **what is the fundamental unit of a math document?**

Bertsekas's answer: **a segment** — an entity intended to be read comfortably from beginning to end. It must be:
- Not too long (to avoid tiring the reader).
- Not too short (to lack content and unity).
- Target length: **half a page to 2–3 pages**.

Examples of segments:
- A mathematical result and its proof.
- An example.
- Several related results/examples with discussion.
- An appendix.
- A long abstract.
- A conclusions section.

Each segment must **stand alone**: identifiable start, identifiable end, and transition material connecting it to what comes before and after.

**Segment structure** (Slide 16):
```
[Title (optional)]
      ↓
[Transition Material]        ← connects from previous segment
      ↓
[Definitions, Examples, Arguments, Illustrations]
      ↓
[Transition Material]        ← connects to next segment
```

**Concrete example** (Slide 17 — Bertsekas/Tsitsiklis probability book, Section 1.2 on Probabilistic Models):
The section is broken into 9 segments: Sample space - Events (1 page), Choosing a sample space (0.5 page), Sequential models (0.75 page), Probability laws - Axioms (1.25 pages), Discrete models (2 pages), Continuous models (1 page), Properties of probability laws (2 pages), Models and reality (1.25 pages), History of probability (1 page). Each is a named, self-contained unit.

The actionable takeaway: before writing, explicitly identify and name your segments. If you cannot name a segment, it probably isn't a clean unit of thought.

---

#### Rule 2: Write Segments Linearly (Slides 18–19)

Question: what is a good order for the flow of deduction and dependency?

General rule: **Arguments should be placed close to where they are used** — this minimizes the thinking strain on the reader (who otherwise must hold context in working memory across long spans).

Similarly: definitions, lemmas, etc. should be placed close to where they are first needed.

Bertsekas frames this as an **optimization problem**: a linear/optimal order is one that positions arguments (definitions, lemmas) so as to **minimize the total number of "crossings" over other arguments**, subject to dependency constraints.

He illustrates with a dependency graph of arguments (Slide 19): if argument 3 depends on arguments 1 and 2, and argument 4 depends on arguments 2 and 3, the nonlinear ordering 1-2-3-4 with backreferences creates many crossings, while the linear ordering 1-3-2-4 (or equivalent depth-first traversal) minimizes crossings.

**Depth-first order is usually better**: complete a thread of reasoning before starting a new one. Do not introduce a concept, then detour, then come back. Write as though the reader cannot flip pages.

---

#### Rule 3: Consider a Hierarchical Development (Slide 20)

When arguments or results are used repeatedly across multiple segments, it can be efficient to extract them into a **dedicated segment** at a higher level of the hierarchy — analogous to a subroutine in a computer program.

Example: if Lemmas 1, 2, and 3 each appear in multiple later analyses, group them in their own Level 1 segment ("Lemmas 1, 2, 3"), then let the Level 2 segments ("Analysis using Lemmas 1 & 2," "Analysis using Lemmas 2 & 3," "Analysis using Lemmas 3 & 1") refer up rather than re-state.

Also: consider creating special segments for special material (math background, notation conventions, etc.). This prevents background material from interrupting the main argument flow.

The hierarchy is not a rigid tree — it is a tool for managing repeated dependencies efficiently and for keeping segments at a consistent level of abstraction.

---

#### Rule 4: Use Consistent Notation and Nomenclature (Slide 21)

Choose a notational style and **stick with it throughout the entire document**.

Specific examples of consistent notational conventions:
- Capitals for random variables, lower case for realized values (e.g., X vs. x).
- Subscripts for sequences, superscripts for components.
- S for set, f for function, B for ball — use suggestive/mnemonic symbols.
- Avoid parenthesized indexes (x(m,n)) in favor of subscripted ones (x_{mn}).
- **Avoid unnecessary notation**: introduce a symbol only if you will actually need it.

BAD (unnecessary notation): "Let X be a compact subset of a space Y. If f is a continuous real-valued function over X, it attains a minimum over X."
GOOD: "A continuous real-valued function attains a minimum over a compact set." (No X, no Y, no f needed if they are not used later.)

The principle: **every named symbol is a cognitive burden on the reader**. Introduce only symbols you will use, and once introduced, use them consistently.

---

#### Rule 5: State Results Consistently (Slide 22)

Keep your language and format **simple and consistent, even boring**. The goal is that distractions are minimized and interesting content stands out by contrast.

Use similar format in similar situations. Do not vary the way you state propositions just for stylistic variety — variety in format is noise.

BAD (inconsistent):
- Proposition 1: "If A and B hold, then C and D hold."
- Proposition 2: "C' and D' hold, assuming that A' and B' are true."

GOOD (consistent):
- Proposition 1: "If A and B hold, then C and D hold."
- Proposition 2: "If A' and B' hold, then C' and D' hold."

The reader's attention is on the mathematical content, not on parsing grammatical variation. Grammatical consistency removes a layer of parsing effort.

---

#### Rule 6: Don't Overexplain — Don't Underexplain (Slide 23)

Choose a **target audience level** of expertise and background (e.g., undergraduate, first-year graduate, research specialist), and calibrate the depth of mathematical explanation to that level. Do not go much over or much under.

Operational guidance:
- Explain potentially unfamiliar material in **separate segment(s)** (not inline, which disrupts the main flow).
- Consider **appendices** for background or difficult/specialized material that would distract from the paper's core audience.

This rule is about calibration: both errors (over-explaining elementary steps to experts, under-explaining advanced steps to the target reader) are costly. Underexplaining loses readers; overexplaining insults and bores them.

The segmentation framework (Rule 1) makes this practical: background material can live in its own segment (or appendix segment), so the main line of argument stays pitched at the target level.

---

#### Rule 7: Tell Them What You'll Tell Them (Slide 24)

**Keep the reader informed about where you are and where you are going** — at all times.

Specific mechanisms:
- Start each segment with a short introduction and perhaps a road map ("In this section we will show X by first establishing Y, then using Y to derive Z").
- Do not string together seemingly aimless statements and then surprise the reader with "we have thus proved so and so."
- **Announce your intentions and results explicitly**: e.g., "It turns out that so-and-so is true. To see this, note …"
- **Tell them what you told them**: end segments with a brief summary or forward pointer.

The reader of a math paper reads selectively and in pieces. A reader who picks up Section 3 mid-paper needs to know immediately what Section 3 is about and how it relates to what came before. Roadmapping at the segment level makes selective reading viable.

---

#### Rule 8: Use Suggestive References (Slide 25)

**Frequent numbered equation/proposition referencing is a cardinal sin.** It forces page-flipping, wastes the reader's time, and breaks concentration.

Instead:
- Refer to equations, results, and assumptions by **content or name** (in addition to number when necessary): "Bellman's equation," "the weak duality theorem," "the compactness assumption," etc.
- **Repeat simple math expressions** rather than forcing the reader to flip back to find them.
- Remind the reader of unusual notation and earlier analysis when invoking it again.
- **Dare to be repetitive** — but don't overdo it.

The test: can a reader who is in the middle of your paper follow a derivation without flipping back? If the answer is no, you are relying too heavily on numbered references. Named references are self-documenting; numbered references are not.

---

#### Rule 9: Consider Examples and Counterexamples (Slide 26)

"Even a simple example will get three-quarters of an idea across." (Ullman)

Examples are not decorative — they are cognitive scaffolding. Specific guidance:
- Examples should have some **spark**: aim at something the reader may have missed, not just at restating the definition.
- Illustrate definitions and results with examples that **clarify the boundaries of applicability** — i.e., show where the result holds tightly.
- Use **counterexamples** to clarify the limitations of the analysis and the need for the assumptions. A counterexample that shows why an assumption cannot be dropped is more pedagogically valuable than a second example where the theorem holds.

For math-dense papers with tight page limits: a single well-chosen counterexample is often worth more than an additional lemma for reader comprehension.

---

#### Rule 10: Use Visualization When Possible (Slide 27)

"A picture is worth a thousand words."

Specific guidance:
- Keep figures **simple and uncluttered**. A figure that tries to show everything shows nothing.
- Use **substantial captions** — captions should reinforce and augment the text, not merely repeat it.
- Use a figure to illustrate the **main idea of a proof or argument** without constraint of math formality — a schematic argument is not a proof but can convey the proof's logic in seconds.
- **Prefer graphs over tables** when the data has a trend or comparison to communicate.

Visualization is especially powerful in intuitive math writing (Rule 11 context, Slide 12): when proofs are deferred, a good figure can give the reader the intuition that enables belief.

---

### Closing (Slide 28)

The document ends with Lamport's maxim: "Bad thinking never produces good writing" — and Bertsekas's inversion: "Good writing promotes good thinking." The structured style is not merely cosmetic; forcing your argument into verifiable, segmented, linearly ordered, consistently stated form reveals gaps in reasoning that would remain hidden in freeform prose.

---

## Key Verbatim Anchors

1. "Easy reading is damn hard writing." — Hawthorne (slide 2)
2. "Word-smithing is a much greater percentage of what I am supposed to be doing in life than I would ever have thought." — Knuth (slide 2)
3. "Composition is the strongest way of seeing." — Weston, quoted by Bertsekas on Rule 1 (slide 14)
4. "A segment [is] an entity intended to be read comfortably from beginning to end." — Bertsekas (slide 14)
5. "Arguments should be placed close to where they are used (minimize thinking strain)." — Bertsekas, Rule 2 (slide 18)
6. "Frequent numbered equation/proposition referencing is a cardinal sin." — Bertsekas, Rule 8 (slide 25)
7. "Even a simple example will get three-quarters of an idea across." — Ullman, cited by Bertsekas on Rule 9 (slide 26)
8. "Don't cut corners: better to skip a proof than to give a sloppy proof." — Bertsekas on intuitive writing (slide 12)
9. "Bad thinking never produces good writing." — Lamport (slide 28)
10. "Intuitive math writing is trickier/more demanding than rigorous proof-based writing." — Bertsekas (slide 12)

## How a Writing Agent Should Use This

**Primary agent: Layer 0 Generic Writing Agent** — this is the core theoretical source for verifiable composition rules that apply to any math-heavy technical document.

**Secondary agent: Layer 1a Paper-Writing Agent** — all ten rules apply directly to conference and journal papers. Rules 1, 2, 7, and 8 are especially critical for paper structure; Rules 4 and 5 govern notation discipline; Rules 9 and 10 govern figures and examples.

**Mode — Checklist (pre-submission):**
Run Rules 1–10 as a sequential checklist before finalizing any paper draft:
1. Can I name every segment in the paper? Are segments 0.5–3 pages?
2. Is the ordering depth-first? Do I ever introduce a concept more than a few paragraphs before its first use?
3. Are shared lemmas/arguments in dedicated segments, not re-stated inline?
4. Is notation consistent throughout? Does every symbol earn its introduction?
5. Are all propositions stated in the same grammatical format?
6. Is the paper calibrated to the target reader (ML venue: assume strong ML background, not SDA specialist background)?
7. Does every section open with a roadmap and close with a summary?
8. Can a reader follow every derivation without page-flipping? Are named references used instead of bare numbered citations?
9. Does every key claim have at least one example with "spark"? Are assumptions justified by counterexamples?
10. Does every figure have a substantive caption? Are there tables that should be graphs?

**Mode — Diagnosis (on a draft under review):**
When a reviewer says "unclear" or "hard to follow," map the complaint to the ten rules. "Hard to follow" → usually Rules 2, 7, or 8. "Notation inconsistent" → Rule 4. "Feels padded" → Rule 6. "Missing examples" → Rule 9.

**Mode — Intuitive Writing Flag:**
When writing without detailed proofs (conference papers, workshop papers, SiS technical reports), trigger the intuitive writing tips from Slides 11–12: define terms rigorously, don't call things "random values," include enough explanation for a reader to construct the proof, and use good examples to carry the proof idea.

## Connection to SiS / CTPC

Bilal writes math-dense papers where:
- Physics notation (Hamiltonian H, covariance P, state x) must be consistent across sections that blend orbital mechanics, probability theory, and neural architecture description.
- Proofs of dissipation (Ḣ ≤ 0) may be deferred to appendices while the main text uses intuitive writing.
- Reviewers at AIAA / NeurIPS / ICLR read selectively and will check the experiment section before the theory section.

**Rule 1 (Organize in segments)** maps directly to CTPC paper structure: each of {problem formulation, SGP4 predictor description, ML corrector architecture, Cholesky PSD constraint, experiment on real TLEs} should be a named, self-contained segment of 0.5–2 pages.

**Rule 4 (Consistent notation)** is critical: Bilal must not use x for both state vector and a generic variable, must not alternate between RTN and RSW for the same frame within a paper, and must choose one convention for covariance (P vs. Σ) and apply it everywhere.

**Rule 6 (Calibrate explanation level)** matters for mixed-audience submissions: an AIAA paper assumes orbital mechanics, not ML; a NeurIPS paper assumes ML, not SGP4. The same CTPC derivation needs different background segments for each venue.

**Rule 8 (Suggestive references)** is especially important for physics-architecture papers where the architecture diagram, the energy function H, and the Cholesky decomposition all get referenced many times. Calling it "the dissipation constraint" rather than "equation (7)" throughout makes the paper self-documenting.

**Intuitive writing mode** applies to conference papers where full proofs of Lyapunov stability or PAC-Bayes bounds are deferred: use the Slide 12 checklist to ensure the intuition is precise even when the proof is absent.

## Connections

- [[Strunk-White-Elements-Style]] — small rules (sentence-level grammar, conciseness) that complement Bertsekas's composition rules
- [[Krantz-Primer-Mathematical-Writing]] — book-length treatment of mathematical writing conventions; Bertsekas cites Krantz as a source
- [[HKU-Guide-Writing-Mathematics]] — related guide on mathematical exposition conventions
- [[Milne-Tips-For-Authors]] — mathematical writing tips in the Halmos tradition
- [[Pak-Clear-Math-Paper]] — checklist-style guide for clear mathematical papers
- [[Ramdas-Paper-Checklist]] — ML-paper-specific checklist; use alongside Bertsekas for ML venue submissions
- [[Lipton-Writing-Heuristics]] — ML writing heuristics; pairs with Rule 6 (audience calibration) and Rule 9 (examples)
- [[Langley-Crafting-ML-Papers]] — ML paper writing guide; Bertsekas's rules apply directly
- [[Peyton-Jones-Research-Paper]] — CS paper writing guide; complementary structural advice
- [[Goldreich-How-To-Write-Paper]] — theoretical CS perspective; consistent with Bertsekas's structured style
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — sentence-level reader-expectation theory; pairs with Bertsekas's Rule 7
- [[Schimel-Writing-Science]] — narrative arc approach; contrast with Bertsekas's structured style
- [[Williams-Bizup-Style]] — sentence-level style guide; complements Bertsekas's small rules section
- [[Whitesides-Writing-A-Paper]] — outline-first methodology; consistent with Rules 1–3

## Open Questions

1. **Conflict with narrative style:** Bertsekas explicitly rejects the conversational/Halmos style in favor of structured rules. For ML papers at NeurIPS/ICLR, reviewers often reward storytelling (Schimel-style narrative arc). How does Bilal reconcile Bertsekas's structured style with the narrative expectations of ML venues? Does the 3-group, 10-rule framework subsume narrative, or does it conflict?
2. **Segment length in short papers:** The 0.5–3-page segment guidance was developed for textbooks and theses. In an 8-page conference paper, a "segment" may be only a third of a page. Does the segment framework scale down, or does a different organizational unit apply?
3. **Intuitive writing and hard-constraint proofs:** Bertsekas says "better to skip a proof than give a sloppy proof." For CTPC, the Cholesky PSD constraint is the entire architectural innovation. Skipping the proof of why Cholesky guarantees PSD in a conference paper leaves the key claim unverified. What is the right balance between intuitive writing and the need to validate the hard constraint claim?
4. **Rule 8 and LaTeX referencing practice:** Bertsekas's "cardinal sin" of frequent numbered referencing conflicts with the LaTeX/BibTeX culture in which equation numbers are the default cross-reference mechanism. What specific named-reference patterns should Bilal adopt as defaults in CTPC papers?
5. **Visualization for orbital mechanics:** Rule 10 says "prefer graphs over tables." In SDA papers reporting conjunction probability or TLE-residual statistics, the data is often high-dimensional. What visualization conventions are established in the SDA/astrodynamics literature vs. what ML venue reviewers expect?

## Sources

- raw/writing/generic/Ten_Rules_Math_Writing.pdf
  - Slides 1–6: title, framing, what is math writing, sources
  - Slide 7: taxonomy of rule types (small / broad / composition)
  - Slides 8–9: small rules examples
  - Slide 10: broad rules examples
  - Slides 11–12: intuitive math writing
  - Slide 13: overview of the ten composition rules
  - Slides 14–17: Rule 1 (Organize in Segments) + segmentation process + segment structure + example
  - Slides 18–19: Rule 2 (Write Segments Linearly) + ordering examples
  - Slide 20: Rule 3 (Hierarchical Development)
  - Slide 21: Rule 4 (Consistent Notation)
  - Slide 22: Rule 5 (State Results Consistently)
  - Slide 23: Rule 6 (Don't Overexplain / Underexplain)
  - Slide 24: Rule 7 (Tell Them What You'll Tell Them)
  - Slide 25: Rule 8 (Suggestive References)
  - Slide 26: Rule 9 (Examples and Counterexamples)
  - Slide 27: Rule 10 (Visualization)
  - Slide 28: closing Lamport quote
