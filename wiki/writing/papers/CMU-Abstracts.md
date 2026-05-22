---
title: CMU Guide to Writing Abstracts
tags: [writing, paper-writing, abstracts, IMRD, structure]
sources:
  - raw/writing/papers/guides/CMU_abstracts.pdf
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: no
---

# CMU Guide to Writing Abstracts

## One-Line Intuition
An abstract is a structured 150–250-word summary that lets readers judge in thirty seconds whether your paper is worth reading — every sentence maps to one of four functions: motivation, method, result, or implication.

## Why This Matters
Conference and journal reviewers read the abstract before deciding how much attention to give the rest of the paper. A poorly structured abstract obscures the result (the one thing readers actually need) and destroys first impressions before the introduction is reached. The IMRD scaffold forces writers to budget space proportionally — most people under-report results and over-explain background, which is exactly backwards.

## Core Content

### Definition and Purpose
- An abstract is a **short summary** of a research paper that communicates what the paper is about and its key takeaways.
- Primary purpose: help readers quickly decide whether they want to read the whole paper.
- Typical length: **150–250 words** for journal articles; dissertation abstracts are longer. Conferences, journals, and instructors frequently set explicit word limits — always check.

### The IMRD Structure
Abstracts follow a four-part scaffold:

| Component | Question answered | Typical space allocation |
|---|---|---|
| **Introduction** | What is the problem? Why does it matter? What is novel? | 25% |
| **Methods** | What did you do? | 25% |
| **Results** | What did you find? | 35% |
| **Discussion** | What does it mean for the field? | 15% |

The Results section is deliberately the **largest** because it is "the longest and most important section of an abstract" — it is where readers determine whether the research is useful to them (p. 1).

### Component-Level Detail

**Introduction (25%)**
- State the central problem or question being researched.
- State the goal of the project.
- Provide context or background information relevant to the specific topic.
- Summarize what has been done previously on this topic.
- Set up what makes your research **novel** — this is the novelty hook that distinguishes your work.

**Methods (25%)**
- Describe the research methods used to accomplish the goal.
- State the central research questions or the specific problem addressed.
- State the main reasons, rationale, and goals for the research.

**Results (35%)**
- Report the findings from the research.
- This is the section readers scan first to assess usefulness — make it the densest and most concrete part of the abstract.

**Discussion (15%)**
- State the implications and significance of the research or project.
- Invite the audience to think about the novelty of the work in the field.
- This section is an opening, not a conclusion — it gestures toward broader impact.

### Annotated Examples by Discipline

**Social Sciences** (Soluri 2011, on Chilean salmon aquaculture):
- *Introduction*: "The United Nations describes aquaculture as the fastest-growing method of food production, and some industry boosters have heralded the coming of a sustainable blue revolution."
- *Methods*: "This article interprets the meteoric rise and sudden collapse of Atlantic salmon in southern Chile (1980–2010) by integrating concepts from commodity studies and comparative environmental history." / "I juxtapose salmon aquaculture to twentieth-century export banana production to reveal the similar dynamics that give rise to 'commodity diseases'..."
- *Results*: "...the risks and burdens associated with commodity diseases are borne disproportionately by production workers and residents in localities where commodity disease events occur."
- *Discussion*: "Chile's blue revolution suggests that evaluating the sustainability of aquaculture in Latin America cannot be divorced from processes of accumulation."

**Sciences** (Lin et al. 2020, on botulinum toxin inhibitors):
- *Introduction*: "Botulinum neurotoxins have remarkable persistence (~weeks to months in cells), outlasting the small-molecule inhibitors designed to target them."
- *Methods*: "To address this disconnect, inhibitors bearing two pharmacophores—a zinc binding group and a Cys-reactive warhead—were designed to leverage both affinity and reactivity."
- *Results*: Multi-sentence block covering first-, second-, and third-generation inhibitor series with specific confirmation methods (X-ray crystallography, exhaustive dialysis, mass spectrometry, in vitro evaluation against C165S mutant).
- *Discussion*: "In addition to our findings, an alternative method of modeling time-dependent inhibition that simplifies assay setup and allows comparison of inhibition models is discussed."

**Humanities** (Coulson 2010, on Melville's "Benito Cereno"):
- *Introduction*: Establishes the text, the narrative device (deposition extracts), and the critical problem (defects and contradictions in those extracts).
- *Methods*: "This Article reexamines the reliability of the deposition extracts through the lens of the exaggeration, distortion, and censorship that characterize the records of historical slave rebellions."
- *Results*: "The Article argues that the parallels between the deposition extracts in 'Benito Cereno' and the unique historiographical problems raised by the records of historical slave rebellions have been largely overlooked by critics..."
- *Discussion*: "The Article also concludes that by using a deliberately defective official document to end 'Benito Cereno,' Melville provides important commentary regarding the authorship and distortion of official records in slave rebellion trials and the autonomous agency on which law, narrative, and history depend."

### Cross-Discipline Observations from Examples
- All three disciplines obey the same IMRD sequence — only vocabulary and sentence style vary.
- Sciences passive voice dominates in Methods ("inhibitors were designed to..."); social sciences and humanities use active voice ("I juxtapose...", "This Article argues...").
- Sciences Results section is the most densely concrete (specific assays, named compounds, generational series); humanities Results is the most argumentative in tone.
- Discussion in all three is one to two sentences — short, prospective, field-level.

## Key Verbatim Anchors

- "The results section is the longest and most important section of an abstract because it is where people will look to determine if your research is useful to them." (p. 1)
- "This final section of the abstract invites your audience to think about the novelty of your work in the field." (p. 1, on Discussion)
- "Most abstracts are roughly 150–250 words, but conferences, journals, and professors requesting an abstract will often set a word limit." (p. 1)
- "Abstracts help your reader understand quickly what your paper is about and whether they would want to read your whole paper." (p. 1)

## How a Writing Agent Should Use This

**Mode: Abstract drafting and critique (Paper-Writing-Agent, pre-submission pass)**

Concrete moves:

1. **Budget check first.** Count words assigned to each IMRD component in a draft abstract. If Introduction exceeds ~62 words (25% of 250) or Results is shorter than Methods, flag for rebalancing.

2. **Novelty hook audit.** Verify that the Introduction component explicitly states what has been done previously AND what makes this work novel. Missing novelty setup is the most common abstract failure in ML papers.

3. **Results density check.** The Results section should be the most concrete block — specific numbers, named architectures, quantified improvements. If it contains hedging language ("we believe," "it appears") or is vaguer than Methods, rewrite.

4. **Discussion brevity check.** Discussion should be one to two sentences maximum. If it expands into future work enumeration or limitations, cut and move that content to the paper body.

5. **Active/passive calibration.** For ML/engineering papers (sciences register), passive is acceptable in Methods ("a model was trained..."). But the Introduction should have an active hook stating the problem, and Discussion should be active and forward-looking.

6. **Template for CTPC-style ML abstracts:**
   - *Introduction (2–3 sentences)*: State the orbital propagation accuracy gap; cite SGP4 residual error problem; name the architectural novelty (physics-as-architecture, hard dissipation constraint).
   - *Methods (2 sentences)*: Describe CTPC architecture and training setup.
   - *Results (3–4 sentences)*: Report accuracy metrics (RMSE in RTN, Pc improvement, covariance calibration) with numbers.
   - *Discussion (1–2 sentences)*: Connect to real-time SDA deployment or generalization to perturbed orbits.

## Connection to SiS / CTPC

Every SiS/CTPC paper submission — conference (NeurIPS, ICML, ICLR, AAS, AIAA) or journal (JGCD, JMLR) — must pass through abstract review before submission. The IMRD scaffold directly maps onto the CTPC narrative:

- **Introduction**: SGP4 residual characterization problem + gap in physics-enforcing ML for SDA.
- **Methods**: CTPC architecture (Port-Hamiltonian corrector, Cholesky covariance block, RTN-frame decomposition).
- **Results**: Propagation accuracy, covariance realism, Pc calibration improvements over SGP4 baseline.
- **Discussion**: Implications for real-time space traffic management and catalog maintenance.

The 35% Results allocation is especially important for ML-for-SDA papers because reviewers at venues like AAS/AIAA are physics-domain readers who will not trust the method until they see the numbers. Front-loading the results space signal is not optional.

The cross-discipline examples confirm that sciences-register abstracts (the register for CTPC papers) favor passive voice in Methods and multi-clause Results sentences listing specific experimental confirmations — matching the style used in NeurIPS and ICML abstracts for physics-ML papers.

## Connections

- [[CMU-IMRD-Structuring]] — full IMRD handout cross-referenced from this guide
- [[CMU-IMRD-Examples]] — extended examples of IMRD across paper sections
- [[CMU-Establishing-Novelty-Four-Moves]] — novelty framing in Introduction, which maps directly to the 25% Introduction block in an abstract
- [[Whitesides-Writing-A-Paper]] — Whitesides also addresses abstract structure; compare his "outline first" approach with IMRD space allocation
- [[Peyton-Jones-Research-Paper]] — Peyton Jones covers abstract as the primary selling document; consistent with CMU's gating-decision framing
- [[Langley-Crafting-ML-Papers]] — ML-specific abstract conventions; check against IMRD percentages
- [[Freeman-Writing-Papers]] — Freeman's abstract advice in the context of storytelling structure
- [[Ramdas-Paper-Checklist]] — checklist includes abstract quality as a pre-submission gate

## Open Questions

1. Do ML venue abstracts (NeurIPS, ICML) deviate from the 25/25/35/15 allocation in practice? A corpus study of accepted NeurIPS abstracts would be informative for calibrating the agent's budget-check heuristic.
2. The handout addresses empirical-paper abstracts only. How should the IMRD template be adapted for theoretical/proof papers (e.g., a CTPC convergence guarantee paper with no "results" in the experimental sense)?
3. For AIAA/AAS (aerospace venues), is the passive-voice Sciences register the right default, or do those communities prefer active voice in Methods? Worth checking against published JGCD abstracts.

## Sources

- `raw/writing/papers/guides/CMU_abstracts.pdf` — full document, pp. 1–3
  - p. 1: definition, Abstract specifics, IMRD space allocation percentages, component-level descriptions
  - pp. 2–3: annotated examples (Social Sciences, Sciences, Humanities)
