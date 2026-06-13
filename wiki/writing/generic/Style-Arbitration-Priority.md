---
title: Style Arbitration Priority (Meta-Rule for Resolving Conflicts Between Style Sources)
tags: [writing, generic, meta-rule, arbitration, priority-ordering, cohesion, concision, genre-convention, agent-control]
sources:
  - derived/agent-diagnosis  # not extracted from a book; authored to resolve observed agent failures
created: 2026-06-12
updated: 2026-06-12
sis_relevance: high
hard_constraint_possible: yes
note_type: meta-rule  # governs HOW the other source notes are applied, not a source itself
---

# Style Arbitration Priority (Meta-Rule)

## One-Line Intuition
When two style principles conflict, the agent must know which one wins — without an explicit priority order, the loudest cluster of sources (sentence-level concision) silently overrides quieter but more important principles (cohesion, genre convention).

## Why This Matters
This note is **not a source distillation** like the rest of the corpus. It is a control rule that sits *above* the source notes and dictates how to resolve conflicts between them.

The Layer-0 corpus contains roughly a dozen sentence-level sources that all push in the same direction: omit needless words, split overloaded sentences, prefer strong verbs, de-clutter (Strunk Rule 13, CMU-Concise-Sentences, CMU-Subject-Verb-Separation, CMU-Known-New Steps 4–5). It contains far fewer sources governing cohesion across sentence boundaries (CMU-Known-New core, Gopen-Swan) and almost nothing enforcing genre-conventional phrasing (CMU-Starter-Phrases is the lone bank, and it is advisory).

With all sources weighted equally, the numerically dominant concision/freshness cluster wins every conflict. Three observed failure modes follow directly from this imbalance:

1. **Over-splitting** — sentences chopped into short, same-shaped declaratives to satisfy "one idea per sentence," producing choppy rhythm (violates Strunk Rule 14).
2. **Broken cohesion** — after a split, the new sentence opens by restating the previous sentence's topic instead of chaining from its stress position (violates the known-new contract the corpus already contains).
3. **Invented connectives** — the agent paraphrases conventional academic transitions into fresh-sounding alternatives ("To that end" → "To close it"), because concision/de-cliché instincts treat stock phrasing as something to improve. In academic register this is backwards: connectives should be invisible and conventional; originality belongs to ideas, not transitions.
4. **Imprecise category words** — the agent picks a fluent but denotatively wrong noun ("function" for what is really a "task"), because nothing in the dominant cluster checks whether a word names the correct *category* of referent. The precision principle exists in the corpus (Bertsekas, Krantz) but is buried in special-case sections and tagged soft, so it loses to fluency and concision.

The first three are the same root cause: no arbitration order, so the loudest cluster wins. The fourth is adjacent: a correctness-grade principle exists but is buried and mis-tagged as soft, so it never gets surfaced. This note supplies the ordering **and** promotes the buried precision principle to Level 1.

## Core Content

### The Priority Ladder

When two principles in the corpus conflict, resolve in this fixed order. A higher level **always** overrides a lower one.

| Priority | Principle | Overrides | Governing sources |
|---|---|---|---|
| 1 | **Correctness** | everything | technical accuracy; grammar (Strunk §II); denotative precision (Bertsekas, Krantz) |
| 2 | **Genre convention** | freshness, concision | CMU-Starter-Phrases; field norms |
| 3 | **Cohesion** | concision | CMU-Known-New; Gopen-Swan |
| 4 | **Concision / freshness** | nothing | Strunk 13; CMU-Concise; CMU-Subject-Verb |

---

**Level 1 — Correctness.** Grammatical accuracy, faithful technical claims, and **denotative precision**. Never overridden by any stylistic consideration. A more elegant sentence that misstates the method is always wrong.

Denotative precision means: each load-bearing noun and verb must name the correct *category* of referent — action vs. task vs. capability vs. property vs. object vs. system operation. Do not use a loosely-fitting word when a precise term exists. This is the same discipline Bertsekas applies to natural language ("don't say *likelihood* or *chance* when you mean *probability*") and Krantz applies to quantifiers ("*every*/*each* for universal, never *any*") and to terminology as a precision instrument (a well-chosen term compresses a cluster of conditions into one word; a wrong one compresses to the wrong cluster). See [[Bertsekas-Ten-Rules-Mathematical-Writing]] (intuitive-writing section: "maintain rigor in the use of natural language") and [[Krantz-Primer-Mathematical-Writing]] (§1.3 and the word-rules table).

> Failure signature this prevents: calling orbit determination (an estimation task), collision-risk assessment (an analysis task), and maneuver planning (a planning task) all "functions" — a single noun applied to a list of mismatched categories. "Function" connotes a system input–output mapping; the correct category word is "task."

This principle already exists in the corpus but is buried inside special-case sections of two source notes and tagged there as soft. This note **promotes it to a Level-1 hard constraint**: when natural language carries the precision load, word choice is a correctness issue, not a style preference, and concision (Level 4) may never trade a precise term for a shorter vaguer one.

**Level 2 — Genre convention.** Use conventional academic phrasing for transitions, hedges, and framing moves, drawn verbatim from the starter-phrase bank ([[CMU-Starter-Phrases]]): "To that end," "Motivated by this gap," "Consequently," "There is strong convergent evidence for." Do **not** paraphrase, freshen, or invent alternatives to standard connectives. Originality belongs to ideas and claims, never to connective tissue. A stock transition is doing its job precisely when the reader does not notice it. This level overrides all concision and de-cliché instincts.

> Failure signature this prevents: "To close it," "To wrap up the motivation," and similar invented connectives that no reviewer expects to see.

**Level 3 — Cohesion.** Maintain the known-new chain across every sentence boundary ([[CMU-Known-New-Information-Flow]]). Each sentence's topic position should link back to the prior sentence's stress position. After splitting any sentence for concision or one-idea-per-sentence, immediately recheck the link across the new boundary; if it is weak, repair it with a transitional phrase (CMU-Known-New Step 6) before finalizing. Never leave a split that opens by restating the previous sentence's topic. This level overrides concision.

> Failure signature this prevents: "...accurate state predictions. They also need trustworthy uncertainty estimates..." — sentence 2 restarts instead of chaining from "predictions."

**Level 4 — Concision / freshness.** Omit needless words (Strunk Rule 13); prefer strong active verbs over nominalizations; keep subject and verb close ([[CMU-Concise-Sentences]], [[CMU-Subject-Verb-Separation]]). This is the **lowest** priority. Apply it only when doing so does not violate Levels 1–3. Concision is a default, not a trump card.

---

### Mandatory Paragraph-Level Pass

The sentence-level sources optimize each sentence locally; none governs the paragraph as a unit. A greedy sequence of locally optimal sentences produces a globally flat paragraph. Add one final pass **after** all sentence-level revision:

1. Read the whole paragraph as a unit (read it aloud if possible).
2. If three or more consecutive sentences share the same subject–verb–object shape or similar length, recombine or restructure for rhythmic variation (Strunk Rule 14: *avoid a succession of loose sentences*) — even at the cost of a longer sentence or one carrying two linked ideas.
3. Verify the known-new chain still holds after recombination.
4. At this stage, **cohesion and rhythm outrank concision.** A slightly longer sentence that flows is better than two short ones that stutter.

This pass is permitted to override the output of the sentence-level passes. It is the only stage whose unit of analysis is the paragraph.

## Key Operating Anchors

- Higher priority always wins: Correctness > Genre convention > Cohesion > Concision.
- Denotative precision is a Level-1 correctness issue: pick the word whose category matches the referent; never trade a precise term for a shorter vaguer one.
- Connectives are conventional and invisible by design; never freshen a stock transition.
- After any split, recheck and repair the known-new link before finalizing.
- Run one paragraph-level rhythm pass last; it may override sentence-level results.

## How a Writing Agent Should Use This

**Agent:** Layer-0 Generic-Writing-Agent. Because every upper-layer agent (Layer-1a Paper-Writing, Layer-1b Proposal-Writing) inherits Layer 0, this meta-rule governs all of them.

**Placement:** This note must be **always-loaded**, not retrieval-gated. It belongs in the agent's top-level system prompt or a guaranteed-load context block, positioned **before** the source-note corpus is read. If the agent forms its concision-default by reading the dozen sentence-level notes first and only then encounters this arbitration rule, the rule arrives too late to change the default. Priority instructions must precede the rules they arbitrate.

**Operation — Drafting:**
- Select connectives and framing phrases from [[CMU-Starter-Phrases]] verbatim (Level 2).
- Build the known-new chain as you draft (Level 3), introducing one new idea per stress position.
- Let concision shape word choice (Level 4) but never at the expense of Levels 2–3.

**Operation — Revision (sentence level):**
- Apply concision and subject–verb passes as before, but treat every split as provisional: re-run the known-new check across the new boundary and repair before accepting the split.

**Operation — Revision (paragraph level):**
- Run the mandatory paragraph pass. Flag any run of ≥3 same-shaped or same-length sentences and restructure for variation.

**Self-check before finalizing a paragraph:**
- For each key noun/verb, does the word name the right *category* of referent (action vs. task vs. capability vs. object)? Is a precise term available that I passed over for a vaguer one? Is one noun applied to a list of mixed categories? → re-pick (Level 1).
- Did I invent any connective instead of using a stock one? → revert to Level 2.
- Does any sentence open by restating the prior sentence's topic? → repair the chain (Level 3).
- Are there three+ short same-shaped sentences in a row? → recombine (paragraph pass).

## Connection to SiS / CTPC
The diagnosis that produced this note came from comparing two drafts of a CTPC/GMAT Predictor–Corrector introduction. The weaker draft was technically clean at the sentence level but choppy, thinly connected, and used an invented transition ("To close it") in place of a conventional one ("To that end"). All three defects traced to the missing arbitration order. For SiS papers and proposals specifically — where reviewers from the SDA and PIML communities expect conventional academic register — Level 2 (genre convention) is high-value: the writing should call attention to the physics-as-architecture *idea*, never to its own connective phrasing.

## Connections
- [[CMU-Starter-Phrases]] — the source of Level-2 conventional connectives; this note makes that bank mandatory rather than advisory
- [[CMU-Known-New-Information-Flow]] — the Level-3 cohesion principle; this note enforces a re-check after every concision-driven split
- [[CMU-Concise-Sentences]] — demoted to Level 4 by this note; still applied, but never as a trump card
- [[CMU-Subject-Verb-Separation]] — Level-4 concision companion
- [[Strunk-White-Elements-Style]] — Rule 13 (concision, Level 4) and Rule 14 (avoid successive loose sentences, the basis for the paragraph pass) sit at opposite priorities here; this note reconciles them
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — scholarly source for the cohesion principle at Level 3
- [[Schimel-Writing-Science]] — the only corpus source operating above sentence scale; the paragraph pass is the local complement to Schimel's section-level story structure
- [[Bertsekas-Ten-Rules-Mathematical-Writing]] — source of the Level-1 denotative-precision principle ("maintain rigor in the use of natural language; avoid loose terms"); this note promotes that buried principle to a hard constraint
- [[Krantz-Primer-Mathematical-Writing]] — companion precision source (quantifier discipline; terminology as a precision instrument); also promoted to Level 1 here
- [[CMU-BLUF-Topic-Sentence]] — paragraph-opening claim placement; compatible with and prior to the paragraph rhythm pass

