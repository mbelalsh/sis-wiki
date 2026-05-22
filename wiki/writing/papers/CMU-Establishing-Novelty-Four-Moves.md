---
title: CMU — Establishing Novelty with Four Rhetorical Moves
tags: [writing, paper-writing, novelty, introduction, rhetoric, Swales, CARS, gap-finding]
sources:
  - raw/writing/papers/guides/CMU_establish-novelty-four-moves.pdf
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: no
---

# CMU — Establishing Novelty with Four Rhetorical Moves

## One-Line Intuition

Every research introduction performs four rhetorical moves in sequence — significance, status quo, gap, fill — and skipping any one of them leaves readers unable to judge why the work matters or what it adds.

## Why This Matters

Reviewers and readers decide within the first paragraph whether a paper is worth their attention. The decision hinges on a single question: "Why does this work need to exist?" The four-move framework (rooted in John Swales' CARS model — Creating a Research Space — from his 1990 *Genre Analysis*) gives writers a precise, testable scaffold for answering that question. Each move has a distinct rhetorical job; each can be checked independently. For Bilal, whose papers introduce architecturally novel neural-dynamics methods into a mature orbit-propagation literature, all four moves are active obligations on every submission.

## Core Content

### The Four Moves

The handout names the overall goal as: **"show how your work is important, relevant, and new."** The four moves achieve this in order.

---

#### Move 1 — Explain the Significance

**Rhetorical job:** Tell readers — ideally readers broader than your primary technical audience — *why this problem matters at all*.

- Pitch to the widest plausible audience first. If your primary audience is astrodynamicists, the significance claim should be legible to aerospace engineers broadly, or even to the general public.
- In humanities research, significance can simply be *currency*: "people are currently debating this." In STEM, significance is usually stakes: safety, cost, physical reality, capability.
- This move establishes that the *territory* is worth occupying before any claim about what exists or what is missing.

**Signal phrases:** opening with a broad assertion of importance, scope, or consequence — no citation needed here, or a broad statistic.

---

#### Move 2 — Describe the "Status Quo"

**Rhetorical job:** Summarize what researchers currently do or believe within the *defined, limited scope* of your field.

- Review current practices, existing literature, or the state of affairs.
- Stay within the narrowed scope established at the end of Move 1 — you have already told readers the territory is important; now tell them what has been done there.
- This move is not a full related-work section. It is a compressed, accurate characterization of the dominant approach or dominant understanding. It sets up something to contrast against.

**Signal phrases:** "Recent studies have suggested…", "Scholars have been assiduous in…", "Bioplastics may provide…"

---

#### Move 3 — Identify a "Gap"

**Rhetorical job:** Show that the status quo is *incomplete, unsatisfactory, or inconclusive* and that a need exists for the gap to be filled.

- The gap can be methodological, empirical, theoretical, or domain-based (see Move 4 taxonomy below for the matching novelty types).
- The canonical gap signal is the word **"however."** It is explicitly called out in the handout as the standard pivot word. Additional signals: "yet," "still," "nevertheless," "despite X, Y has not been done."
- The gap is not a criticism of prior researchers as individuals. The handout explicitly warns: **avoid words like "neglected," "failed," or "ignored."** Frame the gap as an open problem the field has not yet reached, not as a failure by named authors.
- The positive framing template from the handout: *"While X pioneered the field of Y, my work contributes/supplements X…"*

**Signal phrases:** "However, the interpretation of these results has been complicated by…", "Yet the striking cultural intimations… have still somehow escaped sustained attention."

---

#### Move 4 — Fill That Gap with Your Present Research

**Rhetorical job:** Show that *your* research is a timely, necessary, or innovative solution that fills the gap identified in Move 3.

- This is the announcement of the paper's contribution. It must be logically tight: the gap in Move 3 must be exactly the gap your paper closes.
- The handout provides a four-way taxonomy of how research can be novel. If you are unsure which kind of novelty your paper has, locate it in this table:

| Novelty type | What it requires |
|---|---|
| **New theory or hypothesis** | Identify a shortcoming in existing theory; propose a new hypothesis to replace or extend it |
| **New solution** | Propose a solution to an existing problem or unresolved controversy; explain why your solution is *better* than existing alternatives |
| **New methodology** | Critique the methodology of prior studies; propose an improved method |
| **New domain** | Investigate a previously unstudied population, site, material, or other phenomenon |

- The CTPC / physics-as-architecture work primarily claims **new methodology** (architectural enforcement of dissipation vs. soft-penalty status quo) and, to a lesser extent, **new domain** (ML correctors for orbit propagation as a class of problem that structured physics-informed ML has not yet fully addressed).

**Signal phrases:** "We report the results of…", "we propose a device that…", "This project helps revivify…"

---

### Non-Linearity of the Moves

The four moves are **not rigidly linear**. A writer can backtrack — for example, moving from identifying a gap (Move 3) back to summarizing more status quo (Move 2) before returning to the gap. Example 3 below demonstrates a two-tier structure (broad status quo + gap, then narrow status quo + gap) before the fill.

**However:** excessive oscillation confuses readers. The trajectory should be perceivable as Move 1 → 2 → 3 → 4 even if there are internal loops.

---

### Annotated Examples (all three from the handout)

#### Example 1 — Medical Science (H. pylori / peptic ulcer)

Full text with move labels:

> *[Significance]* "Peptic ulcer disease is a chronic disease characterized by frequent recurrences."
>
> *[Status Quo]* "**Recent studies have suggested** that the eradication of Helicobacter pylori infection affects the natural history of duodenal ulcer disease such that the rate of recurrence decreases markedly (1-6)."
>
> *[Gap]* "**However,** the interpretation of these results has been complicated by the fact that several of the larger studies did not use control groups or any form of blinding (3, 5, 6). In addition, studies of the effect of H. pylori eradication in patients with gastric ulcer have not been done."
>
> *[Fill the Gap]* "**We report** the results of a randomized, controlled trial in which we evaluated the effect of therapy designed to eradicate H. pylori on the pattern of ulcer recurrence in patients with duodenal or gastric ulcer."

Move structure: clean linear 1-2-3-4 in four sentences. Gap signals two sub-gaps (methodological weakness + unstudied population). The fill names the study design directly.

---

#### Example 2 — Engineering / Materials Science (bioplastics / PLA)

Full text with move labels:

> *[Significance]* "Although plastic has revolutionized modern life, the environmental impact of traditional petroleum plastics is staggering."
>
> *[Status Quo]* "Bioplastics may provide a sustainable alternative to petroleum plastics because they use fewer fossil fuels in production and reduce greenhouse gas emissions as they biodegrade. One particularly promising bioplastic is polylactic acid (PLA). PLA resembles traditional plastic and can be processed on equipment already used for petroleum plastics."
>
> *[Gap]* "**However,** the commercial viability of PLA is currently limited because it is only compostable in industrial facilities and cannot be mixed with other recyclable materials [1, 2]."
>
> *[Fill the Gap]* "To make PLA more commercially viable, **we propose** a device that composts PLA and other bioplastics within a home composting environment [3]. Such a device, we argue, would encourage the production of more sustainable and economic bioplastics."

Move structure: linear. The significance is framed as contrast ("Although… staggering") — a compressed Move 1 that immediately narrows to the domain. The fill claims a **new solution** novelty type and explains *why it is better* (home composting vs. industrial-only).

---

#### Example 3 — Humanities (British colonial state)

Full text with move labels:

> *[Significance]* "Research on the British colonial state has been thriving due to its modern day implications."
>
> *[Broad Status Quo & Gap]* "Scholars have been assiduous in suggesting theories of its nature and relationship to the legal and political structures of Western imperial modernity. However, historians have generally limited their inquiries to the 'fiscal-military state,' as John Brewer famously dubbed it."
>
> *[Narrow Status Quo & Gap]* "Scholars generally agree that this imperial state helped forge some of the unique capacities of modern statehood and contributed to British domination in the eighteenth-century war for trade and empire. Yet the striking cultural intimations and practices of state-building have still somehow escaped sustained attention."
>
> *[Fill the Gap]* "This project helps revivify a cultural perspective on the arts and strategies of colonial state-making in the eighteenth century by examining the practices of governance in three British empire frontiers — Fort Marlborough (Sumatra), St. Helena, and Jamaica."

Move structure: non-linear / two-tier. Status quo and gap are visited *twice* at different levels of specificity — first at the broad level of what the colonial-state literature covers, then again at the narrower level of *cultural* state-building specifically. This is legitimate and the handout uses it as an explicit example of the non-linearity principle. The novelty type is **new domain** (cultural perspective on state-building vs. the fiscal-military focus).

---

### Summary Table

| Move | Rhetorical job | Canonical signal word/phrase | Common failure |
|---|---|---|---|
| 1 — Significance | Why the territory matters | Opening assertion of stakes | Too narrow (pitching only to specialists); omitted entirely |
| 2 — Status Quo | What currently exists / is done | "Recent studies have…", "X has been shown…" | Too vague; just a name-drop list with no synthesis |
| 3 — Gap | What is missing, wrong, or incomplete | **"However,"** "Yet," "Nevertheless" | Negative framing ("X failed to…") instead of positive gap |
| 4 — Fill | What this paper does to close the gap | "We report / propose / investigate…" | Gap and fill are mismatched — paper doesn't close the gap it named |

## Key Verbatim Anchors

All from the single-page handout (p. 1):

- **On Move 1 audience:** "your audience for this 1st move should be broader than your primary target audience (if possible, why the general public would care)."
- **On the gap signal word:** "A common way to signal a gap is the word 'however.'"
- **On positive framing:** "try to avoid utilizing words like 'neglected,' 'failed,' or 'ignored' when critiquing researchers in your field. Instead, frame your contribution in positive terms: 'While X pioneered the field of Y, my work contributes/supplements X…'"
- **On non-linearity:** "these moves do represent the overall trajectory of an introduction to a research article, they are not necessarily linear. A writer can backtrack at any point… However, excessive movement back and forth between moves can confuse readers."
- **Source attribution in handout footnote:** "Adapted from Swales, John. *Genre Analysis: English in Academic and Research Settings.* Cambridge: Cambridge UP, 1990. Although Swales' research is focused on scientific writing, these 'Swales moves' are found in almost all academic disciplines as well as in technology development scenarios."

## How a Writing Agent Should Use This

**Which agent:** Paper-Writing-Agent (Layer 1a).

**Which modes:**

- **Introduction drafting mode:** Apply all four moves in sequence. For any draft introduction, audit paragraph-by-paragraph: which move does each paragraph perform? Is every move present? Is Move 4 logically connected to the exact gap named in Move 3?
- **Revision / feedback mode:** When Bilal pastes a draft introduction, annotate each sentence or paragraph with its move label (Significance / Status Quo / Gap / Fill). Flag: missing moves, inverted order (Fill before Gap), gap-fill mismatch, negative gap framing.
- **Novelty diagnosis mode:** When Bilal is unsure what is novel about a paper, apply the four-type taxonomy from Move 4: new theory, new solution, new methodology, new domain. Identify which one(s) apply and make sure Move 3's gap is framed to match.

**Concrete moves for the agent:**

1. For CTPC-related papers: Move 1 = SDA collision avoidance stakes (public safety, LEO congestion). Move 2 = SGP4 + covariance propagation as status quo; soft-penalty PINN approaches as the ML status quo. Move 3 = SGP4 error accumulation uncorrected by hard architectural constraints; existing ML correctors use soft penalties (the gap). Move 4 = CTPC's hard-constraint dissipative architecture as the fill.
2. For any paper: check that the gap signal word ("however") or equivalent is present and that it pivots *from* a genuine status-quo claim, not from a significance claim. A common error is using "however" to pivot directly from significance to fill, skipping the status quo entirely.
3. For multi-gap introductions: use Example 3's two-tier structure (broad gap, then narrow gap) when the paper addresses a topic where the broad field has gaps but the specific sub-field also has its own independent gap.

## Connection to SiS / CTPC

The four-move framework maps directly onto the rhetorical challenge Bilal faces with every SiS/CTPC paper submission:

- **Move 1 — Significance:** Space debris and LEO congestion are high-stakes public problems. The significance claim is genuinely broad and can be pitched beyond the astrodynamics community.
- **Move 2 — Status Quo:** SGP4 + TLE-based covariance propagation is the operational status quo. Recent ML-for-orbits work (neural ODEs, PINNs) is the ML status quo. Both are well-documented and citable.
- **Move 3 — Gap:** Current ML correctors do not enforce dissipation as a hard architectural constraint — they impose it via soft loss penalties. This is a *methodology* gap, matching the handout's "new methodology" novelty type exactly. The word "however" should pivot from the ML-corrector status quo to this gap.
- **Move 4 — Fill:** CTPC fills the gap with architecture-level dissipation enforcement (Ḣ ≤ 0 baked into the Port-Hamiltonian block, not penalized). The fill must reference the gap precisely — "While prior correctors penalize energy non-compliance, CTPC encodes the dissipation condition as an architectural hard constraint."

The four-move structure also directly applies to how the CTPC introduction should *not* be written: Bilal should not describe prior ML correctors as having "ignored" physics or "failed" to enforce constraints. The positive framing ("while X established the ML-corrector paradigm, CTPC extends it by…") is both more accurate and more reviewer-friendly.

## Connections

- [[CMU-IMRD-Structuring]] — the four moves govern the Introduction section of the IMRD structure; these two guides are directly paired
- [[CMU-IMRD-Examples]] — annotated full-paper IMRD examples that instantiate the four moves in complete introductions
- [[CMU-Literature-Reviews]] — Move 2 (Status Quo) is a compressed literature review; this guide expands on how to synthesize prior work
- [[CMU-Lexical-Bundles-Novelty]] — the signal phrases for each move (especially the gap signal "however") are instantiated as lexical bundles in this guide
- [[CMU-Abstracts]] — the abstract must compress all four moves into ~150–250 words; the four-move framework is the underlying structure of every structured abstract
- [[Langley-Crafting-ML-Papers]] — Langley's advice on framing ML contributions assumes a gap-filling structure; the four moves make that structure explicit
- [[Peyton-Jones-Research-Paper]] — Peyton Jones' "what is the problem?" and "what is the contribution?" framing maps to Moves 3 and 4 respectively
- [[Whitesides-Writing-A-Paper]] — Whitesides' outline-first method is most naturally applied after the four moves have been identified, not before
- [[Freeman-Writing-Papers]] — Freeman's advice on positioning a paper in the literature addresses the same gap-territory logic as Moves 1–3
- [[CMU-BLUF-Topic-Sentence]] — after the four moves establish the introduction, BLUF topic-sentence structure governs how each subsequent paragraph leads

## Open Questions

1. **Nested non-linearity:** Example 3 demonstrates two-tier status-quo-and-gap. For a paper like CTPC that spans both an astrodynamics literature and an ML literature, is it correct to run a broad status-quo/gap for SDA and then a narrow status-quo/gap for physics-informed ML? How many tiers before readers are confused?
2. **Move 1 length in ML venues:** NeurIPS / ICML introductions often compress or omit the broad-significance move entirely and open directly with the ML status quo. Is this a genre divergence, or a failure mode that reviewers have simply normalized?
3. **Gap vs. limitation:** Move 3 frames the gap as a property of the field, not a flaw in a specific paper. When a specific prior paper is the direct prior work (e.g., a previous CTPC version), how does the gap framing stay positive without misdirecting readers about what exactly is being improved?
4. **Move 4 taxonomy completeness:** The handout gives four novelty types. Are there others that appear in physics-informed ML? For example, "new training signal" (using a physics-derived loss term as a supervision signal) or "new theoretical guarantee" (proving convergence or stability) — do these fit inside the four types or do they require a fifth?

## Sources

- `raw/writing/papers/guides/CMU_establish-novelty-four-moves.pdf` — pp. 1–2 (complete document; two-page handout)
  - p. 1: four-move definitions, positive-framing note, non-linearity caveat, Swales footnote
  - p. 2: three annotated examples with move labels (H. pylori, bioplastics/PLA, British colonial state)
