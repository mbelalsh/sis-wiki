---
title: CMU IMRD Cheat Sheet — Worked Examples
tags: [writing, paper-writing, IMRD, structure, abstract, introduction, methods, results, discussion, worked-examples]
sources:
  - raw/writing/papers/guides/CMU_imrd-examples.pdf
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: no
---

# CMU IMRD Cheat Sheet — Worked Examples

## One-Line Intuition

A three-page annotated cheat sheet from Carnegie Mellon's Student Academic Success Center that shows exactly what each IMRD section must accomplish, with move-by-move sentence templates and fully annotated real-paragraph examples so a writer can audit their own draft section by section.

## Why This Matters

Most writing guides describe IMRD abstractly. This handout pairs the abstract description with concrete, labeled extracts from real student papers across multiple disciplines (bioplastics, spam filtering, electrical conductivity, assembly instructions), making the rhetorical moves visible and imitable. For a technical paper writer — including one writing SDA/ML papers for venues like ICML, NeurIPS, or AAS — it supplies a checklist that can be applied sentence by sentence.

---

## Core Content

### Abstract (p. 1 — Cheat Sheet; p. 2 — Annotated Example)

**Proportional space allocation rule (canonical):**

| IMRD Component | Proportion of abstract |
|---|---|
| Introduction (importance/problem) | 25% |
| Methods (what you did) | 25% |
| Results (what you found) | 35% — most important part |
| Discussion (implications) | 15% |

**Annotated abstract example** (p. 2 — gun/shotgun choke experiment):

> *"This experiment tests the effect of choke type and gun selection on target accuracy in order to determine the best gun specifications."* → **Introduction** (states the research question and purpose)
>
> *"Three competent shooters of approximately equivalent marksmanship abilities tested three different choke types (full, modified, and improved) and two different guns (a Remington 11-87 semi-automatic and a Beretta 682 Gold E)."* → **Methods** (who, what was tested, controlled variables)
>
> *"With a confidence level of 95%, the gun selection ended up to be the only significant factor. The Beretta was found more accurate than the Remington possibly because the Beretta's weight is centered in the middle of the gun while the Remington is a little barrel-heavy. However, if the confidence level is lowered to 90%, choke type is also significant, with the improved choke more accurate than the modified or full."* → **Results** (quantified finding, qualifier, conditional secondary result)
>
> *"Thus, for target shooting, the most accurate combination would be the Beretta with an improved choke."* → **Discussion** (practical recommendation / bottom line)

**Key lessons from the abstract example:**
- Results dominate (three sentences vs. one each for I, M, D).
- The qualifier ("if confidence level lowered to 90%") appears in the Results, not Discussion — keeps Discussion to one clean recommendation sentence.
- Specific numbers ("95%", "88%") are present even in the abstract.

---

### Introduction (p. 2 — Annotated Example; p. 1 — Cheat Sheet)

**Cheat sheet prescription (p. 1):**
- Make a case for your new research.
- Explain the problem and why it is necessary.
- Convince readers to continue reading.
- Discuss the current state of research, expose a "gap" or problem in the field, then explain why the present research is a timely and necessary solution. (Cross-reference: Novelty Handout — i.e., [[CMU-Establishing-Novelty-Four-Moves]].)

**Annotated Introduction example** (p. 2 — bioplastics/PLA paper):

The excerpt opens by establishing why bioplastics matter (renewable biomass, fewer fossil fuels, reduced greenhouse emissions), then narrows to PLA specifically (most commercially viable option), establishes PLA's strengths (resembles traditional plastic, already processable on existing equipment), then pivots to the gap:

> *"However, although PLA biodegrades under carefully controlled conditions, it is not yet compostable except in industrial composting facilities and cannot be mixed with other recyclable materials. This limits the commercial viability of PLA because the infrastructure to transport bioplastic waste to appropriate composting facilities has not yet been developed."*

Finally, the Introduction closes with the solution move:

> *"A device that composts PLA and other bioplastics within a home composting environment would make PLA a more viable commercial option."*

**Key lessons from the Introduction example:**
- Classic funnel: broad domain → specific material → strengths → gap → proposed solution.
- The gap is stated twice: functionally ("not yet compostable except in industrial facilities") then economically ("limits commercial viability… infrastructure has not yet been developed"). Double-framing the gap amplifies urgency.
- The solution sentence at the end is forward-looking but does not preview specific methods — that belongs in Methods.
- Sentence-level move: "However" as the gap-signaling pivot word.

---

### Methods (p. 1 — Cheat Sheet; p. 2 — Annotated Example)

**Cheat sheet prescription (p. 1):**
- Answers: "What did you do?"
- Usually written in **past tense** and **passive voice**.
- Lots of **headings and subheadings**.
- This is the **least-read section** of an IMRaD report.

**Annotated Methods example** (p. 2 — Sb-doped SnS thin film paper, two subsections shown):

*Subsection 1: "Sb-Doped SnS Thin Film."*

> *"Pure, stoichiometric, single-phase SnS thin films can be obtained by atomic layer deposition (ALD) from the reaction of bis(N,N′-diisopropylacetamidinato)tin(II) [Sn(MeC(NiPr)2)2, referred here as Sn(amd)2] and hydrogen sulfide (H2S). Rather than using ALD as previously reported, SnS thin films were deposited using a modified chemical vapor deposition (CVD) process, referred here as a pulsed-CVD, to speed up the deposit rate to ~15 times higher than that of ALD…"*

*Subsection 2: "Material Characterization."*

> *"Film morphology was characterized using field-emission scanning electron microscopy (FESEM, Zeiss, Ultra-55). The film thickness was determined from cross-sectional SEM. The elemental composition of the films was determined by Rutherford backscattering spectroscopy (RBS, Ionex 1.7 MV Tandetron) and time-of-flight secondary ion mass spectroscopy (ToF-SIMS)…"*

**Key lessons from the Methods example:**
- Subheadings carve the section: one for fabrication, one for characterization — exactly the "lots of headings" prescription.
- Passive voice throughout ("can be obtained", "were deposited", "was characterized", "was determined").
- Instrument models and vendors cited by name — reproducibility standard.
- A brief methodological choice is justified inline ("to speed up the deposit rate to ~15 times higher") — acknowledges that a departure from convention needs a one-clause rationale.

---

### Results (p. 1 — Cheat Sheet; p. 3 — Annotated Examples)

**Cheat sheet prescription (p. 1) — two-function model:**

Results have two distinct rhetorical functions:

| Function | Required/Optional | Moves |
|---|---|---|
| **Report** | Required | 1. Refer to table/figure, state main trend; 2. Support trend with data; 3. (If needed) Note secondary trends + data; 4. (If needed) Note exceptions/unexpected outcomes |
| **Comment** | Optional (can go in Discussion) | 5. Provide an explanation; 6. Compare to other research; 7. Evaluate vs. hypothesis; 8. State bottom line |

**Sentence starters from the cheat sheet:**

| Move | Example starters |
|---|---|
| 1. Main trend | *"Table 3 shows that Spam Filter A correctly filtered more junk emails than Filter B"* |
| 2. Support with data | *"Filter A correctly filtered…"* / *"The average difference is…"* |
| 3. Secondary trends | *"In addition…"* / *"Figure 1 also shows…"* |
| 4. Exceptions | *"However…"* |
| 5. Explanation | *"A feasible explanation is…"* / *"This trend can be explained by…"* |
| 6. Compare to other research | *"X is consistent with X's finding…"* / *"In contrast, Y found…"* |
| 7. Evaluate vs. hypothesis | (no explicit starter given) |
| 8. Bottom line | *"These findings overall suggest…"* / *"These data indicate…"* |

**Annotated Results Example A** (p. 3 — Spam Filter comparison):

> *"Table 3 shows that Spam Filter A correctly filtered more junk emails than Filter B."* [Move 1 — main trend, figure reference]
>
> *"Filter A correctly filtered 88% of junk emails whereas filter B only filtered 63% correctly."* [Move 2 — quantified data support]
>
> *"However, Filter A takes longer to run than Filter B. This increased run time is due to the type of programming language used in Filter A."* [Move 4 — exception + explanation]
>
> *"These findings overall suggest that Spam Filter A is a better filter than Filter B even though it takes longer to run."* [Move 8 — bottom line]

**Annotated Results Example B** (p. 3 — Cu-doped ZnO electrical conductivity):

> *"Fig. 3 shows that the electrical conductivity of the Cu-doped ZnO is much lower than that of the undoped ZnO."* [Move 1]
>
> *"The electrical conductivity of even the 100 ppm Cu-doped ZnO specimen was about 3 orders of magnitude lower than that of the undoped ZnO."* [Move 2 — dramatic quantification]
>
> *"As the doped Cu content increased, the electrical conductivity gradually decreased."* [Move 3 — secondary trend/monotone pattern]
>
> *"As a result, the 1000 ppm Cu-doped ZnO had the electrical conductivity 5 orders of magnitude lower than that of the undoped ZnO."* [Move 2 again — cumulative quantification at far end of range]

**Key lessons from the Results examples:**
- Every results paragraph begins with a figure/table pointer + one-sentence main trend claim. Never begin with raw numbers.
- Quantification is specific and comparative: "88% vs. 63%", "3 orders of magnitude lower", "5 orders of magnitude lower". Vague language ("substantially higher") is absent.
- Move 4 (exception) appears in Example A right after Move 2 — the concession is immediate, not buried.
- The bottom-line sentence (Move 8) closes Example A but is absent from Example B — the cheat sheet marks these moves as "(if needed)", signaling that the Report function is mandatory and the Comment function is optional.

---

### Discussion (p. 1 — Cheat Sheet; p. 3 — Annotated Example)

**Cheat sheet prescription (p. 1) — five moves:**

1. **Summarize results** — allows readers skipping to Discussion to grasp the main "news".
2. **Connect findings to other research.**
3. **Discuss flaws** in the current study.
4. **Use flaws as reasons to suggest future research.**
5. **(If needed) State implications** for future policy or practice.

**Annotated Discussion example** (p. 3 — visual vs. verbal assembly instructions study):

*Move 1 — Summarize results:*

> *"The data collected from this small study suggests that verbal instructions are not needed to complete a simple assembly task and may even interfere with the task. The participants who received words plus pictures made more errors, took longer to complete the task, and were less confident that they had completed the task correctly than participants who received pictures alone."*

*Move 2 — Explain results (connection):*

> *"One reason for this finding may be the simplicity of the task since none of the guidelines we examined suggest that textual information would interfere with visual instructions."*

*Moves 3+4 — Flaws → Future research (bracketed together in the annotated example):*

> *"Our study is hampered by the small number and homogeneity of our participants. All of our participants were college students and this may have affected our results. Additional research might examine whether older participants would benefit from verbal instructions accompanying pictures. More research is also needed examining different tasks. Our study involved a highly physical task (constructing a lego vehicle). Future research should examine how pictures and verbal instructions might interact on a more conceptual task, such as installing and using a software program."*

*Move 5 — Implications:*

> *"Based on this limited analysis, we recommend that instruction writers consider excluding verbal instructions on a simple assembly task. Our results indicate that verbal instructions may in some cases interfere with users' abilities to follow pictorial directions."*

**Key lessons from the Discussion example:**
- Move 1 is a re-statement of results in plain English, not a copy-paste. The example restates the finding ("made more errors, took longer, were less confident") in a summary clause before explaining it — this is the "news lead" function.
- Moves 3 and 4 are fused: each flaw ("small number", "homogeneous college students", "physical task only") is immediately followed by its future-research corollary. The handout's annotation bracket spans both moves simultaneously.
- Move 5 (Implications) is hedged: "Based on this limited analysis, we **recommend**…" and "verbal instructions **may in some cases** interfere…" — the hedge matches the scope of a small study.
- Move 2 in this example is framed as an explanation ("one reason for this finding") rather than a literature comparison, showing that the "connect to other research" move can be satisfied by theoretical reasoning when no prior literature directly addresses the finding.

---

## Key Verbatim Anchors

| Quote | Section | Page | What it illustrates |
|---|---|---|---|
| *"The reporting function always appears in the results section while the comment function can go in the discussion section."* | Results cheat sheet | p. 1 | Canonical permission to split Report/Comment across Results and Discussion |
| *"Captions go above tables and beneath figures."* | Results cheat sheet | p. 1 | Formatting rule — frequently violated |
| *"Methods are usually written in past tense and passive voice with lots of headings and subheadings. This is the least-read section of an IMRaD report."* | Methods cheat sheet | p. 1 | Justifies front-loading headings; calibrates effort allocation |
| *"These findings overall suggest that Spam Filter A is a better filter than Filter B even though it takes longer to run."* | Results Example A | p. 3 | Move 8 (bottom line) sentence template |
| *"Our study is hampered by the small number and homogeneity of our participants."* | Discussion Example | p. 3 | Move 3 (flaw) opener — hedged but direct |
| *"Based on this limited analysis, we recommend…"* | Discussion Example | p. 3 | Move 5 (implications) opener — hedged recommendation |

---

## How a Writing Agent Should Use This

**Mode: Paper-Writing-Agent, drafting or auditing a section.**

### Auditing a draft (section-by-section checklist)

1. **Abstract audit** — score the draft abstract against the 25/25/35/15 space allocation. If Results < 35% of abstract length, flag for expansion. Confirm specific numbers appear.
2. **Introduction audit** — verify the funnel: broad domain → specific gap → solution. Check for the "gap pivot" word (typically "However" or "Unfortunately"). Ensure the Introduction closes with the solution move, not a preview of Results.
3. **Methods audit** — confirm past tense and passive voice. Confirm subheadings. Confirm instrument models are cited. Check that any methodological departure from convention is justified inline with a brief rationale.
4. **Results audit** — check every paragraph: does it begin with a figure/table pointer + main trend? Are all numbers comparative and specific? Is Move 8 (bottom line) present or deliberately omitted? If the Comment function is used (Moves 5–7), confirm it does not duplicate the Discussion.
5. **Discussion audit** — confirm Move 1 (summary) exists and is distinct from Move 3 (flaw). Confirm each flaw is paired to a future-research suggestion. Confirm Move 5 (implications) is hedged appropriately to study scope.

### Drafting from scratch

- Use the sentence starters table in the Results section as literal fill-in-the-blank templates.
- Use the bioplastics Introduction as a structural template: domain background → material specifics → strengths → gap (stated twice) → solution sentence.
- Use the assembly instructions Discussion as the paragraph-order template: summary → explanation → flaw+future (fused) → implications.

### Cross-reference

When the Paper-Writing-Agent needs the structural theory (why these moves exist rhetorically), load [[CMU-IMRD-Structuring]] alongside this page. This page supplies the *what-it-looks-like*; the structuring handout supplies the *why*.

---

## Connection to SiS / CTPC

For CTPC and SDA papers, this cheat sheet maps directly onto the following writing tasks:

**Abstract:** The 35%-Results rule demands that CTPC's headline number (e.g., "X% improvement in RSO position uncertainty propagation over SGP4 baseline") occupy the majority of abstract space. The temptation in SDA papers is to over-explain the physics architecture in the abstract — this cheat sheet's proportions actively resist that.

**Introduction:** The bioplastics funnel is the template for a CTPC Introduction: SDA need (collision avoidance, conjunction probability) → current gap (SGP4 TLE error unmodeled, covariance unrealism) → solution (CTPC residual corrector with hard dissipation constraint). The double-gap framing ("not yet compostable… infrastructure has not yet been developed" → "not yet ML-corrected… physics-violating corrections degrade downstream Pc computation") is directly importable.

**Results:** The spam filter example shows how to report comparative ML results: "Filter A correctly filtered 88% vs. 63% for Filter B." Equivalent CTPC move: "CTPC reduced mean position uncertainty error by X% compared to SGP4-only baseline (Table 2)." The Move 8 bottom-line sentence is mandatory in any venue with a tight page budget (NeurIPS, ICML, AAS).

**Discussion — Flaws + Future:** The "hampered by small number and homogeneity" flaw move is directly applicable to SDA evaluation: "Our evaluation is limited to LEO objects in the Space-Track catalog; future work should examine GEO and HEO regimes." Pairing flaw to future work in the same paragraph is the expected convention.

---

## Connections

- [[CMU-IMRD-Structuring]] — companion handout giving the theoretical rationale for each IMRD move; this page is the worked-examples companion to it
- [[CMU-Abstracts]] — CMU's dedicated abstract guide; extends the 25/25/35/15 rule with more abstract-specific moves
- [[CMU-Establishing-Novelty-Four-Moves]] — the "Novelty Handout" explicitly referenced on p. 1 under Introduction conventions; covers the gap-establishment moves in depth
- [[CMU-Literature-Reviews]] — relevant to Introduction construction (Move 2: current state of research before gap)
- [[CMU-Known-New-Information-Flow]] — sentence-level principle that underlies why Results paragraphs must open with figure pointer (known) before data (new)
- [[CMU-BLUF-Topic-Sentence]] — the Discussion's Move 1 (summarize results at section opening) is an application of BLUF
- [[Schimel-Writing-Science]] — broader narrative arc of a paper; IMRD is the section-level instantiation of Schimel's story structure
- [[Gopen-Swan-Science-Of-Scientific-Writing]] — stress-position principle applies within Results sentences: main trend claim at end of opening sentence, supporting data in subsequent sentences
- [[Langley-Crafting-ML-Papers]] — ML-specific framing of Introduction and Results sections; read alongside this cheat sheet for NeurIPS/ICML submission
- [[Ramdas-Paper-Checklist]] — operational checklist that maps to several IMRD audit points above

---

## Open Questions

1. The cheat sheet marks the Comment function in Results as optional ("can go in the discussion section"). For ML venues (NeurIPS, ICML), is the convention to keep Comment in Results or defer to Discussion? Clarify by reading exemplars in `raw/writing/exemplars/`.
2. The Methods section is labeled "least-read" — does this hold for systems/SDA papers where the architecture novelty IS the Methods? Consider whether CTPC Methods should carry more move-1-style rhetorical scaffolding than the thin-film example requires.
3. The Discussion flaw move in the example is self-deprecating but brief. How much flaw-disclosure is appropriate for conference venues with blind review vs. journal venues? Check [[CMU-Revise-And-Resubmit]] for reviewer-facing norms.
4. The abstract example does not show any forward-reference to section numbers or figure numbers — confirming the convention that abstracts are self-contained. Verify this holds for AAS/AIAA conference paper formats where some authors include figure references in abstracts.

---

## Sources

- `raw/writing/papers/guides/CMU_imrd-examples.pdf` — full document, 3 pages
  - p. 1: IMRD Cheat Sheet (Abstract proportions; Introduction prescription; Methods prescription; Results Report/Comment two-function model with move list and sentence starters; Discussion five-move list)
  - p. 2: Annotated Examples — Abstract (gun/choke experiment), Introduction (bioplastics/PLA), Methods (Sb-doped SnS thin film — two subsections)
  - p. 3: Annotated Examples — Results A (spam filter), Results B (Cu-doped ZnO conductivity), Discussion (visual vs. verbal assembly instructions)
