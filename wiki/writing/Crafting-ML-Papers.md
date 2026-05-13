---
title: Langley — Crafting Papers on Machine Learning (ICML 2002)
tags: [writing, ml-perspective, evaluation, paper-content, communication, langley]
sources:
  - raw/writing/guides/Crafting Papers on Machine Learning.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Langley — Crafting Papers on Machine Learning (ICML 2002)

**Author:** Pat Langley, Adaptive Systems Group, DaimlerChrysler R&T Center,
Palo Alto. **Venue:** ICML 2002 hosted essay (icml.cc/Conferences/2002/craft.html).
**Length:** 8 pages, ~3500 words. Portions borrowed from Langley's
earlier editorials in *Machine Learning* (1990) and *IEEE Expert* (1996).

## Structure

§1 Intro · §2 Content of the Paper · §3 Evaluation in ML (3.1 Experimental,
3.2 Alternative) · §4 Issues of Communication (4.1 Title/Abstract, 4.2
Partitioning, 4.3 Continuity, 4.4 Figures/Tables, 4.5 Describing System,
4.6 Terminology) · §5 Concluding Remarks.

## §2 — Six Required Topics in an ML Paper (Verbatim Bullets)

> "Different manuscripts may well organize this information in quite
> different ways, but the ideal paper should:"

1. **"State the goals of the research"** and the criteria for evaluation;
   categorize the paper (formal analysis / new algorithm / application /
   cognitive model).
2. **"Specify the performance and learning tasks"** that are the focus,
   distinguishing the two aspects.
3. **"Describe the representation and organization"** of the system's
   knowledge AND the representation of training data; include examples.
4. **"Explain both the performance and learning components"** in enough
   detail that readers can reimplement them; "use some metaphor (like
   search through a hypothesis space) to describe the learning algorithm."
5. **"Evaluate the approach to learning, avoiding unsubstantiated or
   rhetorical claims."** If claiming superiority, include evidence or
   careful arguments.
6. **"Relate the approach to other methods, discussing similarities,
   differences, and advances over previous research. Do more than simply
   list references to relevant work."**
7. **"State the limitations of the approach and suggest directions for
   future research. Go beyond a list of problems to propose tentative
   solutions."**

(Seven bullets; "six topics" is approximate — Langley lists seven distinct
items.) **"Covering each of these will not ensure a high-quality paper,
but omitting even one of them will weaken the manuscript and should be
addressed before it is ready for publication."**

## §3 — Evaluation (the centerpiece of the essay)

### 3.1 Experimental approaches (Verbatim core)

> "A paper should state precisely the dependent variables in each study.
> [...] Using a measure like classification accuracy, despite its
> popularity, can be misguided for domains with skewed error costs or
> class distributions. **In such cases, it may be better to invoke ROC
> curves.**"

**The bake-off critique (Langley's signature point):**

> "By themselves, such 'bake offs' tell one very little about the reasons
> why one method outperforms another, and thus do not provide the insight
> about causes that we expect in science. **Insight is best obtained by
> running additional experiments on synthetic domains designed to test
> explicit hypotheses, typically motivated by the intuitions behind the
> original extension.**"

**The well-rounded experimental paper:**

> "A well-rounded experimental paper will also include **lesion studies**,
> which remove algorithm components to determine their contribution, and
> **studies that examine sensitivity to specific parameter settings.**
> Experiments that systematically vary external resources, such as the
> number of training cases available for learning, can also contribute
> important insights — [collect] **learning curves** [rather than]
> fixed-size training sets."

**Statistical tests are necessary but not sufficient:**

> "Clearly, one should be careful not to draw unwarranted conclusions from
> experimental results. **But it is even more important that these
> differences reveal some insight into the nature of the learning
> process**, and we encourage authors to move beyond simple 'bake offs'
> to studies that give readers a deeper understanding of the reasons
> behind behavioral differences."

### 3.2 Alternative forms of evaluation (Verbatim taxonomy)

Langley enumerates four non-experimental evaluation modes:

1. **Cognitive models** — match human behavior; "improve the same amount,
   under comparable conditions, as does human learning."
2. **Formal analysis** — "derive statements that, under precise conditions,
   relate aspects of learning behavior to characteristics of the learning
   task." Caveat: many true statements hold little intrinsic interest;
   relevance to experimental findings matters.
3. **Exploratory research** (Dietterich 1990 criteria) — for new problems
   not yet ready for full experimental study: "(i) identify and state
   precisely a new learning problem, (ii) show the inability of existing
   methods to solve this problem, (iii) propose novel approaches that show
   potential, (iv) discuss the important issues that arise, (v) suggest an
   agenda for future research."
4. **New-functionality demonstrations** — system handles tasks not
   accessible to any component algorithm.
5. **Applied research** (Provost & Kohavi 1998) — "the ideal applied
   paper examines [practical issues'] importance to the problem at hand,
   characterizes the key issues in more general terms, and challenges the
   research community to address those issues."

> "The success of any given paper should be judged, not on which type of
> evaluation it embraces, but on **the extent to which its evaluation
> provides evidence that supports its central claims.**"

## §4 — Communication Rules (Verbatim Highlights)

### 4.1 Title/Abstract

- "Use a title that is informative but not overly long."
- "The ideal abstract will be brief, limited to one paragraph and **no
  more than six or seven sentences.**"
- **"Do not repeat text from the abstract in your introduction; they
  should serve different purposes."**

### 4.2 Partitioning the text

- Informative section headings, never generic ones like "Representation"
  or "Evaluation".
- "Never use pronouns (such as 'it') in headings."
- "Make your sections roughly the same length, except possibly for the
  introduction and conclusion."
- "**Never include only one subsection in a section.**"
- "Avoid subsections that contain only one paragraph."
- **"The ideal paragraph will run no more than six sentences and no fewer
  than three sentences."**
- Footnotes "take the form of complete sentences."

### 4.3 Continuity and Flow

- Transition sentences at section starts.
- "Treat conceptually distinct topics separately and in their proper
  order."
- Itemize at the right level; close lists with a concluding sentence.
- Parentheticals only for a few words; longer → footnote.
- **"Readers usually understand active sentences more easily than passive
  ones, so use active constructions whenever possible. [...] 'ID5
  constructs decision trees incrementally' is better than 'Decision trees
  are constructed incrementally.'"**
- "Avoid long chains of adjectives, such as 'incremental instance-based
  learning algorithms.' Instead, break such chains into more manageable
  chunks, as in 'incremental algorithms for instance-based learning.'"
- "Avoid contractions in the text."

### 4.4 Figures and Tables

- Label all distinct components (axes, legends, panels A/B/C).
- Captions: descriptive ("Improvement in classification accuracy for
  three induction methods on the congressional voting domain") not
  generic ("Learning curves on the voting domain").
- **"Do *not* include a title above the figure, as the caption already
  serves this function."**
- Tables: title ABOVE; figures: caption BELOW.
- Refer to each figure/table in text. **"Do not refer to their location,
  as in 'the table below,' since their exact position may change during
  typesetting."**

### 4.5 Describing Your System

- Name your system — but **"if the system name appears more than three
  times in one paragraph, you should remove some occurrences."** Never
  end a sentence with the system name and start the next sentence with
  it.
- Avoid language-specific terms / implementation details.
- **"Detailed program traces [...] are not a replacement for a careful
  system description. If you do include one, make sure you paraphrase it
  in English and include running commentary. Consider placing the trace
  in an appendix."**

### 4.6 Terminology and Notation

- **"Avoid abbreviations, especially if you invoke them only a few
  times."** Never put an abbreviation in a title or heading.
- "Avoid needless jargon. Whenever possible, use terminology shared by
  other researchers in the field rather than inventing your own."
- **"Omit unnecessary formalism that does not occur in proofs or
  otherwise aid communication."** When using formal notation, clarify
  meaning in text.

## Core Actionable Rules (Numbered, Compressed)

1. **Cover all 7 content topics:** goals + criteria, performance vs
   learning tasks, representations + data, components reimplementable,
   evaluation with evidence, related-work with substance, limitations +
   tentative solutions.
2. **State dependent variables precisely.** Justify choice (accuracy is
   often wrong for skewed-cost domains; use ROC).
3. **Go beyond bake-offs.** Include synthetic-domain experiments that
   vary domain characteristics systematically.
4. **Include lesion studies** (component contribution) AND parameter
   sensitivity studies AND learning curves (vary training size).
5. **Statistical significance is necessary but insufficient** — explain
   *why* differences arise.
6. **Match evaluation mode to claim type:** experimental / cognitive /
   formal / exploratory (Dietterich 1990 criteria) / new-functionality /
   applied (Provost-Kohavi 1998).
7. **Abstract ≤ 7 sentences, one paragraph.** Don't echo into intro.
8. **Informative section headings** — no pronouns, no generic
   "Representation".
9. **Paragraphs 3-6 sentences.** Equal-length sections (except intro/
   conclusion).
10. **No single-subsection sections; no single-paragraph subsections.**
11. **Active voice.** System name as subject ("ID5 constructs...").
12. **No long adjective chains** — restructure.
13. **No contractions.**
14. **Figures captioned below (descriptive), tables titled above.** No
    "the figure below"; reference by number only.
15. **System-name use ≤ 3 per paragraph.** Alternate with "the system".
16. **Avoid abbreviations.** Never in titles/headings.
17. **Use field-shared terminology** unless you must coin terms; then
    explain the relation.
18. **No unnecessary formalism.** Clarify formal notation in text.

## Reviewer-Facing Implications

Langley's framing: papers omitting any of the seven content topics will
be weakened. The §3 evaluation taxonomy is operationally how reviewers
distinguish "bake-off" papers (mediocre evaluation) from "insight" papers
(strong evaluation). The communication §4 list is the surface-level
quality signal — sloppy organization, generic headings, long sentences
are taken as proxies for sloppy thinking.

**The Provost-Fawcett-Kohavi (1998) "case against accuracy estimation"
reference** is operationally important: a reviewer trained on Langley
will mark down a paper that reports accuracy on skewed-class domains
without ROC or alternative.

## ML-Paper-Specific Advice (Distinguished from General Writing)

**Heavily ML-specific:**

- The bake-off critique (§3.1) is ML-specific — it's about UCI-repository
  comparison studies.
- "Lesion studies", "learning curves", "ablation" — ML/AI terminology
  conventions.
- The Dietterich-1990 exploratory-research criteria are ML-community-
  internal.
- The 4-mode evaluation taxonomy (experimental / cognitive /
  formal / exploratory / new-functionality / applied) is calibrated to ML's
  particular intellectual landscape.

**General-writing crossover:**

- §4.2-4.3-4.6 (organization, flow, terminology) overlap with Strunk-White,
  Milne, Halmos.
- §4.1 (abstract/title) overlaps with Lipton's heuristic #1.

## Connection to SiS / CTPC

Direct pre-submission audit for [[CTPC-KDD-Submission]]:

- **§2 content topics:** check all 7 against the current draft. CTPC's
  "performance task" is orbital state error prediction at multi-day
  horizons; "learning task" is residual-correction training on RTN-frame
  errors — these need explicit separation per §2.
- **§3.1 lesion studies:** the agent council's "Mind your input networks"
  warning ([[Dissecting-NODE]] §5.3) IS a lesion study — ablate PhyArch
  to check the NCDE is exercising its capacity vs. PhyArch alone solving
  it.
- **§3.1 parameter sensitivity + learning curves:** the K-sample-MC
  uncertainty estimate scales with K; explicit K-sweep would be a
  learning-curve-style study.
- **§3.1 dependent variable critique:** MSE alone is the accuracy
  analogue Langley warns about; CTPC's d̄² ≈ 1 calibration metric is
  the ROC-analogue answer — but the paper should EXPLAIN this choice
  per §3.1's "dependent variables make direct contact with the goals of
  the research."
- **§4.5 system-naming:** "CTPC" name discipline — alternate with "the
  Corrector", "the framework", etc. Don't say "CTPC" more than three
  times per paragraph.
- **§4.6 abbreviations in titles:** the title "Continuous-Time
  Probabilistic Corrector for Orbital..." includes "Continuous-Time
  Probabilistic Corrector" (full) rather than "CTPC" (abbreviation) —
  Langley-compliant. Keep it.

## Connections

- [[ICML-2022-Review-Form]] — reviewer rubric Langley's advice optimizes
  against (separated by 20 years but structurally aligned)
- [[ICML-Best-Practices]] — modern descendant that cites Langley's work
- [[Lipton-Writing-Heuristics]] — 2018 ML-perspective complement
- [[Ramdas-Paper-Checklist]] — stat-ML companion
- [[CTPC-KDD-Submission]] — pre-submission audit target

## Open Questions

1. **Dietterich-1990 exploratory-research framework** — should the SiS
   wiki ingest this as a separate page? It's the canonical "how to write
   an exploratory ML paper" reference and Year-2 SiS work (analytic-Σ,
   CBM-CTPC) leans exploratory.
2. **§3.1's "synthetic domain" prescription** — orbital mechanics doesn't
   have an obvious synthetic analogue. PhyArch-Double-Pendulum-Benchmark
   is the closest ([[PhyArch-Double-Pendulum-Benchmark]]); does this
   satisfy Langley's "vary domain characteristics systematically" bar?

## Sources

- `raw/writing/guides/Crafting Papers on Machine Learning.pdf` — 8 pages,
  Pat Langley, ICML 2002 hosted essay.
- Original URL: https://icml.cc/Conferences/2002/craft.html
- Author affiliation at time of writing: Adaptive Systems Group,
  DaimlerChrysler Research and Technology Center, Palo Alto.
- Lineage: portions borrowed from Langley (1990) *Machine Learning*
  editorial and Langley (1996) *IEEE Expert*.
