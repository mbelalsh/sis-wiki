---
title: "Checklist for Effectively Writing Papers in Stat-ML"
tags: [writing, paper-writing, checklist, stat-ml, pre-submission, LaTeX, figures, theorems, mathematical-writing]
sources:
  - raw/writing/papers/guides/Stat_ML_Paper_Checklist.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# Checklist for Effectively Writing Papers in Stat-ML

**Author:** Aaditya Ramdas (aramdas@cmu.edu), Carnegie Mellon University
**Source framing:** "Some personal suggestions via accumulated wisdom"
**Format:** A single-page, bullet-pointed pre-submission checklist covering 13 categories. Ends with: "When you sign off on a paper, you should be proud of it, and ready to defend its details. Integrity is paramount."

---

## One-Line Intuition

A senior CMU statistician's distilled pre-submission audit — 13 concrete, actionable checks covering file hygiene, abstract tone, related-work framing, figures, equations, theorem classification, mathematical prose style, section flow, bibliography, appendices, and spell-check.

---

## Why This Matters

Stat-ML venues (NeurIPS, ICML, AISTATS, JMLR, Annals of Statistics) have reviewers who are simultaneously mathematicians and empiricists. A paper that loses trust on presentation — sloppy bibliography, misleading theorem labels, equations that float disconnected from prose, figures with unreadable legends — gets desk-rejected or scored down before the math is even evaluated. Ramdas's checklist targets exactly these failure modes: the things that are fixable in a final pass and that signal author maturity and integrity.

For Bilal's SiS/CTPC papers, which combine physics (Hamiltonian structure, dissipation), ML (neural ODEs, uncertainty), and SDA applications, the checklist is doubly important — reviewers span communities and will notice inconsistencies in notation, theorem granularity, and figure accessibility across subdisciplines.

---

## Core Content — Every Checklist Item

The checklist has **13 bullet items** organized thematically. Each is reproduced faithfully below with annotation.

---

### 1. Naming and Planning

**Item:** Everything should be in one file, except possibly macros. Name the file intelligently, not `paper.tex`. Plan papers in a top-down fashion: what is the paper's purpose, the point of each section, etc.

**Annotation:**
- Single-file principle: easier for collaborators, easier for arXiv submission, no missing-include bugs.
- Intelligent filename: `ctpc-neurips2026.tex`, not `paper.tex` or `draft_final_v3.tex`.
- Top-down planning means writing the section headers and one-line purpose statements before filling prose — the outline IS the paper plan.

---

### 2. Use Macros

**Item:** Use macros for all symbols, in case they need to be changed. Name macros or labels so that you recognize them a year later. Use a `~` when referring to equations/sections/figures: write `Section~\ref{sec:power}`. Familiarize yourself with commands like `bmatrix`, `align`, `cases`, `subequations`, `mbox`, `hspace`, `vspace`, etc.

**Annotation:**
- Macro discipline: `\newcommand{\CTPC}{\textsc{ctpc}}`, `\newcommand{\Hcal}{\mathcal{H}}`. One change propagates everywhere.
- Label naming: `eq:lyapunov-dissipation`, `thm:ctpc-stability`, `fig:rtn-frame` — self-documenting a year later.
- `~` before `\ref` prevents line breaks between "Section" and "3.2". Non-breaking space is a typographic correctness signal reviewers notice.
- `subequations` for multi-part equations (e.g., the CTPC predictor-corrector system).

---

### 3. Abstracts

**Item:** Abstracts need not detail every single advance made: summarize the setup and main results succinctly. Avoid citations unless necessary. Avoid future tense in abstract and paper (we will provide → we provide).

**Annotation:**
- Three failures this prevents: (a) over-detailed abstract that exhausts the reader before the intro, (b) citation-heavy abstract that breaks flow for readers who haven't read those papers, (c) future-tense hedging that signals lack of confidence.
- Correct: "We propose CTPC, a framework that enforces dissipation as a hard architectural constraint. Our method achieves X on Y benchmark."
- Incorrect: "We will show that our method, which will be described in Section 3, improves upon [1,2,3] by combining..."

---

### 4. Divide Related Work

**Item:** Divide related work into two parts: (a) those who have studied aspects of the same problem and those whose work your paper directly builds on or uses insights from, (b) those who work on something somewhat orthogonal but complementary papers. Prefer to include (a) in the introduction, and possibly (b) either in the discussion or in an "other related work" section at the end of the paper. Be concise and to the point, and give fair credit when it is due.

**Annotation:**
- The structural recommendation is precise: direct ancestors → intro; orthogonal neighbors → end of paper or discussion.
- "Fair credit" is ethical AND strategic: shortchanging a reviewer's own work in related-work is the fastest path to a hostile review.
- For CTPC: direct ancestors (HNNs, port-Hamiltonian nets, Neural ODEs) → intro related work. Orthogonal cousins (pure symbolic regression, graph neural networks for orbit prediction) → appendix or discussion.

---

### 5. Figures

**Item:** Any text inside a figure, such as in a legend, must have a readable font size, almost matching but not exceeding the size of the other text. Any lines in a figure must have multiple complementary sources of identifiable information: for example, use color, texture and symbols (red vs. blue, dashed vs. dotted, triangles vs. circles). Figures should have error bars; if negligible, explain why explicitly in the caption.

**Annotation — three sub-rules:**

| Sub-rule | Requirement | Why |
|---|---|---|
| Font size | Legend/axis text ≈ body text size; never smaller | Reviewers read on-screen; tiny legends are invisible |
| Redundant encoding | Color + linestyle + marker shape (not color alone) | Colorblind reviewers; B&W printing; accessibility |
| Error bars | Always show them; if absent, caption must justify | Statistical integrity; without them, results are unverifiable |

- Practical failure mode Ramdas is preventing: a figure where the only way to distinguish two curves is color — which disappears in grayscale PDF printout.
- For CTPC trajectory plots: use solid/dashed lines AND different marker shapes AND colors. Error bars on propagated covariance plots are mandatory.

---

### 6. Equations

**Item:** Equations are often part of a sentence, so follow them with the appropriate punctuation (commas or full-stops). Don't number every equation in a proof, only those that a reviewer/reader may want to refer to.

**Annotation:**
- Equations mid-sentence need punctuation. Example:
  - Correct: "The Hamiltonian satisfies $\dot{H} \leq 0$, which ensures stability."
  - Incorrect: "The Hamiltonian satisfies $\dot{H} \leq 0$ which ensures stability."
- Numbered equations: use `\eqref` only for equations you actually cross-reference. Unnumbered equations (in `equation*` or `align*`) reduce visual clutter and implicitly say "this step is obvious; you don't need to find it again."

---

### 7. Theorems Versus Rest

**Item:** Leave the annotation "theorem" for a handful of powerful central results: trivial theorems reduce trust in your judgment. Some may be propositions (interesting results of independent interest), or lemmas (a packaged technical result within a longer proof), or facts (known theorems from classical papers) or corollaries (interesting implications of theorems), or remarks. Number each type separately. Statements must be crisp and yet self-contained. Consider defining technical assumptions and necessary notation before the theorem and discussing what they mean and why they are needed. Even so, refer to all assumptions, by number or phrase, in the theorem statement so that it is self-contained.

**Annotation — this is one of the richest items in the checklist:**

| Label | Use case |
|---|---|
| **Theorem** | 2-5 central results of the paper; the things you'd put on a poster |
| **Proposition** | Interesting standalone results that don't rise to "main theorem" |
| **Lemma** | Technical building blocks used inside a proof |
| **Fact** | Known results you're invoking (cite; don't prove) |
| **Corollary** | Immediate consequence of a theorem; requires minimal proof |
| **Remark** | Informal observation, intuition, or scope clarification |

- **Labeling discipline signals reviewer trust.** If everything is a "Theorem," the label loses meaning. If you call a 1-line calculation a Theorem, reviewers doubt your calibration.
- **Self-containment rule:** Every theorem statement must include or refer to all its assumptions by name/number. A theorem that silently relies on Assumption A from three pages earlier is not self-contained.
- **Pre-theorem setup:** Define notation and assumptions BEFORE the statement, in surrounding prose. Then the statement is clean.

---

### 8. Interpret

**Item:** Interpret any theorem, proposition, corollary or lemma for the reader to provide intuition, and link to its proof. Avoid superlatives like "extremely general" or "very powerful" or "huge improvement". Organize proofs modularly, give proof outlines, and don't end them abruptly.

**Annotation:**
- Every formal result must be followed by a "What this means" paragraph — the intuition the reader should take away.
- Superlative bans: "extremely general" → either state what is general about it precisely, or drop the adjective. Superlatives are unfalsifiable and flag overselling.
- Proof modularization: break long proofs into lemmas with names. A proof outline (numbered steps, each one sentence) before a long proof dramatically improves readability.
- Don't end proofs abruptly: finish with a sentence that ties back to what was being proved.

---

### 9. Mathematical Writing

**Item:** If possible, don't use equations/references as nouns. Change "(12) suggests" or "[12] claimed" to "Definition/condition (12) suggests" or "Maryam [12] claimed". Don't say "This implies..." or "So, ..."; instead try "Invoking the constraint (4), we see that inequality (8) implies...". Avoid ∀, ∃ and say "for all, some" if possible. Change phrases like "we get" to "we derive/conclude/infer/...". Distinguish between a function f and its value f(x) at point x. A function f can be monotone, but f(x) cannot. Similarly, distinguish between random variables Y and a particular instantiation y.

**Annotation — seven specific rules extracted:**

| Rule | Wrong | Right |
|---|---|---|
| No equation-as-noun | "(12) suggests" | "Condition (12) suggests" |
| No citation-as-noun | "[12] claimed" | "Ramdas [12] claimed" |
| No floating "This implies" | "This implies..." | "Invoking constraint (4), we see that..." |
| No ∀, ∃ in prose | "∀ε > 0, ∃δ..." | "For all ε > 0, there exists δ..." |
| Precise verbs | "we get" | "we derive / conclude / infer" |
| Function vs. value | "f(x) is monotone" | "f is monotone" |
| RV vs. realization | "Y = 0.7" | "y = 0.7" (or "Y takes value 0.7") |

- The function-vs-value and RV-vs-realization distinctions are particularly important in stat-ML where conflating them causes ambiguous statements about distributions vs. samples.

---

### 10. Flow

**Item:** Sections should not start or end abruptly. The last line of a section should serve as a natural connector to the next section. Don't have sections with just one subsection. If you have one important point in a section, don't make a subsection, if you have two important and separate points, have two subsections.

**Annotation:**
- Connector sentences at section ends: "Having established the predictor's error bounds, we now turn to the corrector architecture in Section 4."
- Single-subsection rule: a subsection implies contrast with at least one other subsection. One subsection = no subsection; just use a paragraph header or bold lead-in.
- Subsection threshold: 2+ genuinely distinct points. Don't subsection just to add visual structure.

---

### 11. Check Bibliography

**Item:** Check bibliography for capitalization, authors, spurious "et al.", see if arXiv papers have been published.

**Annotation — four specific bibliography bugs:**

| Bug | Fix |
|---|---|
| Capitalization in titles | "hamiltonian neural networks" → "Hamiltonian Neural Networks" (protect with `{H}` in BibTeX) |
| Author name errors | First/last name swaps, diacritics stripped, initials wrong |
| Spurious "et al." | Many BibTeX styles auto-shorten; verify the threshold matches venue style |
| arXiv papers published | If a 2020 arXiv paper was published at ICML 2021, cite the ICML version |

---

### 12. Appendices

**Item:** Appendices must be organized logically, preferably in order of their first reference.

**Annotation:**
- Order appendices by first mention in the main paper. If Table 2 is referenced before Lemma 7, Appendix A covers Table 2's extended results and Appendix B covers Lemma 7's proof — not the other way around.
- Each appendix section should have a clear header and a one-sentence purpose statement at its start.

---

### 13. Spelling and Grammar

**Item:** Spelling and grammar should be checked by software (e.g., copy text into Google Docs or Microsoft Word).

**Annotation:**
- LaTeX spell-checkers (e.g., in TeXstudio, Overleaf) catch most errors, but grammar checking requires exporting to a word processor.
- Overleaf + Grammarly browser extension covers this in one workflow.
- For non-native English writers: this step is not optional — grammar errors break reviewer trust independently of the science.

---

## Quick Audit Table (Yes/No Checklist)

Use this table as a final pre-submission pass. Every "No" is a required fix before submission.

| # | Category | Audit Question | Done? |
|---|---|---|---|
| 1 | Naming/Planning | Is everything in one `.tex` file (except macros)? | ☐ |
| 1 | Naming/Planning | Is the filename informative (not `paper.tex`)? | ☐ |
| 1 | Naming/Planning | Have I written a top-down outline with per-section purpose? | ☐ |
| 2 | Macros | Do all symbols have macros? | ☐ |
| 2 | Macros | Are labels self-documenting? | ☐ |
| 2 | Macros | Is `~` used before all `\ref` calls? | ☐ |
| 3 | Abstract | Does the abstract avoid future tense ("we will")? | ☐ |
| 3 | Abstract | Does the abstract avoid unnecessary citations? | ☐ |
| 3 | Abstract | Is the abstract succinct (setup + main results only)? | ☐ |
| 4 | Related Work | Are direct ancestors placed in the intro? | ☐ |
| 4 | Related Work | Are orthogonal papers placed at the end or in discussion? | ☐ |
| 4 | Related Work | Is fair credit given to all relevant prior work? | ☐ |
| 5 | Figures | Is legend/axis font size ≈ body text? | ☐ |
| 5 | Figures | Do all curves use color + linestyle + marker (not color alone)? | ☐ |
| 5 | Figures | Do all plots have error bars, or caption explains their absence? | ☐ |
| 6 | Equations | Are all in-sentence equations followed by correct punctuation? | ☐ |
| 6 | Equations | Are only cross-referenced equations numbered? | ☐ |
| 7 | Theorems | Are "Theorem" labels reserved for 2–5 central results? | ☐ |
| 7 | Theorems | Are lemmas, propositions, corollaries, facts, remarks used correctly? | ☐ |
| 7 | Theorems | Is every theorem statement self-contained (all assumptions cited)? | ☐ |
| 8 | Interpret | Is every formal result followed by an intuition paragraph? | ☐ |
| 8 | Interpret | Are superlatives ("extremely", "very powerful") removed? | ☐ |
| 8 | Interpret | Do proofs have outlines and end non-abruptly? | ☐ |
| 9 | Math Writing | Are equations/references never used as bare nouns? | ☐ |
| 9 | Math Writing | Is "This implies..." replaced with explicit antecedent invocations? | ☐ |
| 9 | Math Writing | Are ∀, ∃ replaced with "for all", "there exists" in prose? | ☐ |
| 9 | Math Writing | Is "we get" replaced with precise verbs? | ☐ |
| 9 | Math Writing | Is function f distinguished from its value f(x)? | ☐ |
| 9 | Math Writing | Are random variables Y distinguished from realizations y? | ☐ |
| 10 | Flow | Does each section end with a connector to the next? | ☐ |
| 10 | Flow | Are there no sections with exactly one subsection? | ☐ |
| 11 | Bibliography | Are all titles correctly capitalized (with BibTeX `{}` protection)? | ☐ |
| 11 | Bibliography | Are all author names correct? | ☐ |
| 11 | Bibliography | Have arXiv papers been checked for published venue versions? | ☐ |
| 12 | Appendices | Are appendices ordered by first reference in the main paper? | ☐ |
| 13 | Spelling/Grammar | Has text been run through grammar-checking software? | ☐ |

---

## Key Verbatim Anchors

All from page 1 of the source.

1. **On theorem labels:** "trivial theorems reduce trust in your judgment." (p. 1)

2. **On self-containment:** "Statements must be crisp and yet self-contained. Consider defining technical assumptions and necessary notation before the theorem and discussing what they mean and why they are needed. Even so, refer to all assumptions, by number or phrase, in the theorem statement so that it is self-contained." (p. 1)

3. **On mathematical prose:** "Don't say 'This implies...' or 'So, ...'; instead try 'Invoking the constraint (4), we see that inequality (8) implies...'." (p. 1)

4. **On function/value distinction:** "Distinguish between a function f and its value f(x) at point x. A function f can be monotone, but f(x) cannot. Similarly, distinguish between random variables Y and a particular instantiation y." (p. 1)

5. **On figures:** "Any lines in a figure must have multiple complementary sources of identifiable information: for example, use color, texture and symbols (red vs. blue, dashed vs. dotted, triangles vs. circles). Figures should have error bars; if negligible, explain why explicitly in the caption." (p. 1)

6. **On related work structure:** "I prefer to include (a) in the introduction, and possibly (b) either in the discussion or in an 'other related work' section at the end of the paper. Be concise and to the point, and give fair credit when it is due." (p. 1)

7. **On integrity:** "When you sign off on a paper, you should be proud of it, and ready to defend its details. Integrity is paramount." (p. 1)

8. **On avoiding superlatives:** "Avoid superlatives like 'extremely general' or 'very powerful' or 'huge improvement'." (p. 1)

9. **On section flow:** "The last line of a section should serve as a natural connector to the next section." (p. 1)

10. **On abstract tense:** "Avoid future tense in abstract and paper (we will provide → we provide)." (p. 1)

---

## How a Writing Agent Should Use This

### Primary mode: Pre-submission audit

When a draft is complete and a submission deadline is approaching, invoke this checklist as a structured audit pass. The agent should:

1. **Run the 37-item audit table** (above) against the draft, producing a binary pass/fail for each item.
2. **Prioritize the "theorem taxonomy" check** (item 7): count how many results are labeled "Theorem." If more than 5, flag each for reclassification to Proposition/Lemma/Corollary/Remark.
3. **Scan all equations for noun usage** (item 9): grep for patterns like `(\\ref{eq:`, `Eq.~\\eqref`, etc., preceded by no noun — flag them.
4. **Inspect all figures programmatically** if possible: does each figure have both color and linestyle/marker variation? Are error bars present or explained?
5. **Bibliography audit**: for every `@article` with `url={https://arxiv.org}` or `journal={arXiv}`, check if a published version exists.

### Secondary mode: Inline writing feedback

When reviewing a section draft, the agent should flag:
- Future tense ("we will show") → replace with present ("we show")
- Floating "This implies" → ask "what implies what? invoke by name/number"
- Missing punctuation after displayed equations
- Superlative adjectives (extremely, very, huge, groundbreaking)
- "et al." in mid-sentence without a name: "[12] shows" → "Smith et al. [12] show"

### Priority order for a stat-ML paper

The checklist items with highest reviewer-trust impact, ranked:
1. **Theorem taxonomy** — mislabeling signals poor calibration
2. **Figure redundant encoding** — colorblind accessibility is now explicitly checked by many venues
3. **Mathematical prose** (equation-as-noun, function/value distinction) — signals mathematical maturity
4. **Abstract tense and scope** — first thing every reviewer reads
5. **Bibliography correctness** — signals thoroughness; easy to verify

---

## Connection to SiS / CTPC

**Theorem taxonomy is critical for CTPC papers.** The CTPC framework has several mathematical results of different weight:
- The dissipation guarantee ($\dot{H} \leq 0$ enforced architecturally) → **Theorem**
- The Cholesky-parameterization PSD guarantee → **Proposition** (important, but not the central contribution)
- Intermediate steps in the stability proof → **Lemma**
- Consequences for covariance realism → **Corollary**

Labeling all of these "Theorem" would dilute the central contributions. The Ramdas taxonomy directly maps to the CTPC result hierarchy.

**Figure redundant encoding matters for SDA plots.** CTPC trajectory comparison plots (SGP4 baseline vs. CTPC corrected vs. ground truth) must use color + linestyle + marker — satellite operators reviewing the paper may print in grayscale, and colorblindness affects ~8% of reviewers.

**Mathematical prose style matters for cross-community papers.** CTPC spans physics, ML, and aerospace — reviewers from each community will have different tolerance for notation shortcuts. The "for all" vs. ∀ rule, function vs. value distinction, and explicit antecedent invocations all reduce ambiguity for cross-disciplinary reviewers.

**Related work structure maps directly to CTPC's positioning.** Direct ancestors (HNNs, PHNN, Neural ODEs) → intro. Orthogonal cousins (pure GP orbit fitting, Koopman methods, classical SGP4 error correction) → discussion or end-of-paper section.

**Bibliography integrity is non-negotiable for SDA papers.** Many SDA-adjacent ML papers started as arXiv preprints and were subsequently published. Citing the arXiv version when a JMLR or NeurIPS version exists signals that the authors didn't do due diligence. The space/satellite domain is small; reviewers notice.

---

## Connections

- [[Langley-Crafting-ML-Papers]] — complementary high-level ML paper craft guide; Langley covers framing and positioning while Ramdas covers structural/typographic correctness
- [[Peyton-Jones-Research-Paper]] — Simon Peyton Jones on the full paper-writing process; pairs with Ramdas's checklist as the "what to write" vs. "pre-submission audit" combination
- [[Bertsekas-Ten-Rules-Mathematical-Writing]] — Bertsekas's rules for mathematical prose overlap significantly with Ramdas item 9 (mathematical writing) and item 7 (theorem taxonomy)
- [[Williams-Bizup-Style]] — Style: Lessons in Clarity and Grace; deeper treatment of the sentence-level prose issues Ramdas flags in items 9 and 10
- [[Strunk-White-Elements-Style]] — foundational English style guide; the "omit needless words" principle underlies Ramdas's abstract and flow guidance
- [[Krantz-Primer-Mathematical-Writing]] — fuller treatment of mathematical writing conventions that Ramdas condenses to a single bullet
- [[Lipton-Writing-Heuristics]] — Lipton's ML-specific writing heuristics; complements Ramdas on presentation and argument structure
- [[Milne-Tips-For-Authors]] — similar pre-submission checklist tradition from a mathematics perspective
- [[Goldreich-How-To-Write-Paper]] — theoretical CS perspective on paper structure; useful pairing for CTPC's formal guarantees sections
- [[Whitesides-Writing-A-Paper]] — Whitesides's outline-first method maps directly to Ramdas's top-down planning item

---

## Open Questions

1. **Venue-specific figure standards:** Does the redundant-encoding requirement (color + linestyle + marker) now appear explicitly in NeurIPS/ICML reviewer guidelines, or is it still informal community norm? If formalized, the checklist can be updated with venue-specific pointers.

2. **Theorem count calibration:** Ramdas says "a handful of powerful central results" — what is the right count for a 9-page NeurIPS paper? 2? 3? Is there a community norm by venue?

3. **arXiv-vs-published bibliography check tooling:** Is there an automated tool (or arXiv API query) that can flag BibTeX entries pointing to arXiv when a published version exists? Could be integrated into the writing agent's bibliography audit step.

4. **Grammar checking for LaTeX:** What is the current best workflow for grammar checking a `.tex` file without losing math? (Detex + Grammarly? Overleaf's built-in? Writefull?) The checklist says "copy text into Google Docs or Microsoft Word" — this is now suboptimal given available tooling.

5. **Single-subsection detection:** The "don't have sections with just one subsection" rule — can this be caught programmatically via a LaTeX parser? Worth adding to the writing agent's linting pass.

---

## Sources

- `raw/writing/papers/guides/Stat_ML_Paper_Checklist.pdf` — full document, p. 1 (single-page checklist)
- Author: Aaditya Ramdas, Carnegie Mellon University (aramdas@cmu.edu)
- Title: "Checklist for effectively writing papers in stat-ML"
