---
title: Freeman — How to Write a Good Paper (CVPR 2020 tutorial)
tags: [writing, slide-deck, freeman, cvpr, structural-arc, reviewer-pov]
sources:
  - raw/writing/guides/Freeman writing papers 2020.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Freeman — How to Write a Good Paper

**Author:** William T. Freeman, Massachusetts Institute of Technology +
Google Research. **Venue:** CVPR 2020 tutorial on writing reviews, June 14,
2020. **Format:** 34-slide deck. **Note:** explicitly recommended as the
*starter* in [[ICML-Best-Practices]]: "As a starter, we highly recommend
Bill Freeman's slides on how to write good papers."

## Structural Arc (Slide-by-Slide Architecture)

Per user instruction: structural arc, not per-slide content.

### Part 1: Why it matters (slides 1-4)

- **Slide 1**: Title page (Freeman, MIT + Google, CVPR tutorial 2020).
- **Slide 2**: *Paper's impact on career* — sharp nonlinearity: bad/ok/
  pretty-good papers have ~no career effect; **creative, original, good
  papers have huge effect**. The career-payoff function is essentially
  step-function near "creative+original+good." Implication: aim for the
  step, don't optimize bad → ok.
- **Slide 3**: Our image of research community — "scholars, plenty of
  time, pouring over your manuscript."
- **Slide 4**: The reality — "more like a large, crowded marketplace."
  Reviewers and readers are time-starved; the paper must succeed in a
  noisy environment.

### Part 2: Structure (slides 5-7)

- **Slide 5**: **Ted Adelson's 4-point paper structure** (told informally,
  conversation 1990): (1) state the problem and why the audience must
  care, (2) state existing solutions and why they're unsatisfactory,
  (3) explain your solution, compare with others, say why it's better,
  (4) at the end, related work where similar techniques have been used
  on different problems. *"Since I developed this formula, it seems that
  all the papers I've written have been accepted."*
- **Slide 6**: Example paper organization (Fergus et al. "Removing
  Camera Shake from a Single Photograph"). Real CV paper TOC = 1 Intro,
  2 Related work, 3 Image model, 4 Algorithm (with sub-sections), 5
  Experiments (sub-cases), 6 Discussion.
- **Slide 7**: The introduction = the most critical section.

### Part 3: The introduction + main idea (slides 8-11)

- **Slide 8**: **Jim Kajiya's dictum** — "**Write a dynamite
  introduction.**" Must make it easy for anyone to tell what the paper
  is about, what problem it solves, why interesting, what's new, why
  it's neat.
- **Slide 9**: Underutilized technique — **explain the main idea with a
  simple toy example**. Often useful between intro and full algorithm.
- **Slide 10**: Show simple toy examples (visual example of wavelet
  representation of a translated signal — illustrates main idea before
  general theory).

### Part 4: Experiments + ending (slides 11-13)

- **Slide 11**: **Experimental results are critical now at CVPR.**
  "Gone are the days of 'We think this is a great idea and we expect
  it will be very useful... See how it works on this meaningless,
  contrived problem?'"
- **Slide 12**: How to end a paper — **Conclusions or what this opens
  up, or how this can change how we approach problems**.
- **Slide 13**: **How NOT to end a paper** — Freeman: *"I can't stand
  'future work' sections. It's hard to think of a weaker way to end a
  paper."* Bad future-work patterns: (i) "Here's a list of all the
  ideas we couldn't get to work in time" — no partial credit for
  unrealized ideas. (ii) "Here's a list of good ideas you should now
  go do before we get a chance" — gives away the next paper.

### Part 5: General writing tips (slides 14-18)

- **Slide 14**: General writing tips heading.
- **Slide 15**: **Knuth** — *"Perhaps the most important principle of
  good writing is to keep the reader uppermost in mind: What does the
  reader know so far? What does the reader expect next and why?"*
- **Slide 16**: **Treat the reader as a guest in your house** — anticipate
  their needs (food, drink, rest = orientation, examples, summary).
- **Slide 17**: **Strunk & White Rule 13 — Omit needless words.**
  "Vigorous writing is concise." Sample compressions:
  | Wordy | Concise |
  |---|---|
  | the question as to whether | whether |
  | there is no doubt but that | no doubt / doubtless |
  | used for fuel purposes | used for fuel |
  | he is a man who | he |
  | in a hasty manner | hastily |
  | this is a subject which | this subject |
- **Slide 18**: Re-writing exercise. Before/after example with 50%
  compression. *"This editing benefits you twice: (1) you have 50% more
  space to tell your story, and (2) the text is easier for the reader
  to understand."*

### Part 6: Reader behavior + figures (slides 19-23)

- **Slide 19**: **Readership funnel** — most readers glance at title;
  fewer skim abstract; fewer look at figures; very few read every word.
  *"The 'read every word' readers are your most important ones. But you
  should make the paper 'work' for all the other readers, too."*
- **Slide 20**: **Figures and captions** — "It should be easy to read
  the paper in a big hurry and still learn the main points... Most of
  your readers will be skimming the paper." **The caption should tell
  the reader what to notice about the figure.**
- **Slide 21**: **Knuth on equations** — *"Many readers will skim over
  formulas on their first reading of your exposition. Therefore, your
  sentences should flow smoothly when all but the simplest formulas are
  replaced by 'blah' or some other grunting noise."*
- **Slide 22**: **Mermin's Good Samaritan rule** — *"When referring to
  an equation identify it by a phrase as well as a number."* Bad:
  "inserting (2.47) and (3.51) into (5.13)..." Good: "inserting the
  form (2.47) of the electric field E and the Lindhard form (3.51) of
  the dielectric function ε into the constitutive equation (5.13)..."
- **Slide 23**: **Tone: be kind and gracious.** Efros example
  (texture-synthesis paper) treating competitors generously. *"Written
  from a position of security, not competition."*

### Part 7: Reputation + author list + title (slides 24-28)

- **Slide 24**: **Develop a reputation for being clear and reliable.**
  "There are perceived pressures to over-sell, hide drawbacks, and
  disparage others' work. Don't succumb. (That's in both your long and
  short-term interests.)" Conference-chair quote re best-paper prize:
  *"Because the author was [trusted], I knew I could trust the results."*
- **Slide 25**: **Be honest, scrupulously honest.** "Convey the right
  impression of performance." Example: Freeman's MAP-estimation deblur
  result — "We didn't know why it didn't work, but we reported that it
  didn't work."
- **Slide 26**: **Author list pragmatics.** "All that matters is how
  good the paper is. If more authors make the paper better, add more
  authors." "It's much better to be one of many authors on a great
  paper than to be one of just a few authors on a mediocre paper."
  Mediocre paper ≈ worth nothing; only really good papers worth
  anything (re-states slide 2's nonlinearity).
- **Slide 27**: Title — example of bad ToC layout (IEEE Trans Info Theory).
- **Slide 28**: **Our title** example — what was published vs what it
  *should have been*. "Shiftable Multiscale Transforms" → "What's Wrong
  with Wavelets?" The provocative title attracts more readers.

### Part 8: Reviewer point of view (slides 29-34)

- **Slide 29**: **Quick and easy reasons to reject a paper.** "With the
  task of rejecting at least 75% of the submissions, area chairs are
  groping for reasons to reject a paper":
  - Do the authors not deliver what they promise?
  - Are important references missing (and therefore one suspects the
    authors not up on the state-of-the-art)?
  - Are the results too incremental?
  - Are the results believable?
  - Is the paper poorly written?
  - Are there mistakes or incorrect statements?
- **Slide 30**: **Area chair's pile**: about 1/3 obvious rejects; ~1
  out of the pile is a really nice paper (oral presentation). **The
  rest are borderline.** "These fall into two camps..." (slides 31-34
  presumably elaborate the borderline categories — not captured in
  this ingest read, but the structural point is made).

## Core Actionable Rules (Compressed)

1. **Aim for the step-function career impact.** Optimize for "creative,
   original, good", not "ok → pretty good."
2. **Use Ted Adelson's 4-point structure** for the introduction
   (problem → existing failure → my solution → related work at end).
3. **Write a dynamite introduction** (Kajiya): what / why interesting
   / what's new / why neat.
4. **Use a toy example to explain the main idea** before the full
   algorithm.
5. **Experimental results are critical** — vague-promise + contrived-
   problem is dead.
6. **No "future work" sections.** End with conclusions / what this
   opens up.
7. **Keep the reader uppermost in mind** (Knuth).
8. **Treat the reader as a guest** — anticipate their needs.
9. **Omit needless words** (Strunk-White Rule 13).
10. **Rewrite for 50% compression** — frees space + helps reader.
11. **Different reader populations** (title-glancers → every-word
    readers) all need served.
12. **Figure captions self-contained** — tell reader what to notice.
13. **Equations skimmable** — Knuth's "blah test": sentences flow when
    equations replaced with "blah".
14. **Equation reference by phrase + number** (Mermin Good Samaritan).
15. **Tone gracious, not competitive.** Be kind to competitors.
16. **Reputation for clarity + reliability + honesty.** Long-term
    investment.
17. **Be scrupulously honest.** Report failures + uncertainties.
18. **Author list: maximize paper quality** rather than per-author
    credit fraction.
19. **Title should provoke**, attract readers.

## Reviewer-Facing Implications

Slide 29's "quick and easy reasons to reject" IS the reviewer-side
audit checklist:

- Deliver what you promise (matches [[Lipton-Writing-Heuristics]]
  #11 hostage-to-fortune)
- Cite contemporary literature (matches [[ICML-2022-Review-Form]]
  Literature axis)
- Avoid incremental-only positioning
- Make results believable (matches Soundness)
- Good writing (matches Quality of writing/presentation)
- No mistakes (matches Soundness)

Slide 30's "75% rejection / 1 oral / rest borderline" calibrates the
**stakes**: most papers are borderline, decided by small writing-
quality margins.

## ML-Paper-Specific Advice (Distinguished from General Writing)

Freeman is CVPR-calibrated, so heavily ML/CV-applicable:

- Slide 11 (experimental results critical now at CVPR) reflects the
  modern empirical bar.
- Slide 30 (~75% rejection rate) calibrates the ML conference reality.
- Slide 20 (figures + captions) particularly applies to CV's
  figure-heavy norms (overlaps with [[Lipton-Writing-Heuristics]]
  #7 caveat about CV figure conventions).

General-writing crossover: Slides 15-18 (Knuth, Strunk-White, rewriting)
are universal.

## Connection to SiS / CTPC

- **Adelson's 4-point structure** maps directly to
  [[CTPC-KDD-Submission]]: (1) orbital state prediction — readers care
  (SDA stakes), (2) Latent ODE baselines fail (d̄² > 20,000), (3) our
  CTPC achieves d̄² ≈ 1 with 64% MSE reduction, (4) related work on
  hybrid physics-ML at the end.
- **Toy example slide 9** — for CTPC, the toy could be a single TLE +
  J2 perturbation example showing the predictor-corrector decomposition
  before the full Latent NCDE framework.
- **No future-work** — when CTPC paper is finalized, end on what CTPC
  *enables* (SDA pipeline integration, conjunction analysis Pc
  computation), not "things we didn't do."
- **Figure captions self-contained** — applies to PHNN diagram, Cholesky
  parameterization diagram, and benchmark-comparison plots.
- **Quick reject reasons** — pre-submission Bilal-side audit: do we
  deliver what we promise? Are references current? Is calibration
  result believable?
- **Title** — "Continuous-Time Probabilistic Corrector for Orbital
  Prediction" vs more provocative alternative. Worth a Freeman-style
  audit.

## Connections

- [[ICML-Best-Practices]] — recommends this deck as the starter
- [[ICML-2022-Review-Form]] — the reviewer Freeman is modeling
- [[Lipton-Writing-Heuristics]] — Lipton heuristic 8 (contribution
  before page 5) parallels Freeman slide 8 (dynamite introduction)
- [[Strunk-White-Elements-Style]] — explicitly quoted on slide 17
- [[Peyton-Jones-Research-Paper]] — overlapping content; Peyton-Jones
  more focused on suggestions, Freeman more focused on reviewer POV
- [[CTPC-KDD-Submission]] — pre-submission audit target

## Open Questions

1. **Slides 31-34 (area chair borderline category breakdown) not
   captured.** Worth a follow-on read closer to KDD '26 submission.

## Sources

- `raw/writing/guides/Freeman writing papers 2020.pdf` — 34 slides, William T.
  Freeman, MIT + Google Research. CVPR 2020 tutorial on writing reviews,
  June 14, 2020. 5.7 MB (largest file in corpus).
- Slides 1-30 read; slides 31-34 inferred from structural context only.
