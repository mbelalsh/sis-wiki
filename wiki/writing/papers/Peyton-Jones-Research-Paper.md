---
title: "How to Write a Great Research Paper — Simon Peyton Jones"
tags: [writing, paper-writing, structure, narrative, contributions, related-work, feedback, style, reader-first]
sources:
  - raw/writing/papers/guides/How-to-write-a-great-research-paper.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# How to Write a Great Research Paper — Simon Peyton Jones

## One-Line Intuition
Seven concrete rules for transforming any research idea — however small it seems — into a paper that infects the reader's mind and survives long after your code is gone.

## Why This Matters
Most researchers treat writing as the final step that reports completed work. Peyton Jones argues this is backwards: writing is itself the primary mechanism of research, not merely its announcement. This reframe changes everything about when to write, how to structure, and what to say in an introduction. For Bilal, who is simultaneously producing CTPC results and building the SiS publication pipeline, this deck provides the operational checklist for turning prototype results into accepted conference papers.

---

## Core Content

### The Seven Suggestions

#### 1. Don't Wait — Write

**The two models:**

- **Model 1 (wrong):** Your idea → Do research → Write paper
- **Model 2 (right):** Your idea → Write paper → Do research

Writing the paper is how you develop the idea in the first place, not how you report it after the fact. The act of writing forces you to be clear and focused, crystallises what you don't yet understand, and opens the way to dialogue with others (reality check, critique, collaboration).

**Key insight:** "Writing papers is a primary mechanism for doing research (not just for reporting it)." (slide 7)

**Corollary:** Do not wait for a "fantastic" idea before writing. Write about any idea, no matter how weedy and insignificant it seems. It usually turns out to be more interesting and challenging than it seemed at first. Writing the paper is how you develop the idea in the first place.

**Practical move:** Start a paper skeleton — problem, idea, contributions placeholder — as soon as you have a hypothesis, not after you have results.

---

#### 2. Identify Your Key Idea

**Goal:** Infect the mind of your reader with your idea, like a virus. Papers are far more durable than programs (Peyton Jones's framing: "think Mozart"). The greatest ideas are worthless if you keep them to yourself.

**The "one ping" principle** (attributed to Joe Touch):
- Your paper should have just one "ping": one clear, sharp idea.
- You may not know exactly what the ping is when you start writing, but you must know when you finish.
- If you have lots of ideas, write lots of papers — don't stuff them all into one.

**Making the ping audible:**
- Many papers contain good ideas but fail to distil what they are.
- Be 100% explicit. Use phrases like:
  - "The main idea of this paper is..."
  - "In this section we present the main contributions of the paper."
- Do not leave the reader to guess. Make certain the reader is in no doubt what the idea is.

**Anti-intimidation note:** You need not have a fantastic idea to start. Write about any idea. Writing the paper is how you develop the idea in the first place.

---

#### 3. Tell a Story

**The narrative flow (whiteboard model):**

Imagine you are explaining at a whiteboard. The structure of your argument should follow this sequence:
1. Here is a problem
2. It's an interesting problem
3. It's an unsolved problem
4. Here is my idea
5. My idea works (details, data)
6. Here's how my idea compares to other people's approaches

This is the skeleton of the entire paper — not just the introduction.

**The reader-funnel (conference paper structure with shrinking readership):**

| Section | Length | Readers |
|---|---|---|
| Title | — | ~1000 |
| Abstract | 4 sentences | ~100 |
| Introduction | 1 page | ~100 |
| The problem | 1 page | ~10 |
| My idea | 2 pages | ~10 |
| The details | 5 pages | ~3 |
| Related work | 1–2 pages | ~10 |
| Conclusions and further work | 0.5 pages | — |

The funnel insight: most of your readers will never get past the abstract. Each section must earn the reader's continued attention. The title and abstract are not bureaucratic formalities — they are the primary sales mechanism.

**"Molehills not mountains" (opening the problem):**

Bad opening (mountain): "Computer programs often have bugs. It is very important to eliminate these bugs [1,2]. Many researchers have tried [3,4,5,6]. It really is very important." → *Yawn.*

Good opening (molehill): "Consider this program, which has an interesting bug. [brief description]. We will show an automatic technique for identifying and removing such bugs." → *Cool.*

The tactic: **use a specific, concrete example to introduce the problem in sentence one or two**. Do not lead with a generic importance claim. Drop the reader directly into an interesting instance of the problem.

---

#### 4. Nail Your Contributions to the Mast

**The introduction = problem description + contributions list. That is all. ONE PAGE.**

**Writing the contributions first** is the key tactic. The list of contributions drives the entire paper: the paper's job is to substantiate the claims you have made. This makes the reader think: "gosh, if they can really deliver this, that's exciting; I'd better read on."

**Contributions must be refutable** — they must be falsifiable claims, not vague descriptions:

| No | Yes |
|---|---|
| We describe the WizWoz system. It is really cool. | We give the syntax and semantics of a language that supports concurrent processes (Section 3). Its innovative features are... |
| We study its properties | We prove that the type system is sound, and that type checking is decidable (Section 4) |
| We have used WizWoz in practice | We have built a GUI toolkit in WizWoz, and used it to implement a text editor (Section 5). The result is half the length of the Java version. |

**Evidence flow:** Your introduction makes claims. The body of the paper provides evidence to support each claim. For each claim in the introduction, identify the evidence, and forward-reference it from the claim. Evidence can be: analysis and comparison, theorems, measurements, case studies.

**No "rest of this paper is structured as follows":** This is a bureaucratic placeholder. Instead, use forward references from the narrative in the introduction. The introduction (including the contributions) should survey the whole paper, and therefore forward-reference every important part.

---

#### 5. Related Work: Later

**Put related work after the body, not before it** (1–2 pages, near the end):

- Putting it early (after introduction) means: (1) the reader knows nothing about the problem yet, so your highly compressed description of technical tradeoffs is incomprehensible; (2) describing alternative approaches gets between the reader and your idea.

**Correct paper structure:**
Abstract → Introduction (1 page) → The problem (1 page) → My idea (2 pages) → The details (5 pages) → **Related work (1–2 pages)** → Conclusions (0.5 pages)

**Credit is not like money — giving credit to others does not diminish the credit you get from your paper:**
- Warmly acknowledge people who have helped you.
- Be generous to the competition: "In his inspiring paper [Foo98] Foogle shows.... We develop his foundation in the following ways..."
- Acknowledge weaknesses in your approach.

---

#### 6. Put Your Readers First (Use Examples)

**Presenting the idea:**
- Explain it as if you were speaking to someone using a whiteboard.
- Conveying the intuition is primary, not secondary.
- Once your reader has the intuition, she can follow the details (but not vice versa).
- Even if she skips the details, she still takes away something valuable.

**The example-first rule:**
> "Introduce the problem, and your idea, using **EXAMPLES** and only then present the general case."

Peyton Jones tests papers by asking: "is there any typewriter font?" — meaning, is there a concrete code example in the first two pages? If not, there should be. Abstract theorem-first presentation ("Consider a bifurcated semi-lattice D, over a hyper-modulated signature S...") sends readers to sleep and/or makes them feel stupid.

**Do not recapitulate your personal journey of discovery.** This route may be soaked with your blood, but that is not interesting to the reader. Instead, choose the most direct route to the idea — the route optimised for reader comprehension, not for chronological honesty about your struggle.

---

#### 7. Listen to Your Readers

**Getting help — before submission:**
- Experts are good; non-experts are also very good.
- Each reader can only read your paper for the first time once — use them carefully.
- Explain carefully what you want. "I got lost here" is much more important feedback than "Jarva is mis-spelt."
- "Get your paper read by as many friendly guinea pigs as possible."

**Expert help tactic:** When you think you are done, send the draft to the competition saying "could you help me ensure that I describe your work fairly?" They will often respond with helpful critique (they are interested in the area). They are likely to be your referees anyway, so getting their comments or criticism up front is Jolly Good.

**Listening to reviewers — after rejection or review:**
- Treat every review like gold dust. Be (truly) grateful for criticism as well as praise. This is really, really, really hard — but it's really, really, really, really, really, really, really, really, really, really important.
- Read every criticism as a positive suggestion for something you could explain more clearly.
- DO NOT respond "you stupid person, I meant X". INSTEAD: fix the paper so that X is apparent even to the stupidest reader.
- Thank them warmly. They have given up their time for you.

---

### Language and Style (Bonus Section)

Peyton Jones adds a "Language and Style" section after the main seven suggestions:

**Visual structure:**
- Give strong visual structure using: sections and sub-sections, bullets, italics, laid-out code.
- Find out how to draw pictures, and use them.

**Basic submission hygiene:**
- Submit by the deadline.
- Keep to the length restrictions: do not narrow the margins, do not use tiny fonts.
- On occasion, supply supporting evidence (experimental data, proof) in an appendix.
- Always use a spell checker.

**Use the active voice:**
The passive voice is "respectable" but it deadens your paper. Avoid it at all costs.

| No (passive) | Yes (active) |
|---|---|
| It can be seen that... | We can see that... |
| 34 tests were run | We ran 34 tests |
| These properties were thought desirable | We wanted to retain these properties |
| It might be thought that this would be a type error | You might think this would be a type error |

**Use simple, direct language:**

| No (bloated) | Yes (direct) |
|---|---|
| The object under study was displaced horizontally | The ball moved sideways |
| On an annual basis | Yearly |
| Endeavour to ascertain | Find out |
| It could be considered that the speed of storage reclamation left something to be desired | The garbage collector was really slow |

---

## Key Verbatim Anchors

1. **"Writing papers is a primary mechanism for doing research (not just for reporting it)"** — slide 7. The core inversion of the entire deck.

2. **"You want to infect the mind of your reader with your idea, like a virus"** — slide 9. The purpose of a paper is viral transmission of one idea.

3. **"The greatest ideas are (literally) worthless if you keep them to yourself"** — slide 9. Moral urgency to publish.

4. **"Write a paper, and give a talk, about any idea, no matter how weedy and insignificant it may seem to you"** — slides 11–12. Permission to start without a "big" result.

5. **"Your paper should have just one 'ping': one clear, sharp idea"** — slide 13. The single-ping test for focus.

6. **"The list of contributions drives the entire paper: the paper substantiates the claims you have made"** — slide 21. Contributions are the load-bearing frame.

7. **"Contributions should be refutable"** — slide 23. A vague description is not a contribution.

8. **"Introduce the problem, and your idea, using EXAMPLES and only then present the general case"** — slide 37. The example-first mandate.

9. **"Do not recapitulate your personal journey of discovery. This route may be soaked with your blood, but that is not interesting to the reader."** — slide 39. Choose the reader's route, not yours.

10. **"Get your paper read by as many friendly guinea pigs as possible"** — slide 41. Volume of pre-submission feedback matters.

11. **"Treat every review like gold dust. Be (truly) grateful for criticism as well as praise."** — slide 43. The emotional posture toward reviewer feedback.

12. **"The passive voice is 'respectable' but it deadens your paper. Avoid it at all costs."** — slide 49. Active voice as non-negotiable.

---

## How a Writing Agent Should Use This

**Mode:** Paper-writing agent, pre-draft and during-draft review.

**Concrete moves a writing agent should execute using this source:**

1. **Ping extraction:** When given a draft or a set of results, ask "what is the one ping?" and make it explicit in the abstract and introduction with the exact phrase "The main idea of this paper is..."

2. **Contributions audit:** Before touching any other section, write a bulleted contributions list with refutable claims (theorems proven, benchmarks run, systems built, measurements taken). Each bullet must name a section that delivers evidence for it. Vague bullets ("We study X") get flagged and rewritten.

3. **Introduction structure check:** Enforce the two-part introduction rule — problem description + contributions list, ONE PAGE. Flag any "rest of this paper is structured as follows" sentences and replace with narrative forward references.

4. **Reader-funnel test:** Check that abstract is approximately 4 sentences. Check that introduction is approximately 1 page. If either is over-length, identify what to cut.

5. **Related work placement:** Flag any related work section placed immediately after the introduction and recommend moving it to after "the details" section. Remind: related work early gets between the reader and the idea.

6. **Example check:** Scan the first two pages for at least one concrete, specific example (ideally with typewriter/code font). If none present, identify the best candidate and draft it.

7. **Opening paragraph test:** Apply the "molehill not mountain" test to the first paragraph. If it begins with a generic importance claim ("X is important..."), replace with a specific, interesting instance of the problem.

8. **Active voice pass:** Scan all passive constructions and convert. Flag the most egregious passive constructions for revision.

9. **Write-early prompt:** When Bilal starts a new research thread, prompt him to draft a 1-page paper skeleton (problem + contributions placeholder) before doing any experiments, not after.

10. **Review response protocol:** When Bilal receives reviewer feedback, apply the Peyton Jones frame: every criticism = a specific place to improve clarity; do not argue the reviewer is wrong; fix the paper so the misunderstanding is impossible.

---

## Connection to SiS / CTPC

**Direct application to CTPC paper writing:**

- **The ping for CTPC:** The one ping is: "Physics-as-architecture is strictly superior to physics-as-penalty for orbital dynamics correction." Every CTPC paper draft should open with this claim made explicit and should substantiate it through ablation experiments.

- **Contributions list for CTPC:** Bilal should draft a refutable contributions list before running any new experiment. Example structure:
  - We prove that Ḣ ≤ 0 can be enforced as a hard architectural constraint via Cholesky parameterisation of the dissipation matrix (Section X).
  - We demonstrate on N TLE epochs that CTPC reduces position error by Y% over SGP4 baseline (Section X).
  - We show that soft-constraint baselines fail to satisfy the dissipation condition on Z% of test trajectories (Section X, Table Y).

- **Introduction for CTPC:** Open with a specific TLE epoch where SGP4 fails (the molehill — a concrete interesting failure case), not with "orbital prediction is important for space safety" (the mountain). Show the residual. Then introduce CTPC as the solution.

- **Related work placement:** Put the NeurIPS/ICML related work section after the architecture section, not before it. The reader needs to understand CTPC before they can appreciate how it differs from Neural ODEs, HNNs, or PINN baselines.

- **Write early:** The CTPC wiki and this research knowledge base itself instantiates the "write early" principle — Bilal maintains a running synthesis of ideas as they develop, not only when they are fully proven.

- **The durability argument:** Peyton Jones notes that papers are far more durable than programs (think Mozart). SiS's codebase may change, but a well-written CTPC paper becomes a permanent contribution that survives SiS's product pivots.

---

## Connections

- [[Langley-Crafting-ML-Papers]] — Langley's companion guide for ML-specific paper structure; compare to Peyton Jones's general CS framing
- [[Freeman-Writing-Papers]] — Freeman's complementary advice on scientific writing
- [[Lipton-Writing-Heuristics]] — Lipton's ML-specific writing heuristics; active voice advice overlaps strongly
- [[Ramdas-Paper-Checklist]] — Checklist format complements Peyton Jones's suggestions as a pre-submission gate
- [[Whitesides-Writing-A-Paper]] — Whitesides's chemistry-context parallel: outline-driven writing, contributions first
- [[Bertsekas-Ten-Rules-Mathematical-Writing]] — Math-specific voice and precision rules complement Peyton Jones's general style advice
- [[Strunk-White-Elements-Style]] — Style authority behind the active voice and simplicity rules Peyton Jones endorses
- [[Schimel-Writing-Science]] — Schimel's story-structure model is the same narrative arc (problem → interesting → unsolved → idea → evidence → comparison)
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — Reader-expectation theory of why passive voice deadens prose (mechanistic grounding for Peyton Jones's intuition)
- [[Goldreich-How-To-Write-Paper]] — Another senior CS researcher's perspective; compare contribution-framing advice

---

## Open Questions

1. **Abstract sentence structure:** Peyton Jones prescribes "4 sentences" for the abstract but does not specify what each sentence should contain. What is the canonical 4-sentence breakdown? Compare to Schimel's opening-challenge structure and the Langley template.

2. **When does the "one ping" rule break down?** Systems papers (e.g., a paper presenting CTPC end-to-end) often have multiple contributions at different levels (architecture + training procedure + benchmark). How do you maintain the single-ping focus without under-selling?

3. **Non-expert guinea pig threshold:** Peyton Jones says non-experts are "also very good" pre-submission readers. What is the right expertise level for a CTPC pre-submission reader — should Bilal seek orbital mechanics experts, ML experts, or domain-naive readers?

4. **Competition-as-reviewer tactic:** Sending a draft to potential competitors/referees is high-leverage but high-risk. What are the norms in the SDA/ML community for this tactic? Is it common at ICML/NeurIPS or primarily a CS theory norm?

5. **Visual structure for ML papers:** Peyton Jones emphasises "strong visual structure" including laid-out code and pictures, but for an ML paper the key figures are training curves, architecture diagrams, and ablation tables. What is the minimal figure set for a CTPC conference submission?

---

## Sources

- `raw/writing/papers/guides/How-to-write-a-great-research-paper.pdf`
  - Slides 1–2: Title and framing ("Seven simple, actionable suggestions")
  - Slides 3–7: Suggestion 1 — Don't wait; two models of writing
  - Slides 8–14: Suggestion 2 — Identify your key idea; the one-ping principle
  - Slides 15–19: Suggestion 3 — Tell a story; narrative flow; reader-funnel / conference paper structure
  - Slides 20–25: Suggestion 4 — Nail contributions; refutable contributions; evidence flow; no "rest of this paper"
  - Slides 26–32: Suggestion 5 — Related work later; credit is not like money
  - Slides 33–39: Suggestion 6 — Put readers first; conveying intuition; examples-first; reader-first not journey-of-discovery
  - Slides 40–44: Suggestion 7 — Listen to readers; getting help; listening to reviewers
  - Slides 45–52: Language and Style bonus section — visual structure, basic hygiene, active voice, simple direct language
