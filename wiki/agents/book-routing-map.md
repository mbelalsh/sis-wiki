---
title: Book Routing Map — raw/books/ → SiS Agent Domains
tags: [agents, routing, foundation-corpus, books, ingest-planning]
sources:
  - raw/books/classical-mechanics/Arnold_1989_MMCM.pdf
  - raw/books/classical-mechanics/GoldsteinPooleSafkoClassicalMechanics.pdf
  - raw/books/classical-mechanics/Fundamentals_of_Astrodynamics_VersieRepository_250115.pdf
  - raw/books/classical-mechanics/Orbital Mech.Eng. Student-BOOK.pdf
  - raw/books/differential-geometry/Intro_Manifolds.pdf
  - raw/books/differential-geometry/Differential_topology.pdf
  - raw/books/dynamical-systems/Khalil-3rd.pdf
  - raw/books/dynamical-systems/Port_Hamiltonian_Book.pdf
  - raw/books/ml-theory/GaussianProcesses.pdf
created: 2026-05-12
updated: 2026-05-13
sis_relevance: critical
hard_constraint_possible: n/a
---

# Book Routing Map — raw/books/ → SiS Agent Domains

## Purpose

This routing map governs which chapters of which books in `raw/books/` will be ingested
into the wiki, and which agent each ingested chapter will ground. It is the planning
artifact between Step 1 (inventory) and Step 3 (ingest priority recommendation).

**Status:** Step 2 of the foundation-agent build. **No ingest has occurred yet.**

## Identification — All 9 Books Confirmed

| File | Author/Title | Edition | Pages |
|------|--------------|---------|-------|
| `raw/books/classical-mechanics/Arnold_1989_MMCM.pdf` | Arnold, *Mathematical Methods of Classical Mechanics* | Springer GTM 60, 2nd ed (1989) | 536 |
| `raw/books/classical-mechanics/GoldsteinPooleSafkoClassicalMechanics.pdf` | Goldstein, Poole, Safko, *Classical Mechanics* | 3rd ed (Addison-Wesley) | 665 |
| `raw/books/classical-mechanics/Fundamentals_of_Astrodynamics_VersieRepository_250115.pdf` | Karel F. Wakker, *Fundamentals of Astrodynamics* | TU Delft Repository (Jan 2015) | 690 |
| `raw/books/classical-mechanics/Orbital Mech.Eng. Student-BOOK.pdf` | Howard D. Curtis, *Orbital Mechanics for Engineering Students* | Elsevier 1st ed (2005) | 692 |
| `raw/books/differential-geometry/Intro_Manifolds.pdf` | Loring W. Tu, *An Introduction to Manifolds* | Springer Universitext 2nd ed (2008) | 350 |
| `raw/books/differential-geometry/Differential_topology.pdf` | **Bott & Tu, *Differential Forms in Algebraic Topology* (GTM 82, 1982)** — note filename is misleading | 3rd revised printing | 342 |
| `raw/books/dynamical-systems/Khalil-3rd.pdf` | Hassan K. Khalil, *Nonlinear Systems* | Prentice Hall 3rd ed (2002) | 767 |
| `raw/books/dynamical-systems/Port_Hamiltonian_Book.pdf` | van der Schaft & Jeltsema, *Port-Hamiltonian Systems Theory: An Introductory Overview* | Foundations and Trends in Systems and Control (2014) | 228 |
| `raw/books/ml-theory/GaussianProcesses.pdf` | Rasmussen & Williams, *Gaussian Processes for Machine Learning* | MIT Press (2006) | 266 |

Total: ~4,536 pages across 9 books.

## Agent Domains (Targets)

The wiki currently has 5 READY agents and several SHALLOW/future agents. Routing
targets are:

- **HNN** = Hamiltonian-NN-Agent (status: ready)
- **INT** = Interpretability-Agent (status: ready)
- **GDL** = Geometric-DL-Agent (status: ready)
- **NODE** = Neural-ODE-Agent (status: ready)
- **PIML** = Physics-Informed-Agent (status: ready)
- **SDA** = future Space Domain Awareness / orbital-mechanics agent (not yet created)
- **UQ** = future Uncertainty-Quantification agent (not yet created)
- **SKIP** = chapter is not relevant to any current or near-term agent; do not ingest

CTPC-Design-Rationale anchors (where ingested content will pin to existing wiki):
Part I D1-D7 (decisions), Part II Q1-Q8 (open questions), Part III Barbiero four-symmetry
framework, Core Design Philosophy (hard-vs-soft hierarchy).

---

## Book 1 — Arnold, *Mathematical Methods of Classical Mechanics* (536 p)

The geometric / symplectic textbook canonical reference. Rigorous, manifold-based.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| **Part I Newtonian Mechanics** | | | | |
| 1 Experimental facts | 3-14 | SKIP | LOW | — |
| 2 Investigation of equations of motion | 15-54 | SKIP | LOW | — |
| **Part II Lagrangian Mechanics** | | | | |
| 3 Variational principles (§15 Hamilton's eq, §16 Liouville) | 55-74 | HNN | MEDIUM | D6, Part III |
| 4 Lagrangian on manifolds (§18 Manifolds, §19 Lagrangian dynamics, §20 Noether) | 75-97 | HNN, GDL | MEDIUM | D7, Part III, Q1 |
| 5 Oscillations | 98-122 | SKIP | LOW | — |
| 6 Rigid bodies | 123-162 | SDA | MEDIUM | (future) |
| **Part III Hamiltonian Mechanics** | | | | |
| **7 Differential forms** | 163-200 | GDL | MEDIUM | Q1, Q2 |
| **8 Symplectic manifolds (§38 Hamiltonian phase flow, §40 Lie algebra of H functions)** | 201-232 | **HNN** | **HIGH** | **D6, Q8b-discrete-time, Part III** |
| **9 Canonical formalism (§47 Hamilton-Jacobi)** | 233-270 | HNN | HIGH | Q8b, Part III |
| 10 Perturbation theory (§52 averaging) | 271-300 | SDA | MEDIUM | (future) |
| **Appendices** | | | | |
| App 5 Dynamical systems with symmetries | 371-380 | HNN, GDL | MEDIUM | Q1, Part III |
| App 6 Normal forms of quadratic Hamiltonians | 381-396 | HNN | LOW | D6 |
| App 14 Poisson structures | 456-481 | HNN | MEDIUM | Q8b, Part III |
| (Other appendices) | | SKIP | LOW | — |

**Per-agent yield:** HNN-priority chapters total ~110 pages (Ch 8 + 9 alone).

---

## Book 2 — Goldstein, Poole, Safko, *Classical Mechanics 3rd ed* (665 p)

The engineering-physics canonical reference. Less geometric, more computational than
Arnold. Significant overlap with Arnold in Hamilton/canonical chapters.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Elementary Principles (§1.5 dissipation function) | 1-33 | HNN | LOW | D6 |
| 2 Variational Principles + Lagrange (§2.6 conservation, symmetry, Noether) | 34-69 | HNN, GDL | MEDIUM | D7, Q1, Part III |
| 3 Central Force Problem (Kepler, three-body) | 70-133 | SDA | MEDIUM | (future) |
| 4 Kinematics of Rigid Body (Euler angles, SO(3)) | 134-183 | GDL, SDA | MEDIUM | Q1 |
| 5 Rigid Body Equations of Motion (§5.5 Euler eq, §5.8 Precession of satellite orbits) | 184-237 | SDA | MEDIUM | (future) |
| 6 Oscillations (damped/driven, Josephson) | 238-275 | SKIP | LOW | — |
| 7 Classical Mechanics of Special Relativity | 276-333 | SKIP | LOW | — |
| **8 Hamilton Equations of Motion (Legendre transformations, §8.6 least action)** | 334-367 | **HNN** | **HIGH** | **D6, Q8b** |
| **9 Canonical Transformations (§9.4 Symplectic approach, §9.5 Poisson brackets, §9.8 Symmetry groups, §9.9 Liouville)** | 368-429 | **HNN, GDL** | **HIGH** | **Q1, Q8, Part III** |
| 10 Hamilton-Jacobi Theory and Action-Angle Variables | 430-482 | HNN, SDA | MEDIUM | Q8b, (future) |
| 11 Classical Chaos (§11.2 KAM, §11.4 Lyapunov exponents) | 483-525 | NODE | MEDIUM | NODE-stiffness, Q8 |
| 12 Canonical Perturbation Theory | 526-557 | SDA | LOW | (future) |
| 13 Continuous Systems and Fields (§13.7 Noether) | 558-624 | SKIP (advanced) | LOW | — |
| App A Euler Angles, App B Groups and Algebras | 625-665 | GDL | LOW | Q1 |

**Decision rule for HNN Hamilton/canonical content:** prefer **Goldstein Ch 8-9** over
Arnold Ch 8-9 for first ingest because (a) more accessible engineering notation matches
PHNN literature, (b) Poisson brackets in §9.5 directly grounds Casimirs in PH-book Ch 8,
(c) Liouville's theorem §9.9 grounds the symplectic-volume-preservation argument for
Q8b. Defer Arnold's geometric treatment to a follow-up "rigorous foundation" round.

---

## Book 3 — Wakker, *Fundamentals of Astrodynamics* (TU Delft, 690 p)

Spacecraft orbital mechanics from the foundations-of-dynamics viewpoint. Heavy on
analytical orbit perturbation theory (J2, drag, third-body) and reference frames.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Some basic concepts (§1.3 deterministic and chaotic motion, Newton, gravity) | 1-24 | SDA | LOW | (future) |
| 2 Many-body problem (integrals of motion, barycenter, n-body) | 25-44 | SDA | MEDIUM | (future) |
| 3 Three-body problem (Jacobi integral, Hill surfaces, Lagrange libration, invariant manifolds) | 45-102 | SDA | MEDIUM | (future) |
| 4 Relative motion in many-body problem (sphere of influence) | 103-116 | SDA | LOW | (future) |
| 5 Two-body problem (conservation laws, eccentricity vector, Roche, relativistic) | 117-156 | SDA | MEDIUM | (future) |
| 6 Elliptical and circular orbits (Kepler's eq, Lambert) | 157-180 | SDA | MEDIUM | (future) |
| 7-8 Parabolic, Hyperbolic orbits | 181-202 | SKIP for now | LOW | — |
| **9 Relative motion of two satellites (Clohessy-Wiltshire eq, RTN-frame baseline)** | 203-218 | **SDA, HNN** | **HIGH** | **D2 (RTN frame definition), Q8** |
| 10 Regularization (Burdet, Stiefel) | 219-242 | SKIP | LOW | — |
| **11 Reference frames, coordinates, time, orbital elements (§11.5 orbital elements, §11.8-11.11 transformations)** | 243-284 | **SDA** | **HIGH** | (future SDA agent foundation) |
| 12-15 Orbit transfers, rendezvous | 285-378 | SKIP | LOW | — |
| 16-19 Launching, lunar, interplanetary, low-thrust | 379-526 | SKIP | LOW | — |
| **20 Perturbing forces and perturbed satellite orbits (§20.1 Earth gravity, §20.2 atmospheric drag, §20.6 special/general perturbations methods)** | 527-554 | **SDA** | **HIGH** | **(future SDA: physics-predictor baseline)** |
| 21 Elementary analysis of orbit perturbations (J2-term, drag, radiation) | 555-584 | SDA | MEDIUM | (future) |
| 22 Method of variation of orbital elements (Lagrange/Gauss planetary equations) | 585-612 | SDA | MEDIUM | (future) |
| 23 Orbit perturbations due to Earth's gravity field (secular variation, periodic variation, Kaula) | 613-668 | SDA | MEDIUM | (future) |

**Routing note:** Wakker is SDA-agent material. **Defer ingest until SDA agent is
created.** No chapters route to the 2 currently ready agents (HNN ground for §9 is
weak — Clohessy-Wiltshire is more relevant as data definition than as dynamical core).

---

## Book 4 — Curtis, *Orbital Mechanics for Engineering Students* (Embry-Riddle, 692 p)

Undergraduate-targeted, MATLAB-illustrated orbital mechanics. Less analytical depth than
Wakker but more algorithmic / implementation-oriented.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Dynamics of point masses (kinematics, Newton, gravitation, relative motion) | 1-31 | SDA | LOW | (future) |
| 2 Two-body problem (energy, conic sections, perifocal frame, Lagrange coefficients, restricted three-body) | 33-105 | SDA | MEDIUM | (future) |
| 3 Orbital position as function of time (Kepler eq, universal variables) | 107-147 | SDA | MEDIUM | (future) |
| **4 Orbits in three dimensions (state vector ↔ orbital elements, §4.7 Earth oblateness J2)** | 149-191 | **SDA** | **HIGH** | (future SDA agent) |
| 5 Preliminary orbit determination (Gibbs, Lambert, Gauss methods) | 193-253 | SDA | MEDIUM | (future) |
| 6 Orbital maneuvers (Hohmann, plane change) | 255-313 | SKIP | LOW | — |
| 7 Relative motion and rendezvous (Clohessy-Wiltshire) | 315-345 | SDA | MEDIUM | (future) |
| 8 Interplanetary trajectories | 347-397 | SKIP | LOW | — |
| 9 Rigid-body dynamics (Euler angles, Euler's eq) | 399-473 | SDA | MEDIUM | (future) |
| 10 Satellite attitude dynamics | 475-549 | SDA | LOW | (future) |
| 11 Rocket vehicle dynamics | 551-579 | SKIP | LOW | — |
| App D MATLAB algorithms (Kepler solver, state-vector from elements, Gibbs, Lambert, Gauss) | 595-655 | SDA | MEDIUM | (future: reference implementations) |

**Routing note:** Curtis is also SDA-agent material; defer entirely. **Curtis vs.
Wakker decision for SDA agent:** Wakker is the deeper analytical text; Curtis is the
algorithm-illustrated companion. Both are valuable when SDA agent is built — Wakker
for derivation depth, Curtis for MATLAB algorithm patterns to port to Python.

---

## Book 5 — Tu, *An Introduction to Manifolds* (350 p)

Modern, exposition-friendly differential geometry. Bilal's main DG reference. Strong
candidate for grounding Geometric-DL agent's continuous-SO(2)/SO(3) ambitions.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| **Part I Euclidean Spaces** | | | | |
| 1 Smooth Functions | 5-10 | SKIP | LOW | — |
| 2 Tangent Vectors as Derivations | 11-18 | GDL | LOW | (foundational) |
| 3 Alternating k-Linear Functions | 19-31 | GDL | LOW | (foundational) |
| 4 Differential Forms on Rⁿ | 33-42 | GDL | MEDIUM | (groundwork for Ch 17-19) |
| **Part II Manifolds** | | | | |
| 5 Manifolds (topological, charts, smooth) | 47-55 | GDL | MEDIUM | (foundational) |
| 6 Smooth Maps | 57-62 | GDL | LOW | (foundational) |
| 7 Quotients (projective space) | 63-73 | SKIP | LOW | — |
| **Part III The Tangent Space** | | | | |
| 8 The Tangent Space (point, differential, chain rule, curves) | 77-90 | GDL, NODE | MEDIUM | NODE-flow-grounding |
| 9 Submanifolds (regular level set, examples) | 91-99 | GDL | LOW | — |
| 10 Categories and Functors | 101-104 | SKIP | LOW | — |
| 11 The Rank of a Smooth Map (constant rank, immersions, submersions, tangent plane to surface) | 105-117 | GDL | LOW | — |
| **12 The Tangent Bundle (vector bundles, smooth sections, smooth frames)** | 119-126 | **GDL** | **HIGH** | **Q1 (vector bundles for equivariance)** |
| 13 Bump Functions and Partitions of Unity | 127-134 | SKIP | LOW | — |
| **14 Vector Fields (smoothness, integral curves, local flows, Lie bracket, push-forward)** | 135-147 | **NODE, GDL** | **HIGH** | **D2 (NODE flow theory), Q1** |
| **Part IV Lie Groups and Lie Algebras** | | | | |
| **15 Lie Groups (examples, subgroups, matrix exponential, trace, differential of det)** | 149-159 | **GDL** | **HIGH** | **Q1 (continuous SO(2)/SO(3) — currently missing in wiki!)** |
| **16 Lie Algebras (tangent space at identity, gl/sl/so, left-invariant vector fields, Lie bracket, differential as Lie algebra homomorphism)** | 161-171 | **GDL** | **HIGH** | **Q1 (so(3), so(2) for continuous group convolution)** |
| **Part V Differential Forms** | | | | |
| 17 Differential 1-Forms | 175-180 | GDL | LOW | — |
| 18 Differential k-Forms (wedge product, invariant forms on Lie group) | 181-187 | GDL | LOW | — |
| 19 The Exterior Derivative | 189-197 | GDL | LOW | — |
| **Part VI Integration** | | | | |
| 20 Orientations | 201-209 | SKIP | LOW | — |
| 21 Manifolds with Boundary | 211-220 | SKIP | LOW | — |
| 22 Integration on a Manifold (Stokes' theorem, Green's theorem) | 221-232 | SKIP | LOW | — |
| **Part VII De Rham Theory** | | | | |
| 23-28 De Rham cohomology, Mayer-Vietoris, Homotopy invariance | 235-280 | SKIP | LOW | (out of scope) |
| App A Point-Set Topology | 281-292 | SKIP | LOW | — |

**Routing note:** Tu Ch 14-16 is the single highest-yield differential-geometry block
in the corpus for SiS: it's exactly the Lie-group / Lie-algebra machinery the
Geometric-DL agent flags as missing in its system prompt
(`raw/papers/geometric-dl/` corpus handles discrete groups; SiS needs continuous SO(2)/SO(3)).
**This is the highest-priority ingest for the GDL agent.**

---

## Book 6 — Bott & Tu, *Differential Forms in Algebraic Topology* (GTM 82, 342 p)

**Filename `Differential_topology.pdf` is misleading — the PDF is actually Bott-Tu's
algebraic topology text via differential forms.** Advanced graduate material: de Rham
cohomology, Mayer-Vietoris spectral sequences, Thom isomorphism, Euler class.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| Ch I De Rham Theory (§1-7) | 13-87 | SKIP for now | LOW | — |
| Ch II Čech-de Rham Complex (§8-12) | 89-139 | SKIP | LOW | — |
| Ch III Spectral Sequences and Applications | (later) | SKIP | LOW | — |
| Ch IV Characteristic Classes | (later) | SKIP | LOW | — |

**Routing note:** **DEFER ENTIRELY.** Bott-Tu is too advanced and too specialized for
current SiS needs. The relevant de Rham material (Stokes, differential forms,
basic cohomology) is covered more accessibly in Tu's *Introduction to Manifolds*
Parts V-VII, which are already deprioritized as LOW. Bott-Tu becomes relevant
later if/when SiS wants gauge-theoretic / characteristic-class machinery for
equivariant neural architectures — a Year 2+ concern.

---

## Book 7 — Khalil, *Nonlinear Systems 3rd ed* (767 p)

The engineering-systems canonical reference. Lyapunov, passivity, input-output stability.
**The most operationally important book in the corpus for the Hamiltonian-NN agent.**

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Introduction (§1.2.5 ANN example) | 1-34 | SKIP | LOW | — |
| 2 Second-Order Systems (phase portraits, bifurcation) | 35-86 | SKIP | LOW | — |
| **3 Fundamental Properties (existence/uniqueness §3.1, continuous dependence, sensitivity)** | 87-110 | **NODE** | **MEDIUM** | **NODE Picard uniqueness (NODE §6)** |
| **4 Lyapunov Stability (Autonomous §4.1, Invariance principle §4.2, Linearization §4.3, Comparison functions §4.4, Nonautonomous §4.5, Converse theorems §4.7, ISS §4.9)** | 111-194 | **HNN, NODE, PIML** | **HIGH** | **D6, Q5, Q8b (Lyapunov certificate is the foundation of "Ḣ ≤ 0" claim)** |
| 5 Input-Output Stability (L stability, small-gain) | 195-226 | HNN | MEDIUM | Q8b (PHNN-NODE composition) |
| **6 Passivity (Memoryless §6.1, State Models §6.2, Positive Real §6.3, L₂ and Lyapunov §6.4, Passivity Theorems §6.5, Feedback Interconnection §6.6)** | 227-262 | **HNN** | **CRITICAL** | **D6 — direct grounding for PHNN's `Ḣ ≤ 0` passivity argument** |
| 7 Frequency Domain Analysis | 263-302 | SKIP | LOW | — |
| 8 Advanced Stability Analysis (center manifold, region of attraction) | 303-338 | NODE | LOW | NODE-stiffness |
| **9 Stability of Perturbed Systems (vanishing perturbation, nonvanishing perturbation, comparison method)** | 339-380 | **HNN, NODE** | **HIGH** | **Q8b-discrete-time (integrator error ≈ perturbation)** |
| 10 Perturbation Theory and Averaging | 381-422 | SDA | LOW | (future) |
| 11 Singular Perturbations (slow/fast manifolds) | 423-468 | NODE | LOW | NODE-stiffness |
| 12 Feedback Control | 469-504 | SKIP | LOW | — |
| 13 Feedback Linearization | 505-550 | SKIP | LOW | — |
| **14 Nonlinear Design Tools (§14.4 Passivity-Based Control)** | 551-end | **HNN** | **MEDIUM** | **D6 (PBC bridges PHNN to control design)** |

**Routing note:** Khalil Ch 4 + Ch 6 are the single most operationally important
ingest pair in the corpus for the Hamiltonian-NN agent. Together they ground the
core SiS claim that `Ḣ ≤ 0` is not a mere name but a Lyapunov-passivity certificate
sourced to standard nonlinear-systems theory.

---

## Book 8 — van der Schaft & Jeltsema, *Port-Hamiltonian Systems Theory* (228 p)

**The single canonical citation for the entire SiS Port-Hamiltonian thread.**
This is the reference the Hamiltonian-NN agent's commitments are downstream of.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Introduction (origins of PH theory) | 3-10 | HNN | LOW | (context) |
| **2 From modeling to port-Hamiltonian systems (port-based modeling, Dirac structures, energy storage/dissipation, external ports, PH dynamics, PH-DAEs, detailed-balanced chemical networks)** | 11-40 | **HNN** | **CRITICAL** | **D6 — canonical PH definition: `dx/dt = (J-R)∇H + g(x)u`** |
| 3 PH systems on manifolds (modulated Dirac, integrability) | 41-52 | HNN, GDL | MEDIUM | D6, Q1 |
| **4 Input-state-output PH systems (linear resistive, ISO PH, memristive dissipation, relation to classical Hamiltonian)** | 53-62 | **HNN** | **HIGH** | **D6, Q8b** |
| 5 Representations of Dirac structures | 63-68 | HNN | LOW | (advanced) |
| 6 Interconnection of PH systems (composition of Dirac structures) | 69-74 | HNN | MEDIUM | Q8b composition |
| **7 PH systems and passivity (linear PH, available/required storage, shifted PH + passivity)** | 75-82 | **HNN** | **HIGH** | **D6 — the passivity proof that connects to Khalil Ch 6** |
| 8 Conserved quantities + algebraic constraints (Casimirs, dissipation obstacle) | 83-90 | HNN | MEDIUM | Q8b (conservation laws) |
| 9 Incrementally PH systems (incremental/differential passivity) | 91-100 | HNN | LOW | — |
| 10 Input-output Hamiltonian systems (with dissipation, positive feedback) | 101-110 | HNN | LOW | — |
| 11 Pseudo-gradient representations (Brayton-Moser) | 111-117 | SKIP | LOW | — |
| (Remaining chapters beyond p117 not yet TOC'd; likely advanced control / observers) | | TBD | LOW | — |

**Routing note:** van der Schaft & Jeltsema is the **non-negotiable** ingest for
the Hamiltonian-NN agent. Without it, the agent's PHNN commitments are
unfounded — they currently sit on the Furieri et al. 2024 paper alone, which
itself cites this book repeatedly.

---

## Book 9 — Rasmussen & Williams, *Gaussian Processes for Machine Learning* (266 p)

The canonical GP4ML reference. Important for variance-head theory (Q2) and as a
Bayesian-baseline / interpretability comparison point.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Introduction (Bayesian modelling) | 1-6 | UQ | LOW | (foundational) |
| **2 Regression (Weight-space view, Function-space view, Smoothing, Equivalent Kernels, Marginal Likelihood)** | 7-32 | **UQ, INT** | **HIGH** | **Q2 (variance head as function-space posterior), Part III mech-interp baseline** |
| 3 Classification (Laplace approx, EP, multi-class) | 33-78 | SKIP for now | LOW | — |
| **4 Covariance Functions (Stationary, Dot Product, Eigenfunction Analysis, Kernels for non-vectorial)** | 79-104 | **UQ, GDL** | **MEDIUM** | **Q1 (kernel choice under SO(2) equivariance), Q2** |
| 5 Model Selection and Adaptation of Hyperparameters | 105-128 | UQ | MEDIUM | — |
| 6 Relationships between GPs and Other Models (RKHS, regularization, splines, SVMs, RVMs) | 129-150 | UQ | LOW | (cross-method context) |
| 7 Theoretical Perspectives (Equivalent kernel, PAC-Bayesian) | 151-170 | UQ | MEDIUM | Q5 (PAC-Bayes is a verification angle) |
| 8 Approximation Methods for Large Datasets (Nyström, sparse GP) | 171-188 | UQ | LOW | (scaling) |
| **9 Further Issues (Multiple outputs §9.1, Noise models §9.2, Non-Gaussian §9.3, Derivative observations §9.4, Prediction with uncertain inputs §9.5)** | 189-198 | **UQ** | **MEDIUM** | **Q2 (multi-output GP for vector variance), Q8 (uncertain inputs ≈ moment propagation)** |
| App A Math Background | 199-206 | SKIP | LOW | — |
| **App B Gaussian Markov Processes (Continuous-time GMP, SDE on circle, discrete-time GMP)** | 207-220 | **UQ, NODE** | **MEDIUM** | **D2 (continuous-time stochastic latent ≈ Latent-NCDE prior)** |

**Routing note:** R&W routes mainly to the future UQ agent, with secondary
relevance to Interpretability (Ch 2 as a Bayesian-regression interpretability
baseline against which CBM-CTPC and HNN-CTPC can be compared) and Geometric-DL
(Ch 4 covariance functions for equivariance-respecting kernels).

---

## ml-theory/ — Four New Books (Added 2026-05-13)

Four additional books were dropped into `raw/books/ml-theory/` after the
initial 9-book routing map was written, bringing ml-theory/ to 5 books total
(R&W Ch 9 above is the existing fifth). The 4 new books are routed primarily
to a future **ML-Theory-Agent** (abbreviated **MLT** below, alongside the
existing `SDA-future` and `UQ-future` precedent) that has not been created
yet. Slot is reserved; agent file will be bootstrapped when the corpus
crosses the 5-page ready threshold.

**New agent abbreviation:** `MLT` = future ML-Theory-Agent (not yet created).
Natural home: Q3 (generalization / PAC bounds), D3 (probabilistic /
Bayesian methods), D4 (heavy-tailed / robust prediction-head choices) of
[[CTPC-Design-Rationale]], with cross-cutting relevance to D1 (NCDE
substrate), Q5 (formal verification), and Q8 (variance head).

### Book 10 — Shalev-Shwartz & Ben-David, *Understanding Machine Learning: From Theory to Algorithms* (Cambridge, 2014, 449 p)

The canonical PAC-theory / generalization-bounds textbook. Strong **MLT**
anchor for Q3. PAC-Bayes treatment is short (Ch 31, ~3 pp) but the canonical
introductory citation. Rademacher / VC / covering-number machinery is the
spine.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Introduction | 19-30 | SKIP | LOW | — |
| 2 A Gentle Start (Statistical Learning Framework, ERM) | 33-42 | MLT | MEDIUM | Q3 (foundational) |
| **3 A Formal Learning Model (PAC, Agnostic PAC)** | 43-53 | **MLT** | **HIGH** | **Q3, Q5 (PAC = formal-verification baseline)** |
| 4 Learning via Uniform Convergence | 54-59 | MLT | MEDIUM | Q3 |
| 5 The Bias-Complexity Tradeoff (No-Free-Lunch §5.1, Error Decomposition §5.2) | 60-66 | MLT | MEDIUM | Q3 |
| **6 The VC-Dimension (Sauer's Lemma §6.5.1, Fundamental Theorem of PAC §6.4)** | 67-82 | **MLT** | **HIGH** | **Q3 (NN capacity bounds — pair with Ch 20.4)** |
| 7 Nonuniform Learnability (SRM §7.2, MDL/Occam §7.3) | 83-99 | MLT | MEDIUM | Q3 |
| 8 The Runtime of Learning | 100-114 | SKIP | LOW | — |
| 9 Linear Predictors (Halfspaces, LinReg, LogReg) | 117-129 | SKIP | LOW | (baseline) |
| 10 Boosting (AdaBoost §10.2, Weak Learnability §10.1) | 130-143 | MLT | MEDIUM | D3 (ensemble aggregation precedent) |
| 11 Model Selection and Validation (SRM, k-fold CV) | 144-155 | MLT | MEDIUM | training methodology |
| **12 Convex Learning Problems (Convexity, Lipschitzness §12.1.2, Smoothness §12.1.3)** | 156-170 | **MLT, NODE** | **HIGH** | **NODE Picard/Lipschitz uniqueness (commitment #2); PIML loss landscape** |
| 13 Regularization and Stability (Tikhonov §13.3, Fitting-Stability Tradeoff §13.4) | 171-183 | MLT | MEDIUM | Q3, D3 |
| 14 Stochastic Gradient Descent | 184-201 | MLT | MEDIUM | training foundation |
| 15 Support Vector Machines | 202-214 | MLT | LOW | — |
| 16 Kernel Methods (Mercer §16.2.2, Kernel Trick §16.2) | 215-226 | MLT, GDL | MEDIUM | Q1 (kernel ↔ equivariance bridge); R&W Ch 4 companion |
| 17 Multiclass, Ranking, Complex Prediction | 227-249 | SKIP | LOW | — |
| 18 Decision Trees + Random Forests | 250-257 | MLT | LOW | D3 (ensemble precedent) |
| 19 Nearest Neighbor (1-NN Generalization §19.2.1, Curse of Dim §19.2.2) | 258-267 | SKIP | LOW | — |
| **20 Neural Networks (Expressive Power §20.3, Sample Complexity §20.4, SGD+Backprop §20.6)** | 268-286 | **MLT** | **HIGH** | **Q3 (NN sample-complexity bounds = theoretical complement to GAINS empirical verification)** |
| 21 Online Learning | 287-306 | SKIP | LOW | — |
| 22 Clustering (§22.4 **Information Bottleneck**) | 307-322 | INT, MLT | MEDIUM | **Part III (Information Bottleneck grounds Barbiero Symmetry 2 information invariance)** |
| 23 Dimensionality Reduction (PCA, Random Projections, Compressed Sensing) | 323-341 | MLT, SDA-future | LOW | (future: compressed sensing for sparse orbital observations) |
| 24 Generative Models (MLE, Naive Bayes, EM) | 342-356 | MLT | LOW | — |
| 25 Feature Selection and Generation | 357-372 | SKIP | LOW | — |
| **26 Rademacher Complexities (Generalization Bounds for SVM §26.3, low-ℓ₁ norm §26.4)** | 375-387 | **MLT** | **HIGH** | **Q3 (modern generalization theory — supersedes VC-only treatment)** |
| 27 Covering Numbers (Chaining §27.2) | 388-391 | MLT | MEDIUM | Q3 |
| 28 Proof of Fundamental Theorem of Learning Theory | 392-401 | MLT | LOW | (proof depth) |
| 29 Multiclass Learnability (Natarajan Dimension) | 402-409 | SKIP | LOW | — |
| 30 Compression Bounds | 410-414 | MLT | MEDIUM | Q3 + Part III (compression-based generalization aligns with Barbiero information invariance) |
| **31 PAC-Bayes (PAC-Bayes Bounds §31.1)** | 415-417 | **MLT, INT** | **CRITICAL** | **Q3 (PAC-Bayes is the canonical Bayesian generalization framework for CTPC's variance head); Part III (PAC-Bayes posteriors are interpretable)** |
| App A Technical Lemmas | 419-421 | SKIP | LOW | — |
| App B Measure Concentration | 422-429 | MLT | LOW | Q3 (foundational inequalities) |
| App C Linear Algebra | 430-434 | SKIP | LOW | — |

### Book 11 — Goodfellow, Bengio & Courville, *Deep Learning* (MIT 2016, 801 p)

The canonical DL reference. Strong **MLT** anchor but most chapters are
already-known territory for the existing agents (CNN/RNN basics are covered
elsewhere). Highest-value chapters are regularization, optimization, and
approximate inference — each anchoring a different SiS decision.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Introduction | 1-28 | SKIP | LOW | — |
| 2 Linear Algebra | 31-51 | SKIP | LOW | (math primer) |
| 3 Probability and Information Theory (§3.13 Information Theory) | 53-79 | INT, MLT | MEDIUM | Part III (informa­tion-theory primitives for Barbiero) |
| 4 Numerical Computation | 80-97 | SKIP | LOW | — |
| 5 Machine Learning Basics (MLE, Bayesian, SGD, Bias-Variance, Capacity) | 98-165 | MLT | MEDIUM | D3 (Bayesian baseline) |
| 6 Deep Feedforward Networks | 168-227 | MLT | MEDIUM | (DL background) |
| **7 Regularization for Deep Learning** | 228-273 | **MLT** | **HIGH** | **D3 (§7.11 Bagging + Ensembles, §7.12 Dropout = ensemble theory); D4 (§7.5 Noise Robustness); §7.7 Multi-Task Learning relevant to PIML** |
| **8 Optimization for Training Deep Models** | 274-329 | **MLT, PIML** | **HIGH** | **PIML (§8.2 Challenges in NN Optimization → PINN loss-landscape grounding); training convergence** |
| 9 Convolutional Networks | 330-372 | GDL | LOW | (CNN basics; supersedes by geometric-dl papers) |
| 10 Sequence Modeling: Recurrent and Recursive Nets (§10.7 Long-Term Dependencies, §10.10 LSTM) | 373-420 | NODE | MEDIUM | D1 (RNN as discrete-time NCDE analog; long-horizon caveat for orbital) |
| 11 Practical Methodology | 421-442 | SKIP | LOW | — |
| 12 Applications | 443-485 | SKIP | LOW | — |
| 13 Linear Factor Models (PPCA, ICA, Slow Feature, Sparse Coding; §13.5 Manifold Interpretation of PCA) | 489-501 | MLT, INT | LOW | Part III (manifold-interp primitive) |
| **14 Autoencoders** | 502-525 | **MLT** | **MEDIUM** | **D2 (Latent NCDE Corrector is autoencoder-like; §14.5 Denoising autoencoders, §14.6 Manifolds with AEs)** |
| 15 Representation Learning (§15.3 Semi-Supervised Disentangling of Causal Factors) | 526-557 | MLT, INT | MEDIUM | Part III (disentangling = interpretable representation) |
| 16 Structured Probabilistic Models for Deep Learning | 558-589 | MLT | LOW | (graphical-model primitive) |
| 17 Monte Carlo Methods | 590-604 | MLT, UQ-future | LOW | — |
| 18 Confronting the Partition Function | 605-630 | SKIP | LOW | — |
| **19 Approximate Inference (EM §19.2, Variational Inference §19.4)** | 631-653 | **MLT** | **HIGH** | **D2 (VI is the training methodology behind Latent NCDE Corrector's VAE-style ELBO)** |
| 20 Deep Generative Models (Boltzmann Machines, VAE, GAN history) | 654-720 | MLT | LOW | — |

### Book 12 — Murphy, *Probabilistic Machine Learning: An Introduction* (Vol 1, MIT 2022, 860 p)

The modern probabilistic-ML textbook. Strong **MLT** anchor for the
probability / Bayesian / GP / ensemble side. **§2.7.1 Student-t** is the
direct citation for CTPC's Student-t prediction head — the most
operationally important chapter in this book for current SiS work.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Introduction | 1-30 | SKIP | LOW | — |
| **2 Probability: Univariate Models (§2.7.1 Student t, §2.7.2 Cauchy, §2.7.3 Laplace, §2.7.4 Beta, §2.7.5 Gamma)** | 33-76 | **MLT** | **HIGH** | **D4 (Student-t prediction head canonical citation — directly grounds [[CTPC-KDD-Submission]] NLL); D3 (Bayes' rule §2.3)** |
| **3 Probability: Multivariate Models (§3.2 MVN, §3.3 Linear Gaussian systems incl §3.3.5 sensor fusion, §3.4 Exponential family)** | 77-106 | **MLT** | **HIGH** | **Q2 (analytic moment propagation grounding); D3** |
| 4 Statistics (§4.6 Bayesian statistics, §4.7 Frequentist, §4.7.6 bias-variance tradeoff) | 107-166 | MLT | MEDIUM | D3 |
| 5 Decision Theory | 167-206 | MLT | LOW | — |
| **6 Information Theory (§6.1 Entropy, §6.2 KL divergence, §6.3 Mutual information)** | 207-228 | **INT, MLT** | **HIGH** | **Part III (canonical Barbiero Symmetry 2 grounding — H(Z)≪H(X), I(Y;X\|Z)=0)** |
| 7 Linear Algebra | 229-274 | SKIP | LOW | (covered elsewhere) |
| 8 Optimization (§8.4 SGD, §8.5 KKT/Lagrange, §8.6 Proximal gradient) | 275-320 | MLT | MEDIUM | training methodology |
| 9 Linear Discriminant Analysis | 323-338 | SKIP | LOW | — |
| 10 Logistic Regression (§10.5 Bayesian logistic regression) | 339-370 | MLT | LOW | D3 |
| 11 Linear Regression (§11.6 Robust linear regression — Laplace, **Student-t §11.6.2**, Huber, RANSAC; §11.7 Bayesian linear regression) | 371-414 | MLT | MEDIUM | **D4 (robust regression with Student-t and Laplace likelihoods — pairs with Ch 2.7)**; D3 (Bayesian) |
| 12 Generalized Linear Models | 415-422 | SKIP | LOW | — |
| 13 Neural Networks for Tabular Data (§13.5.5 Bayesian NN, §13.5.7 Over-parameterized models, §13.6.2 Mixtures of Experts) | 425-466 | MLT | MEDIUM | D3 (Bayesian NN preview — full treatment in Vol 2 Ch 17) |
| 14 Neural Networks for Images | 467-502 | GDL | LOW | (CNN basics; covered by geometric-dl papers) |
| 15 Neural Networks for Sequences (RNN, Transformers §15.5, §15.5.7 Other transformer variants) | 503-544 | NODE | LOW | (sequence-modeling background) |
| 16 Exemplar-based Methods (KNN, KDE §16.3, Kernel regression §16.3.5) | 547-566 | MLT | LOW | — |
| **17 Kernel Methods (§17.1 Mercer kernels, §17.2 Gaussian processes incl §17.2.8 Connections with DL, §17.4 RVMs)** | 567-602 | **MLT, GDL** | **HIGH** | **Q1 (equivariance-respecting kernels); Q2 (GP variance head as analytical baseline); augments R&W Ch 4** |
| **18 Trees, Forests, Bagging, and Boosting (§18.2 Ensemble learning, §18.3 Bagging, §18.4 RF, §18.5 Boosting, §18.6 Interpreting tree ensembles)** | 603-624 | **MLT, INT** | **HIGH** | **D3 (ensemble theory — bagging + boosting); Part III (§18.6.2 Partial dependency plots = classical interpretability primitive)** |
| 19 Learning with Fewer Labeled Examples (§19.1 Data augmentation, §19.2 Transfer learning, §19.3 Semi-supervised, §19.4 Active learning, §19.5 Meta-learning) | 627-656 | MLT | MEDIUM | training-data efficiency |
| 20 Dimensionality Reduction (§20.1 PCA, §20.2 Factor analysis, §20.3 Autoencoders incl §20.3.5 VAE, §20.4 Manifold learning) | 657-714 | MLT, INT | MEDIUM | D2 (VAE grounding); Part III (interpretable latent representations) |
| 21 Clustering | 715-740 | SKIP | LOW | — |
| 22 Recommender Systems | 741-752 | SKIP | LOW | — |
| 23 Graph Embeddings (§23.4 Graph Neural Networks) | 753-772 | GDL | LOW | (GNN background) |
| App A Notation | 773-779 | SKIP | LOW | — |

**Note on user hint:** the user flagged "Kalman filter / state space models in Murphy Vol 1 → ML-Theory-Agent, SDA-future". Murphy Vol 1's confirmed TOC does **not** contain a dedicated Kalman / state-space chapter — the closest entries are §3.3 Linear Gaussian systems (sensor-fusion example) and §3.3.5 sensor fusion in Ch 3. The full Kalman / EKF / UKF / state-space treatment lives in **Vol 2 Ch 8 (Gaussian filtering and smoothing) + Vol 2 Ch 29 (State-space models)** — routed there below.

### Book 13 — Murphy, *Probabilistic Machine Learning: Advanced Topics* (Vol 2, MIT 2023, 1370 p)

The deeper companion to Vol 1. **Critical** anchor for CTPC's
methodologically heaviest components: Bayesian neural networks (Ch 17),
variational inference (Ch 10), Gaussian filtering / Kalman (Ch 8),
state-space models (Ch 29), Gaussian processes (Ch 18), and a dedicated
Interpretability chapter (Ch 33). The single highest-yield ml-theory book
for SiS.

| Chapter | Pages | Routing | Priority | CTPC anchor |
|---------|-------|---------|----------|-------------|
| 1 Introduction | 1-4 | SKIP | LOW | — |
| **2 Probability (§2.3 Gaussian joint distributions incl §2.3.3 general calculus for linear Gaussian systems, §2.4 Exponential family, §2.7 Divergence measures incl §2.7.3 MMD)** | 5-62 | **MLT** | **HIGH** | **Q2 (analytic linear-Gaussian moment-propagation calculus — the analytical scaffold for [[Analytic-Sigma-CTPC-Composition]]); §2.7.3 MMD = distance for distribution-equivariance verification** |
| 3 Statistics (§3.2 Bayesian statistics, §3.4 Conjugate priors) | 63-142 | MLT | MEDIUM | D3 |
| 4 Graphical Models | 143-218 | MLT | MEDIUM | (Bayesian network primitive) |
| **5 Information Theory** | 219-260 | **INT, MLT** | **HIGH** | **Part III (deeper Barbiero grounding than Vol 1 Ch 6)** |
| **6 Optimization** | 261-342 | **MLT** | **HIGH** | training methodology depth |
| 7 Inference algorithms: an overview | 345-358 | MLT | LOW | (overview) |
| **8 Gaussian filtering and smoothing (Kalman / EKF / UKF)** | 359-400 | **MLT, SDA-future** | **CRITICAL** | **Q2 (variance route via Kalman analytical baseline); SDA agent's canonical orbit-state-estimation citation; D3** |
| 9 Message passing algorithms | 401-438 | MLT | LOW | — |
| **10 Variational inference** | 439-482 | **MLT** | **HIGH** | **D2 (Latent NCDE Corrector ELBO/KL training — canonical VI citation); D3** |
| 11 Monte Carlo methods | 483-498 | MLT, UQ-future | MEDIUM | UQ |
| 12 Markov chain Monte Carlo | 499-542 | MLT, UQ-future | MEDIUM | UQ |
| 13 Sequential Monte Carlo (particle filters) | 543-572 | MLT, SDA-future | MEDIUM | (future: particle filter alternative for orbital state) |
| 14 Predictive models: an overview | 575-590 | MLT | LOW | (overview) |
| 15 Generalized linear models | 591-630 | MLT | LOW | — |
| 16 Deep neural networks | 631-646 | MLT | MEDIUM | (DL with probabilistic framing) |
| **17 Bayesian neural networks** | 647-680 | **MLT** | **CRITICAL** | **D3 (canonical Bayesian-DL citation — directly grounds CTPC variance head + predictive uncertainty story)** |
| **18 Gaussian processes** | 681-734 | **MLT, GDL** | **HIGH** | **Q2 (GP analytical variance baseline); deep-kernel-learning grounding; augments R&W** |
| **19 Beyond the iid assumption (out-of-distribution generalization, time series)** | 735-770 | **MLT, NODE, SDA-future** | **HIGH** | **D1 (orbital trajectories are non-iid time series — direct CTPC operating regime)** |
| 20 Generative models: an overview | 773-790 | MLT | LOW | (overview) |
| **21 Variational autoencoders** | 791-820 | **MLT, INT** | **HIGH** | **D2 (Latent NCDE Corrector IS a VAE-style architecture — canonical citation); Part III (latent-interpretability)** |
| 22 Autoregressive models | 821-828 | MLT | LOW | — |
| **23 Normalizing flows (Continuous Normalizing Flows = NODE in disguise)** | 829-848 | **MLT, NODE** | **HIGH** | **D1 (CNF is the generative-modeling face of NODE — [[NODE]] §4 already in wiki; this chapter is the textbook grounding)** |
| 24 Energy-based models | 849-866 | MLT | LOW | — |
| **25 Diffusion models (SDE formulation, score-matching)** | 867-892 | **MLT, NODE** | **MEDIUM** | **D1 (continuous-time SDE side of NODE; relevant if SiS extends to stochastic NCDE in future)** |
| 26 Generative adversarial networks | 893-924 | MLT | LOW | — |
| 27 Discovery methods: an overview | 927-928 | MLT | LOW | (overview) |
| 28 Latent factor models | 929-978 | MLT | LOW | — |
| **29 State-space models (continuous-time SSMs, deep SSMs)** | 979-1042 | **MLT, NODE, SDA-future** | **HIGH** | **D1 (continuous-time SSM is the state-space-model face of NCDE); SDA agent's orbital state-space prediction grounding** |
| 30 Graph learning | 1043-1046 | GDL | LOW | (sparse) |
| 31 Nonparametric Bayesian models | 1047-1048 | MLT | LOW | (sparse) |
| 32 Representation learning | 1049-1072 | MLT, INT | MEDIUM | Part III (representation interpretability) |
| **33 Interpretability** | 1073-1102 | **INT, MLT** | **CRITICAL** | **Part III (full textbook treatment of interpretability — direct grounding for Interpretability-Agent; companion to Barbiero position paper)** |
| 34 Decision making under uncertainty | 1105-1144 | MLT | LOW | — |
| 35 Reinforcement learning | 1145-1184 | MLT | LOW | — |
| **36 Causality** | 1185-end | **MLT, INT** | **MEDIUM** | **Part III (causal-vs-correlational interpretability bears on Barbiero concept-closure soundness)** |

**Note on user hint:** the user flagged "Neural ODE chapter in Murphy Vol 2 → both ML-Theory-Agent AND Neural-ODE-Agent". Vol 2's Brief Contents (the only level extracted, page-count constraint blocked deeper reading) does **not** show a dedicated "Neural ODE" chapter. The closest matches all routed above to **NODE** are: **Ch 23 Normalizing Flows** (Continuous Normalizing Flows = NODE in disguise per [[NODE]] §4), **Ch 25 Diffusion Models** (SDE formulation = stochastic-NODE), and **Ch 29 State-space models** (continuous-time SSMs that subsume NCDEs). NODE itself may be a *section* within one of these chapters (most likely §23 CNF or §29 deep SSMs) rather than a standalone chapter — please confirm by reading Ch 23 or Ch 29 contents directly when needed.

### Cross-cutting summary for the new ml-theory/ block

**Top ml-theory ingest targets (sorted by SiS marginal value):**

1. **Murphy Vol 2 Ch 17 Bayesian neural networks** (pp 647-680, ~34 p) — D3, CRITICAL. The canonical Bayesian-DL textbook chapter; directly grounds CTPC's variance-head + predictive-uncertainty story.
2. **Murphy Vol 2 Ch 8 Gaussian filtering and smoothing** (pp 359-400, ~42 p) — Q2 + SDA-future, CRITICAL. Kalman / EKF / UKF — the orbital-state-estimation canonical citation for the future SDA agent.
3. **Murphy Vol 2 Ch 33 Interpretability** (pp 1073-1102, ~30 p) — INT, CRITICAL. Full textbook grounding for the Interpretability-Agent; pairs with the Barbiero position paper already in corpus.
4. **Murphy Vol 1 Ch 2 §2.7 + Ch 11 §11.6** (Student-t + robust regression, ~15 p total) — D4, HIGH. The canonical Student-t / heavy-tailed prediction-head citation directly grounding [[CTPC-KDD-Submission]].
5. **Shalev-Shwartz Ch 31 PAC-Bayes** (pp 415-417, ~3 p) — Q3, CRITICAL. Short but canonical; pairs with Ch 26 (Rademacher) and Ch 6 (VC) for full Q3 grounding.
6. **Murphy Vol 2 Ch 10 Variational Inference** (pp 439-482, ~44 p) — D2, HIGH. The training methodology behind Latent NCDE Corrector's ELBO+KL loss.
7. **Murphy Vol 2 Ch 18 Gaussian processes** (pp 681-734, ~54 p) — Q2 + GDL, HIGH. Modern complement to R&W.
8. **Murphy Vol 2 Ch 23 Normalizing flows** (pp 829-848, ~20 p) — NODE, HIGH. CNF textbook grounding.
9. **Murphy Vol 2 Ch 29 State-space models** (pp 979-1042, ~64 p) — NODE + SDA-future, HIGH. Continuous-time SSM machinery directly relevant to NCDE substrate.
10. **Shalev-Shwartz Ch 6 + Ch 20 + Ch 26** (VC + NN sample complexity + Rademacher, ~50 p combined) — Q3, HIGH. The theoretical-bounds complement to GAINS empirical verification.

**Books not currently top-priority but valuable later:**

- Goodfellow Ch 7 (Regularization) + Ch 8 (Optimization) + Ch 19 (Approximate Inference) — D3 + PIML + D2 grounding; valuable for the ML-Theory-Agent once corpus is bootstrapped.
- Murphy Vol 2 Ch 21 Variational Autoencoders — D2, HIGH. Could be promoted to top-tier if [[CTPC-KDD-Submission]] is re-framed explicitly in VAE language.

**Skip-entirely candidates inside ml-theory/:**

- Shalev-Shwartz Ch 17 (Multiclass), Ch 19 (Nearest Neighbor), Ch 21 (Online Learning), Ch 22 §22.1-§22.3 (Clustering basics — keep §22.4 Info Bottleneck), Ch 25 (Feature Selection), Ch 29 (Multiclass Learnability proof depth).
- Goodfellow Ch 1, Ch 2, Ch 4, Ch 11, Ch 12, Ch 18, Ch 20 (Boltzmann-machine specifics).
- Murphy Vol 1 Ch 9, Ch 12, Ch 21, Ch 22.
- Murphy Vol 2 Ch 1, Ch 9, Ch 14, Ch 15, Ch 22, Ch 24, Ch 26, Ch 27, Ch 28, Ch 31, Ch 34, Ch 35.

---

## Cross-Cutting Routing Summary by Agent

### Hamiltonian-NN-Agent (READY) — top targets

1. **van der Schaft & Jeltsema Ch 2 (pp 11-40)** — canonical PH definition. CRITICAL.
2. **van der Schaft & Jeltsema Ch 4 (pp 53-62)** — input-state-output PH. HIGH.
3. **van der Schaft & Jeltsema Ch 7 (pp 75-82)** — passivity proof. HIGH.
4. **Khalil Ch 6 Passivity (pp 227-262)** — nonlinear-systems passivity backbone. CRITICAL.
5. **Khalil Ch 4 Lyapunov Stability (pp 111-194)** — Lyapunov foundation. HIGH.
6. **Khalil Ch 9 Stability of Perturbed Systems (pp 339-380)** — for Q8b. HIGH.
7. **Goldstein Ch 8-9 (pp 334-429)** — Hamilton eq + canonical transformations + symplectic + Poisson. HIGH.
8. **Arnold Ch 8-9 (pp 201-270)** — symplectic manifolds + Hamilton-Jacobi (more rigorous variant). HIGH.
9. **Khalil §14.4 Passivity-Based Control** — PBC bridge. MEDIUM.

### Interpretability-Agent (READY) — top targets

1. **R&W Ch 2 Regression (pp 7-32)** — Bayesian-regression baseline for interpretable
   posterior. HIGH for cross-comparison with CBM/HNN/LNN interpretability claims.
2. Indirect: **Khalil Ch 4 Lyapunov + Ch 6 Passivity** ground the certificate-style
   interpretability of `Ḣ ≤ 0`. MEDIUM.
3. Indirect: **Goldstein Ch 8 Hamilton eq + Ch 2 Lagrange Noether** ground the physical
   semantics of what HNN/LNN actually learn. LOW-MEDIUM.

(Note: the Interpretability agent's natural corpus is mostly already in
`raw/papers/interpretability/` — CBM, HNN, LNN, etc. The books contribute marginal
foundational depth rather than core content.)

### Geometric-DL-Agent (READY) — top targets

1. **Tu Ch 15 Lie Groups (pp 149-159)** — closes the continuous-group gap flagged in
   the agent's commitment #4. HIGH.
2. **Tu Ch 16 Lie Algebras (pp 161-171)** — `so(3)`, `so(2)`, exponential map. HIGH.
3. **Tu Ch 12 Tangent Bundle + Ch 14 Vector Fields (pp 119-147)** — bundles as
   foundation for equivariant network kernels. HIGH.
4. **Arnold App 5 Dynamical systems with symmetries (pp 371-380)** — symmetry
   reduction. MEDIUM.
5. **Goldstein Ch 9 §9.8 Symmetry groups** — for Q1 of CTPC-Design-Rationale. MEDIUM.

### Neural-ODE-Agent (READY) — top targets

1. **Tu Ch 14 Vector Fields (pp 135-147)** — integral curves, local flows = NODE
   conceptual home. HIGH.
2. **Khalil Ch 3 Fundamental Properties (pp 87-110)** — Picard existence/uniqueness =
   NODE §6 well-definedness. MEDIUM.
3. **Khalil Ch 9 Stability of Perturbed Systems** — for Q8b integrator-error analysis. MEDIUM.
4. **Goldstein Ch 11 Classical Chaos (Lyapunov exponents)** — grounding for
   high-Lyapunov-regime caution flagged in agent commitment #3. MEDIUM.

### Physics-Informed-Agent (READY) — top targets

The PIML corpus is best served by ML/physics-informed papers (already ingested).
Books contribute foundational grounding rather than core content. **No book chapter
is critical** for this agent right now. Lower-priority candidates:
- **Khalil Ch 4 Lyapunov** for stability-aware physics-informed loss design.
- (Books are not the bottleneck for this agent.)

### Future SDA Agent — when created, ingest from:

1. **Wakker Ch 9 (Clohessy-Wiltshire) + Ch 11 (reference frames) + Ch 20 (perturbations)**.
2. **Curtis Ch 4 (orbits in 3D, J2) + Ch 2 (two-body) + Appendix D (MATLAB algorithms)**.
3. **Goldstein Ch 3 (Central Force) + Ch 5.8 (Precession of satellite orbits)**.
4. **Arnold Ch 10 (Perturbation theory, averaging)**.

### Future UQ Agent — when created, ingest from:

1. **R&W Ch 2 + Ch 4 + Ch 9 + App B** as anchor chapters.

### Skip-entirely candidates

- **Bott & Tu (Differential Forms in Algebraic Topology)** — defer entirely; too
  specialized for current SiS scope.
- Most of Wakker Ch 7-8, 10, 12-19 (specialized astrodynamics scenarios).
- Curtis Ch 6, 8, 11 (maneuvers, rocket dynamics — not orbit-prediction relevant).
- Goldstein Ch 6 (oscillations), Ch 7 (special relativity), Ch 13 (continuous fields).
- Arnold Ch 1, 2, 5 (Newtonian intro), Ch 6 (rigid bodies — defer to SDA agent).
- Tu Part VI Integration, Part VII De Rham (out of scope for current agents).

---

## Step 3 — Recommended First Ingest Wave for the 2 Already-Ready Agents

Optimizing for **Hamiltonian-NN-Agent** and **Interpretability-Agent** specifically,
in order of marginal value to those agents:

### Priority 1 — van der Schaft & Jeltsema Ch 2 (pp 11-40, ~30 p)
**Why first:** This is the canonical PH definition. Currently
`wiki/concepts/Port-Hamiltonian-System.md` and `wiki/architectures/Port-Hamiltonian-Neural-Network.md`
cite Furieri et al. 2024 as their main source — but Furieri itself routes back to
van der Schaft. Ingesting this chapter gives the Hamiltonian-NN agent its true
canonical source instead of a downstream citation. Closes a foundational gap.

### Priority 2 — Khalil Ch 6 Passivity (pp 227-262, ~36 p)
**Why second:** This is the nonlinear-systems engineering text that makes `Ḣ ≤ 0`
a passivity certificate rather than just a name. Pairs immediately with Priority 1.
Together they form the rigorous foundation behind the Hamiltonian-NN agent's
commitment #1 (`Ḣ ≤ 0` as hard architectural constraint). The combination of
Priority 1 + Priority 2 is the single highest-yield pair of ingests in the corpus
for the HNN agent.

### Priority 3 — Goldstein Ch 8 + Ch 9 (pp 334-429, ~96 p — large; could split)
**Why third:** Closes the *classical*-Hamiltonian gap. The wiki currently has
PHNN extensively documented but lacks ingested grounding for the underlying
classical Hamiltonian mechanics (Hamilton's equations, Legendre transformation,
symplectic structure §9.4, Poisson brackets §9.5, Liouville's theorem §9.9).
These pin Q8 variance-route discussion (Liouville → phase-space-volume preservation
→ structure-preserving integrators in Q8b). Also pins Casimirs in PH-book Ch 8
to their canonical Poisson-bracket origin. Goldstein chosen over Arnold for first
ingest because of engineering accessibility — Arnold's Ch 8-9 is a planned
follow-up "rigorous foundation" round.
**Note:** Ch 8 (~33 p) is the more critical half if size is a concern; Ch 9 can
be split off.

### Priority 4 — Khalil Ch 4 Lyapunov Stability §4.1-4.4 + §4.9 ISS (selected sections, ~40 p)
**Why fourth:** Provides the Lyapunov certificate foundation that Khalil Ch 6
Passivity references. Currently the wiki uses "Lyapunov-stable" terminology in
multiple places (PHNN, certified NODE, GAINS) without a single grounded source
on what Lyapunov stability actually means in the autonomous-systems setting and
how the Invariance principle (§4.2) extends it. ISS (§4.9) is the relevant
robustness generalization. Ingesting selected sections keeps page count
manageable. Could be Priority 3 if Priority 1+2 want a longer pause for synthesis.

### Conditional Priority 5 — R&W Ch 2 Regression (pp 7-32, ~26 p)
**Why fifth (and conditional):** The only book chapter that lands directly on the
**Interpretability-Agent's** corpus rather than the Hamiltonian-NN agent's. R&W
Ch 2 gives the GP / Bayesian-regression baseline against which CBM, HNN, LNN
interpretability claims can be benchmarked. Conditional because the Interpretability
agent already has a strong direct-paper corpus (CBM, mech-interp, etc.); this is
the *grounding* depth-ingest rather than a content-gap fill.
**If Bilal wants the first wave to focus tightly on Hamiltonian-NN:** defer R&W
Ch 2 to second wave. **If he wants balanced foundation-grounding for both ready
agents:** include in first wave.

### Total first-wave page count

- Priorities 1+2+3+4: ~202 pages (5-6 wiki pages expected)
- Priorities 1+2+4+5 (HNN-light, INT-balanced): ~132 pages (4-5 wiki pages expected)
- Priorities 1+2 alone (minimum viable wave): ~66 pages (~2-3 wiki pages)

## Recommendation

**Run Priorities 1 + 2 + 3 + 4 as the first foundation ingest wave (HNN-heavy).**
This brings the Hamiltonian-NN agent from "cited a single paper" to "fully grounded
in canonical PH theory + canonical nonlinear-systems passivity + canonical
classical Hamiltonian mechanics + Lyapunov certificates." Interpretability agent
gets indirect benefit (certificate-style interpretability of `Ḣ ≤ 0`).

R&W Ch 2 (Priority 5) becomes the natural opening of the second foundation wave —
together with R&W Ch 4 (Covariance Functions) and Tu Ch 15-16 (Lie Groups +
Lie Algebras) — when the focus rotates to the Interpretability + Geometric-DL
agents and the future UQ agent.
