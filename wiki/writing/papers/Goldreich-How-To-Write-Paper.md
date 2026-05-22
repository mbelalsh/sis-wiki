---
title: "How to Write a Paper — Goldreich's Reader-Centric Framework"
tags: [writing, paper-writing, reader-centric, scientific-communication, structure, abstract, introduction, technical-writing]
sources:
  - raw/writing/papers/guides/How_To_Write_Paper.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# How to Write a Paper — Goldreich's Reader-Centric Framework

**Author:** Oded Goldreich (Department of Computer Science and Applied Mathematics, Weizmann Institute of Science, Rehovot, Israel)
**Date:** March 2004, last revised November 8, 2015
**Origin:** Revised and augmented version of the earlier essay "How NOT to write a paper" (Spring 1991, revised Winter 1996). The title change signals a deliberate shift from purely negative critique to actionable positive guidance.

---

## One-Line Intuition

A paper's purpose is to communicate ideas to a community drowning in information — every structural and stylistic decision should be made by asking "what does the reader need here?" not "what do I want to say?"

---

## Why This Matters

Goldreich opens with a blunt claim: **badly written papers are the product of poor understanding of the role of papers in the scientific process, or failure to implement that understanding in the writing process.** The fix is not better prose talent — it is total awareness of purpose. Once a writer is *totally aware* of his goals in writing papers, the quality of his papers (at least as far as form is concerned) will drastically improve.

The practical pressure that makes this urgent: the scientific community is drowning in a flood of mostly irrelevant information. Readers have very little time to sort it out. It is therefore the writer's duty to do his best to help potential readers extract the information relevant to them from the paper. The writer should spend much time writing the paper so that the potential readers can spend much less time extracting what is relevant to them.

---

## Core Content

### 2. Why We Write Papers: Grounding the Philosophy

Goldreich begins by anchoring writing in purpose. The purpose of writing scientific papers is to **communicate an idea (or set of ideas) to people who have the ability to either carry the idea further or make other good use of it.** Communication of good ideas is the medium through which science progresses.

Before writing, ask: *what is the idea the paper is intended to communicate?* An idea can be:
- A new way of looking at objects — a **model**
- A new way of manipulating objects — a **technique**
- New facts concerning objects — **results**

If no such idea can be identified, reconsider whether to write the paper at all.

**Step 1: Identify the key ideas.**
**Step 2: Identify the relevant community** — not just the experts, but also their current and future graduate students and researchers who do not have direct access to the experts. The implied reader model: *an intelligent person with basic background in the field, but not more. A good example to keep in mind is a good student at the beginning stages of graduate studies.*

This target reader is often the person the writer cares least about — recent graduates who have just moved past this stage tend to underestimate its difficulty. The corrective: try to imagine the difficulties you would have had trying to read the paper half a year ago.

### 3. How to Serve the Reader's Needs

This is the philosophical core of the essay, organized into four subsections.

#### 3.1 Focusing on the Reader's Needs Rather Than the Writer's Desires

The writer is often overwhelmed by his own desires to say certain things and neglects to ask what the real needs of the reader are. Goldreich names five **named symptoms** of writer-centric writing:

**Symptom 1: The "Checklist" Phenomenon**
The writer wishes to put in the paper everything he knows about the subject matter. He inserts his insights in the *first* possible location rather than the *most suitable* one. In extreme cases, the writer has a list of things he wants to say and his only concern is that they are all said *somewhere* in the paper. Such a writer has forgotten the reader.

**Symptom 2: Obscure Generality**
The writer chooses to present his ideas in the most general form instead of in the most natural (or easy to understand) one. Utmost generality is indeed a virtue in some cases, but even then one should consider whether it is not preferable to present a meaningful special case first. It is often preferable to postpone the more general statement and prove it by a modification of the basic ideas (which may be presented in the context of such a special case).

**Symptom 3: Idiosyncrasies**
Some writers use terms, phrases, and notations that only have personal appeal — e.g., shorthand for terms in another language that the reader does not share. The justification for using a particular term, phrase, or notation should be its *appeal to the intuition or the associations of the reader*, not the writer.

**Symptom 4: Lack of Hierarchy/Structure**
Some people can maintain and manipulate their own ideas without keeping them within a hierarchy/structure. But it is very rare to find a person who will not benefit from having new ideas presented in a structured/hierarchical manner. The write-up should make *clear distinctions* between the more important ideas/statements and the less important ones. Important ideas/statements should be highlighted; secondary discussions should be marked as such. The specific ways of highlighting and marking may vary, but they should be conspicuous.

**Symptom 5: "Talmud-ism"**
The writer explores all the subtleties and refinements of his ideas when *first* introducing them and before clarifying the basic ideas. Furthermore, the writer discusses all possible criticisms (and answers them) before providing a clear presentation of the basic ideas. This is the expository equivalent of burying the lede.

All five symptoms share a common root: the writer is concentrated on satisfying his own needs, not the reader's.

#### 3.2 Awareness of the Knowledge Level of the Reader

Another major difficulty: lack of constant awareness of what the reader may be expected to know *at a particular point in the paper.* Concrete guidelines:

1. **Whenever presenting a complex concept/definition**, beware that the reader cannot be assumed to fully grasp the new concept and all its implications immediately.
2. **Whenever presenting proofs**, be sure to *elaborate* on the conceptual steps rather than on the standard technical analysis. Having done the conceptual steps yourself, they seem rather evident to you, but they may not be evident to the reader. Furthermore, these conceptual steps are typically the most important ideas in the paper and the ones with which readers have most difficulties.
3. **Present special cases before general ones.** Try first to present a special case that captures the main ideas; derive the general results by use of reductions to the special case, or by high-level modifications to it. Avoid syntactical (local) technical modifications of the special case as a way to obtain the general case.
4. **Don't hide a fundamental difficulty** by using a definition that ignores it without first discussing the issue — i.e., what is the difficulty and why bypassing it does not deem the entire investigation meaningless.
5. **Minimize new concepts and definitions.** The reader's capacity for absorbing concepts and definitions is bounded.

#### 3.3 Balancing Between Contradictory Requirements

The suggestions in 3.1 and 3.2 may sometimes be contradictory. Such cases call for the application of judgment. Application of judgment requires *flexibility* — the writer should NOT try to follow a canonical example or structure, but rather allow the paper's form to be determined by the application of good principles to the concrete problems and dilemmas emerging in writing the current paper.

This is Goldreich's explicit anti-template stance: there is no single canonical paper structure. Good papers are the result of judgment applied flexibly to concrete tensions, not mechanical application of a fixed format.

#### 3.4 Making Reading a Non-Painful Experience

Four mechanical writing mistakes that make reading painful:

**Pain Point 1: A Labyrinth of Implicit Pointers**
The words "it" and "this" are commonly used as implicit pointers to entities mentioned in previous sentences, but the reader can find it difficult to figure out to which entities the writer was referring. Example: "A is interested in doing X. It has property Y but not Z. This property allows it to do this." The writer should make these pointers explicit by referring to objects by their names.

**Pain Point 2: Sentences with Complex Logical Structure**
Sentences with complicated logical structure — conditional sentences having multiple and sometimes nested conditions and consequences, like "if X and Y or Z then P or Q" — introduce parsing problems that slow the reader significantly.

**Pain Point 3: Mixture of Mathematical Symbols and Text**
Example of bad: "on input x, y, A runs B^y on f(x)". A clearer alternative: "on input x and y, algorithm A runs the oracle machine B on input f(x) placing y on B's oracle tape". It never hurts reminding the reader of the categorical status of the objects.

**Pain Point 4: Cumbersome Notations and Terms**
For example: an object denoted M_{i,j,k_t}^{O_k^e}, or a multiple-parameters term like an (a,b,c,d,e,f,g,h,i,j)-system, or a multiple-qualifications term like a *kuku-muku popo-toto system*. "Yes, we have seen such things!"

### 4. Some Concrete Suggestions

Goldreich prefaces this section with an important caveat: he does not believe there is one good format that should be followed in all papers. *On the contrary, a key ingredient in the process of writing is flexibility — the selection of a form that fits the contents at hand.* Concrete suggestions are useful in most cases, not in all cases.

#### 4.1 The Title and the List of Authors

- The title should be **as informative as possible and yet not too cumbersome or too long.**
- Provide some clue about (or at least hint to) the contents — do not make jokes.
- The title should fit into a sequence of past and future work: it should be sufficiently different from titles of previous works and should allow room for subsequent works.
- **Author order:** The tradition in theoretical computer science is alphabetical order, linked to the norm that each author has made a fair (if not significant) contribution. Deviating from alphabetical order in special cases does not make much sense; instead find alternative ways to compensate for vast inequality in contribution. Failure to follow alphabetical order may harm the person pushed forward (e.g., it may encourage committees to ask why this person is not first author in other papers).

#### 4.2 The Abstract

The abstract should be **as informative as possible and yet not too cumbersome or too long.** Key rules:

- **Self-contained as a high-level description** — not truly self-contained (which is impossible), but self-contained *as a high-level description of the contents of the paper.* Do not refer to other parts of the paper (e.g., references list).
- **Typically should not exceed 200 words.**
- The abstract is all that may be available to some readers (web services, digital libraries). Provide maximum information within this constraint.

What the abstract does NOT need to do (but may choose to):
1. Motivate the model (this will typically be done in the introduction).
2. List and/or recall the contents of prior work (but may describe the nature of the improvement over possibly unspecified "prior work").
3. Provide an accurate description of the paper's results (may describe them in imprecise but clear terms using warning phrases such as "loosely speaking"). In cases where even an imprecise but clear description is infeasible, the abstract may merely convey the flavor or nature of the new results.
4. Provide a description of all the paper's results (may confine itself to the most important ones while clarifying that these are merely the main results).

The judgment: making choices in the abstract requires deep understanding of the work and what will serve the uninitiated reader.

#### 4.3 The Introduction

The introduction should provide three things: (1) a clear description of the work, (2) good motivation, and (3) comparison to prior works.

**On description of the work:**
- Provide a clear statement of the main results and a high-level description of the techniques.
- In most cases it should be possible to provide sketchy versions of the main theorems and describe the main ideas underlying the techniques.
- Highlight important new ideas and novel conceptual observations. If these cannot be described without detailed technical context, state their existence and refer the reader to the place in the paper where they can be found.

**On motivation:**
- Provide a clear motivation to the questions studied. Exceptional cases: well-known questions with well-known motivation (but even then, consider providing a reference to a text where this motivation can be found).
- Assuming readers know the motivation is a calculated risk — sometimes worthwhile, sometimes not.
- The motivation need not be argued from scratch. If dozens of works deal with a particular type of question, that type requires no motivation; the specific question within that type may require it.
- A good motivation connects the current study to central notions and questions of the relevant area. The connection should be natural (not contrived) and should make sense with respect to the actual study. There is no need to provide an "actual application" (although a good one may demonstrate the viability of the connection).

**On prior work:**
- Place the current paper in context. Point out and fairly evaluate the main differences: aspects in which the new paper improves over prior work, and aspects in which it is worse, should be clearly stated and discussed.

**On structure of the introduction:**
- There is no specific order; no canonical pattern. Select a structure that makes sense and follow it with care, keeping the reader in mind.
- The final test: did the reader obtain a good idea about the contents of the paper?
- Important conclusions and natural open problems (rather than well-known ones) may be stated in the introduction rather than saved for a conclusion section — unless those elements are significantly easier to understand after reading the main part.

#### 4.4 The Technical Part

Implications of the general principles for the main body:

**On discussing definitional choices:**
Definitions embody a host of decisions ranging from the choice of the notion that the definitions intend to capture to very technical choices. Provide insightful discussions of definitional choices — both high-level and low-level.

High-level choices: link them to the notions that the paper studies (motivated in the introduction). Low-level choices: state explicitly whether each is:
- **Arbitrary** — almost any other reasonable choice will have the same effect.
- **Adopted for sake of simplicity** — can be replaced by more natural choices at the cost of (merely) complicating the discussion.
- **Seemingly essential** — essential to the claimed results, which are not *known* to hold when adopting an alternative equally natural/reasonable choice.

The "seemingly essential" case is disturbing, especially if no good explanation exists. Still, be honest about it.

**On elaborating conceptual steps:**
Whenever presenting proofs, elaborate on the conceptual steps rather than on standard technical steps. The conceptual steps are the most important ideas in the paper; they are also the ones with which readers have most difficulties. The difficulties you had mastering standard techniques are a personal experience to be overcome — helping others overcome standard techniques is a suitable topic for a survey; refer the reader to standard texts if needed.

**On numbering technical elements:**
Unless the paper is very short (less than five pages), use a numbering system that supports easy searches. Goldreich advocates a *single numbering system* (rather than separate numbering per element type) so items can be found by binary search. For long papers: a double-numbering system by which Theorem 5.2 is the second item in Section 5.

**On informative citations:**
The primary role of citations is to be *informative* — to provide the reader with useful information. Scholarly duty is only a secondary concern. If referring to a result in text T, provide more specific information than just [T]: e.g., [Thm X, T] or [Sec X, T] or [P. X, T]. This makes the reader's life much easier.

#### 4.5 Conclusions and/or Suggestions for Further Work Are Not a "Must"

A conclusion section that merely re-iterates things said in the abstract and/or introduction has little value. Similarly, listing well-known open problems or re-iterating questions already raised in the introduction is pointless.

A conclusion section is valuable when it:
- Contains high-level material that better fits *after* the main part (and thus is not placed in the introduction).
- Raises important questions that are more appealing after reading the technical part (even if raised already in the technical part but not in the introduction).

Summary: less than 5% of papers benefit from a conclusion section. It should not be the default.

#### 4.6 References and Acknowledgments

Two governing principles: **truth and kindness.** Since the primary concern is providing information, truth is of utmost importance. Never mislead the reader by unjustified or inaccurate credits attributed to other works. Within the domain of truth, be kind — e.g., acknowledge each person with whom you had a relevant discussion.

### 5. Benefiting from Readers' Comments

Friends and close colleagues are typically not useful as reviewers because:
1. They feel reluctant to point out major expositional problems.
2. They may know the work before reading it, or have better a priori knowledge of the work than an average reader.

It is dangerous to conclude from the fact that the writer's friend liked the write-up that it is indeed good. If you ask a friend to give comments, make sure the friend understands you are interested in a critical reading, not compliments.

**On reviewers:** Readers who may be assumed to be critical are reviewers. One should *not necessarily* follow the reviewer's suggestions, but one must always bear in mind that (almost always) these suggestions indicate problems in the current write-up. It may be that the reviewer is not suggesting a good solution, but *for sure there is a problem.* The working assumption (which is almost always correct): **any comment of a reviewer indicates a problem in the write-up.** Typically, reviewers are not idiots, and one can learn even from idiots.

---

## Key Verbatim Anchors

1. **p. 1 (Introduction):** "We believe that badly written papers are the product of either poor understanding of the role of papers in the scientific process or failure to implement this understanding in the actual writing process."

2. **p. 1 (Introduction):** "Once a person becomes *totally aware* of his goals in writing papers, the quality of the papers he writes (at least as far as form is concerned) will drastically improve."

3. **p. 2 (§2):** "It is the writer's duty to do his best to help the potential readers extract the relevant information from his paper. The writer should spend much time in writing the paper so that the potential readers can spend much less time in the process of extracting the information *relevant to them* out of the paper."

4. **p. 2 (§3.1, Checklist Phenomenon):** "In extreme cases, the writer has a list of things he wants to say and his *only concern* is that they are *all* said *somewhere* in the paper. Clearly, such a writer has forgotten the reader."

5. **p. 3 (§3.1, Talmud-ism):** "The writer explores all the subtleties and refinements of his ideas when *first* introducing them and before clarifying the basic ideas. Furthermore, the writer discusses all possible criticisms (and answers them), before providing a clear presentation of the basic ideas."

6. **p. 4 (§3.3):** "Application of judgment requires *flexibility*. The writer should NOT try to follow a canonical example or structure, but rather allow the paper's form to be determined by the application of good principles to the concrete problems and dilemmas emerging in writing the current paper."

7. **p. 5 (§4.2):** "It should be self-contained only as a high-level description of the contents of the paper."

8. **p. 6 (§4.3):** "The final test is the reader: did he/she obtain a good idea about the contents of the paper?"

9. **p. 7 (§4.4):** "These conceptual steps are typically the most important ideas in the paper and the ones with which the readers have most difficulties. In contrast, the difficulties you have had in mastering the standard techniques are a personal experience to be overcome."

10. **p. 8 (§4.5):** "There are papers that may benefit from a conclusion section, but they are relatively few (say, less than 5% of the papers). Certainly, the inclusion of a conclusion section should not be the default."

11. **p. 9 (§5):** "The working assumption (which is almost always correct) is that any comment of a reviewer indicates a problem in the write-up: Typically, reviewers are not idiots, and one can learn even from idiots!"

12. **p. 8 (§4.6):** "Two (sometimes contradictory) principles that may govern our decision are truth and kindness. Since our primary concern is providing information, truth is of utmost importance."

---

## How a Writing Agent Should Use This

**Mode: Pre-draft planning and post-draft critique.**

### Concrete moves for a paper-writing agent:

**Before drafting:**
1. **Identify the idea.** Force Bilal to answer: what is the one idea this paper communicates — a model, a technique, or a result? If the answer is unclear, do not proceed to drafting.
2. **Identify the relevant community.** Who is the implied reader? Use Goldreich's proxy: an intelligent graduate student at the beginning stages, not yet an expert. Write for that person.
3. **Plan hierarchy explicitly.** Before writing any section, classify each claim or idea: primary (highlighted), secondary (marked as such). This prevents Symptom 4 (Lack of Hierarchy/Structure).

**During drafting — per-section checks:**
- **Abstract:** Is it self-contained as a high-level description? Does it exceed 200 words? Does it refer to the references list? Does it motivate the model (it need not)? Does it use "loosely speaking" to handle imprecise results?
- **Introduction:** Does it deliver (a) what the paper does, (b) why the question matters (natural connection, not contrived), (c) how it relates to prior work (including what is worse)? Does it pass the final test: would a beginning graduate student have a good idea of the paper's contents?
- **Technical sections:** For every definition, is the definitional choice explained as arbitrary / simplifying / seemingly essential? For every proof, are the conceptual steps elaborated, with standard technical steps summarized and referenced?
- **Numbering:** Is there a single numbering system for easy binary search?
- **Citations:** Do they include specific theorem/section/page numbers?

**During revision — symptom scan:**
Run Goldreich's five-symptom checklist:
1. Checklist Phenomenon — is there material placed at the first possible location rather than the most suitable one?
2. Obscure Generality — is there a more natural special case that should be presented first?
3. Idiosyncrasies — is any notation or term justified only by personal appeal?
4. Lack of Hierarchy — are primary and secondary ideas visually and structurally distinguished?
5. Talmud-ism — does the paper discuss subtleties, refinements, or criticisms before presenting the basic idea clearly?

**On reviewer comments:**
Treat every reviewer comment as evidence of a problem in the write-up (even if the reviewer's proposed solution is wrong). Do not dismiss comments; diagnose the underlying problem they point to.

---

## Connection to SiS / CTPC

Goldreich's framework applies directly to Bilal's paper-writing in two ways:

**1. CTPC paper structure.** CTPC combines physics (SGP4/GMAT), ML corrector, and a hard architectural constraint (Ḣ ≤ 0). For a reader unfamiliar with port-Hamiltonian systems, this is a multi-concept paper with real risk of the Talmud-ism symptom — burying the core idea (dissipation as architecture, not loss) under formalism. Goldreich's prescription: present a simple illustrative case (e.g., a 1D harmonic oscillator with dissipation) before the full equinoctial orbital element formulation. Elaborate on the conceptual step (why Ḣ ≤ 0 can be baked into architecture via the Cholesky parameterization), not the standard technical steps (the Cholesky decomposition itself, which can be referenced).

**2. Identifying the idea.** Goldreich insists the first task is to identify the idea. For CTPC papers, candidates are: (a) physics-as-architecture as a model, (b) the Cholesky Ḣ ≤ 0 block as a technique, (c) results on orbit propagation accuracy. Picking the right one determines which section carries the most weight and how the introduction is structured.

**3. Defining the relevant community.** SDA papers can aim at ML conferences (where orbital mechanics is exotic) or astrodynamics venues (where ML is exotic). Goldreich's proxy reader should be explicitly chosen before drafting: the intelligent graduate student at the beginning of the relevant field. For NeurIPS/ICML submissions, assume orbital mechanics expertise cannot be assumed; for AAS submissions, assume ML expertise cannot be assumed. This choice cascades through every definitional discussion, background section, and motivational argument.

**4. Reviewer comments.** When CTPC receives reviewer comments, Goldreich's working assumption is: each comment indicates a problem in the write-up, even if the reviewer's proposed solution is misguided. The correct response is to diagnose the underlying problem, not defend the original text.

---

## Connections

- [[Peyton-Jones-Research-Paper]] — complementary framework for ML conference papers; compare structure recommendations for introduction
- [[Whitesides-Writing-A-Paper]] — similar reader-centric philosophy, from chemistry; good triangulation for universal principles
- [[Schimel-Writing-Science]] — book-length treatment of many of Goldreich's themes, with extensive coverage of hierarchy and story structure
- [[Williams-Bizup-Style]] — sentence-level mechanics to cure the Pain Points in §3.4 (implicit pointers, complex logical structure)
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — the classic paper on reader expectations at the sentence and paragraph level; directly complements §3.4
- [[Lipton-Writing-Heuristics]] — ML-specific heuristics; check against Goldreich's flexibility principle
- [[Langley-Crafting-ML-Papers]] — ML venue-specific guidance; apply Goldreich's general principles first, then ML-specific adjustments
- [[Ramdas-Paper-Checklist]] — operational checklist; can be read as an implementation of Goldreich's symptom scan
- [[Freeman-Writing-Papers]] — compare introduction structure recommendations
- [[Strunk-White-Elements-Style]] — sentence-level complement to Goldreich's discourse-level framework
- [[Bertsekas-Ten-Rules-Mathematical-Writing]] — mathematical writing specifics that extend Goldreich's §4.4

---

## Open Questions

1. **Tension between Obscure Generality and ML norms.** ML papers often present the most general result first (theorem statement), then discuss special cases. Goldreich recommends special case first. How should a CTPC paper navigate this — especially when reviewers expect the general framing upfront?

2. **The 5% conclusion section rule.** Does this hold for ML conference papers where a "Conclusion" section is a strong community norm? Is the norm so entrenched that violating it (even with good reason) signals inexperience? Needs triangulation with [[Peyton-Jones-Research-Paper]] and [[Langley-Crafting-ML-Papers]].

3. **Alphabetical author order.** Goldreich strongly endorses alphabetical order (CS norm). SiS papers involve Bilal as CTO and academic author. How does this interact with funding acknowledgment norms at engineering venues (AAS, AIAA) where corresponding-author / lead-author conventions differ from CS?

4. **Definitional choice classification.** Goldreich's three-way classification (arbitrary / simplifying / seemingly essential) is a powerful diagnostic. Which definitional choices in CTPC are in the "seemingly essential" category — i.e., the results are not known to hold under alternative equally natural choices? These are the places where the paper must be most honest and explicit.

5. **The reviewer-comment protocol.** Goldreich says do not necessarily follow the reviewer's suggestion, but always treat the comment as evidence of a problem. Developing a systematic template for this (problem diagnosis separate from solution acceptance) would strengthen Bilal's revision workflow.

---

## Sources

- `raw/writing/papers/guides/How_To_Write_Paper.pdf`
  - p. 1: Abstract, Introduction (§1), Why We Write Papers (§2)
  - pp. 2–4: How to Serve the Reader's Needs (§3): §3.1 (five symptoms), §3.2 (knowledge level), §3.3 (contradictory requirements), §3.4 (painful reading)
  - pp. 4–8: Some Concrete Suggestions (§4): §4.1 (title/authors), §4.2 (abstract), §4.3 (introduction), §4.4 (technical part), §4.5 (conclusions), §4.6 (references)
  - pp. 8–9: Benefiting from Readers' Comments (§5)
