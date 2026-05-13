---
title: ML with Physics Knowledge for Prediction — A Survey
tags: [physics-informed, survey, neural-odes, hamiltonian-nn, port-hamiltonian, pinn, neural-operators, geometric-dl, navigational]
sources:
  - raw/papers/physics-informed/MLWithPhyKnowledge.pdf   # Watson et al. 2025 (TMLR, arXiv:2408.09840v2)
created: 2026-05-12
updated: 2026-05-12
sis_relevance: high
hard_constraint_possible: yes
---

# PIML Survey (Watson et al. 2025)

## One-Line Intuition

A 61-page navigational map of how physics knowledge enters machine learning — across
architectures (NODE, HNN, LNN, PHNN), objectives (PINN, variational PINN), data (operator
learning, neural operators), and learning algorithms (multi-task, meta-learning) — with
explicit alignment table mapping methods to "solve simulation / infer system / generalize
across systems."

## Why This Was Invented

By 2024 the PIML literature had ballooned (Karniadakis et al. 2021, Karpatne et al. 2022,
Wang & Yu 2021 covered subsets); no single survey treated *all four* entry points for
physics knowledge — model, objective, data, algorithm — under one taxonomy. Watson et
al. (TMLR, May 2025) close that gap, with explicit industrial perspective (ABB
co-authors) and a structured table (Table 1 of the paper) mapping each section to which
of {solve simulation, infer system, generalize across systems} it serves.

## What's In It (Navigational Table)

This page functions primarily as a **map** to existing SiS wiki pages. The survey itself
is a textbook-style reference; I won't restate canonical content already in wiki pages.

| Survey § | Topic | Maps to existing wiki | Status in SiS wiki |
|---|---|---|---|
| §2.1 | ODEs (analytical/numerical, explicit/implicit, Euler/RK4) | [[Math-Foundations-of-GDL]] §3 | covered |
| §2.4 | Function approx with NNs (FFNN, RNN, attention, GNN) | implicit baseline | — |
| §3.1 | Neural ODEs + adjoint method + augmented NODE | [[NODE]], [[Adjoint-Sensitivity-Method]], [[Dissecting-NODE]] | covered |
| §3.2 | Algebraic structures: DeLAN, LNN, HNN, port-Hamiltonian | [[LNN]], [[HNN]], [[D-HNN]], [[Dissipative-SymODEN]], [[Port-Hamiltonian-Neural-Network]] | covered |
| §3.3 | Physics-informed losses: PINN, variational PINN, separable PINN | [[PINN-Challenges]] (failure modes) | covered (criticism) |
| §3.4 | Neural operators: DeepONet, FNO, CNO | _not yet_ — `raw/papers/neural-operators/{CNOperator, GroupEquiFNO}` un-ingested | gap |
| §3.5 | Latent variable models | [[Latent-NCDE-Corrector]] (NCDE variant) | partial |
| §3.6 | System identification | [[CTPC-KDD-Submission]] (SDA-specific instance) | partial |
| §3.7 | Model invariances (geometric DL) | [[Geometric-Deep-Learning]], [[Group-Equivariant-CNN]], [[Group-Convolution]], [[Geometric-Priors]] | covered |
| §4.1 | Multi-task learning | _not yet_ | gap |
| §4.2 | Meta-learning | _not yet_ | gap |
| §4.3 | Neural processes | _not yet_ | gap |

**Coverage assessment.** The SiS wiki has substantial overlap with §§3.1, 3.2, 3.3, 3.7 of
the survey (the "model and objective" axes). Operator learning (§3.4) and the
multi-task/meta-learning thread (§§4.1–4.3) are wiki gaps — future ingest priorities.

## Three Things Worth Calling Out (Specific to SiS)

1. **§3.1 raises a SiS-relevant caveat on the adjoint method.** "The gradient estimation
   based on the adjoint method is error-prone due to the numerical inaccuracies of solving
   the backward ODE, due to the typical high Lyapunov characteristic exponent of highly
   nonlinear ODEs" (citing Gholaminejad et al. 2019; Onken & Ruthotto 2020; Zhuang et al.
   2020, 2021a; Krishnapriyan et al. 2023). **For orbital dynamics this matters** — high
   Lyapunov exponents are characteristic of chaotic and stiff regimes (close approaches,
   resonance crossings). Mitigations cited: improved backward integrators, checkpointing,
   constraints on solver step size. Added as caveat to [[Adjoint-Sensitivity-Method]] in
   this ingest.

2. **§3.2 §"Parameterizing dynamical system equations" recapitulates the full
   HNN/LNN/PHNN taxonomy SiS already covers**, citing Lutter & Peters 2023 (DeLAN),
   Greydanus et al. 2019 (HNN), Toth et al. 2019 (Hamiltonian flow from pixels), Chen
   et al. 2019 (SRNN — symplectic recurrent NN with leapfrog), Jin et al. 2020 (separable
   and non-separable Hamiltonians), **Roth et al. 2025 (Lyapunov + global stability
   constraints into port-Hamiltonian NNs by enforcing the Hamiltonian to be convex and
   possess a strict minimum)** — the last is a **new structural extension** beyond what
   [[Port-Hamiltonian-Neural-Network]] currently captures. **Flag this for future ingest**:
   convex-strict-minimum Hamiltonian as an additional SiS hard-constraint mechanism for
   stability proofs.

3. **§3.3 cites Krishnapriyan et al. 2021 ([[PINN-Challenges]])** on the "bootstrapping"
   issue and the curriculum/seq2seq remedies. The survey's framing is more conciliatory
   than the original paper — treating the failure modes as engineering challenges to
   manage rather than fundamental indictments of the soft-constraint approach. **The SiS
   reading remains the harder one**: when hard architectural encoding is available
   (PhyArch, PHNN, G-CNN), the entire bootstrapping pathology disappears, so the
   curriculum/seq2seq engineering is unnecessary.

## Connection to SiS / CTPC

**This survey is the navigational landmark for the SiS agent council.** Specifically:

1. **Geometric-DL agent corpus.** §3.7 of the survey maps cleanly to the geometric-dl
   agent's domain — equivariance, group convolution, gauge theory framing. The survey
   confirms that the geometric-dl thread sits within the "model invariances" axis of
   physics-informed learning.

2. **Hamiltonian-NN agent corpus.** §3.2 of the survey is the Hamiltonian-NN agent's
   domain in textbook form — DeLAN / LNN / HNN / port-Hamiltonian taxonomy. The survey's
   Equation 16 is exactly the port-Hamiltonian form `[q̇; ṗ] = (J − D(q))∇H + g(q)u` already
   in [[Port-Hamiltonian-Systems]]. The Roth et al. 2025 convex-Hamiltonian extension is a
   new candidate for the Hamiltonian-NN agent's corpus.

3. **PINN-Challenges and the soft-constraint thread.** §3.3 of the survey provides the
   broader context for [[PINN-Challenges]] — situating it within the practical engineering
   literature on PINN training (sampling, optimization, loss weighting). The SiS
   conclusion remains: prefer hard architectural priors when available.

4. **Neural operators gap.** §3.4 is the most striking SiS wiki gap — DeepONet, FNO,
   CNO are unsummarized. The neural-operators agent (`raw/papers/neural-operators/` has 2
   PDFs) lacks any wiki content. This survey's §3.4 is the right introductory ingest
   target to bootstrap that agent.

5. **CTPC = §3.5 latent variable + §3.6 system identification + §3.7 model invariances.**
   CTPC sits at the intersection of three survey sections: latent NCDE (§3.5),
   spacecraft-trajectory system ID from sparse data (§3.6), and PhyArch's
   symmetry-hardwiring (§3.7). The survey doesn't have a section explicitly for
   "compositions" of these — that's the SiS contribution.

**Hard vs soft constraint.** The survey adopts a *taxonomic* stance (just classifying
where physics enters) rather than a *prescriptive* one (preferring hard over soft). SiS
takes the prescriptive view explicitly via the CLAUDE.md design hierarchy. The two views
are compatible — SiS chooses from the survey's taxonomy according to the hierarchy.

## Connections

- [[Geometric-Deep-Learning]], [[Group-Equivariant-CNN]], [[Group-Convolution]],
  [[Geometric-Priors]] — survey §3.7 (model invariances).
- [[NODE]], [[Adjoint-Sensitivity-Method]], [[Dissecting-NODE]] — survey §3.1 (Neural ODEs).
- [[HNN]], [[LNN]], [[D-HNN]], [[Dissipative-SymODEN]],
  [[Port-Hamiltonian-Neural-Network]], [[Port-Hamiltonian-Systems]] — survey §3.2
  (algebraic structures).
- [[PINN-Challenges]] — survey §3.3 (physics-informed losses).
- [[Latent-NCDE-Corrector]], [[CTPC-KDD-Submission]] — survey §3.5 + §3.6.
- [[CTPC-Design-Rationale]] — SiS-specific composition of survey §§3.2 + 3.5 + 3.7.

## Open Questions

1. **Operator learning ingest gap.** §3.4 introduces DeepONet, FNO, neural operators —
   the wiki has zero content here. Priority next batch: ingest
   `raw/papers/neural-operators/CNOperator.pdf` and `GroupEquiFNO.pdf` to bootstrap the
   neural-operators agent.

2. **Convex Hamiltonian extension (Roth et al. 2025).** Survey §3.2 mentions this as a
   PHNN extension that enforces Lyapunov and global stability via convexity + strict
   minimum of `H`. Worth a focused ingest if the paper is locatable —
   tracks for Q5 (formal verification) of [[CTPC-Design-Rationale]] as well as Q8b
   (`Ḣ ≤ 0` is the energy-decreasing condition; convexity + strict minimum gives the
   Lyapunov function explicitly).

3. **Multi-task / meta-learning for cross-spacecraft generalization.** Survey §§4.1–4.2
   covers the "generalize across systems" column of Table 1. SiS hasn't engaged this
   thread yet but it's directly relevant for SDA — one CTPC trained on multiple
   spacecraft should generalize to a new spacecraft with limited data. Future-batch
   ingest target.

## Sources

- Watson, J., Song, C., Weeger, O., Gruner, T., Le, A. T., Pompetzki, K., Hendawy, A.,
  Arenz, O., Trojak, W., Cranmer, M., Bülow, F., Goyal, T., Peters, J., Hoffmann, M. W.
  (2025). *Machine Learning with Physics Knowledge for Prediction: A Survey.* Transactions
  on Machine Learning Research, May 2025. arXiv:2408.09840v2. Pages 1–15 read for this
  ingest (Abstract, §1 Introduction, §2 Background, §3.1 Learning Differential Models
  from Data, §3.2 Learning Models with Algebraic Structures, §3.3 Physics-Informed Losses,
  §3.4 Operator Learning intro). Pages 16–61 (rest of §3.4, §3.5–3.7, §4 Learning, §5
  Industrial Perspective, References) deferred — survey-style content; topic-driven
  re-ingest when a specific SiS milestone requires the depth.
