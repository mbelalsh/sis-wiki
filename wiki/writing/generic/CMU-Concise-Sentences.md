---
title: CMU — Creating Concise Sentences
tags: [writing, generic, concision, wordiness, sentence-level, style]
sources:
  - raw/writing/generic/CMU_creating-concise-sentences.pdf
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: no
---

# CMU — Creating Concise Sentences

## One-Line Intuition
Find the real agent and action in a wordy sentence, move them to the front, then cut everything else.

## Why This Matters
Technical and academic writers pack dense information into single sentences, often burying the main point under stacked prepositions, weak "to be" verbs, and nouns that should be verbs. Readers miss ideas not because the ideas are unclear but because sentence structure obscures them. A systematic two-step method — identify the wordiness symptoms, then treat them — recovers clarity without losing content. The CMU handout demonstrates a 41% word-count reduction on a real sentence while retaining every essential idea.

## Core Content

### The Two-Step Concision Method

**Step 1 — IDENTIFY:** Locate the three worst offenders in the sentence.
**Step 2 — TREAT:** Place agent and action together at the front; prune the rest.

---

### Step 1: Identify the Three Worst Offenders

#### 1. Prepositional Phrases (box them)
- Prepositions are small words (`of`, `for`, `on`, `by`, `from`, `to`, `in`) showing relationships between nouns and other sentence elements.
- When multiple prepositions appear in succession, readers must stack relationship after relationship in working memory before reaching the main verb.
- Example of stacked prepositions from the handout: *"from Java to Singapore by the British Royal Navy"* — three prepositions in a row, three relationships to hold simultaneously.
- **Signal:** more than one or two prepositions in quick succession is a wordiness flag.

#### 2. "To Be" Verbs (circle them)
- Forms: *is, are, was, were, be, been, being.*
- "To be" verbs act as glue, holding together long noun phrases and subordinate clauses that could instead be expressed as a single strong verb.
- They frequently introduce nominalizations (see below) and passive constructions.
- Example: *"was the dominant factor in ensuring"* → replace with *"ensured"* or *"ensuring"* (one word versus five).
- **Signal:** any "to be" verb followed by a noun phrase or adjective string is a candidate for replacement.

#### 3. Nominalizations (underline them)
- A nominalization converts a verb or adjective into a noun: *shift → shifting, migrate → migration, decide → decision, differ → difference.*
- Noun forms require additional supporting words (articles, prepositions, auxiliary verbs) to function grammatically, inflating word count and weakening the sentence's drive.
- Example from handout: *"a westward shifting"* (noun) → *"shifted westward"* (verb). The verb form eliminates the article *a* and allows the sentence to move forward without a relative clause.
- **Signal:** abstract nouns ending in *-tion, -ment, -ance, -ence, -ity, -ing* used as sentence subjects or objects are almost always nominalizations.

---

### Step 2: Treat — Two Moves Only

#### Move A: Place Agent + Action Together at the Beginning
- Every sentence has a central **agent** (main noun / subject that acts) and **action** (main verb).
- **Rule:** agent and action belong together, at the start, before any additional details.
- Passive voice hides the agent or implies it — passive constructions are therefore a wordiness risk.
- **Procedure:**
  1. Decide what the sentence is really about (choose your agent).
  2. Find or construct the verb form of the action (de-nominalize if needed).
  3. Open the sentence: `[Agent] [verb]...`
- Example from handout:
  - Wordy opening: *"The migration away from Java to Singapore by the British Royal Navy..."* — nominalization (*migration*) as subject, agent buried after a preposition (*by the British Royal Navy*).
  - Treated opening: *"The British Royal Navy migrated..."* — agent first, active verb second.

#### Move B: Prune Unnecessary Words
- After fixing the opening, return to the boxed prepositions, circled "to be" verbs, and underlined nominalizations.
- Cut or convert each one:
  - Delete redundant prepositional phrases that restate information already implied by the verb.
  - Replace "to be" + noun/adjective with a single strong verb.
  - Convert nominalization back to its verb or adjective form; eliminate the supporting apparatus that came with the noun form.
- Retain all essential content; discard only the scaffolding that was propping up weak constructions.

---

### Worked Example (Full Walkthrough)

**Original sentence (41 words):**
> *The migration away from Java to Singapore by the British Royal Navy, which was a westward shifting of their base of power in Southeast Asia, was also the dominant factor in ensuring the Dutch were able to control Indonesia for decades.*

**Diagnosis:**
| Type | Instances found |
|---|---|
| Prepositions | *away from, to, by, of, of, in, in, for* |
| "To be" verbs | *was* (×2), *were* |
| Nominalizations | *migration, shifting* |

**Treatment — Move A (agent + action first):**
> *The British Royal Navy migrated...* — agent (*British Royal Navy*) + de-nominalized verb (*migrated*) placed at front.

**Treatment — Move B (prune):**
- *"away from Java to Singapore"* → retained (essential location detail; prepositions here are load-bearing).
- *"which was a westward shifting of their base of power in Southeast Asia"* → *"shifting their Southeast Asian power base westward"* (relative clause + nominalization + "to be" → participial phrase + verb).
- *"was also the dominant factor in ensuring"* → *"ensuring"* (five words → one).
- *"the Dutch were able to control"* → *"the Dutch controlled"* ("to be" + adjective + infinitive → simple past).

**Revised sentence (24 words, 41% shorter):**
> *The British Royal Navy migrated from Java to Singapore, shifting their Southeast Asian power base westward and ensuring the Dutch controlled Indonesia for decades.*

All essential content preserved; none of the wordiness scaffolding preserved.

---

### Summary Table of Techniques

| Symptom | Diagnosis signal | Treatment |
|---|---|---|
| Stacked prepositions | ≥2 prepositions in quick succession | Cut redundant ones; restructure with verbs |
| "To be" verb | *is/was/were* + noun phrase or adjective | Replace with a single strong active verb |
| Nominalization | Abstract noun (*-tion, -ment, -ing*) as subject/object | Convert back to verb/adjective form |
| Buried agent | Agent appears after a preposition (passive) | Move agent to sentence subject position |
| Delayed action | Verb appears far from subject | Open with agent + verb; push details after |

## Key Verbatim Anchors

- p. 1: *"Prepositions... require readers to stack different relationships in their heads, especially when multiple prepositions appear in succession."*
- p. 1: *"'To be' verbs like is and was often glue together strings of unnecessary words."*
- p. 1: *"Nominalizations turn adjectives and verbs into nouns... when we use adjectives or verbs in place of nouns, it often eliminates the long, circuituous phrases we see in our example sentence."*
- p. 2: *"Clear sentences usually start with the agent and action together before including additional details."*
- p. 2: *"Voilà! We have a clearer, sleeker sentence retaining all essential content in just over half the words!"* [41 → 24 words, 41% reduction]
- p. 2: *"Be careful, though! Sometimes we only imply agents, especially when using passive voice."*

## How a Writing Agent Should Use This

**Agent:** Layer-0 Generic-Writing-Agent (sentence-level revision pass).

**When to invoke:** During any revision or rewrite operation on a draft sentence — especially when a sentence exceeds ~25 words, or when the subject is an abstract noun, or when the verb is a form of "to be."

**Concrete moves for the agent:**

1. **Scan for wordiness symptoms first** — do not rewrite blindly. Mark prepositions (box), "to be" verbs (circle), and nominalizations (underline) before touching prose.
2. **Identify agent and action** — ask: *who or what is doing the thing that matters here?* If the answer is buried after a preposition or implied by passive voice, that is the primary fix.
3. **Open with agent + active verb** — restructure the sentence opening before pruning the middle.
4. **Prune in order:** nominalizations first (each one removed often eliminates its surrounding prepositions and "to be" verbs automatically), then residual "to be" verbs, then stacked prepositions.
5. **Verify content preservation** — after cutting, confirm every essential idea from the original still appears. The goal is 41%-type reduction *with zero information loss.*
6. **Apply at the paragraph level** — run the same scan on every sentence; a paragraph of concise sentences compounds clarity.

**Integration with other CMU handouts:**
- Pair with [[CMU-BLUF-Topic-Sentence]]: concise sentences + BLUF topic sentence = maximum paragraph clarity.
- Pair with [[CMU-Known-New-Information-Flow]]: once sentences are concise, check that the known→new progression is preserved — pruning sometimes disrupts the information chain.
- Pair with [[CMU-Subject-Verb-Separation]]: the agent+action rule here and the subject-verb proximity rule there are two expressions of the same principle — keep subject and verb close and early.

## Connection to SiS / CTPC

SiS research writing (paper submissions, grant proposals, technical reports) carries a high density of multi-part concepts per sentence: coordinate frames, constraint types, architectural layers, probabilistic correctors. The wordiness patterns flagged here — nominalized *"propagation of uncertainty"* instead of *"propagates uncertainty"*; passive *"dissipation is enforced by the architecture"* instead of *"the architecture enforces dissipation"* — are exactly the patterns that appear when technical writers describe complex systems. Applying this handout's two-step method to CTPC paper drafts will help reviewers reach the physics-as-architecture argument faster, reducing the cognitive load of parsing sentence structure so reviewers spend their attention on evaluating the ideas. The agent+action rule also enforces a useful discipline for SiS writing specifically: put the architectural component (the agent) first, and the constraint it enforces (the action) immediately after — e.g., *"The Cholesky parameterization guarantees positive-definiteness"* rather than *"Positive-definiteness is guaranteed through the use of a Cholesky parameterization."*

## Connections

- [[CMU-BLUF-Topic-Sentence]] — BLUF principle operates at sentence-opening level; pairs directly with agent+action rule
- [[CMU-Known-New-Information-Flow]] — information flow across sentences; concision must preserve the known→new chain
- [[CMU-Subject-Verb-Separation]] — same underlying principle: keep subject and verb proximate and early
- [[CMU-Starter-Phrases]] — starter phrases to avoid are often nominalizations or "to be" constructions
- [[Williams-Bizup-Style]] — Williams and Bizup's *Style* develops the same agent/action and nominalization analysis at book length; this handout is the two-page distillation
- [[Schimel-Writing-Science]] — Schimel's OCAR and paragraph-level argument structure; concise sentences are the sentence-level complement
- [[Strunk-White-Elements-Style]] — Strunk Rule 17 ("Omit needless words") is the one-sentence version of this entire handout
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — Gopen & Swan's stress position analysis assumes sentences are already concise; apply this handout first, then Gopen-Swan
- [[Langley-Crafting-ML-Papers]] — ML paper writing; sentence-level concision applies directly to Methods and Results sections

## Open Questions

- The handout treats all prepositions as suspects, but some prepositional phrases are genuinely load-bearing (the retained *"from Java to Singapore"* in the example). Is there a principled test for distinguishing necessary from unnecessary prepositions beyond content-preservation checking?
- The 41% reduction figure is striking — is there empirical evidence for an optimal sentence length in technical/scientific writing, or is length reduction purely a proxy for structural clarity?
- The handout focuses on single sentences. How does the agent+action rule interact with paragraph-level cohesion when the agent changes across sentences — does front-loading every sentence's agent disrupt the known→new flow?

## Sources

- `raw/writing/generic/CMU_creating-concise-sentences.pdf` — Carnegie Mellon University Student Academic Success Center, Communication Support. 2 pages.
  - p. 1: Identify section — prepositions, "to be" verbs, nominalizations; annotated example sentence.
  - p. 2: Treat section — agent+action rule, prune unnecessary words; full before/after worked example with word counts (41 → 24 words).
