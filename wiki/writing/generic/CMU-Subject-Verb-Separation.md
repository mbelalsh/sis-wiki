---
title: CMU Subject–Verb Separation
tags: [writing, generic, sentence-structure, clarity, subject-verb, cmu-handout]
sources:
  - raw/writing/generic/CMU_subject-verb-separation.pdf
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: no
---

# CMU Subject–Verb Separation

## One-Line Intuition
Readers expect the verb to appear immediately after the subject; anything wedged between them forces the reader to hold the subject in working memory while parsing the interruption, making the sentence unnecessarily hard to follow.

## Why This Matters
Scientific and technical prose routinely buries verbs under stacked qualifiers, participial phrases, and parenthetical asides — often because the writer is trying to be precise by front-loading every caveat. The result is sentences that are grammatically correct but cognitively expensive. This handout (adapted from Gopen & Swan) names the specific structural flaw so writers can diagnose and fix it in revision.

## Core Content

### The Problem
When **interrupting material** separates the subject from its verb, readers must:
1. Identify the subject.
2. Hold the subject in memory across the interruption.
3. Find the delayed verb.
4. Re-process the interrupting phrase in light of the subject–verb relationship now finally established.

Each extra clause between subject and verb adds one more cognitive load unit. For dense technical writing — where every noun is already unfamiliar — this load compounds quickly.

**Diagnostic rule:** Find the grammatical subject of a sentence. Find its main verb. Count the words between them. If the gap is more than a few words, the sentence is a candidate for revision.

### Before/After Examples (verbatim from handout)

| # | BEFORE (separated) | AFTER (close) |
|---|---|---|
| 1 | *Precipitation* in the form of crystalline water ice, consisting of snowflakes that fall from clouds, **is** called snow. | *Snow* **is** precipitation in the form of crystalline water ice that consists of snowflakes that fall from clouds. |
| 2 | The *meeting*, inconveniently scheduled at 9 a.m., causing many commuters to become delayed and frustrated, **was postponed**. | The *meeting* **was postponed**, since its inconvenient scheduling at 9 a.m. caused many commuters to become delayed and frustrated. |
| 3 | A *variety* of measures to quantify happiness, some more effective and accurate than others, **were used**. | A *variety* of measures **were used** to quantify happiness, some more effective and accurate than others. |

### Fix Strategies

Three moves cover virtually every case:

1. **Reorder: put the simplest form of the subject at the front, verb immediately after.**
   Move all qualifying material to a dependent clause or phrase *after* the verb.
   - Before: *Precipitation* … consisting … **is** called snow.
   - After: *Snow* **is** precipitation … that consists …

2. **Demote the interruption to a trailing clause or separate sentence.**
   The interrupting content is usually still important — do not delete it; move it to where it won't block the subject–verb bond.
   - Before: The *meeting*, inconveniently scheduled … causing … **was postponed**.
   - After: The *meeting* **was postponed**, since … caused … (the reason clause trails safely).

3. **Promote the most concrete noun to subject position.**
   Often the interruption exists because the writer chose an abstract or process noun as subject (e.g., "A variety of measures … were used"). Asking "what actually did the thing?" surfaces a better subject and collapses the gap.
   - Before: A *variety* … to quantify happiness, some more effective … **were used**.
   - After: A *variety* … **were used** to quantify happiness, some more effective … (the infinitive phrase moves to post-verb position).

### Summary Rule
> **The subject and verb should be as close together as possible in a sentence.**

## Key Verbatim Anchors

- "When interrupting material separates the subject and the verb, readers may struggle to understand the relationship between the subject and verb, making the sentence unclear." (p. 1)
- "The Subject and verb should be as close together as possible in a sentence." (p. 1)
- Handout subtitle: "Adapted from Gopen and Swan 'The Science of Scientific Writing'" (p. 1)

## How a Writing Agent Should Use This

**Agent:** Layer-0 Generic-Writing-Agent (sentence-level revision mode).

**Trigger:** Any time the agent is asked to revise prose for clarity, or when a sentence has more than ~6 words between subject and verb.

**Concrete moves:**
1. *Scan pass* — locate every sentence; identify subject and main verb; flag sentences where ≥5 words intervene.
2. *Triage* — ask: is the interrupting material (a) a qualifying prepositional phrase, (b) a participial phrase, (c) a parenthetical aside, or (d) a stacked list? Each has a preferred fix above.
3. *Rewrite* — apply the three strategies: reorder, demote to trailing clause, or promote a concrete noun to subject.
4. *Check* — verify the rewritten sentence preserves meaning; confirm that demoted content is not lost.

**In paper-writing (Layer 1a):** Apply to Methods sentences where experimental setup is described (these routinely stack modifiers between the subject "We" or a technique name and the verb "used/evaluated/trained"). Apply also to Abstract sentences where "A model that … trained on … evaluated with … achieves…" is common.

**In proposal-writing (Layer 1b):** Apply to Specific Aims and Background sentences, where writers often front-load rationale before the verb, causing reviewers to lose the thread.

## Connection to SiS / CTPC

SiS paper writing (CTPC, SWM, SDA system descriptions) involves dense technical sentences with multi-word noun phrases as subjects (e.g., "The Cholesky-parameterized covariance correction module, trained jointly with the physics predictor, …"). These are high-risk for subject–verb separation. Applying this rule during revision of every SiS manuscript section — especially Methods and Architecture descriptions — directly reduces reviewer friction and increases the likelihood of correct technical interpretation. Clear subject–verb proximity is a hard craft constraint: it cannot be softened without losing clarity.

## Connections

- [[Gopen-Swan-Science-Of-Scientific-Writing]] — this handout is a direct adaptation of Gopen & Swan's reader-expectation framework; subject–verb separation is one of the central structural principles in that paper
- [[Williams-Bizup-Style]] — Williams' "Style: Lessons in Clarity and Grace" treats the same principle under his "actions as verbs" and "characters as subjects" rules; the two sources reinforce each other
- [[Schimel-Writing-Science]] — Schimel's "story spine" and flow principles depend on keeping subject–verb bonds tight so readers can follow the narrative thread
- [[CMU-Concise-Sentences]] — conciseness and subject–verb proximity are complementary: removing filler words often closes the subject–verb gap as a side effect
- [[CMU-Known-New-Information-Flow]] — known-new flow governs *what* to put at the front of a sentence; subject–verb separation governs *how close* the verb must be once the subject is placed; the two constraints interact at every sentence boundary

## Open Questions

- Are there sentence types where a short, deliberate interruption between subject and verb is rhetorically justified (e.g., a single appositive that serves as emphasis)? The handout does not address the minimal-interruption case.
- How does this interact with passive voice? In passive constructions the "subject" is the grammatical patient — does the same proximity rule apply, or does passive inherently loosen the constraint?
- Is there a corpus-level heuristic (mean subject–verb gap in tokens) that distinguishes acceptable from problematic technical prose? Could be operationalized as an automated lint step.

## Sources

- `raw/writing/generic/CMU_subject-verb-separation.pdf` — single page; all content is on p. 1
  - Carnegie Mellon University Student Academic Success Center, Communication Support
  - Adapted from: Gopen, G. D., & Swan, J. A. (1990). "The Science of Scientific Writing." *American Scientist*, 78(6), 550–558.
