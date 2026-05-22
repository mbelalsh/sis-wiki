---
title: "How to Write a Good Paper — William T. Freeman (MIT / Google Research)"
tags: [writing, paper-writing, structure, reviewer-pov, figures, clarity, tone, honesty, title, experiments]
sources:
  - raw/writing/papers/guides/Writing_Good_Paper.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# How to Write a Good Paper — William T. Freeman (MIT / Google Research)

## One-Line Intuition
A research paper is not a monograph read by patient scholars — it is a vendor's stall in a crowded marketplace, and you have seconds to convince a tired, overloaded reviewer that your work is worth their attention.

## Why This Matters
Freeman (MIT CSAIL / Google Research) delivered this as a CVPR 2020 tutorial on writing AND reviewing papers. It synthesises advice from Kajiya, Adelson, Knuth, Mermin, and Strunk & White into a single, opinionated, practitioner's guide. The central argument: career impact from papers is a step function — mediocre and "pretty good" papers produce essentially zero long-term benefit; only creative, original, well-written papers generate lasting impact. Given that rejection rates at top venues exceed 75 %, the quality of your writing determines which side of the step function you land on among the vast pool of borderline submissions.

---

## Core Content

### Slide-Group 1: The Career Economics of Papers (slides 1–2)

Freeman opens with a stark graph: **Effect on your career** (y-axis) vs. **Paper quality** (x-axis, ranging Bad → Ok → Pretty good → Creative, original and good). The curve is flat near zero until the "Creative, original and good" threshold, then spikes vertically. Key implication:

- Writing a mediocre-but-correct paper wastes effort. The marginal effort to make a good paper great is the highest-ROI investment in your research career.
- The nonlinearity means: be one of many authors on a great paper >> be a sole author on an ok paper. (This is revisited in the author-list advice on slide 26.)

### Slide-Group 2: The Reviewer Is Not a Scholar (slides 3–4)

**The fantasy:** Scholarly monks with time on their hands, carefully reading every word of your manuscript.

**The reality:** A large, crowded marketplace. Reviewers are overloaded, reviewing many papers simultaneously, looking for reasons to reject. Area chairs must reject at least 75 % of submissions. They are *hunting* for grounds to dismiss your paper quickly.

Implication for writing: You are not writing for an ideal careful reader. You are writing for a distracted, skeptical gatekeeper. Every unclear sentence is a gift to a reviewer who needs a rejection reason.

### Slide-Group 3: Paper Structure — The Adelson Formula (slide 5)

Ted Adelson (MIT) distilled the universal narrative arc of a good technical paper into four moves. Freeman quotes this informally from a 1990 conversation:

1. **State the problem** — keeping the audience in mind. They must care about it. If the problem is not self-evidently important, *tell them why they should care.*
2. **State why existing solutions are unsatisfactory** — briefly. If existing solutions were satisfactory, there would be no need for your work.
3. **Explain your solution and why it is better** — compare directly to prior art.
4. **Related work goes at the end** — work that uses similar techniques on *different* problems. (Note: this is a structural choice; many venues now prefer related work second. The point is that related work is *not* the same as prior failed attempts at your problem, which belongs in step 2.)

Freeman's endorsement: "Since I developed this formula, it seems that all the papers I've written have been accepted." (Adelson, informally, 1990.)

**Example paper structure illustrated** (slides 6–10): Freeman uses "Removing Camera Shake from a Single Photograph" (Fergus, Singh, Hertzmann, Roweis, Freeman, SIGGRAPH 2006) throughout the deck as a running structural example. TOC: Introduction / Related work / Image model / Algorithm / Experiments / Discussion.

### Slide-Group 4: The Introduction — Make It Dynamite (slides 7–8)

Freeman quotes **Jim Kajiya** (SIGGRAPH 1993 Papers Chair):

> "You must make your paper easy to read. You've got to make it easy for anyone to tell what your paper is about, what problem it solves, why the problem is interesting, what is really new in your paper (and what isn't), why it's so neat. And you must do it up front. In other words, you must write a **dynamite introduction**."

What the introduction must accomplish — all up front:
- What problem are you solving?
- Why is the problem interesting / important?
- What is really new here (and what is *not* new — honesty matters)?
- Why is your solution neat?

The introduction is the highest-leverage section. Reviewers often form their acceptance/rejection lean after the introduction alone.

### Slide-Group 5: Use a Toy Example to Explain the Main Idea (slides 9–10)

Freeman flags this as an **underutilized technique**. In the paper TOC, Section 3 ("Main idea") is where a simple, toy example belongs — before the full algorithm.

**Why:** The main idea of a complex method can almost always be illustrated on a small, clean example that lets the reader build intuition before wading into full detail. Freeman shows a figure from his "Shiftable Multiscale Transforms" paper: eight small signal plots that illustrate the shift-sensitivity problem in wavelets with zero prose required.

Concrete advice:
- Show a toy figure that makes the core phenomenon visible.
- Place it in a "Main idea" section between Related Work and the full Algorithm.
- The figure should be self-explanatory: if the reader cannot grasp your main contribution by looking at this one figure, re-design the figure.

### Slide-Group 6: Experiments — The Bar Has Risen (slide 11)

Referencing a CVPR depth estimation paper with six evaluation metrics across multiple datasets, Freeman states bluntly:

> "Gone are the days of, 'We think this is a great idea and we expect it will be very useful in computer vision. See how it works on this meaningless, contrived problem?'"

What modern venues require:
- **Quantitative comparison** on standard benchmarks against current SOTA methods.
- Results that are believable (reviewers are now more sophisticated at spotting cherry-picked metrics or hand-picked test sets).
- The experiments section is no longer a courtesy — it is the load-bearing section for acceptance.

### Slide-Group 7: How to End a Paper (slides 12–13)

**Good ending:** Conclusions, or "what this opens up," or "how this can change how we approach [domain] problems." Endings should leave the reader with a sense of significance and forward momentum.

**Bad ending (Freeman is emphatic):**

> "I can't stand 'future work' sections. It's hard to think of a weaker way to end a paper."

Two anti-patterns he names explicitly:
1. *"Here's a list of all the ideas we wanted to do but couldn't get to work in time for the conference submission deadline. We didn't do any of the following things: (1)..."* — You get no partial credit for neat things you intended to do.
2. *"Here's a list of good ideas that you should now go and do before we get a chance."* — This signals your ideas to competitors before you can execute them.

Better: A conclusion that summarises what was shown, or a forward-looking statement in general terms about where the field may now go. Not a to-do list.

### Slide-Group 8: General Writing Tips — The Knuth / Strunk & White Cluster (slides 14–18)

**Principle 1 — Knuth: Keep the reader uppermost in your mind.**

> "Perhaps the most important principle of good writing is to keep the reader uppermost in mind: What does the reader know so far? What does the reader expect next and why?"

At every paragraph, ask: what does my reader currently know, and what do they need to know next? Write to answer that question, not to document your internal logic.

**Principle 2 — Treat the reader as a guest in your house.**
Anticipate their needs. Guide them. Would you like a drink? Something to eat? Perhaps after eating, you'd like to rest? Good writers provide orientation, motivation, and pacing — just as a good host anticipates what a guest needs before they ask.

**Principle 3 — Strunk & White Rule 13: Omit needless words.**

> "Vigorous writing is concise. A sentence should contain no unnecessary words, a paragraph no unnecessary sentences, for the same reason that a drawing should have no unnecessary lines and a machine no unnecessary parts."

Strunk & White substitution table (all verbatim from the slide):

| Wordy | Concise |
|---|---|
| the question as to whether | whether |
| there is no doubt but that | no doubt |
| used for fuel purposes | used for fuel |
| he is a man who | he |
| in a hasty manner | hastily |
| this is a subject which | this subject |
| His story is a strange one. | His story is strange. |

**Principle 4 — The re-writing exercise (slide 18).**
Freeman shows a before/after paragraph. The "Before" paragraph is 183 words of passive, hedged prose explaining a locality assumption in image processing. The "After" is 119 words — the same content, active voice, no hedging. Key note in blue:

> "This editing benefits you twice: (1) you have 50% more space to tell your story, and (2) the text is easier for the reader to understand."

Compact writing is not just cleaner — it buys you space to say more.

### Slide-Group 9: The Readership Pyramid and Figures (slides 19–20)

**The readership pyramid** (slide 19): A bar chart shows the *conjectured* relative number of readers at each engagement level:

1. **Glance at title** — by far the largest bar (the whole community)
2. **Skim abstract** — substantial fraction
3. **Look at the figures** — slightly fewer than abstract skimmers
4. **Read every word** — the smallest bar

Key note (in blue on slide 19):

> "The 'read every word' readers are your most important ones. But you should make the paper 'work' for all the other readers, too."

Implication: design your paper so that a reader who only glances at the title, skims the abstract, and looks at figures can still walk away with the correct high-level understanding of what you did and why it matters.

**Figures and captions** (slide 20):

> "Probably most of your readers will be skimming the paper."

Three concrete rules:
1. It should be easy to read the paper **in a big hurry** and still learn the main points.
2. The figures and captions can **tell the story** — they are not decorations.
3. Figure captions must be **self-contained**: the caption should tell the reader what to notice about the figure. Do not write "Results of our method." Write what the results show, what to compare, and what conclusion to draw.

Freeman illustrates with a four-panel stereo-depth figure whose caption explicitly tells the reader what each subfigure shows and why the comparison matters (belief propagation removing spurious frame-assignment changes).

### Slide-Group 10: Equations (slides 21–22)

**Knuth on equations:**

> "Many readers will skim over formulas on their first reading of your exposition. Therefore, your sentences should flow smoothly when all but the simplest formulas are replaced by 'blah' or some other grunting noise."

Test: read your paper aloud, replacing every equation with "blah." If the surrounding prose still forms coherent, grammatical, meaningful sentences, the equations are integrated correctly. If the prose becomes incoherent, you have written around equations rather than through them.

**Mermin's Good Samaritan Rule** (slide 22 — from "What's Wrong with These Equations?", *Physics Today*, Oct 1989):

> "When referring to an equation, identify it by a phrase as well as a number."

Never write "inserting (2.47) and (3.51) into (5.13)..." when you can write "inserting the form (2.47) of the electric field E and the Lindhard form (3.51) of the dielectric function ε into the constitutive equation (5.13)..." A reader who has lost their place should be able to re-orient from a single equation reference without hunting backwards through the manuscript.

### Slide-Group 11: Tone — Kindness, Honesty, Reliability (slides 23–25)

**Be kind and gracious** (slide 23). Freeman quotes Efros's related-work paragraph from their texture synthesis paper:

> "A number of papers to be published this year, all developed independently, are closely related to our work... (in particular, see the elegant paper by Hertzmann et al. [11] in these proceedings)."

Note the word "elegant." Freeman's gloss: **"Written from a position of security, not competition."** Disparaging competing work signals insecurity and damages your reputation with reviewers. Acknowledge concurrent work generously.

**Develop a reputation for being clear and reliable** (slide 24):

- There are perceived pressures to over-sell, hide drawbacks, and disparage others' work. **Don't succumb.** (Freeman: "That's in both your long and short-term interests.")
- A conference chair's comment on a best paper award: *"because the author was [name], I knew I could trust the results."* Reputation for reliability is built paper by paper and is ultimately worth more than any single accepted paper.

**Be honest, scrupulously honest** (slide 25):

> "Convey the right impression of performance."

Freeman gives a personal example: MAP estimation of deblurring — they didn't know why it didn't work, so they reported honestly that it didn't work. Later they understood why. Others "went through contortions to show why their method worked." Honest reporting of failure modes is not weakness — it is the foundation of scientific credibility.

### Slide-Group 12: Author List (slide 26)

Freeman's rule of thumb:

- "All that matters is how good the paper is. If more authors make the paper better, add more authors."
- If someone feels they should be an author and you trust them and are on the fence: add them.
- "It's much better to be one of many authors on a great paper than to be one of just a few authors on a mediocre paper."
- The benefit of a paper to you is non-linear in quality: a mediocre paper is worth nothing; only really good papers are worth anything.

This directly reinforces the career-economics argument from slide 2.

### Slide-Group 13: Title (slides 27–28)

Freeman shows a crowded IEEE Transactions on Information Theory table of contents. In a long list of technical titles, most look alike. The question: **what makes a title pop out?**

His personal example: His paper was titled "Shiftable Multiscale Transforms." He argues the better title would have been **"What's Wrong with Wavelets?"** — because it:
- States the problem (something is wrong with wavelets)
- Creates curiosity (what is wrong with them?)
- Is concrete and memorable
- Stands out in a dense TOC

Key principle: A title should be a hook, not a filing label.

### Slide-Group 14: The Reviewer's Checklist — Grounds for Rejection (slide 29)

Freeman lists the most common grounds area chairs use to reject papers. With a 75 %+ rejection mandate, reviewers need only one of these to apply:

1. **Do the authors not deliver what they promise?** (Paper title/abstract promises X; body delivers something weaker or different.)
2. **Are important references missing?** (Suggests the authors are not current on SOTA — damages credibility of the entire contribution.)
3. **Are the results too incremental?** (Too similar to previous work — no clear advance.)
4. **Are the results believable?** (Suspicious numbers, cherry-picked baselines, unverifiable claims.)
5. **Is the paper poorly written?** (Hard to follow → easy to reject.)
6. **Are there mistakes or incorrect statements?** (Any factual error gives cover for rejection.)

**Strategic implication:** Each of these is individually sufficient for rejection. A paper that is strong on five but fails one can still be rejected. Audit your paper against all six before submission.

### Slide-Group 15: Area Chair Taxonomy — Cockroach vs. Puppy (slides 30–32)

From the area chair's perspective, a submission pile contains three types:

- **~1/3 obvious rejects** — weak idea, poor execution. Easy to dismiss.
- **~1 in the whole pile: genuinely excellent** — well-written, great results, good idea. Will be an oral.
- **The rest: borderline.** These fall into two camps:

**The Cockroach:** You try, but you can't find a way to kill this paper. There's nothing exciting about it — incrementally correct, reasonably well-written, reviews are ok. Boring but defensible. About 2/3 of cockroaches get accepted as posters; 1/3 rejected.

**The Puppy with 6 Toes:** A delightful paper — fresh, creative, potentially important — but with one easy-to-point-to flaw. The flaw may be minor (like 6 toes on a puppy), but it gives reviewers a clean hook to reject it. About 2/3 of puppies get rejected; 1/3 accepted as posters. Key advice: **if you have a rejected puppy, address the flaws, resubmit, and it may be selected for an oral.** A puppy with the flaw fixed is a star paper.

**Strategic implication for authors:** Aim to write a puppy, not a cockroach. But then fix the puppy's extra toes before submission. Boring incremental work that is technically correct is not a safe bet — it competes poorly against exciting work that survives resubmission.

### Slide-Group 16: Process Advice — Start Early, Rewrite (slides 33–34)

Freeman closes with a process principle:

> "Good writing is re-writing, and it often helps to put the paper down and return to it fresh later. This means you need to start writing the paper early!"

The implication is operational: you cannot write a good paper in the week before the deadline. The re-reading-with-fresh-eyes step requires elapsed time. Budget for at least two full passes with at least several days between them.

---

## Key Verbatim Anchors

1. **Career impact graph framing (slide 2):** The graph itself is the anchor — impact is near zero for Bad/Ok/Pretty good, then jumps to "Lots of impact" only for "Creative, original and good."

2. **Kajiya on the introduction (slide 8):** "You must make your paper easy to read. You've got to make it easy for anyone to tell what your paper is about, what problem it solves, why the problem is interesting, what is really new in your paper (and what isn't), why it's so neat. And you must do it up front. In other words, you must write a dynamite introduction."

3. **Knuth on the reader (slide 15):** "Perhaps the most important principle of good writing is to keep the reader uppermost in mind: What does the reader know so far? What does the reader expect next and why?"

4. **Strunk & White on concision (slide 17):** "Vigorous writing is concise. A sentence should contain no unnecessary words, a paragraph no unnecessary sentences, for the same reason that a drawing should have no unnecessary lines and a machine no unnecessary parts."

5. **Knuth on equations (slide 21):** "Many readers will skim over formulas on their first reading of your exposition. Therefore, your sentences should flow smoothly when all but the simplest formulas are replaced by 'blah' or some other grunting noise."

6. **Tone in related work (slide 23):** "Written from a position of security, not competition." (Freeman's gloss on Efros's gracious related-work paragraph.)

7. **Reputation as capital (slide 24):** "Because the author was [name], I knew I could trust the results." [conference chair on best paper prize selection]

8. **Future work anti-pattern (slide 13):** "I can't stand 'future work' sections. It's hard to think of a weaker way to end a paper."

9. **Readership pyramid annotation (slide 19):** "The 'read every word' readers are your most important ones. But you should make the paper 'work' for all the other readers, too."

10. **Re-writing process (slide 34):** "Good writing is re-writing, and it often helps to put the paper down and return to it fresh later. This means you need to start writing the paper early!"

11. **Cockroach vs. Puppy (slide 32):** The Cockroach: "You try, but you can't find a way to kill this paper." The Puppy: "A delightful paper, but with some easy-to-point-to flaw. This flaw may not be important (like 6 toes on a puppy), but makes it easy to reject the paper."

12. **Editing ROI (slide 18):** "This editing benefits you twice: (1) you have 50% more space to tell your story, and (2) the text is easier for the reader to understand."

---

## How a Writing Agent Should Use This

**Mode: paper-writing agent, pre-submission audit and structure planning.**

### Concrete agent moves derived from this source:

**Move 1 — Structure audit (Adelson formula).**
Before writing or reviewing a draft, check: Does the introduction follow the four Adelson moves? (Problem → why existing solutions fail → your solution + comparison → related work). If not, restructure.

**Move 2 — Introduction completeness check (Kajiya checklist).**
Scan the introduction against six questions: (a) What problem? (b) Why important? (c) What have others tried? (d) Why do those fail? (e) What is genuinely new here? (f) What is NOT new? All six must be answered up front.

**Move 3 — Toy example check.**
Is there a "Main idea" section between Related Work and the Algorithm with a simple figure that illustrates the core insight? If not, flag this as the highest-leverage addition.

**Move 4 — Figure/caption audit.**
For each figure: can a reader who only looks at figures + captions understand the paper's story? Is each caption self-contained, telling the reader what to notice? Are there "Results of our method" captions that need to be rewritten?

**Move 5 — "Blah" equation test.**
Read the paper aloud, replacing equations with "blah." Flag any sentence that becomes grammatically broken or meaningless — those equations are not properly integrated.

**Move 6 — Rejection checklist (Slide 29 items 1–6).**
Run each rejection criterion as a yes/no question. Flag any "yes" answer for revision.

**Move 7 — Cockroach-or-Puppy diagnosis.**
After reading a draft: is this paper a cockroach (incrementally safe but boring) or a puppy (exciting but with a fixable flaw)? If puppy, identify the extra toes and fix them before submission.

**Move 8 — Conclusion check.**
Does the paper end with conclusions/significance, or with a future-work list? If the latter, rewrite.

**Move 9 — Concision pass (Strunk & White Rule 13).**
Flag passive constructions, wordy phrases, and hedging clauses. Apply the substitution table pattern. Goal: every paragraph should be 20–30 % shorter without losing information.

**Move 10 — Tone check.**
Scan the related-work section for disparaging language. Replace with gracious, security-position phrasing ("see the elegant paper by X").

---

## Connection to SiS / CTPC

Freeman's framework maps directly onto Bilal's writing challenges for CTPC and SiS papers at ML venues (NeurIPS, ICML, ICLR, AISTATS):

**1. The Adelson formula for CTPC.**
The CTPC paper must follow moves 1–4 precisely: (1) SDA orbit prediction is degraded by unmodeled perturbations — SGP4 fails at operationally relevant accuracy; (2) existing ML correctors (black-box NNs) violate conservation laws, produce non-physical covariances, and fail silently; (3) CTPC: a physics-predictor + hard-constraint ML corrector in RTN frame, with dissipation baked into architecture; (4) related work (node, PINN, equivariant nets) as prior art on tools, not on the specific SDA problem.

**2. The toy example gap.**
CTPC papers currently have dense method sections with no toy figure showing the intuition. Freeman's advice: add a "Main idea" section with a 2D toy orbit showing how the physics predictor handles the bulk dynamics and the ML corrector handles the residual — visible as a simple trajectory plot. This is the highest-ROI addition to a CTPC paper draft.

**3. Experiments bar.**
SDA venues and ML venues both now require rigorous baselines. Reviewers will ask: compared to what? CTPC experiments must show quantitative head-to-head against SGP4 alone, black-box NN, and soft-constraint PINN on a standard benchmark (e.g., Space-Track TLE propagation error). "We think this is a great idea" is dead.

**4. The puppy diagnostic for SiS papers.**
A CTPC paper with strong architecture but no real orbital data experiments is a Puppy with 6 toes. The architecture is the delightful part; the missing real-data ablation is the extra toe. Fix the toe before submission; don't submit hoping reviewers won't notice.

**5. Reputation compounding.**
As a PhD student AND a startup CTO, Bilal's name will appear on papers in two roles simultaneously. Freeman's point about reputation-as-capital is amplified: each clear, honest, well-executed paper compounds both research reputation (CSL) and SiS technical credibility.

**6. Figure design for skim readers.**
SDA papers often bury the key result in Table 3. Freeman's advice: design Figure 1 to be the paper's argument in a single image — e.g., a before/after trajectory showing SGP4 error vs. CTPC error on a real conjunction event, with caption that explicitly tells the reader what to conclude.

---

## Connections

- [[Peyton-Jones-Research-Paper]] — complementary structural advice; Peyton Jones's "related work late" position matches Adelson's formula
- [[Langley-Crafting-ML-Papers]] — ML-specific extension of Freeman's general advice; covers ablations, baselines, and claims
- [[Strunk-White-Elements-Style]] — the Strunk & White Rule 13 (omit needless words) is quoted directly in Freeman's deck
- [[Lipton-Writing-Heuristics]] — ML-community heuristics; complements Freeman's reviewer-POV material
- [[Ramdas-Paper-Checklist]] — pre-submission checklist format; pairs with Freeman's rejection-reasons checklist
- [[Bertsekas-Ten-Rules-Mathematical-Writing]] — expands on Freeman's Knuth/Mermin equation advice
- [[Whitesides-Writing-A-Paper]] — complementary practitioner guide from chemistry; similar start-early, re-write philosophy
- [[Goldreich-How-To-Write-Paper]] — theoretical CS perspective; covers related-work and contribution framing
- [[Pak-Clear-Math-Paper]] — math-paper companion to Freeman's general-paper advice

---

## Open Questions

1. **Puppy vs. Cockroach for early-career researchers:** Freeman implicitly assumes you have enough prestige that a puppy can survive resubmission. For an early-career researcher with one shot at a venue per cycle, does the calculus change? When should you submit a cockroach to secure a publication record?

2. **Related work placement:** Adelson's formula puts related work *at the end* (move 4). Many ML venues now expect related work in Section 2. Does moving it early hurt the narrative arc, or is the Adelson formula superseded by community convention?

3. **Toy example in limited page budgets:** Freeman's advice is from a computer vision context (8-page CVPR papers). NeurIPS and ICML have 8–9 page limits with competitive length pressure. Where does a toy example section fit without crowding out experiments?

4. **The honesty-salesmanship tension:** Freeman says "convey the right impression of performance" and be scrupulously honest. But the paper-writing context also requires framing contributions favorably. Where is the exact line between good framing and over-selling? This deserves a synthesis page.

5. **Title construction for SDA/ML hybrid papers:** Freeman's "What's Wrong with Wavelets?" example is a pure methods paper. CTPC spans domain (SDA) + method (physics-constrained ML) + task (orbit prediction). How do you title a paper that must attract both SDA and ML reviewers simultaneously?

---

## Sources

- `raw/writing/papers/guides/Writing_Good_Paper.pdf`
  - Slides 1–2: Career impact; the step-function career graph
  - Slides 3–4: Reviewer reality vs. fantasy; marketplace framing
  - Slide 5: Adelson's four-move paper structure formula
  - Slides 6–7: Running example — camera shake paper TOC
  - Slide 8: Kajiya on the dynamite introduction (SIGGRAPH 1993)
  - Slides 9–10: Toy example as underutilized technique; shiftable transforms figure
  - Slide 11: Experiments bar has risen; CVPR depth estimation example
  - Slides 12–13: Conclusions vs. future-work anti-pattern
  - Slides 14–18: Knuth reader-first principle; hospitality metaphor; Strunk & White Rule 13; before/after rewriting exercise
  - Slides 19–20: Readership pyramid; figures and self-contained captions
  - Slides 21–22: Knuth "blah" test for equations; Mermin Good Samaritan rule
  - Slides 23–25: Gracious tone; reputation for reliability; scrupulous honesty
  - Slide 26: Author list — quality is non-linear
  - Slides 27–28: Title as hook ("What's Wrong with Wavelets?")
  - Slide 29: Six quick grounds for rejection
  - Slides 30–32: Area chair taxonomy — obvious rejects, one star, cockroach, puppy
  - Slides 33–34: Further reading references; good writing is re-writing, start early
