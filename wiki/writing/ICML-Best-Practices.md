---
title: ICML 2022 Paper Writing Best Practices — Prescriptive Checklist
tags: [writing, checklist, icml, prescriptive, reproducibility, experimental-design]
sources:
  - raw/writing/guides/Paper Best Practices.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# ICML 2022 Paper Writing Best Practices — Prescriptive Checklist

## One-Line Intuition

The ICML PC's prescriptive checklist — modeled after AAAI 2021 +
Joelle Pineau's reproducibility checklist — that authors should
self-evaluate against pre-submission. Eleven sections, ~40 sub-items, with
deep reproducibility focus on the experimental section.

## Origin Statement (Verbatim)

> "The following checklist, modeled after the AAAI 2021 paper submission
> checklist, inspired by similar previous checklists (in particular, Joelle
> Pineau's checklist), may also be useful. When writing the paper, think
> of whether your paper satisfies these points. **The main point is that we
> expect ICML papers to advance the knowledge of the field. Write papers
> that achieve this.**"

## Document's Recommended Starter

> "As a starter, we highly recommend Bill Freeman's slides on how to write
> good papers. To state the obvious, one writes a paper for others to read
> it. **The first advice is that the paper should be written so that it
> finds readers (within our field) who will care about it.** A specific
> type of reader is the reviewer (and the other PC members). **In fact,
> their job is to model the typical reader.**"

This is the meta-frame: optimize for the reader; the reviewer is a *model*
of the reader. Pair this with [[ICML-2022-Review-Form]] (the reviewer-side
ground truth).

## The Eleven Sections (Verbatim Numbering)

### Section 1-7: Core writing axes

**1. Claims, problems** — "The paper clearly states what claims are being
made, and in particular it clearly describes the problem addressed."

**2. Soundness** — "The paper clearly explains how the results substantiate
the claims."

**3. Honesty** — "The paper explicitly identifies limitations or technical
assumptions."

**4. Novelty, significance** — "If the paper is studying a new problem, the
problem is well motivated and interesting for the readers in our community,
at the time the paper is written."

**5. Self-containment** — "The paper is essentially self-contained: An
expert reader (reader familiar with the topic) has a good chance to
understand the paper."

**6. Context** — "The paper discusses prior work and, in particular, how it
is related to the contributions made. Future readers will be able to
understand why this paper was targeting the specific problems that it
described."

**7. Background pointers** — "The paper helps readers not familiar with all
the background necessary to understand the paper by citing appropriate
review articles, tutorials, or books that the readers can check for
background."

### Section 8: Writing, organization (6 sub-items)

> 8.1. The abstract is short and gives an objective summary of the
> contributions
>
> 8.2. The title expresses the main contribution and is easy to remember
>
> 8.3. The paper separates the problem description from the contributions
>
> 8.4. The paper includes a conceptual (human readable) outline and/or
> pseudocode description of the algorithms and methods if there are any
>
> 8.5. **The paper provides random access for the expert reader (this
> means, expert readers have a good chance of understanding the
> contributions by reading the problem definition, skimming the notation,
> then looking at the claims and their justification)**
>
> 8.6. The paper is proofread for typos and grammatical errors

The "random access" principle (8.5) is the most operationally specific:
expert reader should be able to jump non-linearly through the paper and
still extract the contribution.

### Section 9: Theoretical contributions (9 sub-items)

> 9.1. All assumptions are stated in a clear, formal fashion
>
> 9.2. The novel claims are formally stated (e.g., in theorem statements)
>
> 9.3. The theorem statements are self-contained, **or the reader will
> find it easy to identify in the paper the conditions under which the
> statements hold. Expect the reader to check the problem setup and
> notation, and assumptions if any, but do not expect the reader to check
> all the text before the statements.**
>
> 9.4. Proofs of nontrivial statements are included
>
> 9.5. **A superbly written paper will also give intuitive arguments for
> why the statements hold.**
>
> 9.6. If the paper improves results compared to state of the art, the
> paper clearly explains the reason of what made this improvement possible,
> or where it is coming from: What is improved in the algorithm/analysis?
>
> 9.7. **The reader is told which part of the proof is new and the rest is
> appropriately and generously attributed** (e.g., "this proof essentially
> follows that of ..")
>
> 9.8. Nonstandard definitions are included
>
> 9.9. All external results are appropriately cited (**do not cite a book
> or a long paper for a theorem, give the reader precise information where
> to find the result, e.g., the number of the theorem or the page number**)

Items 9.6 and 9.7 are operationally important: SOTA-comparison must
explain *what changed* (algorithm or analysis), and proof contributions
must be *attributed* (what's new vs. what's borrowed).

### Section 10: Computational experiments (11 sub-items, reproducibility-heavy)

> 10.1. **The paper explains the design choices made that leads to the
> specific choice of experiments: What questions are answered by the
> experiments**
>
> 10.2. The paper separates the presentation of the experiment design, the
> description of the experiment (including how data is obtained and the
> properties of data), the results, and the interpretation of the results
>
> 10.3. **If an algorithm depends on randomness, then the method used for
> setting seeds is described in a way sufficient to allow replication of
> results**
>
> 10.4. The paper formally describes evaluation metrics used and explains
> the motivation for choosing these metrics
>
> 10.5. This paper states which algorithms are used to compute each
> reported result (giving appropriate references). If a result is obtained
> by running an algorithm multiple times, the number of runs is included
>
> 10.6. **The analysis of experiments goes beyond single-dimensional
> summaries of performance (e.g., average; median) to include measures of
> variation, confidence, or other distributional information, as
> appropriate. The measure of variation included is justified (e.g., do
> not report standard deviation if the data is far from normally
> distributed)**
>
> 10.7. This paper lists all final (hyper-)parameters used for each
> model/algorithm in the paper's experiments
>
> 10.8. **This paper states the number and range of values tried per
> (hyper-)parameter during development of the paper, along with the
> criterion used for selecting the final parameter setting. Beware of
> overfitting to benchmarks.**
>
> 10.9. All source code required for conducting experiments is included
> in the supplementary material or the appendix
>
> 10.10. All source code required for conducting experiments will be made
> publicly available upon publication of the paper with a license that
> allows free usage for research purposes
>
> 10.11. The paper specifies the computing infrastructure used for running
> experiments (hardware and software), including GPU/CPU models; amount of
> memory; operating system; names and versions of relevant software
> libraries and frameworks

### Section 11: Data sets / benchmarks (5 sub-items)

> 11.1. All novel benchmarks introduced are included in a data appendix /
> supplementary
>
> 11.2. All novel benchmarks introduced in this paper will be made publicly
> available upon publication of the paper with a license that allows free
> usage for research purposes
>
> 11.3. All benchmarks drawn from the existing literature (potentially
> including authors' own previously published work) are accompanied by
> appropriate citations
>
> 11.4. All benchmarks drawn from the existing literature (potentially
> including authors' own previously published work) are publicly available
>
> 11.5. **All benchmarks that are not publicly available are described in
> detail (how the benchmark was designed, the data was obtained,
> descriptive statistics, etc.)**

## Core Actionable Rules (Condensed, Numbered)

These compress the 11 sections into pre-submission yes/no:

1. **Are claims stated clearly and the problem clearly described?**
2. **Does the paper explain how results substantiate claims?**
3. **Are limitations and technical assumptions explicit?**
4. **Is the problem motivated for the community at this point in time?**
5. **Can an expert reader self-contain understand the paper?**
6. **Is prior work discussed with explicit contribution-relationship?**
7. **Are background citations provided for non-expert readers?**
8. **Is the abstract short and objective?**
9. **Does the title express the main contribution memorably?**
10. **Is problem-description separated from contribution-description?**
11. **Is there pseudocode / human-readable algorithm outline?**
12. **Does the paper support expert random-access reading?**
13. **Are theoretical assumptions stated formally?**
14. **Are theorems self-contained (or assumptions easy to locate)?**
15. **Are proofs of nontrivial statements included?**
16. **Do theorems have intuitive arguments alongside formal statements?**
17. **For SOTA improvements: is the reason for improvement explicit?**
18. **Are new vs. borrowed proof parts attributed?**
19. **Are external results cited with precise locations (theorem # / page)?**
20. **For experiments: are design choices justified (what question
    answered)?**
21. **Are experiment design / description / results / interpretation
    separated?**
22. **Is seed-setting method described for randomized algorithms?**
23. **Are evaluation metrics formally described AND motivated?**
24. **Is number of runs reported for multi-run results?**
25. **Are variation measures (not just means) reported AND justified?**
26. **Are all final hyperparameters listed?**
27. **Are hyperparameter search ranges + selection criteria reported?**
28. **Will code be released with research-permissive license?**
29. **Is hardware + software + library-version infrastructure specified?**
30. **Are novel benchmarks released + appropriately licensed?**
31. **Are reused benchmarks cited and publicly available?**
32. **Are private benchmarks described in detail (design + descriptive
    statistics)?**

## Reviewer-Facing Implications

This is the **ICML PC's prescriptive companion** to the
[[ICML-2022-Review-Form]] descriptive rubric. Mapping:

- Best-Practices items 1, 4 → Review-Form **Novelty, relevance, significance**
- Best-Practices items 2, 9, 10.6 → Review-Form **Soundness**
- Best-Practices items 5, 8 (all sub-items), 10.1-10.2 → Review-Form
  **Quality of writing/presentation**
- Best-Practices items 6, 9.7, 9.9 → Review-Form **Literature**
- Best-Practices items 10.3-10.11, 11 (all sub-items) → Review-Form
  **Soundness** (experimental design) + presentation-issue subset

If a reviewer marks down on Soundness or Quality-of-writing, the
Best-Practices checklist is *the specific list* of items the author
should self-audit.

## ML-Paper-Specific Advice (Distinguished from General Writing)

**Heavily ML-specific:**

- Section 10 (computational experiments) is entirely ML/CS-specific. Seed
  reporting, hyperparameter search ranges, GPU/CPU specification,
  software-version listing — these are ML-community norms.
- Section 11 (benchmarks) reflects ML's benchmark-driven evaluation
  culture. General-writing guides don't have this.
- Item 10.8's "Beware of overfitting to benchmarks" is ML-specific — it
  captures the modern reproducibility-crisis concern that hyperparameter
  search on a test benchmark *is* a form of overfitting.
- Item 10.6's "standard deviation if data is far from normally distributed"
  is statistically-precise ML-evaluation guidance, not general advice.

**General-writing crossover:**

- Section 8 (abstract, title, organization, proofreading) overlaps with
  Strunk-White, Milne, Peyton-Jones.
- Section 9 (theoretical contributions) overlaps with Halmos / Bertsekas
  / Krantz mathematical-writing tradition.

## Cross-Corpus Identification (from Resource List)

The Best Practices doc lists 14 additional resources — several of which
are other files in `raw/writing/guides/`. This is invaluable cross-identification:

| Resource cited in Best Practices | Corresponds to PDF in corpus |
|---|---|
| Bill Freeman's slides | `Freeman writing papers 2020.pdf` (the 5.7 MB slide deck) |
| Aaditya Ramdas' stat-ML paper checklist | `aadi-paper-checklist.pdf` (already ingested as [[Ramdas-Paper-Checklist]]) |
| **Pat Langley's ICML 2002 piece on Crafting Papers on Machine Learning** | `Crafting Papers on Machine Learning.pdf` |
| **Zach Lipton's Heuristics for Scientific Writing** | `Heuristics for Scientific Writing (a Machine Learning Perspective) – Approximately Correct.pdf` |
| **Simon Peyton Jones: How to write a great research paper?** | `How-to-write-a-great-research-paper.pdf` |
| **D. Bertsekas: Ten Simple Rules for Mathematical Writing** | `Ten_Rules.pdf` (very likely) |
| Guide to Writing Mathematics (U. of Hong Kong) | `guide_to_writing_mathematics.pdf` (very likely) |
| **Elements of Style by Strunk and White** | `StrunkWhite.pdf` |
| **JS Milne's Tips for Authors** | `Tips for Authors -- J.S. Milne.pdf` |

This identifies **9 of the 14 corpus PDFs by author/title**, removing
uncertainty for upcoming ingests (#5, #6, #8, #9, #10, #13, #14). Still
uncertain: `1612.04888v1.pdf`, `how-to-write1.pdf`, `re-writing.pdf`.

## Connection to SiS / CTPC

Direct pre-submission audit map for [[CTPC-KDD-Submission]]:

- **Items 1-4 (claims, soundness, honesty, novelty):** the four-axis
  CTPC-KDD framing (Predictor + Corrector + calibration + reduction)
  must each have explicit claim + substantiation + acknowledged limits.
- **Item 8.5 (random access):** a reviewer should be able to jump to
  the methods section and understand CTPC without reading the
  introduction. The architecture diagram + loss equation must stand
  alone.
- **Items 9.5 (intuitive arguments) + 9.6 (improvement reason):** the
  64% MSE reduction and d̄² ≈ 1 vs. d̄² > 20,000 results need explicit
  causation — *why* does CTPC work where Latent ODE fails? Per the
  current writeup this is the predictor-vs-corrector split, but the
  explicit reason should be unmissable.
- **Item 10.1 (design choices for experiments):** every experimental
  choice (4-day horizon, NASA CDDIS data, K-sample MC) must answer "what
  question does this answer?"
- **Item 10.6 (variation measures):** the d̄² calibration metric IS the
  variation-aware measure — but the standard MSE reductions need their
  own variance reporting (per-trajectory variance, not just mean).
- **Item 10.8 (hyperparameter overfitting):** ν_min = 4.5 was a design
  choice with a *justification* (finite kurtosis) — this is exactly the
  kind of choice 10.8 wants stated.
- **Section 11 (benchmarks):** NASA CDDIS is a public benchmark — 11.3
  + 11.4 satisfied; the SiS-derived PhyArch Double Pendulum bench is
  novel and falls under 11.1-11.2 (needs supplementary release).

## Connections

- [[ICML-2022-Review-Form]] — reviewer-side companion (descriptive rubric)
- [[Ramdas-Paper-Checklist]] — Ramdas's complementary stat-ML checklist
- [[Lipton-Writing-Heuristics]] — Lipton's heuristics (cited in resources)
- [[Crafting-ML-Papers]] — Langley's ICML 2002 piece (cited in resources)
- [[Peyton-Jones-Research-Paper]] — Peyton-Jones slides (cited)
- [[Freeman-Writing-Papers]] — Freeman slides (explicitly recommended as
  the starter)
- [[Strunk-White-Elements-Style]] — Strunk & White (cited)
- [[Milne-Tips-Authors]] — Milne's tips (cited)
- [[Ten-Rules-Mathematical-Writing]] — Bertsekas's Ten Simple Rules (cited)
- [[Guide-Writing-Mathematics]] — U. of Hong Kong guide (cited)
- [[CTPC-KDD-Submission]] — the SiS submission this audits against

## Open Questions

1. **Pineau-checklist alignment.** Section 10's reproducibility focus
   tracks Joelle Pineau's reproducibility checklist — for the SiS
   target venue (KDD '26), are there KDD-specific reproducibility
   requirements that diverge from this ICML version?
2. **CRPS reporting under 10.4.** CTPC reports CRPS as part of the
   loss; should CRPS *also* be in the metrics-list per 10.4? Probably
   yes — strictly-proper-scoring-rule is the motivation.

## Sources

- `raw/writing/guides/Paper Best Practices.pdf` — ICML 2022 official Paper
  Writing Best Practices, 3 pages including the resource list.
- Original URL: https://icml.cc/Conferences/2022/BestPractices
- Modeled after: AAAI 2021 paper submission checklist + Joelle Pineau's
  reproducibility checklist (cited but not in corpus).
