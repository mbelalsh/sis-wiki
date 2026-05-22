---
title: MP-PDE — OpenReview Reviews (ICLR 2022, accepted SPOTLIGHT)
tags: [writing, exemplar-reviews, openreview, iclr-2022, spotlight, neural-pde-solver, reviewer-interaction, containment-framing]
sources:
  - raw/writing/reviews/MessagePassingNPDE-OpenReview.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# MP-PDE — OpenReview Reviews

**Paper:** Brandstetter, Worrall, Welling. *Message Passing Neural PDE
Solvers*. ICLR 2022. **Paper3519.** Paired with [[MessagePassingNPDE]]
structural exemplar. **OpenReview URL fragment:** `forum?id=vSix3HPYKSU`.

This page captures reviewer interaction patterns only, NOT research
content.

**Operationally most important pattern:** **SPOTLIGHT acceptance** at
ICLR 2022 — highest-tier outcome in this 5-paper exemplar corpus. The
other four were all poster acceptances. The factors that produced
Spotlight vs Poster are the most actionable lessons here.

---

## Final Decision

**Accept (SPOTLIGHT).** ICLR 2022 main track.

**PC decision letter (verbatim):**

> "This paper proposes a message passing neural network to solve PDEs.
> The paper has sound motivation, clear methodology, and extensive
> empirical study. However, on the other hand, some reviewers also
> raised their concerns, especially regarding the lack of clear
> notations and sufficient discussions on the difference between the
> proposed method and previous works. Furthermore, there is no
> ablation study and the generalization to multiple spatial resolution
> is not clearly explained. **The authors did a very good job during
> the rebuttal period: many concerns/doubts/questions from the
> reviewers were successfully addressed and additional experiments have
> been performed to support the authors' answers. As a result, several
> reviewers decided to raise their scores, and the overall assessment
> on the paper turned to be quite positive.**"

Three operational signals:

1. **SPOTLIGHT requires near-unanimous strong acceptance.** A 4-paper
   review thread with two 8s and two 6s qualified.
2. **PC explicitly credits rebuttal effort.** The PC noted score
   raises (though the OpenReview record shows fewer than implied —
   see below).
3. **Concerns named in decision letter were not blockers.** Clarity,
   ablations, related-work-differentiation all flagged but didn't
   prevent Spotlight.

---

## Score Distribution

ICLR 2022 uses a **1-10 Recommendation scale** (similar to NeurIPS).

| Reviewer | Recommendation | Confidence | Tech Novelty | Empirical Novelty | Correctness |
|---|---|---|---|---|---|
| **wxXT** | **8 (accept, good paper)** | 4 | **4 (significant + do not exist in prior works)** | **4 (significant + do not exist in prior works)** | 4 |
| **mh3h** | **8 (accept, good paper)** | 4 | 3 (significant + somewhat new) | 3 | 4 |
| 5QFf | 6 (marginally above) → **explicit endorsement (no number change)** | 3 | 2 | 2 | 3 |
| 6VsU | 6 (marginally above) | 4 | 3 | 3 | 3 |

**Final aggregate:** 8, 8, 6, 6 — mean 7.0.

**Critical observation: wxXT gave "4" on BOTH Technical Novelty AND
Empirical Novelty.** This is the **strongest possible novelty rating
at ICLR 2022** ("The contributions are significant, and do not exist
in prior works"). No other exemplar in this corpus has a comparable
score.

**This is what made the difference between Poster and Spotlight:**
**at least one reviewer giving the maximum novelty rating.**

5QFf edited their review with: "*--Edit-- The authors either explained
or addressed the issues in the paper with other reviewers. I am happy
to recommend the paper for publication.*" but did not numerically
change the 6.

---

## What the Strongest Reviewers (wxXT, mh3h — both 8) Praised Specifically

### wxXT — explicit list of 4 pros (verbatim)

1. "The paper tackles the important task of solving many families of
   PDEs, obtaining accuracy and generalization results upon the
   state-of-the-art of neural solvers."
2. **"The approach is well motivated as classical solver schemes
   (FDM, FVM, WENO) are instances of message passing, as the authors
   themselves stated."** — **The containment claim is explicitly
   validated by a reviewer.**
3. **"The pushforward trick is by itself a valuable contribution as
   it enhances the performance of other existing neural solvers
   ([Li et al 2020 a]) as the authors show in Table 2."** — The
   cross-pollination experiment (apply our trick to a competitor)
   directly worked.
4. "The paper is clear and well written, providing sufficient
   background for people not experts in the field."

**Three of four pros are positioning-related** (containment claim,
cross-pollination, clarity-with-background). Only one is
performance-related. **Positioning > performance for novelty score.**

### mh3h — 4 enumerated strengths

1. "Thorough and detailed literature review"
2. "Intuitive explanation of motivation"
3. "Clear methodology and approach used in designing the architecture"
4. "Extensive details in empirical study and design of experiment"

**Same craft elements** [[MessagePassingNPDE]] explicitly invested in
(combined Background+Related Work §2, "splitter vs lumper" rhetorical
lever, three-stage Encoder-Processor-Decoder figure, comprehensive
experiments §4.1-4.4).

---

## What the Weakest Reviewers (5QFf, 6VsU at 6) Objected to

### 5QFf — clarity objections

> "I think the paper has a clarity issues and still in its early stages
> from that perspective."

> "**The METHOD section is very vague and not clearly explained.
> Notation are introduced without definition and no explanation for the
> background needed to understand the notation.** I suggest you lead
> with an example and motivate from there instead of going the general
> route."

> "It is not clear what is the difference between using GNN and other
> regular networks in solving PDEs — the authors did not mention that
> in the paper."

**5QFf later edited:** "The authors either explained or addressed the
issues in the paper with other reviewers. **I am happy to recommend the
paper for publication.**"

### 6VsU — disconnected-contributions critique

> "The ideas of the paper are interesting: the pushforward trick is
> appealing. **The architecture, however, looks like a separate idea,
> which is not connected to the loss. I.e., the paper proposes two
> separate innovations, puts one into the title, but experiments
> confirm the effectiveness of only one.**"

Plus four sub-objections:
- No ablation study
- No parameter-count vs performance analysis
- No convergence analysis with grid pitch
- "Some of the important points are not discussed/analyzed
  experimentally"

**This is a substantive critique:** the title only names the
architecture (Message Passing Neural PDE Solvers), but the
strongest empirical result is for the training trick (pushforward).
**Contribution-claim layering can backfire** if the title doesn't
match the strongest empirical demonstration.

---

## What Rebuttal Responses Changed Scores

The PC's claim that "several reviewers decided to raise their scores"
is **not fully visible in the OpenReview record** — only 5QFf's edit
(no numerical change but explicit endorsement) is documented. The PC
may be counting non-numerical endorsements or score changes from
reviewers not in the visible thread.

### Move 1 — Cross-reviewer-citation defense (5QFf clarity)

Authors deflected 5QFf's clarity concern using **other reviewers as
evidence**:

> "Your point is somewhat in contradiction to reviewers mh3h and wxXT,
> who found the paper is clear and well written. **Nonetheless, it
> would certainly be helpful to indicate where you are having
> difficulties.**"

**This is a notable rhetorical move not seen in other exemplars:**
citing peer-reviewers' positive assessments as evidence in own defense.
Combined with genuine offer to fix specific issues, it worked — 5QFf
endorsed acceptance.

### Move 2 — New Appendix G with CNN-vs-GNN ablation (6VsU)

Authors generated a new appendix:
> "We compared a simple CNN against our GNN architecture for the same
> training setup… **the CNN has roughly twice the accumulated error
> compared to the GNN, for the exact same training setup.**"

This directly addressed 6VsU's "no ablation study" critique.

### Move 3 — Field-shared-limitation framing (6VsU convergence)

Authors conceded the convergence analysis gap honestly:
> "We have not studied the convergence properties of the method with
> grid spacing and indeed we have no guarantees that we will converge
> to exact PDE solutions as grid pitch is taken to zero. **This is, of
> course, a general limitation of purely learned methods, such as
> ours**, and it is indeed an interesting area of research to finding
> hybrid methods…"

Same template as the paper's own Conclusion limitation framing.
**Honest concession + subfield contextualization** to defuse the
objection.

### Move 4 — Spatial-resolution generalization clarification (wxXT)

wxXT's "Cons #1" was the spatial-resolution-generalization
clarification request. Authors added Appendix H mid-rebuttal testing
out-of-domain spatial resolution. Authors explicitly distinguished
"deploy on different resolutions" from "statistical generalization
across resolutions."

This converted a clarity concern into an empirical answer. **wxXT's
score was already 8** so the move maintained rather than raised.

---

## Recurring Objections Across Multiple Reviewers

### 1. **NOTATION / CLARITY — 5QFf + (mildly) 6VsU**

- **5QFf:** "Notation are introduced without definition"
- **6VsU (implicit):** "is not so easy to read"
- **mh3h + wxXT explicitly disagreed:** "clear methodology" / "the
  paper is clear and well written"

**Reviewer disagreement on clarity** is operationally significant.
The math-heavy combined-Background-and-Related-Work approach worked
for 2/4 reviewers and didn't work for 2/4. **For Spotlight tier, 2/4
strong endorsements on clarity was sufficient.**

### 2. **ABLATION STUDIES MISSING — 6VsU only**

Authors added Appendix G during rebuttal. Single-reviewer ablation
concern is addressable.

### 3. **CONVERGENCE ANALYSIS MISSING — 6VsU only**

Authors honestly admitted "not studied" + "general limitation of
purely learned methods" framing. Single-reviewer concern, honestly
admitted, did not block.

### 4. **MULTIPLE-SPATIAL-RESOLUTION GENERALIZATION — wxXT only**

Authors added Appendix H. Single-reviewer concern, fully resolved.

### 5. **NO RECURRING OBJECTIONS** at high severity

Unlike [[GeometryNN-reviews]] (4/4 evaluation-scope concern) or
[[HopCPT-reviews]] (4/4 limitations-section concern), MP-PDE had no
single critique that propagated to 3+ reviewers. **This is unusual
in this corpus** — the typical pattern is at least one
~unanimous critique.

**Operational lesson:** SPOTLIGHT-tier acceptance correlates with
**dispersed, individually-addressable critiques** rather than a
shared substantive concern.

---

## Why This Paper Got SPOTLIGHT — Differentiating Factors from Poster

Compared to the four poster exemplars in this corpus:

| Factor | MP-PDE (Spotlight) | The 4 posters |
|---|---|---|
| **Best novelty rating** | Tech 4 + Empirical 4 (max) from wxXT | Highest seen elsewhere: 3 |
| **Best contribution rating** | "Contributions are significant, and do not exist in prior works" verbatim | "Significant and somewhat new" |
| **Containment framing** | Explicit + validated by reviewer | Not used as positioning move |
| **Cross-pollination experiment** | Applied pushforward to FNO (competitor); reviewer cited as pro | Not present (or limited) |
| **No unanimous critique** | Dispersed individually-addressable | Each had at least one ~unanimous concern |
| **Rebuttal Appendix additions** | 2 new appendices (G, H) generated | Variable, some added |
| **PC mention of score raises** | Explicit | Sometimes |
| **Cross-reviewer-citation defense** | Used (vs 5QFf) | Not observed |

**Single most distinguishing factor:** **at least one reviewer
giving the maximum novelty rating with explicit reasoning tied to
the containment claim.**

For SiS [[CTPC-KDD-Submission]] to aim for Spotlight-tier acceptance
at KDD '26, the containment claim ("CTPC representationally contains
[GMAT-only, Latent-ODE, standard CP] as limit cases") is the most
promising lever.

---

## Patterns Worth Replicating (Spotlight-Tier Specific)

1. **Containment claim that one reviewer can validate explicitly.**
   wxXT's pro #2 verbatim cited the FDM/FVM/WENO containment
   statement. This single move generated maximum novelty rating.
2. **Cross-pollination experiment** — apply own technique to a
   competitor. wxXT's pro #3 verbatim cited the FNO+pushforward
   result. Reviewers respect generosity.
3. **Cross-reviewer-citation defense** when one reviewer disagrees
   on clarity: "Your point is somewhat in contradiction to reviewers X
   and Y, who found the paper clear." Use other reviewers as third-
   party endorsements.
4. **Dispersed-critique optimization:** instead of preventing a
   ~unanimous critique (impossible to do for every weakness), aim for
   distinct individual critiques — each individually addressable
   in rebuttal.
5. **Field-shared-limitation framing** for honest concessions:
   "this is, of course, a general limitation of [subfield X]."
   Acknowledges + contextualizes.
6. **2 new appendices generated mid-rebuttal** (G, H here). PC
   explicitly noted this as score-changing.
7. **Three-stage architecture figure** (Encoder-Processor-Decoder
   in MP-PDE's Figure 3) — mh3h praised "intuitive explanation of
   motivation" tied to such figures.

---

## Patterns to Avoid

1. **Title that only names one of multiple contributions.** 6VsU
   critiqued: "the paper proposes two separate innovations, puts one
   into the title, but experiments confirm the effectiveness of only
   one." MP-PDE survived this but at score cost (6 instead of 7-8
   from 6VsU).
2. **Background + Related Work combined section** can confuse some
   readers. 5QFf wanted leading-with-example. Combined section is
   compact but not always reader-friendly.
3. **Notation introduced without explicit definitions.** 5QFf's
   primary objection. Even with cross-reviewer-citation defense,
   this remained a knock against full score.

---

## Connection to SiS / CTPC

Direct rebuttal-strategy lessons for [[CTPC-KDD-Submission]]:

| MP-PDE pattern | CTPC application |
|---|---|
| Containment claim validated by reviewer | "CTPC representationally contains [GMAT-only as zero corrector] and [Latent ODE as zero physics predictor] as limit cases" — frame formally in §2 |
| Cross-pollination experiment | Apply Student-t prediction head to Latent ODE baseline; apply RTN frame to a non-CTPC corrector; show generality |
| Cross-reviewer-citation defense | "Reviewer X's concern about Y is in contradiction to Reviewers A and B who found Y was a strength" |
| Three-stage architecture figure | GMAT Predictor → NCDE Corrector → Student-t Head as three labeled blocks in Figure 1 |
| 2 new appendices mid-rebuttal | Pre-generate 1-2 ablation experiments to deploy if reviewers ask |
| Dispersed-critique optimization | Don't try to be perfect on every axis; aim for individually-addressable weaknesses |
| Max novelty rating from one reviewer | Frame at least one part of CTPC as "significantly novel — does not exist in prior work" (the specific combination of NCDE + Cholesky-PSD + Student-t + RTN with formal calibration verification) |

---

## Connections

- [[MessagePassingNPDE]] — paired structural exemplar
- [[LiePointSymmetryPINN]] — same author (Brandstetter), follow-up
  paper. MP-PDE's Conclusion pointed to this direction explicitly
- [[HopCPT-reviews]] — contrast: NeurIPS 2023 poster, 4/4 limitations
  critique addressed via mid-paper § subsection. MP-PDE has no such
  recurring critique
- [[GeometryNN-reviews]] — contrast: 4/4 evaluation-scope critique,
  unmovable holdout. MP-PDE had dispersed critiques
- [[Disparate-DeepEns-reviews]] — contrast: 1-2-3-4 distribution
  accepted vs MP-PDE's 8-8-6-6 Spotlight
- [[NeurIPS-2025-Reviewer-Guidelines]] — Q2.4 originality "novel
  combination of existing techniques" is satisfied here at max rating
  (containment of FDM/FVM/WENO)
- [[ICML-2022-Reviewer-Tutorial]] — slide 17 novelty fallacy is the
  failure mode wxXT's max-rating response defeats
- [[CTPC-KDD-Submission]] — direct Spotlight-tier template target

---

## Open Questions

1. **What was 5QFf's pre-edit numerical score?** The 6 may have been
   updated downward from 5 (or up from 4 to 6) before the visible
   edit. OpenReview revision history not fully captured.
2. **Did 6VsU change score?** Score 6 shown but PC said "several
   reviewers decided to raise their scores." If 6VsU stayed at 6, the
   raises may have come from non-visible reviewers (some ICLR threads
   include more than 4 reviewers).
3. **wxXT's "Tech Novelty 4 + Empirical Novelty 4"** — how often does
   ICLR see this combination? Tracking would help calibrate
   expectations for SiS's submissions.

---

## Sources

- `raw/writing/reviews/MessagePassingNPDE-OpenReview.pdf` — 6 pages,
  full OpenReview thread including PC decision, 4 official reviews +
  post-rebuttal comments, 4 author rebuttals (general response + per-
  reviewer responses).
- Paper3519, ICLR 2022, accepted as SPOTLIGHT.
- Note: ICLR 2022 had ~32% acceptance rate; Spotlight typically <5%
  of accepted papers. This is a high-tier outcome.
