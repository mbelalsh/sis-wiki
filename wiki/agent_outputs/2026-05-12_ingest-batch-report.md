# Ingest Batch Report — 2026-05-12

**Trigger:** user-requested batch ingest of 10 papers across geometric-dl,
neural-odes, physics-informed, and formal-verification domains.

**Result:** 9 successful new ingests + 1 re-verification (no failures);
12 new wiki pages created + 2 existing pages updated; 3 agents promoted from
SHALLOW to READY.

---

## 1. Per-Ingest Status

| # | Source PDF | Status | Pages created / updated |
|---|---|---|---|
| 1 | `geometric-dl/GeometricDL.pdf` (Bronstein et al. 2021, 160p proto-book) | ✅ Success | papers/Geometric-Deep-Learning + concepts/Geometric-Priors |
| 2 | `geometric-dl/GroupEquiCNN.pdf` (Cohen & Welling 2016, ICML, 12p) | ✅ Success | papers/Group-Equivariant-CNN + concepts/Group-Convolution |
| 3 | `geometric-dl/GeometricDLMath.pdf` (Sáez de Ocáriz Borde & Bronstein 2025, 78p) | ✅ Success | books/Math-Foundations-of-GDL (first wiki/books/ entry) |
| 4 | `neural-odes/NODE.pdf` (Chen et al. 2018, NeurIPS, 18p) | ✅ Success | papers/NODE + concepts/Adjoint-Sensitivity-Method |
| 5 | `neural-odes/DissectingNODE.pdf` (Massaroli et al. 2020, NeurIPS, 23p) | ✅ Success | papers/Dissecting-NODE; updated concepts/Adjoint-Sensitivity-Method (generalized adjoint) |
| 6 | `physics-informed/PINNChallenges.pdf` (Krishnapriyan et al. 2021, NeurIPS, 13p) | ✅ Success | papers/PINN-Challenges |
| 7 | `physics-informed/MLWithPhyKnowledge.pdf` (Watson et al. 2025, TMLR, 61p) | ✅ Success | papers/PIML-Survey; updated concepts/Adjoint-Sensitivity-Method (stiff-system caveat) |
| 8 | `physics-informed/EncodingPhysics.pdf` (Rao et al. 2023, Nat. Mach. Intell., 18p) | ✅ Re-verification | No new pages — already canonical at papers/PeRCNN (ingested 2026-05-08); re-read confirmed existing summary accurate |
| 9 | `formal-verification/FVerify_NODE.pdf` (Zeqiri et al. 2023, ICLR, GAINS, 28p) | ✅ Success | papers/GAINS-Certified-NODE |
| 10 | `formal-verification/SafeDNN_NASA.pdf` (Pasareanu et al., NASA Ames, 37p slide deck) | ✅ Success | papers/SafeDNN-NASA |

**Failures: 0.** All 10 source PDFs were under canonical `raw/papers/<topic>/`
(none in `_inbox/`); no CLAUDE.md triage rule triggered.

---

## 2. New Wiki Pages

**12 new pages** (page count 33 → 45):

### Papers (8)
- `wiki/papers/Geometric-Deep-Learning.md`
- `wiki/papers/Group-Equivariant-CNN.md`
- `wiki/papers/NODE.md`
- `wiki/papers/Dissecting-NODE.md`
- `wiki/papers/PINN-Challenges.md`
- `wiki/papers/PIML-Survey.md`
- `wiki/papers/GAINS-Certified-NODE.md`
- `wiki/papers/SafeDNN-NASA.md`

### Concepts (3)
- `wiki/concepts/Geometric-Priors.md`
- `wiki/concepts/Group-Convolution.md`
- `wiki/concepts/Adjoint-Sensitivity-Method.md`

### Books (1, first entry)
- `wiki/books/Math-Foundations-of-GDL.md`

**Existing pages updated:** `wiki/concepts/Adjoint-Sensitivity-Method.md` was
extended in both ingests 5 (generalized adjoint for path-distributed losses) and
7 (stiff-system / high-Lyapunov caveat). Note: it was *created* in ingest 4 and
*touched* by 5 and 7 — counted once in the new-pages list.

**`wiki/index.md`** and **`wiki/log.md`** updated after every ingest.

---

## 3. Lint Findings

| Check | Status |
|---|---|
| Page counts consistent across `index.md` + filesystem | ✅ 45 pages across 7 dirs (concepts 14, architectures 6, papers 16, books 1, connections 1, sis 5, agents 2) |
| All 12 new pages have valid `sis_relevance` frontmatter | ✅ Verified — values: 4 critical (Geometric-DL, Geometric-Priors, NODE, PINN-Challenges) + 6 high (Group-Equivariant-CNN, Group-Convolution, Adjoint, Dissecting-NODE, PIML-Survey, SafeDNN-NASA) + 1 medium (Math-Foundations-of-GDL) + 1 critical (GAINS) |
| All 12 new pages have `hard_constraint_possible: yes` | ✅ Verified |
| No pages with `hard_constraint_possible: unknown` | ✅ Clean |
| Broken wikilinks | ⚠️ One historical reference `[[Port-Hamiltonian-Neural-Networks]]` (plural) in `wiki/log.md` lines 13 + 20 — both inside append-only log entries from 2026-05-09 and 2026-05-10. The 2026-05-10 log explicitly documents the singular rename. **Not actionable** per append-only log philosophy. |
| Orphan check (inbound wikilinks on new pages) | ⚠️ Imperfect — some new pages are referenced only by other new-this-batch pages (e.g. Geometric-Deep-Learning ↔ Group-Equivariant-CNN ↔ Group-Convolution form a self-contained cluster). All new pages ARE wikilink-reachable from existing wiki content via the SiS/CTPC connection sections + index entries; no true orphans. |
| `wiki/agent_outputs/` directory | ⚠️ **New directory, not yet in CLAUDE.md canonical schema.** Same status as `wiki/agents/` (codified 2026-05-12). Flag for next CLAUDE.md update if Bilal wants the agent-outputs layer formalized. |

**No contradictions detected** between new pages and existing content. All
new pages explicitly link to existing pages they touch (PhyArch,
CTPC-Design-Rationale, Latent-NCDE-Corrector, PHNN, etc.).

---

## 4. Agent Promotions (Relaxed Criterion: 5+ Total Wiki Pages Anchored)

| Agent | Before | New pages added by this batch | After | Status |
|---|---|---|---|---|
| **Hamiltonian-NN-Agent** | READY (18 pages) | — | READY (18 pages) | unchanged |
| **Interpretability-Agent** | READY (6 pages) | — | READY (6 pages) | unchanged |
| **Geometric-DL-Agent** | SHALLOW (0) | Geometric-Deep-Learning, Geometric-Priors, Group-Equivariant-CNN, Group-Convolution, Math-Foundations-of-GDL (5) | **READY (5 pages)** | ✅ **PROMOTED** |
| **Neural-ODE-Agent** | SHALLOW (2 pre-existing: NCDE concept, Latent-NCDE-Corrector) | NODE, Dissecting-NODE, Adjoint-Sensitivity-Method (3) | **READY (5 pages)** | ✅ **PROMOTED** |
| **Physics-Informed-Agent** | SHALLOW (4 pre-existing: PeRCNN paper + 3 concept pages) | PINN-Challenges, PIML-Survey (2) | **READY (6 pages)** | ✅ **PROMOTED** |
| **Formal-Verification-Agent** | THIN (0) | GAINS-Certified-NODE, SafeDNN-NASA (2) | SHALLOW (2 pages) | bootstrapped, needs 3 more to hit relaxed threshold |
| **SDA-Agent** | THIN (0) | — | THIN (0) | unchanged |
| **Symbolic-Physics-Agent** | THIN (0) | — | THIN (0) | unchanged |
| **Uncertainty-Propagation-Agent** | THIN/SHALLOW (2) | — | THIN/SHALLOW (2) | unchanged |
| **Neural-Operators-Agent** | THIN (0) | — | THIN (0) | unchanged |

**Net effect:** 3 new READY agents (Geometric-DL, Neural-ODE,
Physics-Informed) — the agent-council layer now has **5 of 10 domains**
ready to wire into a retrieval system, vs 2 before this batch.

### Recommended next steps to promote remaining SHALLOW/THIN agents

- **Formal-Verification-Agent → READY**: ingest 3 more from
  `raw/papers/formal-verification/` (14 PDFs available). Priority candidates:
  Reluplex (NEUROSPF cites it), DeepCert (SAFECOMP'21), Probabilistic Analysis of NNs
  (SEAMS'20/ISSRE'20 — directly relevant to probabilistic Latent NCDE Corrector).
- **Neural-Operators-Agent → SHALLOW**: ingest both PDFs already in
  `raw/papers/neural-operators/` (CNOperator, GroupEquiFNO). Flagged explicitly by
  PIML-Survey §3.4 as the closest SiS wiki gap.
- **Symbolic-Physics-Agent → SHALLOW**: ingest `symbolic-physics/MCranmerPhDThesis.pdf`
  — already heavily forward-referenced from `Pi-Block-Polynomial-Approximator.md` but
  the thesis itself is unsummarized.
- **Uncertainty-Propagation-Agent → READY**: ingest
  `uncertainty-propagation/MaybeUncertaitnyProp.pdf` + add probabilistic-NN-verification
  thread from formal-verification corpus to bridge to UQ.
- **SDA-Agent → SHALLOW**: ingest the 4 existing SDA PDFs (`Aurora_FoundationModel`,
  `ClimaX`, `FourCastNet`, `SpaceWeather`).

---

## 5. SiS-Specific Highlights (the "what was surprising" cross-batch)

Captured here because they touch multiple agent corpora and didn't fit cleanly
into any single page's SiS connection section.

1. **Q5 of [[CTPC-Design-Rationale]] is now operationally addressable.**
   Pre-batch it was the most aspirational of the deferred questions. Post-batch,
   the GAINS + SafeDNN pair gives a concrete two-pronged verification toolkit
   (continuous-time ODE solver + discrete NN components). Both have direct
   PhysioNet / ACAS-Xu precedent benchmarks at SDA-relevant scale.

2. **Q8b-discrete-time has new candidates.** The original three (discrete gradient
   method, AVF, Gauss-collocation) are joined by GAINS's CAS (Controlled Adaptive
   Solver) — discrete exponential step-size grid that retains adaptive efficiency.
   CAS is not itself structure-preserving; whether it composes with a symplectic
   correction is an open question.

3. **The "Mind your input networks" warning (Dissecting NODE §5.3) is a directly
   testable PhyArch+CTPC ablation.** If PhyArch alone (no NCDE corrector) gets
   near PhyArch+CTPC performance on some regime, the corrector is wasted capacity
   there. New entry on the experimental-questions backlog.

4. **PINN-Challenges is the foundational citation for the SiS thesis.**
   Krishnapriyan et al.'s "vanilla PINNs fail at 90% relative error on
   closed-form-solvable PDEs because soft regularization ill-conditions the loss
   landscape" is the empirical evidence that the SiS design hierarchy is correct.
   Should be cited every time a hard-vs-soft choice comes up in CTPC discussion.

5. **Bronstein's "Mind your input networks" warning (Dissecting NODE) + Krishnapriyan
   bootstrapping issue + Watson PIML survey §3.3 framing all converge:** the
   "physics-informed loss" literature has acquired a substantial body of empirical
   evidence that soft physics constraints don't work well. SiS's hard-constraint
   thesis is no longer a niche position; it's where this thread of literature is
   landing.

6. **Two new PHNN extensions worth tracking for future PHNN agent ingest:**
   - **Roth et al. 2025** (cited in PIML-Survey §3.2): Lyapunov + global stability
     via convex Hamiltonian with strict minimum.
   - **GAINS-style certified training of PHNN with `Ḣ ≤ 0` as verification target**:
     unpublished open SiS research question seeded by the GAINS ingest.

7. **CBM-CTPC + Programmatic Explainability convergence:** the k=9 concept layer in
   [[CBM-CTPC-Composition]] *is* the SCENIC-style vocabulary that SafeDNN's
   Programmatic Explainability uses to extract semantic rules. This is the
   operational realization of Barbiero §2.4.1 structural invariance for an
   orbital-mechanics-trained user. Year 1 + Year 3 of the SiS arc line up tightly.

---

## 6. Schema Notes

- `wiki/agent_outputs/` is a **new directory** introduced by this batch report.
  Not yet in CLAUDE.md canonical schema (`wiki/agents/` was codified 2026-05-12;
  `wiki/agent_outputs/` is the natural sibling for ephemeral agent deliverables).
  Recommended codification: add to directory tree with description like
  *"Ephemeral agent-produced reports — batch reports, audit summaries, lint
  results. Distinct from `wiki/agents/` (persistent agent definitions). Files
  here MAY be deleted; canonical knowledge belongs in `wiki/concepts/`,
  `wiki/papers/`, etc."*

- **No new wiki frontmatter spec needed.** This report is a single deliverable
  with no agent_name; standard CLAUDE.md frontmatter would arguably apply, but
  the `agent_outputs` directory is for ephemeral reports rather than wiki
  content pages. Frontmatter is omitted from this file by design.

---

## 7. Log Trail

12 log entries appended during this batch (10 ingests + agent-create entries
from earlier in the day + 2 follow-up CLAUDE.md updates):

```
grep "^## \[2026-05-12\]" wiki/log.md
```

A summary entry for the batch run will be appended after this report is
written.
