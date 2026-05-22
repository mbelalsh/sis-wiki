---
title: CMU Known-New Information Flow (The Known-New Contract)
tags: [writing, generic, cohesion, information-flow, sentence-structure, known-new, topic-stress]
sources:
  - raw/writing/generic/CMU_improving-writing-known-new.pdf
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: no
---

# CMU Known-New Information Flow (The Known-New Contract)

## One-Line Intuition
Every sentence has two slots — the beginning (topic) should hold what the reader already knows, and the end (stress) should hold the new, important idea you want to emphasize.

## Why This Matters
Complex technical writing fails not because the ideas are wrong but because new information lands in the topic position and old or redundant information lands in the stress position — inverting what readers expect. Readers expend interpretive effort trying to connect mid-sentence surprises to prior context, and they look for significance in sentence endings that contain nothing new. The known-new contract corrects this by aligning sentence structure with reader cognition.

## Core Content

### The Two Positions of a Sentence

Every sentence divides into two functional parts:

| Position | Role | Reader expectation |
|----------|------|--------------------|
| **Topic** (sentence beginning) | Orients the reader | "What is this sentence about? How does it connect to what I just read?" |
| **Stress** (sentence end) | Delivers the payload | "This is the new, important information I should remember." |

**Example sentence with positions labeled:**

> [Topic: *Accounts of depression*] evolved after psychologists introduced [Stress: *the concepts of defeat and entrapment*].

- In the **topic position**, readers expect to understand what the sentence is about and try to connect the sentence to what they have already read.
- In the **stress position**, readers expect to see new and important ideas and information, and they focus most of their interpretive effort there.

---

### The Known-New Contract (Core Principle)

> Comprehension is increased when the **topic position** contains information that links back to what the reader already knows, and the **stress position** contains new information that the writer wants to emphasize.

This creates a chain across sentences: each sentence's stress (new idea) becomes the next sentence's topic (known idea), producing a coherent, linked flow:

```
Sentence 1:  [Known] ——————→ [NEW idea A]
Sentence 2:             [links back to A] ——→ [NEW idea B]
Sentence 3:                         [links back to B] ——→ [NEW idea C]
```

**Before (contract violated):**
> Accounts of depression evolved after psychologists introduced the concepts of defeat and entrapment. Theoretical accounts of anxiety and suicide have been implicated by these concepts. Such theories…

(The stress of sentence 1 is "defeat and entrapment"; the topic of sentence 2 is "theoretical accounts" — no link back, the chain breaks.)

**After (contract honored):**
> Accounts of depression evolved after psychologists introduced the concepts of defeat and entrapment. *These concepts* have been implicated in theoretical accounts of anxiety and suicide. *Such theories*…

(Stress → Topic → Stress → Topic: each sentence begins by echoing the end of the previous one.)

---

### What if I have multiple ideas I want to stress in a sentence?

**Rule:** Introduce just one major new idea per sentence, especially in complex or technical writing.

If your text is highly technical and you have multiple ideas worth emphasizing, split into two sentences — and reapply the known-new contract while revising.

---

### The Three-Step Editing Procedure

Apply these three steps in order to diagnose and fix any paragraph:

**Step 1 — Identify stress positions**
- Is this new information?
- Is this the most important info to emphasize?

**Step 2 — Identify topic positions**
- Is this old information that links back to the previous sentence?

**Step 3 — Rearrange**
- Put old information in the topic position.
- Put new, important information in the stress position.

---

### Worked Example A: Slipping-and-Falling Passage

**Original (broken):**
> People are injuring themselves at home, work and out in public from slipping and falling. The material of the shoe sole, the material of the floor surface that the individual is walking across, and a contaminant, like water or oil, that may decrease friction between the two materials **all contribute to slipping**.

Diagnosis:
- Sentence 1 stress: "slipping and falling" ✓ (new, important)
- Sentence 2 stress: "all contribute to slipping" ✗ — stress position does not contain new information (it merely restates the topic)
- Sentence 2 topic: "The material of the shoe sole" ✗ — topic position does not link back to "slipping and falling"

**After Step 3 (fixed):**
> People are injuring themselves at home, work and out in public from slipping and falling. **Factors contributing to slipping** include the material of the shoe sole, the material of the floor surface that the individual is walking across, and a contaminant, like water or oil.

Now: sentence 2's topic ("Factors contributing to slipping") links back to sentence 1's stress ("slipping and falling"), and the stress of sentence 2 holds the new enumerated causes.

---

### Worked Example B: Five-Year Plan Passage

**Original (broken):**
> The 5-year plan does not indicate a clearly defined commitment to long-range environmental research. For instance, the development of techniques rather than the identification and definition of important long-range issues is the subject of the plan where it does address long-range research.

Diagnosis:
- Sentence 1 stress: "long-range environmental research" ✓
- Sentence 2 topic: "the development of techniques" ✗ — does not link back to "long-range environmental research"
- Sentence 2 stress: "the plan where it does address long-range research" ✗ — stress position does not contain new information

**After Step 3 (fixed):**
> The 5-year plan does not indicate a clearly defined commitment to long-range environmental research. **Where the plan does address long-range research**, it focuses on the development of techniques rather than the identification and definition of important long-range issues.

---

### Worked Example C: Nucleoside Chemistry Passage (all six steps)

This example demonstrates all six editing steps on a highly technical passage.

**Original (broken):**
> The enthalpy of hydrogen bond formation between the nucleoside bases 2'deoxyguanosine (dG) and 2'deoxycytidine (dC) has been determined by direct measurement. dG and dC were derivatized at the 5' and 3' hydroxyls with triisopropylsilyl groups to obtain solubility of the nucleosides in non-aqueous solvents and to prevent the ribose hydroxyls from forming hydrogen bonds.

Diagnoses: subject and verb are far apart; stress position does not contain the most important information; topic position of sentence 2 does not link back.

**After Steps 3 & 4 (move most important info to stress; subject and verb close together):**
> We directly measured the enthalpy of hydrogen bond formation between the nucleoside bases **2'deoxyguanosine (dG) and 2'deoxycytidine (dC)**. dG and dC were derivatized at the 5' and 3' hydroxyls with triisopropylsilyl groups to obtain solubility of the nucleosides in non-aqueous solvents and to prevent the ribose hydroxyls from forming hydrogen bonds.

**After Step 5 (break long sentence into two):**
> We directly measured the enthalpy of hydrogen bond formation between the nucleoside bases 2'deoxyguanosine (dG) and 2'deoxycytidine (dC). dG and dC were derivatized at the 5' and 3' hydroxyls **with triisopropylsilyl groups**. **These groups** allowed us to obtain solubility of the nucleosides in non-aqueous solvents and to prevent the ribose hydroxyls from forming hydrogen bonds.

**After Steps 5 & 6 (break + add transitional phrase to show relationship):**
> We directly measured the enthalpy of hydrogen bond formation between the nucleoside bases 2'deoxyguanosine (dG) and 2'deoxycytidine (dC). dG and dC were derivatized at the 5' and 3' hydroxyls with triisopropylsilyl groups. **These groups** allowed us to obtain solubility of the nucleosides in non-aqueous solvents. **In addition, this process** prevented the ribose hydroxyls from forming hydrogen bonds.

---

### Additional Steps (4–6): Beyond Known-New

These three steps extend the core three-step procedure for highly complex writing:

**Step 4 — Move the subject and verb closer together.**
Long noun-phrase subjects separate from their verbs create reading lag and obscure structure. Reconstruct the sentence so subject and predicate are adjacent.

**Step 5 — Break apart sentences that contain too much new information.**
One new idea per sentence in technical writing. Use the known-new contract while revising the split.

**Step 6 — Add transitional phrases to indicate relationships.**

| Relationship | Phrases |
|---|---|
| Accumulation | Moreover, In addition, Also, Furthermore |
| Sequence | First, Second…, Next, After, Finally |
| Cause/effect | Consequently, Therefore, Because |
| Example | For example, For instance |
| Contrast | However, Although |

---

## Key Verbatim Anchors

- "The known-new contract suggests that comprehension is increased when the topic position of a sentence contains information that links back to what the reader already knows and the stress position contains new information that the writer wants to emphasize." (p. 1)
- "Try to introduce just one major new idea per sentence, especially if you anticipate your reader will have difficulty because of your text's complexity." (p. 1)
- "Rearrange to put old info in the topic position and new, important info in the stress position." (p. 2)

## How a Writing Agent Should Use This

**Agent:** Layer-0 Generic-Writing-Agent (and by inheritance, Layer-1a Paper-Writing-Agent and Layer-1b Proposal-Writing-Agent).

**Mode / operation — Drafting:**
- Before writing a paragraph, identify the chain of new ideas to be introduced. Assign each to a stress position; echo each in the next sentence's topic position.
- Limit each sentence to one new idea. If two new concepts must appear, split into two sentences and verify the topic-stress chain is maintained.

**Mode / operation — Revision (sentence-level):**
- Run the three-step diagnostic on every paragraph:
  1. Mark stress positions — are they new and important?
  2. Mark topic positions — do they link back?
  3. Rearrange if either check fails.
- Check subject-verb distance (Step 4). If a noun phrase spanning more than ~8 words precedes the verb, restructure.
- Check sentence length (Step 5). Sentences with multiple embedded new ideas should be split; add a transitional phrase from the Step-6 table to preserve logical relationship.

**Mode / operation — Revision (paragraph-level):**
- Verify the stress→topic chain holds across all consecutive sentence pairs in the paragraph.
- Use the Step-6 transitional-phrase table to make logical relationships between sentences explicit after splitting.

**Common failure modes to flag:**
- Stress position contains old information (restating, summarizing) — move new material to sentence end.
- Topic position introduces brand-new concept without linking back — restructure or add a bridging phrase.
- Subject and verb separated by a long modifier clause — reconstruct.

## Connection to SiS / CTPC

SiS paper sections that are hardest to read — methods dense with notation, orbital-mechanics derivations, CTPC architecture descriptions — fail precisely because new mathematical objects appear mid-sentence or in topic position before any linking context is established. Applying the known-new contract to CTPC writing means:

- **Introducing notation in stress position first**: define a new symbol (e.g., the Cholesky factor L of the covariance) at sentence end, then open the next sentence with that symbol as the topic.
- **Architecture descriptions**: each layer's output becomes the known topic of the sentence describing the next layer's operation.
- **Experiment sections**: known setup goes in topic, new result or comparison goes in stress — ensuring reviewers see the most important finding at each sentence's end.

This is especially critical for SiS grant proposals (Layer-1b) where reviewers skim: every stress position must pay off with a compelling new claim or result.

## Connections

- [[Gopen-Swan-Science-Of-Scientific-Writing]] — the same cohesion principle formulated as "old before new" with extensive scientific-prose worked examples; Gopen & Swan are the scholarly source behind this CMU handout's core principle
- [[Williams-Bizup-Style]] — Chapter on Cohesion and Coherence formalizes topic strings and stress positions at the paragraph level; directly extends this handout
- [[Schimel-Writing-Science]] — OCAR and ABDCE story structures operationalize known-new flow at the section and paper level
- [[CMU-Subject-Verb-Separation]] — Step 4 (move subject and verb closer) is the companion handout to this one; both address sentence-level readability
- [[CMU-Concise-Sentences]] — complements Step 5 (break apart overloaded sentences)
- [[CMU-BLUF-Topic-Sentence]] — BLUF (Bottom Line Up Front) is the paragraph-level analogue: topic sentence carries the known frame, body sentences execute the known-new chain
- [[Strunk-White-Elements-Style]] — Rule 17 ("Omit needless words") and the principle of emphatic sentence endings align with putting new information in stress position
- [[Krantz-Primer-Mathematical-Writing]] — discusses mathematical sentence structure; known-new applies directly to theorem/proof prose
- [[Langley-Crafting-ML-Papers]] — paper-level advice that presupposes sentence-level cohesion this handout provides

## Open Questions

- The handout focuses on sentence-to-sentence cohesion. Is there a compatible known-new analysis at the paragraph-to-paragraph level (section cohesion)? Gopen & Swan extend the principle there — worth synthesizing.
- For heavily mathematical writing (CTPC methods), the "known" material is often a symbol just defined in the same sentence (via appositive or parenthetical). Does that count as a valid topic-position link, or does the link need to span sentence boundaries? Needs explicit rule.
- Step 6's transitional-phrase list is short. A richer table keyed to SiS writing patterns (contrast of physics-as-architecture vs. soft-penalty, causal chains in derivations) would be useful to add.

## Sources

- `raw/writing/generic/CMU_improving-writing-known-new.pdf` — full handout, 4 pages (Carnegie Mellon University Student Academic Success Center, Communication Support)
  - p. 1: Topic/stress position definitions; the known-new contract; multiple-ideas rule
  - p. 2: Three-step editing procedure; Worked Example A (slipping-and-falling)
  - p. 3: Worked Example B (5-year plan); Additional Steps 4–6; transitional-phrase table
  - p. 4: Worked Example C (nucleoside chemistry) demonstrating all six steps combined
