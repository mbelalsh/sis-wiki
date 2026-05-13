---
title: Peyton Jones — How to Write a Great Research Paper
tags: [writing, slide-deck, peyton-jones, contributions, intuition-first, related-work, reader-first]
sources:
  - raw/writing/guides/How-to-write-a-great-research-paper.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Peyton Jones — How to Write a Great Research Paper

**Author:** Simon Peyton Jones, Microsoft Research Cambridge.
**Format:** 52-slide deck. **Origin lineage:** widely-cited talk at multiple
PhD schools and conferences; the canonical "7 suggestions" list cited by
ICML 2022 Best Practices.

## The Seven Suggestions (Verbatim Names)

1. **Don't wait: write**
2. **Identify your key idea**
3. **Tell a story**
4. **Nail your contributions to the mast**
5. **Related work: later**
6. **Put your readers first**
7. **Listen to your readers**

## Suggestion 1 — Don't wait: write

**Model 1 (wrong):** Your idea → Do research → Write paper

**Model 2 (right):** Your idea → **Write paper** → Do research

Why model 2:

- "Forces us to be clear, focused"
- "Crystallises what we don't understand"
- "Opens the way to dialogue with others: reality check, critique, and
  collaboration"

> **"Writing papers is a primary mechanism for doing research (not just
> for reporting it)."**

Don't be intimidated:

> "**Fallacy:** You need to have a fantastic idea before you can write a
> paper. (Everyone else seems to.)"
>
> "**Write a paper, and give a talk, about any idea, no matter how weedy
> and insignificant it may seem to you.**"
>
> "Writing the paper is how you develop the idea in the first place. It
> usually turns out to be more interesting and challenging that it seemed
> at first."

## Suggestion 2 — Identify your key idea

> "Your goal: to convey a **useful and re-usable idea**.
> You want to **infect the mind of your reader with your idea, like a
> virus**. Papers are far more durable than programs (think Mozart).
> **The greatest ideas are (literally) worthless if you keep them to
> yourself.**"

The "ping" framing:

> "Your paper should have **just one 'ping': one clear, sharp idea**. You
> may not know exactly what the ping is when you start writing; but you
> must know when you finish. **If you have lots of ideas, write lots of
> papers.**"

> "Make certain that the reader is in no doubt what the idea is. **Be
> 100% explicit:** 'The main idea of this paper is...' / 'In this section
> we present the main contributions of the paper.'"

(Credit to Joe Touch for "one ping".)

## Suggestion 3 — Tell a story

**Imagine you are explaining at a whiteboard. Your narrative flow:**

1. Here is a problem
2. It's an interesting problem
3. It's an unsolved problem
4. **Here is my idea**
5. My idea works (details, data)
6. Here's how my idea compares to other people's approaches

**Conference paper structure (with reader-funnel):**

- **Title** (1000 readers)
- **Abstract** (4 sentences, 100 readers)
- **Introduction** (1 page, 100 readers)
- **The problem** (1 page, 10 readers)
- **My idea** (2 pages, 10 readers)
- **The details** (5 pages, 3 readers)
- **Related work** (1-2 pages, 10 readers)
- **Conclusions and further work** (0.5 pages)

The reader-funnel is the operational frame: each section serves a
specific reader population. The title fights for 1000 readers; the
abstract fights for the next 100; the details serve only the 3 readers
who care about reproduction.

## Suggestion 4 — Nail your contributions to the mast

> **"The introduction (1 page): Describe the problem. State your
> contributions. ...and that is all. ONE PAGE!"**

Describe the problem with an example, not abstractions.

**Molehills not mountains (verbatim contrast):**

> **Yawn:** "Computer programs often have bugs. It is very important to
> eliminate these bugs [1,2]. Many researchers have tried [3,4,5,6]. It
> really is very important."
>
> **Cool:** "Consider this program, which has an interesting bug. <brief
> description>. We will show an automatic technique for identifying and
> removing such bugs."

**State your contributions:**

> "Write the list of contributions **first**. The list of contributions
> **drives the entire paper**: the paper substantiates the claims you
> have made. Reader thinks 'gosh, if they can really deliver this,
> that's be exciting; I'd better read on.'"

**Contributions should be refutable (verbatim contrast):**

| **No!** | **Yes!** |
|---------|----------|
| "We describe the WizWoz system. It is really cool." | "We give the syntax and semantics of a language that supports concurrent processes (Section 3). Its innovative features are..." |
| "We study its properties" | "We prove that the type system is sound, and that type checking is decidable (Section 4)" |
| "We have used WizWoz in practice" | "We have built a GUI toolkit in WizWoz, and used it to implement a text editor (Section 5). The result is half the length of the Java version." |

> "Your introduction makes claims. The body of the paper provides
> **evidence to support each claim**. Check each claim in the
> introduction, identify the evidence, and **forward-reference it from
> the claim**. 'Evidence' can be: analysis and comparison, theorems,
> measurements, case studies."

**No "rest of this paper is..." paragraph:**

> "**Not:** 'The rest of this paper is structured as follows. Section 2
> introduces the problem. Section 3 ...Finally, Section 8 concludes.'
>
> **Instead, use forward references from the narrative in the
> introduction.** The introduction (including the contributions) should
> survey the whole paper, and therefore forward reference every important
> part."

## Suggestion 5 — Related work: later

**Wrong structure** (related work after intro): Abstract → Intro →
**Related work** → Problem → Idea → Details → Conclusion.

**Right structure** (related work near the end): Abstract → Intro →
Problem → Idea → Details → **Related work (1-2 pages)** → Conclusion.

> "**Problem 1:** the reader knows nothing about the problem yet; so your
> (highly compressed) description of various technical tradeoffs is
> absolutely incomprehensible.
>
> **Problem 2:** describing alternative approaches gets between the
> reader and your idea."

**Credit is not like money (verbatim fallacy + truth):**

> **Fallacy:** "To make my work look good, I have to make other people's
> work look bad."
>
> **Truth:**
> - "Warmly acknowledge people who have helped you"
> - "Be generous to the competition. 'In his inspiring paper [Foo98]
>   Foogle shows.... We develop his foundation in the following ways...'"
> - "Acknowledge weaknesses in your approach"
>
> **"Giving credit to others does not diminish the credit you get from
> your paper."**

## Suggestion 6 — Put your readers first

**Right structure ordering (re-emphasized):** Abstract → Introduction →
**The problem** → **My idea** → **The details** → Related work →
Conclusions.

**Anti-pattern (verbatim):**

> "3. The idea
> Consider a bifurcated semi-lattice D, over a hyper-modulated signature
> S. Suppose pi is an element of D. Then we know for every such pi there
> is an epi-modulus j, such that p_j < p_i.
>
> **Sounds impressive...but sends readers to sleep, and/or makes them
> feel stupid.**"

**Presenting the idea:**

> "Explain it as if you were speaking to someone using a whiteboard.
> **Conveying the intuition is primary, not secondary**. Once your reader
> has the intuition, she can follow the details (but not vice versa).
> Even if she skips the details, she still takes away something
> valuable."

**Conveying the intuition:**

> **"Introduce the problem, and your idea, using EXAMPLES and only then
> present the general case."**

(The Simon-PJ recurring question: "is there any typewriter font?" —
i.e., show the code/example immediately, in a distinct visual register.)

**Putting the reader first (verbatim):**

> "**Do not recapitulate your personal journey of discovery.** This route
> may be soaked with your blood, but that is not interesting to the
> reader.
>
> Instead, **choose the most direct route to the idea.**"

## Suggestion 7 — Listen to your readers

(Pages 41-52 of the deck not fully captured in summary read, but the
canonical Peyton-Jones content for this section covers: get **expert
friends** to read the draft (small number, deep feedback); get
**non-expert friends** to read it (catch confusion); explain to people
verbally; revise heavily; treat each reader's bafflement as **your bug,
not their bug**.)

## Core Actionable Rules (Numbered, Compressed)

1. **Write first, research second.** Writing develops the idea.
2. **Don't gate writing on having a great idea.** Write about whatever
   you have; the idea sharpens through writing.
3. **One "ping" per paper.** State the main idea in 100% explicit terms.
4. **Narrative arc = problem → interesting → unsolved → my idea → works
   → comparison.**
5. **Conference paper structure** with reader-funnel awareness (1000 /
   100 / 100 / 10 / 10 / 3 / 10 readers).
6. **Introduction is exactly 1 page: problem + contributions.**
7. **Open the intro with a concrete example/instance, not abstract
   importance claims.** (Molehills not mountains.)
8. **Write contributions first.** They drive the whole paper.
9. **Make contributions refutable.** "We have built a GUI toolkit and
   used it to implement a text editor — half the length of the Java
   version" beats "We studied its properties."
10. **Forward-reference evidence from each claim in the intro.**
11. **No "rest of this paper is..." paragraph.** Embed forward
    references in the narrative.
12. **Related work: AFTER the idea, not before.** 1-2 pages.
13. **Credit is not zero-sum.** Acknowledge helpers, be generous to
    competitors, name your own weaknesses.
14. **Intuition first, details second.** Explain as if at a whiteboard.
15. **Examples before general case.**
16. **Don't recapitulate your discovery journey.** Pick the most direct
    route to the idea.
17. **Get expert + non-expert readers** and treat their confusion as a
    bug in your writing.

## Reviewer-Facing Implications

Peyton-Jones explicitly models a one-page-introduction reviewer who:

- Reads the title, abstract, and intro (the 100-reader population)
- Looks for the explicit contribution claims
- Forms an early opinion that the rest of the paper either substantiates
  or fails to substantiate

The reader-funnel framing aligns directly with how ICML reviewers
behave (per [[ICML-2022-Review-Form]]): a Phase 1 reviewer making the
binary Yes/No call may read only the title + abstract + intro before
deciding, so suggestion 4 (nail contributions in 1-page intro) is
operationally critical.

The "molehills not mountains" pattern is reviewer-protective: a generic
opening signals a forgettable paper before the reviewer even reaches the
contribution claim.

## ML-Paper-Specific Advice (Distinguished from General Writing)

This deck is **discipline-agnostic** — Peyton Jones writes from the
PL/CS background but explicitly targets all of computer science +
adjacent fields. ML-paper specifics aren't called out, but the structural
advice transfers cleanly:

- The conference-paper structure (4-sentence abstract, 1-page intro)
  matches ICML/NeurIPS conventions precisely.
- The reader-funnel numbers (1000 → 100 → 10 → 3) calibrate well to ML's
  large-conference review dynamics.
- The "refutable contributions" frame maps to ML's quantitative-result
  expectation (specific numbers, specific benchmarks, specific
  comparisons).

The deck *predates* the modern ML reproducibility-checklist tradition
(Pineau 2018, AAAI 2021), so it doesn't address seed-setting / code
release — those gaps are filled by [[ICML-Best-Practices]].

## Connection to SiS / CTPC

Direct pre-submission audit for [[CTPC-KDD-Submission]]:

- **Suggestion 2 (one ping):** the CTPC "ping" — is it (a) the
  Predictor+Corrector decomposition, (b) the d̄² ≈ 1 calibration vs.
  d̄² > 20,000 baselines, or (c) the Student-t head + RTN decomposition?
  Per [[CTPC-Design-Rationale]] this looks like multiple pings — choose
  ONE for the KDD submission; the others become satellite papers per
  "if you have lots of ideas, write lots of papers."
- **Suggestion 3 (narrative):** CTPC narrative = (1) orbital prediction
  is hard, (2) it's interesting (SDA stakes), (3) Latent ODE baselines
  fail (d̄² > 20,000), (4) our idea = predictor + probabilistic
  corrector, (5) 64% MSE reduction at 4-day horizon + d̄² ≈ 1, (6)
  compare to Latent ODE + ML baselines.
- **Suggestion 4 (refutable contributions):**
  - **No:** "We propose a continuous-time probabilistic corrector for
    orbital prediction."
  - **Yes:** "We prove that the Latent NCDE Corrector with Student-t
    NLL+CRPS+KL loss achieves 64% MSE reduction at 4-day horizon on
    NASA CDDIS data, with d̄² ≈ 1 calibration vs. Latent ODE's
    d̄² > 20,000."
- **Suggestion 5 (related work later):** the current draft's related
  work position — audit whether it comes before "the problem" / "my
  idea." If so, move it after.
- **Suggestion 6 (intuition first):** the Latent NCDE Corrector
  description should open with a concrete trajectory example (TLE noisy
  observation → physics prediction → ML correction → calibrated
  posterior), not the encoder-decoder architecture diagram.

## Connections

- [[ICML-2022-Review-Form]] — reader-funnel target audience
- [[ICML-Best-Practices]] — cites this deck explicitly
- [[Lipton-Writing-Heuristics]] — heuristic 1 (4-bullet abstract) and 3
  (delete generic opening) overlap directly with Peyton-Jones's
  suggestions 4 and 4-molehills
- [[Crafting-ML-Papers]] — Langley's §2 content topics complement
  Peyton-Jones's suggestion 4 contribution list
- [[Ramdas-Paper-Checklist]] — overlap on abstract structure +
  related-work positioning
- [[CTPC-KDD-Submission]] — pre-submission audit target

## Open Questions

1. **Multi-idea papers.** Peyton-Jones's "one ping" rule prescribes
   splitting multi-idea work. SiS's CTPC has at least 3 distinct
   contributions (architectural + loss + calibration). Should KDD '26
   focus on one, with the others as separate papers? Or is "Year 1 CTPC
   as integrated framework" a single ping?
2. **Listen-to-readers suggestion 7 protocol.** Who are SiS's
   expert+non-expert reader pool? Internal SiS team + Coordinated
   Systems Lab + external SDA contacts? The agent council is one
   automated proxy but Peyton-Jones means real human readers.

## Sources

- `raw/writing/guides/How-to-write-a-great-research-paper.pdf` — 52 slides,
  Simon Peyton Jones, Microsoft Research Cambridge.
- Identified via citation in [[ICML-Best-Practices]] resource list.
- Pages 1-40 read in detail; pages 41-52 (suggestion 7 expansion) summarized
  from canonical Peyton-Jones content. The widely-circulated version of
  this talk emphasizes: expert vs. non-expert readers, treating confusion
  as the writer's bug, iterating heavily based on feedback.
