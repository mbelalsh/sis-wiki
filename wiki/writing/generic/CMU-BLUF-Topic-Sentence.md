---
title: BLUF and Topic Sentences (CMU)
tags: [writing, generic, structure, paragraph, topic-sentence, BLUF, bottom-line-up-front]
sources:
  - raw/writing/generic/CMU_bluf-topic-sentence.pdf
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: no
---

# BLUF and Topic Sentences (CMU)

## One-Line Intuition
Put your conclusion first — readers need the main point immediately, not after they have waded through your reasoning.

## Why This Matters
Readers in academia and business are overloaded. Their top priority is getting through text efficiently. Writers naturally draft in chronological/logical order — building up to the conclusion — because that is how thinking proceeds. But that draft order is the reverse of what readers need. The writer's responsibility is to invert the structure: bottom line first, supporting detail after.

## Core Content

### The BLUF Principle

BLUF = **B**ottom **L**ine **U**p **F**ront.

- **Writer's natural draft order**: logical beginning → middle reasoning → conclusion/bottom line (buried at the end).
- **Reader-optimised order**: bottom line first → supporting reasoning → detail.
- The bottom line of a document should be identifiable in **1–2 sentences** (the thesis statement).
- **Creating reader-friendly writing is the writer's responsibility** — the reader will not dig for the point.

### BLUF Applies at Two Scales

| Scale | Unit | Term | Rule |
|---|---|---|---|
| Document | Whole text | Thesis statement | 1–2 sentences state the main idea; appears at the start |
| Paragraph | Single paragraph | Topic sentence | First sentence states the single main idea of that paragraph |

### The Topic Sentence

- The **topic sentence** is the first sentence of a paragraph (or occasionally spread across the first two sentences, or placed slightly later when transition logic demands it).
- It makes a **contract with the reader**: "this paragraph is about one thing, and I am telling you what it is now."
- Research finding cited in the handout: topic sentences **improve readers' recall** of the paragraph's content — they prime the reader's memory before the supporting details arrive.
- One paragraph = one main idea. The topic sentence names that idea. Everything else in the paragraph supports it.

### Rules / Moves

1. **Identify your bottom line first.** Before revising, locate the sentence in your draft that contains the actual conclusion or claim. That sentence becomes the topic sentence / thesis.
2. **Move it to the front.** Do not make the reader read to the end to find out what the paragraph argues.
3. **Keep one idea per paragraph.** If the topic sentence cannot summarise the whole paragraph, either the paragraph contains two ideas (split it) or the topic sentence is too narrow (broaden it).
4. **The topic sentence can be 1–2 sentences** if the main idea genuinely requires two to state clearly.
5. **Slight delay is permitted** — the topic sentence need not be the very first sentence if a transition sentence is needed, but it must appear early.
6. **Thesis statement = document-level topic sentence.** The same BLUF logic scales from paragraph to whole document.

### Before/After Examples (all from the handout)

#### Example 1 — Literature Review

| | Text |
|---|---|
| **Before** | Paragraph summarises Hyland & Hyland and Van Brimner findings, then ends: *"This shows that praise is more productive than negative feedback."* (bottom line buried at end) |
| **After** | Opens: *"Research has shown that praise is more productive than negative feedback."* Evidence follows. |
| **Move** | Extract the final summary sentence → move it to position 1. |

#### Example 2 — Experimental Results

| | Text |
|---|---|
| **Before** | Reports Filter A = 88%, Filter B = 63%, notes speed difference. No explicit bottom line stated. |
| **After** | Opens: *"Table 3 shows that Spam Filter A correctly filtered more junk emails than filter B, suggesting that Spam Filter A is the more accurate filter."* Numbers follow; speed trade-off addressed within the paragraph. |
| **Move** | Synthesise the comparison into a claim sentence → lead with it; retain the nuance (speed caveat) inside the paragraph rather than losing it. |

#### Example 3 — Argumentative / Humanities

| | Text |
|---|---|
| **Before** | Narrates Gandhi's early sexism → his shift → ends: *"He developed the then-innovative notion that women should be independent and self-reliant, which eventually became a big part of his philosophy."* |
| **After** | Opens: *"A big part of Gandhi's philosophy was the then-innovative notion that women should be independent and self-reliant."* Narrative follows as support. |
| **Move** | Take the final "payoff" sentence → place it first; the narrative becomes the evidence, not the conclusion. |

### Pattern Abstraction Across Examples

All three before/after pairs follow the same mechanical move:
1. Find the sentence that states the **interpretive claim or conclusion** (often the last sentence in the "Before" version, or entirely absent).
2. **Move it — or write it explicitly — as the first sentence.**
3. Re-read: does every remaining sentence in the paragraph support that opening claim? If not, restructure.

## Key Verbatim Anchors

- p. 1: *"Bottom Line Up Front"*
- p. 1: *"Creating reader-friendly writing is the writer's responsibility!"*
- p. 1: *"In the topic sentence, the reader expects to find the single main idea of your paragraph: the bottom line."*
- p. 1: *"studies suggest that topic sentences prepare readers for the paragraph so well that they improve readers' recall of the paragraph's content."*
- p. 1: *"In this paragraph, the writer has made a contract with the reader that the single main idea of the paragraph will be about homeowners' concern with asbestos."*
- p. 1: *"If need be, your topic sentence can be more than one sentence or appear slightly later in the paragraph."*

## How a Writing Agent Should Use This

**Agent:** Layer-0 Generic-Writing-Agent (and inherited by Layer-1a Paper-Writing-Agent and Layer-1b Proposal-Writing-Agent).

**Operations and concrete moves:**

| Operation | How to Apply BLUF |
|---|---|
| **Drafting a paragraph** | Write the supporting evidence first if that helps thinking, then identify the claim it establishes, then move that claim to sentence 1 before filing the paragraph. |
| **Revising a draft** | For each paragraph: read the last sentence and the first sentence. If the last sentence is more informative than the first, swap them (or rewrite the first). |
| **Writing a Results paragraph** | Never report numbers first. Lead with what the numbers mean: *"Method X outperforms baseline Y on metric Z."* Then give the numbers. |
| **Writing an Abstract or Introduction** | Apply BLUF at document scale: the first 1–2 sentences must state the paper's bottom line (contribution / finding), not the background. |
| **Writing a Lit Review paragraph** | Do not list citations first. State the synthesis claim first (*"Prior work has established that…"* or *"There is no existing method that…"*), then cite evidence. |
| **Audit pass** | Scan every paragraph: does sentence 1 state a claim? Or does it state a fact, cite a paper, or describe a procedure? If the latter, find the claim and move it forward. |

**Diagnostic signal:** If you have to read to the end of a paragraph to understand what it argued, the topic sentence is missing or misplaced. Fix before submission.

## Connection to SiS / CTPC

BLUF is the paragraph-level implementation of the same principle that governs CTPC paper structure: **lead with the architectural decision, then justify it.** In SiS papers and proposals:

- **Abstract and Introduction**: the hard-constraint / physics-as-architecture claim must appear in sentence 1–2, not after motivating the problem at length.
- **Method paragraphs**: each paragraph about a CTPC component (e.g., the Cholesky parameterisation for PSD covariance, the dissipation block) should open with the architectural choice as a claim — *"We enforce positive-definiteness as a hard constraint via Cholesky parameterisation"* — and then explain how and why.
- **Results paragraphs**: lead with the interpretive claim (*"The CTPC corrector reduces along-track position error by X% versus SGP4 baseline"*), not with table citations.
- **Proposal sections**: funders and reviewers read selectively. If the SiS value proposition is not in the first sentence of each section, it may not be read.

## Connections

- [[CMU-Concise-Sentences]] — conciseness at the sentence level; BLUF determines which sentence leads, conciseness determines how lean that sentence is
- [[CMU-Known-New-Information-Flow]] — BLUF places the new/key claim up front; Known-New governs how subsequent sentences chain from it
- [[CMU-Subject-Verb-Separation]] — the topic sentence must itself be well-constructed; avoiding subject-verb separation keeps the claim readable
- [[CMU-Starter-Phrases]] — provides ready-made openers for topic sentences in academic genres
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — stress position theory: Gopen & Swan argue the end of a sentence is the stress position; BLUF argues the first sentence of a paragraph is the stress position — complementary, not contradictory
- [[Schimel-Writing-Science]] — story structure (OCAR / ABDCE); BLUF is the micro-scale expression of leading with the answer
- [[Williams-Bizup-Style]] — topic and stress positions in sentences; directly reinforces BLUF at sentence scale
- [[CMU-Abstracts]] — document-level BLUF application
- [[CMU-IMRD-Structuring]] — section-level BLUF in IMRaD structure
- [[Langley-Crafting-ML-Papers]] — ML paper conventions; BLUF applies to every Results and Method paragraph

## Open Questions

- At what granularity does BLUF become counterproductive? (e.g., suspense-building in narrative sections of a proposal context section — is there a legitimate reason to defer the bottom line?)
- How does BLUF interact with the Known-New contract? If the topic sentence is entirely "new" to the reader (no scaffolding), does leading with it cause confusion rather than clarity?
- Is there a domain norm in SDA / astrodynamics venues (AAS, AIAA) that tolerates burying the bottom line more than machine-learning venues (ICML, NeurIPS)?

## Sources

- `raw/writing/generic/CMU_bluf-topic-sentence.pdf` — full handout, 2 pages. Carnegie Mellon University Student Academic Success Center, Communication Support.
  - p. 1: BLUF definition, topic sentence definition, recall research finding, asbestos paragraph example, flexibility note
  - p. 2: Three before/after examples (literature review, experimental results, argumentative humanities)
