---
title: Mathematical Foundations of Geometric Deep Learning
tags: [geometric-dl, math-prerequisites, reference, oxford]
sources:
  - raw/papers/geometric-dl/GeometricDLMath.pdf   # Sáez de Ocáriz Borde & Bronstein 2025, arXiv:2508.02723v1
created: 2026-05-12
updated: 2026-05-12
sis_relevance: medium
hard_constraint_possible: yes
---

# Math Foundations of GDL (Sáez de Ocáriz Borde & Bronstein, 2025)

## One-Line Intuition

The math-prerequisites companion to the Bronstein proto-book — 78 pages of "what you need
to know before reading [[Geometric-Deep-Learning|Bronstein 2021]]," covering sets through
spectral theory through graph theory.

## Why This Was Invented

The 2021 Bronstein proto-book ([[Geometric-Deep-Learning]]) assumes comfort with groups,
manifolds, Hilbert spaces, and spectral methods — material "often overlooked in standard
computer science curricula." This tutorial fills that gap. Originally developed for the
ANAIS 2024 Geometric DL course in Kathmandu, plus Bronstein's USI Lugano (2019) and Oxford
(2024) courses.

## What's In It (Table of Contents)

This page is primarily a **navigational reference** — the contents are standard textbook
material and are not re-summarized here. Use it as a lookup index for "where do I find a
clean exposition of X for GDL purposes."

| § | Topic | Pages | When to consult |
|---|---|---|---|
| 1 | Algebraic Structures (Sets, Maps, Groups, Vector Spaces) | 4–19 | Group axioms, subgroups, homomorphisms, group actions, orbits — foundation for [[Group-Convolution]] |
| 2 | Geometric and Analytical Structures (Norms, Metrics, Inner Products) | 20–25 | Hilbert-space inner product on signal spaces (used implicitly in [[Geometric-Priors]]) |
| 3 | Vector Calculus (Continuity, Differentiability, Scalar/Vector Fields, Derivatives, Integrals, Divergence, Laplacian, Gradient Descent) | 26–36 | Lipschitz functions, smoothness classes; backprop / optimization formalism |
| 4 | Topological Foundations and Differential Geometry (Topology, Manifolds, Manifold Hypothesis) | 37–50 | Smooth manifolds, tangent bundles — foundation for any future ingest of geodesic CNNs / mesh CNNs / Lie-group convolutions |
| 5 | Functional Analysis (Cauchy Sequences, Banach/Hilbert Spaces, Operators) | 51–54 | Operator theory framing for layers as bounded linear maps |
| 6 | Spectral Theory (Eigenfunctions/values, Fourier Analysis) | 55–64 | Foundation for spectral GNNs, graph Fourier transform, Laplacian-based methods |
| 7 | Graph Theory (Preliminaries, Group Theory and Graphs, Vector Fields on Graphs) | 65–78 | Cayley graphs, message-passing foundations |

The treatment is pedagogical (lots of worked examples, side-margin annotations, intuition
before formalism) — closer to a course handout than a research paper.

## Three Things Worth Calling Out

1. **§1.2 Groups → §1.2 Group Actions → §1.2 Group Orbits, Invariance, Equivariance.** The
   §1.2 progression (pages 10–15) gives the cleanest under-50-pages exposition of group
   actions and the invariance/equivariance distinction that I've seen. It also presents
   the continuous-group convolution formula:
   ```
   (f ★ ψ)(x) = ∫_G f(g · x) ψ(g⁻¹) dg
   ```
   with `dg` the Haar measure — the integral version of what [[Group-Convolution]] presents
   discretely. Worth re-reading when continuous-`SO(2)` equivariance becomes the
   operational target for [[PhyArch]].

2. **§4 Manifold Hypothesis.** The "data lives on a low-dimensional manifold embedded in
   high-dimensional ambient space" framing is the geometric justification for why
   [[Geometric-Priors]] work — generic Lipschitz learning is hopeless, but learning on a
   manifold is tractable. This is the precise version of the curse-of-dimensionality
   argument in the Bronstein proto-book.

3. **§7.2 Group Theory and Graphs.** Cayley graphs as visualization of group structure
   (Figure 5 of the doc: triangle symmetry group). When future SDA ingests model satellite
   constellations as graphs, the symmetry group of the constellation can be visualized
   this way.

## Connection to SiS / CTPC

**Reference material, not direct architectural impact.** This doc doesn't change any SiS
design decision. It's the prerequisite layer — what you'd hand a new SiS team member who
needs to read the [[Geometric-Deep-Learning]] proto-book or the geometric-dl agent's
corpus.

Specific operational links:

- The Hilbert-space inner product on signal spaces (§2.3) is implicitly assumed in
  [[Analytic-Sigma-CTPC-Composition]]'s analytic-Σ-NCDE theorem (state of an NCDE lives in
  a Hilbert space; covariance is the Gram matrix under that inner product).
- The manifold hypothesis (§4.4) is the formal justification for PhyArch's geometric-feature
  embedding — orbital state lives on a low-dimensional submanifold of `ℝ⁶`.
- §6 Spectral Theory groundwork for any future graph-Fourier-based corrector if SDA moves
  to constellation-level modeling.

## Connections

- [[Geometric-Deep-Learning]] — the proto-book this companion is built for.
- [[Geometric-Priors]] — symmetry + scale separation built on the group-action / inner-product
  formalism §1.2 + §2.3.
- [[Group-Convolution]] — operational realization of the §1.2 group-action machinery.

## Open Questions

This is a textbook — by design it has none. Open questions live in the papers it grounds.

## Sources

- Sáez de Ocáriz Borde, H. & Bronstein, M. M. (2025). *Mathematical Foundations of
  Geometric Deep Learning.* University of Oxford. arXiv:2508.02723v1, 1 Aug 2025. Pages
  1–15 read for this ingest (Introduction, §1.1 Sets, §1.2 Groups + Group Actions + Orbits
  /Invariance/Equivariance). Pages 16–78 (vector spaces, geometric/analytical structures,
  vector calculus, differential geometry, functional analysis, spectral theory, graph
  theory) are referenced in the TOC table above and deferred to topic-driven re-ingest when
  a SiS milestone requires the specific math.
