---
title: Draft Section Scaffolds — Paper and Grant
tags: [synthesis, writing, paper-writing, proposal-writing, structure, sections, imrd, drafting-workflow]
sources: [raw/writing/papers/guides/CMU_imrd-structuring-your-paper.pdf, raw/writing/proposals/guides/Kraicer_Art_Of_Grantsmanship.pdf, raw/writing/proposals/guides/CMU_proposals.pdf, raw/writing/proposals/guides/Grant-Writing-Cheat-Sheet-Strategic-Language.pdf]
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: no
---

# Draft Section Scaffolds — Paper and Grant

## One-Line Intuition
Never start a paper or grant from a blank page: first drop in every section header and a bullet of *intent* under each, then write prose. This page holds the two canonical skeletons — one for a research paper, one for a grant proposal — so the structure decision is made once, here, and reused.

## Why This Synthesis Matters
The wiki already knows how every section of a paper and a grant should be structured — but that knowledge is spread across ~13 writing-craft pages ([[CMU-IMRD-Structuring]], [[CMU-Abstracts]], [[Kraicer-Art-Of-Grantsmanship]], [[CMU-Proposals]], [[Seattle-U-Grant-Writing-Cheat-Sheet]], [[GradLIFE-Proposal-As-Roadmap]], [[Langley-Crafting-ML-Papers]], and others). Until now, instantiating a draft skeleton meant reassembling it from four or five pages each time. This page is the single consolidation: copy a scaffold, fill it, write.

It does not replace the detailed pages — each scaffold row links to the page that explains *how* to execute that section. It is the index and the starting move.

## The Scaffold-First Rule
Apply to every paper or grant draft:

1. **Pick the genre** — paper → Scaffold 1; grant → Scaffold 2.
2. **Check for an imposed template** — the venue or funder may dictate sections (a NeurIPS style file, NSF PAPPG, an AFOSR BAA). If so, that template wins; use the scaffold below to *map intent onto* it, not to override it.
3. **Copy the section headers** into the draft, verbatim.
4. **Under each header, write 2–4 bullets of intent** — what this section must accomplish and what content lands here. Bullets, not prose.
5. **Review the skeleton whole** — is every contribution / aim placed exactly once? Gaps and double-placements are structural smells; fix them now, while it is cheap.
6. **Only then write prose**, section by section.

The point of the rule: structural decisions are cheap to change as bullets and expensive to change as paragraphs.

## Scaffold 1 — Research Paper (IMRD)

IMRD — Introduction, Methods, Results, Discussion — is the universal empirical-paper skeleton. Each section has one job; the jobs do not overlap.

| # | Section | Job (one line) | What goes in it | Length cue |
|---|---|---|---|---|
| 0 | Abstract | The whole paper in miniature | Why → What → How → So-what | 25/25/35/15 budget — Results biggest |
| 1 | Introduction | Make the case for the work | problem → field state → **gap** → your work as the timely fix; **no findings** | — |
| 2 | Methods | Reproducible account of what you did | procedure only; past tense, passive voice, heavy subheadings | least-read |
| 3 | Results | Deliver the news | the **8 moves**: Report (1–4) then Comment (5–8) | largest |
| 4 | Discussion | Say what it means | the **5 moves**: summary → connect → flaws → future work → implications | — |

**Results — the 8 moves** (Move 1 and Move 8 mandatory; 2–7 conditional on the data): (1) point to figure/table + main trend; (2) support with data; (3) secondary trends; (4) exceptions/unexpected outcomes; (5) explanation; (6) compare to other research; (7) support/contradict hypothesis; (8) bottom line — what the data means.

**Discussion — the 5 moves** (in order): (1) summarize main findings; (2) connect to other research; (3) name flaws in this study; (4) turn flaws into future work; (5) implications for policy/practice.

> **ML-venue adaptation.** ML papers (NeurIPS / ICML / ICLR) keep the IMRD *spine* but commonly: rename **Methods → Approach / Architecture** (and treat it as a headline contribution, not a skipped section), add a standalone **Related Work** section (placement varies — see [[GeometryNN]], [[HopCPT]]), and place **Limitations** explicitly (mid-paper or in the Conclusion). Cross-check section content against [[Langley-Crafting-ML-Papers]]'s seven mandatory content topics. Full detail: [[CMU-IMRD-Structuring]].

## Scaffold 2 — Grant Proposal

The canonical default is Kraicer's full proposal anatomy. The **seven-part Proposed Research narrative** is the heart — that is where the writing effort goes — wrapped by Title, Abstract, Budget, and back matter.

| # | Section | Job (one line) | What goes in it |
|---|---|---|---|
| 0 | Title | First impression; routes the proposal | two-part colon title — stable general part : specific modifiable part |
| 1 | Abstract / Summary | The most-read, most-scored page — **write last** | hypothesis, long-term objectives, specific aims, plan, significance, agency-mission fit |
| — | **— Proposed Research (the narrative core) —** | | |
| 2 | Hypothesis & Long-Term Objectives | The *why* | testable claim + scientific destination |
| 3 | Specific Aims | The *what* | concrete, unitary, non-compound deliverables for this funding period; priorities marked |
| 4 | Background & Significance | The *where we are* | what is known / what is not / why it is essential — the gap |
| 5 | Progress / Preliminary Studies | The *why us* | track record + actual preliminary data (turns a hundred objections into zero) |
| 6 | Research Design & Methods | The *how* | design + methods, numbered to match the Aims; controls; alternative strategies |
| 7 | Timetable | Sequencing | month-by-month milestones |
| 8 | Strengths & Weaknesses | Self-aware critique | name weaknesses + explicit mitigations — preempt the reviewer |
| — | — | | |
| 9 | Budget & Justification | Resourcing | realistic, line-item-justified; not inflated, not padded |
| 10 | Other Support / Appendices / References | Compliance | current & pending grants; required appendices only; references |

Source: [[Kraicer-Art-Of-Grantsmanship]] §3.3–3.10.

### Per-funder overlays
The seven-part narrative spine (gap → aims → significance → approach → preliminary data → weaknesses) is invariant. The *labels and packaging* change by funder — map the spine onto whatever the funder imposes:

- **Generic / dissertation** — [[CMU-Proposals]]'s six-section structure: Introduction → Background & Need → Research Question & Plan → Resources → Conclusion → References.
- **NSF** — the 12-section PAPPG checklist (Cover Sheet, Project Summary, TOC, Project Description, References Cited, Biographical Sketches, Budget & Justification, Current & Pending Support, Facilities & Equipment, Special Info, Data Management Plan, Postdoc Mentoring Plan). The seven-part narrative lives inside *Project Description*. See [[CMU-Proposals]].
- **The opening few paragraphs** — regardless of funder, draft the first two paragraphs with the Kelsky funnel: large general topic → two literature refs → "HOWEVER" gap pivot → urgency + hero → research question. See [[Seattle-U-Grant-Writing-Cheat-Sheet]].
- **Reviewer-findability** — make every solicitation evaluation criterion a labelled, scannable section. See [[GradLIFE-Proposal-As-Roadmap]].

## Paper vs Grant — the Structural Contrast
The two scaffolds are not the same shape, and confusing them is the most common structural failure:

| | Paper (IMRD) | Grant |
|---|---|---|
| Orientation | retrospective — reports completed work | prospective — argues for future work |
| Register | expository / evidentiary | persuasive |
| Centre of gravity | **Results** | **Specific Aims** + **Significance** |
| Evidence role | the findings *are* the contribution | preliminary data *supports feasibility* of a promised contribution |
| "Flaws" section | Discussion move 3, then future work | Strengths & Weaknesses, with mitigations up front |

A paper proves; a proposal promises. Keep paper-writing habits out of proposals — see [[Porter-Why-Academics-Struggle-Proposals]].

## Connection to SiS / CTPC
- **A CTPC paper** instantiates Scaffold 1: Introduction gap = orbital prediction error from unmodeled non-conservative forces; Methods/Approach = the physics-as-architecture corrector (SGP4 predictor, RTN-frame residual, Cholesky covariance layer, dissipation constraint); Results = accuracy + calibration vs the SGP4 baseline; Discussion = limitations (TLE quality, drag assumptions) → future work. The 35% Results abstract budget matters — reviewers accept on the numbers, not the physics motivation. [[CTPC-KDD-Submission]] is the live instance.
- **A SiS proposal** (NSF CAREER, AFOSR YIP, SBIR) instantiates Scaffold 2: Specific Aims = the CTPC milestone ladder; Background & Significance = the SDA conjunction-screening gap; Progress / Preliminary Studies = the KDD results + the [[PhyArch-Double-Pendulum-Benchmark]]; Strengths & Weaknesses = compute cost, data dependence, cross-regime generalization — each with a stated mitigation.
- This is writing-craft guidance — soft, advisory: `hard_constraint_possible: no`.

## Connections
- [[CMU-IMRD-Structuring]] — the full IMRD section template; Scaffold 1's source of record
- [[CMU-IMRD-Examples]] — worked examples for each IMRD section
- [[CMU-Abstracts]] — the 25/25/35/15 abstract budget in detail
- [[CMU-Establishing-Novelty-Four-Moves]] — the Introduction gap move (paper) and the "HOWEVER" pivot (grant)
- [[Langley-Crafting-ML-Papers]] — seven mandatory content topics for ML papers
- [[Ramdas-Paper-Checklist]] — section-by-section pre-submission checklist
- [[Kraicer-Art-Of-Grantsmanship]] — the full grant anatomy; Scaffold 2's source of record
- [[CMU-Proposals]] — the six-section generic structure + the NSF 12-section list
- [[Seattle-U-Grant-Writing-Cheat-Sheet]] — the Kelsky opening funnel
- [[GradLIFE-Proposal-As-Roadmap]] — section-by-section reviewer psychology
- [[Porter-Why-Academics-Struggle-Proposals]] — why paper habits break proposals
- [[Paper-Writing-Agent]] / [[Proposal-Writing-Agent]] — the agents whose DRAFT mode this scaffold seeds

## Open Questions
1. ML venues vary on Related Work placement (standalone §2 vs §1 subsection vs late) — should Scaffold 1 carry a venue sub-table, or stay venue-agnostic and defer to the exemplar pages?
2. Kraicer's seven-part narrative assumes a hypothesis-driven proposal. SiS SBIR proposals are partly commercialization-driven — does the scaffold need an SBIR variant (commercial-potential, transition plan)?
3. Should this page be wired into the DRAFT mode of [[Paper-Writing-Agent]] and [[Proposal-Writing-Agent]] as a mandatory first step, or stay a referenced resource? (Deferred — the standing-rule decision was declined for now.)

## Sources
- `raw/writing/papers/guides/CMU_imrd-structuring-your-paper.pdf` — IMRD section jobs, the 8 Results moves, the 5 Discussion moves, abstract budget (Scaffold 1)
- `raw/writing/proposals/guides/Kraicer_Art_Of_Grantsmanship.pdf` — §3.3–3.10 full proposal anatomy (Scaffold 2)
- `raw/writing/proposals/guides/CMU_proposals.pdf` — six-section generic structure + NSF 12-section checklist (overlays)
- `raw/writing/proposals/guides/Grant-Writing-Cheat-Sheet-Strategic-Language.pdf` — the Kelsky opening funnel (overlay)
- Synthesizes the wiki pages cited under Connections above.
