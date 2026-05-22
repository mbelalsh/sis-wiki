---
title: Crafting Papers on Machine Learning (Langley, ICML 2002)
tags: [writing, paper-writing, ml-papers, evaluation, communication, structure, bake-off-critique]
sources:
  - raw/writing/papers/guides/ML_Paper_Crafting.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# Crafting Papers on Machine Learning (Langley, ICML 2002)

## One-Line Intuition

A well-crafted ML paper is not a lab notebook dump — it is a purposeful argument that states goals, describes methods, evaluates claims with appropriate evidence, and communicates clearly enough that a reader can understand and reimplement the work with minimal friction.

## Why This Matters

Written as an editorial guide for ICML 2002, this essay addresses a persistent failure mode in the ML community: papers that are technically sound but poorly communicated, or that mistake horse-race benchmarking for scientific insight. Langley (then at DaimlerChrysler Research, previously editor of the *Machine Learning* journal) draws on his experience reviewing hundreds of papers to identify what separates publishable work from truly influential work. The guide is concise (8 pages) but dense — every paragraph is a actionable heuristic. It applies directly to the paper-writing agent's job: auditing drafts for missing content, weak evaluation, and communication failures.

---

## Core Content

### 2. Content of the Paper — Seven Mandatory Topics

Langley argues that any well-balanced ML paper must cover all of the following. Omitting even one weakens the manuscript. These are necessary but not sufficient conditions for publication.

| # | Topic | What It Requires |
|---|-------|-----------------|
| 1 | **State the goals of the research** | Categorize the paper clearly: formal analysis, new learning algorithm, application of established methods, or computational model of human learning. State the criteria by which readers should evaluate the approach. |
| 2 | **Specify the performance and learning tasks** | Clearly distinguish the two. If there is no performance system (e.g., the paper is purely about learning), propose an alternative evaluation of the learning behavior. |
| 3 | **Describe the representation and organization** | Explain the knowledge representation and training data representation. Give examples of each — unless the representation is standard and familiar to most readers. |
| 4 | **Explain both the performance and learning components** | Describe in enough detail that readers can reimplement the system. Use a metaphor (e.g., "search through a hypothesis space") to make the learning algorithm graspable. |
| 5 | **Evaluate the approach to learning** | Avoid unsubstantiated or rhetorical claims. If claiming superiority over alternatives, provide experimental or theoretical evidence, successful accounts of psychological phenomena, or demonstrations of new functionality. |
| 6 | **Relate the approach to other methods** | Discuss similarities, differences, and advances over prior work. Place the method in historical context. Specify intellectual debts explicitly, including work done within other paradigms on the same task. Do more than list references. |
| 7 | **State the limitations of the approach** | Suggest directions for future research. Go beyond listing problems — propose tentative solutions. |

> "Of course, covering each of these will not ensure a high-quality paper, but omitting even one of them will weaken the manuscript and should be addressed before it is ready for publication." (p. 2)

---

### 3. Evaluation in Machine Learning — Full Taxonomy

Langley treats evaluation as the central technical challenge of ML papers. He identifies five distinct evaluation paradigms, each appropriate for different claim types. The key principle: **the success of a paper should be judged not by which type of evaluation it uses, but by the extent to which its evaluation provides evidence that supports its central claims** (p. 4).

#### 3.1 Experimental Approaches to Evaluation

The most common mode. Mirrors the natural sciences in structure: identify dependent variables, identify and control independent variables, average across random variables outside one's control.

**Dependent variables:**
- Primary: measures of performance (behavior when learning is disabled)
- Secondary legitimate: characteristics of the learned knowledge
- Caution: classification accuracy is popular but can be misguided for domains with skewed error costs or class distributions — use ROC curves instead (cite: Provost, Fawcett & Kohavi, 1998)

**Independent variables (typical in supervised learning):**
- The induction method (new algorithm vs. established baselines)
- The domain (often UCI data sets treated as random samples)

**The bake-off critique — Langley's central argument against pure benchmarking:**

> "By themselves, such 'bake offs' tell one very little about the reasons why one method outperforms another, and thus do not provide the insight about causes that we expect in science." (p. 3)

Bake-offs treat the domain as a random variable to average over, making it "interesting in its own right" only incidentally. They establish *that* one method beats another, but not *why*. The fix is to run **additional experiments on synthetic domains** designed to test explicit hypotheses motivated by the intuitions behind the original extension. Synthetic data sets allow systematic variation of:
- Number of relevant vs. irrelevant attributes
- Amount of noise
- Complexity of the target concept

This lets researchers test hypotheses about each method's ability to scale under conditions of increasing difficulty.

**Lesion studies:** Remove algorithm components to determine their contribution. Essential for well-rounded experimental papers.

**Sensitivity studies:** Examine sensitivity to specific parameter settings.

**Learning curves:** Vary training set size to understand data efficiency. Langley criticizes the norm of reporting results on fixed-size training sets: "which tells one nothing about how the methods would fare given more or less data" (p. 3). Learning curves (with confidence intervals) are far more informative.

**Statistical significance:** Important but insufficient. "It is even more important that these differences reveal some insight into the nature of the learning process" (p. 3). Significance tests confirm differences are non-accidental; they do not explain the differences.

#### 3.2 Alternative Forms of Evaluation

Langley identifies four non-experimental paradigms, each legitimate for the right claim type:

**A. Computational models of human behavior (cognitive modeling)**
- Evaluate the algorithm as a model of human learning
- Goal: match *amount* of improvement under comparable conditions, not maximize performance
- Run on many tasks to determine average behavior under various conditions
- Identify which components are most responsible for matching human behavior
- Examine influence of domain characteristics on learning

**B. Formal analysis**
- Derive statements that, under precise conditions, relate learning behavior to characteristics of the learning task
- Reviewers check whether the derivation or proof is correct
- Caution: "there exist many true statements about learning that hold little intrinsic interest, making relevance to experimental findings an important factor" (p. 3-4)
- Some average-case analyses introduce approximations that require direct comparison between predicted and observed behavior (cite: Langley & Sage, 1999)

**C. Exploratory research** (citing Dietterich, 1990)
- Appropriate for papers that: identify and precisely state a new learning problem, show the inability of existing methods to solve it, propose novel approaches with potential, discuss important issues that arise, and suggest an agenda for future research
- "By its very nature, is not ready for careful experimental studies or final formal analyses, but it has an essential role to play in the field" (p. 4)
- Without exploratory papers, researchers would spend energies only on minor variations of established tasks

**D. Demonstration of new functionality**
- Claim: the new approach has capabilities not available to existing systems
- Evidence: illustrating its ability to handle challenging tasks that existing systems cannot handle
- Common in systems that combine multiple mechanisms not typically used together (cite: Nordhausen & Langley, 1993 — integrated computational scientific discovery system)

**E. Applied research** (citing Provost & Kohavi, 1998)
- Superficially: use established methods to solve real-world problems
- More commonly: application reveals difficulties that challenge basic research assumptions
- Examples of what applied papers actually contribute: problem reformulation, representation engineering, data manipulation, introduction of background knowledge, dealing with error costs
- "The ideal applied paper examines their importance to the problem at hand, characterizes the key issues in more general terms, and challenges the research community to address those issues" (p. 4)
- Result is "more akin to an exploratory research paper than one might expect"

---

### 4. Issues of Communication

#### 4.1 Title and Abstract

**Title:**
- Informative but not overly long
- Describe multiple aspects: approach, domain, or factor of interest
- Examples given: "Genetic Induction of Planning Heuristics", "Ensemble Methods in Noisy Domains"
- If you want to say more, add a brief subtitle — but be succinct
- Never include abbreviations in titles or headings

**Abstract:**
- One paragraph, no more than six or seven sentences
- Purpose: let readers scan quickly for paper overview
- Do NOT repeat text from the abstract in the introduction — they serve different purposes
- Abstract summarizes the text; introduction introduces the reader to the research

#### 4.2 Partitioning the Text

**Sections:**
- Use informative headings, not generic labels like "Representation" or "Evaluation"
- Never use pronouns (e.g., "it") in headings; do not treat a heading as part of the sentence that follows
- Include introductory transition sentences at the beginning of each section and subsection
- Make sections roughly equal length (except possibly introduction and conclusion)
- If you include subsections, always include an introductory paragraph before the first subsection
- Never include only one subsection in a section — subsections divide a section into components
- Avoid subsections that contain only one paragraph; embed such material in another subsection instead

**Paragraphs:**
- Each paragraph discusses one distinct idea and flows naturally from its predecessor
- Ideal paragraph: no more than six sentences, no fewer than three sentences
- Sentences should convey ideas in digestible bites — not too much, not too little

**Footnotes:**
- Use to provide additional information without interrupting flow
- Must be complete sentences
- Keep to a reasonable length: one to three sentences

#### 4.3 Continuity and Flow

**Topic separation:** Treat conceptually distinct topics separately and in their proper order. Do not mix discussion of learning mechanisms with representational issues. Earlier text must lay the groundwork for what comes later.

**Itemization (lists):**
- Use lists to highlight important points or steps
- Do NOT overuse — often a paragraph communicates the same material as well or better
- Itemize at the right level of detail: items should be shorter than paragraphs but more than a few words
- Always close a list with a concluding sentence

**Parenthetical expressions:**
- Useful for side comments, but overuse disrupts sentence flow
- If longer than a few words, move the information to a footnote instead

**Active vs. passive voice:**
- Readers understand active sentences more easily than passive ones
- Achieve active voice by writing in first person OR by using the system name as the subject
- Example: "ID5 constructs decision trees incrementally..." is better than "Decision trees are constructed incrementally..."

**Adjective chains:**
- Avoid long chains: not "incremental instance-based learning algorithms" but "incremental algorithms for instance-based learning"

**Contractions:** Never use contractions in technical papers — they sound "overly chatty"

#### 4.4 Figures and Tables

**Figures:**
- Label all distinct components: letter each graph, label axes, include legends
- Caption below figure with enough detail to understand contents without reading text
- Example of good caption: "Improvement in classification accuracy for three induction methods on the congressional voting domain"
- Do NOT include a title above the figure — the caption serves this function
- Always refer to each figure in the text and discuss it at least briefly
- Figures augment the text; they do not replace it
- Do not use location references ("the table below") since position changes during typesetting

**Tables:**
- Summarize textual/tabular material (contrast with figures, which contain graphical material)
- Label all components clearly; title above the table briefly explaining the content
- Same rules: refer to every table in the text, discuss briefly

#### 4.5 Describing Your System

**Naming:**
- Give the system a name — this provides textual variety and lets other authors cite something concrete
- Do NOT overuse the name: alternate between the system name and "the system"
- If the name appears more than three times in one paragraph, remove some occurrences
- Never end one sentence and start the next with the system name

**Implementation language:**
- Avoid language-specific terms and formalisms — many readers won't know your implementation language
- Reformat list-structure representations to make them readable
- Use mnemonic names for subroutines and parameters, not internal system names

**Program traces:**
- Helpful but not a substitute for careful system description
- If included, paraphrase in English with running commentary
- Consider placing traces in an appendix to preserve flow

#### 4.6 Terminology and Notation

**Abbreviations:**
- Avoid abbreviations — especially ones invoked only a few times
- If using repeatedly, define at first use
- Never use more than a few distinct abbreviations in a given paper
- Alternative to abbreviations: define a macro or use global substitution
- Never include abbreviations in titles or headings

**Jargon:**
- Avoid needless jargon; use terminology shared by the broader research community
- If you must coin new terms, explain their relation to existing concepts
- Legitimate borrowed terms from unfamiliar fields can make a paper very difficult to read — explain them
- Avoid terms that lend themselves to confusion when other words would serve as well
- "Think carefully about the specialized terms that you employ" (p. 7)

**Formal notation:**
- Omit unnecessary formalism that does not occur in proofs or otherwise aid communication
- If using formal notation, clarify its meaning in the text
- "Your goal is to convey ideas and evidence to the audience, not to overwhelm them with your arcane language and formal prowess" (p. 7)

---

## Key Verbatim Anchors

These quotes are the load-bearing sentences an agent should be able to retrieve verbatim.

1. **On the cost of omission (p. 2):** "Of course, covering each of these will not ensure a high-quality paper, but omitting even one of them will weaken the manuscript and should be addressed before it is ready for publication."

2. **On bake-offs (p. 3):** "By themselves, such 'bake offs' tell one very little about the reasons why one method outperforms another, and thus do not provide the insight about causes that we expect in science."

3. **On learning curves (p. 3):** "Typical empirical papers report results on training sets of fixed size, which tells one nothing about how the methods would fare given more or less data."

4. **On statistical significance (p. 3):** "It is even more important that these differences reveal some insight into the nature of the learning process, and we encourage authors to move beyond simple 'bake offs' to studies that give readers a deeper understanding of the reasons behind behavioral differences."

5. **On formal analysis (p. 3-4):** "There exist many true statements about learning that hold little intrinsic interest, making relevance to experimental findings an important factor."

6. **On exploratory research (p. 4):** "Exploratory research, by its very nature, is not ready for careful experimental studies or final formal analyses, but it has an essential role to play in the field. Without such contributions, researchers would continue to spend their energies on minor variations of established tasks."

7. **On evaluation diversity (p. 4):** "The success of any given paper should be judged, not on which type of evaluation it embraces, but on the extent to which its evaluation provides evidence that supports its central claims."

8. **On active voice (p. 5):** "'ID5 constructs decision trees incrementally...' is better than 'Decision trees are constructed incrementally...'"

9. **On formal notation (p. 7):** "Your goal is to convey ideas and evidence to the audience, not to overwhelm them with your arcane language and formal prowess."

10. **On communication cost (p. 7-8):** "Spending an extra hour or two making your paper clear and easy to read can save many more hours across the entire research community, as well as increase the paper's chances of publication and influencing your colleagues."

11. **On the role of the paper (p. 8):** "Writing and publishing papers is only one stage in this process, but one that must put all the previous steps into an integrated, comprehensible package."

12. **On abbreviations in headings (p. 6):** "You should never include an abbreviation in a title or heading, however often it appears elsewhere."

---

## How a Writing Agent Should Use This

**Agent:** Paper-writing agent (Layer 1a)

**Mode 1 — Pre-draft structuring checklist (before writing)**

Before drafting, the agent should verify the author has answers to all seven content topics:
1. What is the paper's category (formal analysis / new algorithm / application / cognitive model)?
2. What are the performance task and learning task — are they clearly distinguished?
3. What is the representation — is there an example to show?
4. Are both the performance component and learning component described to reimplement level?
5. What form of evaluation matches the paper's claims — experimental, cognitive, formal, exploratory, functionality demo, or applied?
6. What prior work must be positioned against, with intellectual debts explicit?
7. What limitations are acknowledged, and what tentative solutions are proposed?

**Mode 2 — Evaluation design (during structuring)**

Use the evaluation taxonomy to match evidence to claim type:
- If claiming "our method outperforms X": experimental study required; bake-off alone is insufficient; must add synthetic domain experiments or lesion studies to explain *why*
- If claiming "our method is principled / convergent": formal analysis required, but relevance to experimental findings must be argued
- If claiming "we are first to solve problem X": exploratory research mode; use Dietterich's (1990) five criteria
- If claiming "our method has capabilities Y that no existing system has": functionality demonstration mode
- If "we applied ML to domain Z and found problem P": applied mode; characterize P in general terms and challenge the community

**Mode 3 — Draft auditing (after draft)**

The agent should audit any draft against the following checklist derived from Section 4:

- [ ] Title is informative, describes approach + domain or factor; no abbreviations
- [ ] Abstract is one paragraph, at most 7 sentences; does not repeat intro text verbatim
- [ ] Section headings are informative (not generic "Evaluation"), no pronouns, not treated as sentence continuations
- [ ] Each section/subsection opens with a transition sentence
- [ ] No section has only one subsection
- [ ] No subsection has only one paragraph
- [ ] Paragraphs discuss one idea, 3-6 sentences
- [ ] Lists are closed with a concluding sentence
- [ ] Passive voice flagged and converted to active where possible
- [ ] Long adjective chains broken up
- [ ] No contractions in body text
- [ ] Figures: labeled components, caption below (not above), discussed in text, no location references
- [ ] Tables: title above, all columns/rows labeled, discussed in text
- [ ] System name not overused (flag if >3 in one paragraph; flag if consecutive sentences start with it)
- [ ] Abbreviations minimized; none in titles or headings
- [ ] Formal notation has prose explanation
- [ ] Limitations section goes beyond listing problems — tentative solutions proposed
- [ ] Related work does more than list references — historical context, intellectual debts, paradigm comparisons

**Mode 4 — Revision of evaluation section**

If the evaluation section is pure bake-off (UCI comparison + accuracy table), the agent should flag this explicitly and prompt: "Add at least one of: (a) synthetic domain experiment to test the mechanism hypothesis, (b) lesion study, (c) learning curves varying training set size, (d) sensitivity analysis on key parameters."

---

## Connection to SiS / CTPC

The CTPC paper will be an ML paper at a venue like ICML, NeurIPS, or AISTATS. Langley's framework applies directly to the core structural decisions:

**Content topics applied to CTPC:**
- *Goals:* Category = new learning algorithm (ML corrector) + application (SDA/orbital propagation); evaluation criterion = covariance realism + propagation accuracy vs. SGP4 baseline
- *Tasks:* Performance task = orbit prediction at time T; learning task = residual modeling in RTN/RSW frame — these must be explicitly distinguished in the paper
- *Representation:* Equinoctial orbital elements + RTN residuals; examples of TLE → state → corrector input chain needed
- *Components:* Physics predictor (SGP4/GMAT) + ML corrector (Port-Hamiltonian or dissipative NN) — both must be described to reimplement level
- *Evaluation:* Mixed mode — experimental (propagation accuracy) + functionality demo (dissipation enforcement that existing methods cannot provide as a hard constraint) + potentially exploratory (framing covariance realism as an underexplored problem)
- *Related work:* Must go beyond listing GP/neural ODE papers — position CTPC within the history of physics-informed ML and orbital ML, specifying what CTPC adds that prior work does not have (hard dissipation constraint as architecture prior, not loss penalty)
- *Limitations:* SGP4 model error as input; TLE uncertainty propagation; scalability of Port-Hamiltonian block to high-dimensional state

**Evaluation design for CTPC:**
- Bake-off (CTPC accuracy vs. SGP4 vs. plain neural ODE on UCI-style orbital datasets) is necessary but not sufficient by Langley's standard
- Must add: (a) synthetic orbital scenarios varying J2 perturbation strength, atmospheric density, eccentricity to test *when* the dissipation constraint matters most (mechanism experiment); (b) lesion study removing the Port-Hamiltonian block to show contribution of hard constraint vs. learned residuals; (c) learning curves showing sample efficiency vs. purely data-driven baselines
- ROC-curve analogy: use calibration curves and covariance consistency scores (NCI/NIS), not just RMSE — per Langley's caution that the most popular metric may be misguided for the domain's error cost structure

**Communication:**
- "CTPC" is an abbreviation — never put in title or section headings; introduce on first use
- Active voice: "CTPC corrects SGP4 residuals..." not "Residuals are corrected by..."
- The dissipation block is the "system" — name it consistently (e.g., "the dissipative corrector") and don't repeat more than three times per paragraph

---

## Connections

- [[Strunk-White-Elements-Style]] — foundational prose style; Langley's active voice and concision heuristics are consistent with Strunk & White's rules
- [[Williams-Bizup-Style]] — deeper treatment of active/passive, cohesion, and emphasis; extends Langley §4.3
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — reader-expectation theory for scientific prose; complements Langley's continuity and flow section
- [[Schimel-Writing-Science]] — story-structure approach to scientific papers; provides narrative framing Langley leaves implicit
- [[Peyton-Jones-Research-Paper]] — complementary ICML-era advice; Peyton Jones's "the paper, and how to write it" covers similar ground from a CS/PL perspective
- [[Lipton-Writing-Heuristics]] — ML-specific writing heuristics; shares Langley's applied ML focus
- [[Ramdas-Paper-Checklist]] — operational checklist format; extends Langley's audit mode with statistics-specific checks
- [[Freeman-Writing-Papers]] — further guidance on paper structure complementing Langley's content topic list
- [[Whitesides-Writing-A-Paper]] — outline-first philosophy consistent with Langley's structuring advice

---

## Open Questions

1. **Bake-off norms have shifted since 2002** — NeurIPS and ICML now have reproducibility checklists and ablation requirements that partially address Langley's critique. Has the community fully internalized the lesson, or do bake-offs still dominate? A review of the 2023-2024 ICML best paper awards would clarify whether lesion studies and synthetic domain experiments are now standard.

2. **Exploratory research mode and ML venues** — Langley defends exploratory research papers (Dietterich 1990 criteria), but top ML venues in 2024 are heavily experimental. How does one write an exploratory CTPC paper (framing covariance realism as a new problem) in a way that passes review at ICML/NeurIPS? This is a live question for Bilal's early-stage CTPC submissions.

3. **ROC curve analogy for orbital UQ** — Langley argues for ROC curves over accuracy when error costs are skewed. The analogous choice for SDA is calibration curves / NCI/NIS scores over RMSE. The paper-writing agent should flag if RMSE is the only reported metric in a CTPC draft.

4. **Learning curves for CTPC** — Langley criticizes fixed training-set papers. CTPC training data comes from TLEs, which are sparse and heterogeneous. What is the right "learning curve" axis — number of TLE observations? number of objects? time window? This is an open experimental design question.

5. **Formal notation caution** — CTPC involves Port-Hamiltonian systems, Cholesky parameterization of covariance, and Riemannian geometry. Langley warns against formalism that does not aid communication. Which derivations belong in the main paper vs. appendix? The paper-writing agent should apply the test: "does this notation aid a reader in reimplementing the system, or does it signal sophistication?"

---

## Sources

- `raw/writing/papers/guides/ML_Paper_Crafting.pdf` — Pat Langley, "Crafting Papers on Machine Learning," ICML 2002 tutorial/editorial essay, 8 pages.
  - p. 1: Abstract and Introduction (§1)
  - p. 1-2: Content of the Paper (§2) — seven mandatory topics
  - p. 2-4: Evaluation in Machine Learning (§3) — experimental approaches, bake-off critique, learning curves, alternative forms
  - p. 4: Alternative evaluation modes — cognitive modeling, formal analysis, exploratory research (Dietterich 1990), functionality demonstration, applied research (Provost & Kohavi 1998)
  - p. 4-5: Issues of Communication (§4) — title, abstract, text partitioning
  - p. 5: Continuity and flow — itemization, parentheticals, active voice, adjective chains
  - p. 6: Figures and tables, describing your system
  - p. 6-7: Terminology and notation — abbreviations, jargon, formal notation
  - p. 7-8: Concluding Remarks (§5), References
