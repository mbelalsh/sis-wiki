---
title: Hamiltonian vs Lagrangian Duality (CTPC Design Decision)
tags: [synthesis, ctpc-design, hamiltonian, lagrangian, conservation-laws, design-decision, dissipation, helmholtz-vs-jr-route]
sources: [raw/papers/interpretability/HNN.pdf, raw/papers/interpretability/LNN.pdf, raw/papers/port-hamiltonian/DissipativeHNN.pdf, raw/papers/port-hamiltonian/DissipativeSymODEN-Zhong2020.pdf]
created: 2026-05-08
updated: 2026-05-10
sis_relevance: critical
hard_constraint_possible: yes
---

# Hamiltonian vs Lagrangian Duality (CTPC Design Decision)

## One-Line Intuition

Hamiltonian and Lagrangian formulations of classical mechanics give *equivalent* physics — connected by the Legendre transform — but the corresponding neural-network architectures ([[Hamiltonian-Neural-Network]] / [[Lagrangian-Neural-Network]]) make different practical trade-offs. **Pick HNN when your data is in canonical `(q, p)` coords; pick LNN when it's in arbitrary `(q, q̇)` coords.** This page is the explicit decision matrix for CTPC.

## Why This Synthesis Matters

The CTPC corrector design has two structural-prior options for the conservative part:

- **HNN**: parameterize scalar `H_θ(q, p)`, dynamics via [[Symplectic-Gradient]]
- **LNN**: parameterize scalar `L_θ(q, q̇)`, dynamics via [[Euler-Lagrange-Equation]]

Both enforce energy conservation as a *structural property* of the architecture (no soft loss penalty). Both have a dissipation gap that requires an extension (see § "A Fourth Axis: Dissipation Route" below — the choice of dissipation extension is non-trivial and *decoupled* from the HNN-vs-LNN choice). The choice between them is determined by:

1. What coordinates the data is in
2. The functional form of the kinetic energy
3. Computational budget per forward pass
4. Compatibility with downstream integrators

Getting this right at the architecture-decision stage saves significant rework — CTPC is a long-running design, and the corrector type is foundational.

## The Math

**Legendre transform** (the formal duality):
```
H(q, p) = p · q̇ − L(q, q̇)        with    p_i = ∂L/∂q̇_i
```
Equivalently:
```
L(q, q̇) = p · q̇ − H(q, p)        with    q̇_i = ∂H/∂p_i
```

For non-degenerate Lagrangians (invertible velocity-Hessian `∇_q̇ ∇_q̇^T L`), the transform is bijective — `H` and `L` describe identical physics. For degenerate Lagrangians (gauge theories, constrained systems), the transform is singular; you need Dirac's constrained-Hamiltonian formalism.

**Forward dynamics — same trajectories, different computations:**

| Step | HNN | LNN |
|---|---|---|
| Input | `(q, p)` | `(q, q̇)` |
| Network output | scalar `H_θ` | scalar `L_θ` |
| Dynamics computation | autograd `(∂H/∂p, −∂H/∂q)` | autograd Hessian + matrix inversion |
| Cost / step | `O(d²)` (one autograd pass) | `O(d³)` (Hessian + `M^(-1)`) |
| Activation constraint | any (smooth-ish) | second derivative ≠ 0 (no ReLU) |
| Equivalent integrator | symplectic (leapfrog, Yoshida) | variational (Marsden-West) |

## CTPC Decision Matrix

| If your data / use case is… | Pick | Why |
|---|---|---|
| Position + canonical momentum `(q, p)` available | **HNN** | Cheaper (`O(d²)`); symplectic integrators natural |
| Position + velocity `(q, q̇)` available, no canonical structure | **LNN** | No need to construct canonical momenta first |
| Equinoctial orbital elements (canonical, ~quadratic KE) | **HNN** | Already canonical; HNN's structural fit matches |
| TLE-derived `(r, v)` straight from Space-Track | **LNN** | These are *not* canonical under perturbed Hamiltonians; LNN avoids the pre-processing |
| Relativistic GEO / cislunar precision | **LNN** | Canonical `p ≠ m·q̇`; HNN fails on raw observables |
| Charged particle in magnetic field (e.g., spacecraft in magnetosphere) | **LNN** | `p = m q̇ + q A(r)`; LNN handles natively |
| Rigid body with `T = q̇^T M(q) q̇` (DeLaN regime) | Either; HNN cheaper | Both work; HNN's lower per-step cost wins |
| Production scale, `d > 100` | **HNN** + sparsity | LNN's `O(d³)` Hessian inversion is the practical blocker |
| Sparse spatial structure (Lagrangian Graph Network territory) | **LNN-GN** | Sparse Hessian → linear-time inversion; matches PDE-style problems |
| Long-horizon energy stability is critical | Either, with structure-preserving integrator | HNN + symplectic, or LNN + variational integrator |

**CTPC's working assumption (provisional, updated 2026-05-10 after D-HNN ingest):**
- **Conservative core:** **HNN** if working in equinoctial elements (canonical), **LNN** if working in raw RTN/RSW position-velocity. Default to LNN for the corrector since RTN is the natural decomposition frame and isn't canonical.
- **Dissipative residual:** *J-R-route, not Helmholtz-route* — PHNN-proper or Rayleigh-extended Lagrangian with PSD damping matrix `R`. The Helmholtz-route alternative ([[Dissipative-Hamiltonian-Neural-Network|D-HNN]]) is structurally complete but does *not* guarantee `Ḣ ≤ 0`. See § "A Fourth Axis" below.
- **Hard physics layer:** SGP4 (frozen, like a [[Physics-Based-FD-Convolutional-Layer]] but for orbital ODEs).

## A Third Axis: PhyArch (Symmetry-Based Hard Constraints)

The HNN ↔ LNN axis above is about **which formalism** to hardwire (Hamiltonian vs Lagrangian). Both axes encode an *energy* structural prior. **[[PhyArch]] occupies an orthogonal axis**: instead of formalism, it hardwires the system's *spatial symmetries* and *equation form* directly into the architecture.

Empirical finding from [[PhyArch-Double-Pendulum-Benchmark]] (Bilal's own work, 2026-05): on the planar double pendulum, PhyArch conserves energy **better than LNN** at 20s rollouts (28.2 J vs 41.3 J on OOD Upright) *despite not modeling energy at all*. Mechanism: hard symmetry constraints keep the trajectory close to the true one, and the true trajectory conserves energy as a consequence.

This implies the design space is at least 2-dimensional, not 1-dimensional:

| | **Symmetry-based hard constraint** | **No symmetry hardwiring** |
|---|---|---|
| **Hamiltonian formalism** | PhyArch + HNN (untested composition) | HNN |
| **Lagrangian formalism** | PhyArch + LNN (untested composition) | LNN |
| **No formalism (direct)** | PhyArch | PINN / vanilla MLP |

**For CTPC's design decision, the question is now 2-fold:**

1. Does the conservative-core corrector need a *formalism* prior (HNN/LNN-style) or a *symmetry* prior (PhyArch-style) — or both?
2. If both, in what composition? PhyArch's invariant features feeding an LNN's scalar `L_θ` is one candidate; PhyArch directly outputting the manipulator-equation skeleton with neural coefficients (no Lagrangian formalism) is another.

The PhyArch DP benchmark suggests symmetry alone may be sufficient when the symmetry group is correctly identified — but DP has *exact* `Z₂`. Orbital has *broken* `SO(3) → SO(2)`. The composition question is open.

## A Fourth Axis: Dissipation Route — Helmholtz vs J-R Structure

The HNN ↔ LNN axis is about *which conservative formalism* to hardwire. PhyArch is about *whether to hardwire spatial symmetries*. **The fourth axis** — surfaced by the [[D-HNN]] ingest (2026-05-10) — is about *how to add dissipation* to a conservative architecture. There are two architecturally distinct routes, and they are *not equivalent*.

### Route 1 — Helmholtz decomposition

[[Dissipative-Hamiltonian-Neural-Network|D-HNN]] (Sosanya & Greydanus 2022). Add a *second scalar* `D_θ(q,p)`; combine via [[Helmholtz-Decomposition]]:

```
dx/dt = (∂H/∂p, −∂H/∂q) + (∂D/∂q, ∂D/∂p)
        └ symplectic ──┘   └── ordinary ──┘
        rotational         irrotational
```

**What it gives:** structural completeness — any smooth vector field decomposes uniquely into these two components. The conservative/dissipative split is *forced*, not emergent. Counterfactual generalization for free: multiplying `D_θ` by `α` simulates a different friction coefficient.

**What it doesn't give:** `Ḣ ≤ 0`. The energy rate `dH/dt = ∇H · ∇D` has indeterminate sign because the two scalars are unconstrained relative to each other — depending on the angle between `∇H` and `∇D` at any point, the model can *add* energy. Empirically the loss + data may keep this aligned correctly, but there's no *architectural* guarantee.

### Route 2 — J-R (port-Hamiltonian) structure

[[Port-Hamiltonian-Systems|Port-Hamiltonian]] dynamics (and its NN realization PHNN-proper / DissipativeSymODEN):

```
dx/dt = [J(x) − R(x)] ∇H(x) + g(x) u
```

with `J` skew-symmetric and `R` positive semi-definite. Energy rate:

```
dH/dt = −∇Hᵀ R ∇H ≤ 0   (for u = 0, R PSD constrained by parameterization)
```

**What it gives:** structural `Ḣ ≤ 0` (passivity guarantee). Native handling of control inputs `u` via `g(x)u`. Compositional with [[Hamiltonian-Mechanics]].

**What it doesn't give:** completeness — not every smooth vector field is `(J − R)∇H` for some triple, so the architecture is restricted (this is the *good* kind of restriction for SiS). Counterfactual on individual dissipation channels requires reparameterizing `R(x)`; less plug-and-play than D-HNN's `α · D` knob.

### Side-by-side

| Property | Helmholtz route (D-HNN) | J-R route (PHNN-proper) |
|---|---|---|
| Form | `symplectic_grad(H) + grad(D)` | `(J − R) ∇H + g u` |
| Two scalars? | Yes: `H, D` | Just `H`; `J`, `R`, `g` are operators |
| Decomposition guarantee | ✓ Helmholtz uniqueness | ✗ Restricted form |
| Passivity `dH/dt ≤ 0` | ✗ Indeterminate sign | ✓ Structural via PSD `R` |
| Counterfactual scaling | ✓ `α · D_θ` (free) | Requires reparameterizing `R` |
| Control input `u` | Not in formulation | ✓ Native via `g(x)u` |
| Lagrangian-side analog | Generalized Rayleigh `D(q, p)` | Rayleigh-extended Euler-Lagrange (`d/dt(∂L/∂q̇) − ∂L/∂q + ∂D/∂q̇ = 0`) with PSD damping |

### Decision matrix for CTPC

| Use case | Route |
|---|---|
| **Deployment-safe orbital corrector** (drag, SRP) | **J-R** — `Ḣ ≤ 0` is a deployment-safety constraint, not a soft preference |
| **Diagnostic / interpretability** (decompose a learned vector field) | Helmholtz (post-hoc on a trained PHNN model) |
| **Counterfactual reasoning over individual dissipation parameters** | Either; J-R needs reparameterized `R`, Helmholtz gets it free via `α · D` — but the J-R route's individual dissipation channels can be parameterized to support similar interventions |
| **Real-world scientific data with both rotational + irrotational features** (oceanography, fluid dynamics) | Helmholtz (D-HNN's actual target use case) |

**For SiS, the choice is J-R, full stop.** The `Ḣ ≤ 0` constraint is non-negotiable for deployment safety — a model that *could* add energy to an orbital trajectory, even if it usually doesn't, fails the SiS hard-constraint criterion. See [[Dissipative-Hamiltonian-Neural-Network]] § "Why D-HNN is not enough for SiS" for the precise architectural analysis. This is also why [[CTPC-Design-Rationale]] Q8 splits into Q8a (Helmholtz route — closed by D-HNN), Q8b-vector-field (J-R route at continuous-time — closed by [[Dissipative-SymODEN]] / [[Port-Hamiltonian-Neural-Network]]), and Q8b-discrete-time (exact discrete passivity — open, awaiting ingest of a structure-preserving integrator).

### The full design space is now 3-dimensional

Combining the three orthogonal axes:

| Axis | Choices | What it controls |
|---|---|---|
| **Conservative formalism** | HNN / LNN | Which scalar (`H` or `L`) to parameterize |
| **Symmetry hardwiring** | PhyArch / no | Whether spatial symmetries are baked into architecture or merely encouraged via loss |
| **Dissipation route** | Helmholtz / J-R | Whether dissipation is added via second scalar (decomposition-complete, no `Ḣ ≤ 0`) or via PSD damping matrix (passive-by-construction) |

For SiS, the principled triple is **(LNN-or-HNN, PhyArch, J-R)**, with the LNN-vs-HNN choice driven by coordinate frame and the J-R choice driven by deployment-safety.

## What Each Formulation Hides

**HNN hides the kinetic energy structure.** `H = T(p) + V(q)` for separable mechanical systems, but `T` is a function of momentum, not velocity. For relativistic systems where `p ≠ m q̇`, defining `T(p)` requires inverting the Legendre transform — exactly the step LNN avoids.

**LNN hides the symplectic structure.** Hamilton's equations have a beautiful symplectic-2-form structure that symplectic integrators exploit for *exact* discrete energy conservation. LNN's variational-integrator analogue exists (Marsden-West) but is less developed in mainstream ML codebases.

**Both hide the action interpretation.** Both architectures conserve energy along trajectories, but neither gives you the *action* `S = ∫L dt` — which is the more fundamental physical quantity. For interpretability ("Why does the system take *this* trajectory?"), the action is the answer. Cranmer's broader thread (symbolic regression on networks; see [[Pi-Block-Polynomial-Approximator]] open question) is partly about recovering action-like interpretability post-training.

## Open Questions

- **Hybrid HNN-LNN architectures.** Could one parameterize `L_θ`, compute `p_θ = ∂L_θ/∂q̇`, then use HNN-style symplectic-gradient on the derived `(q, p_θ)` for forward dynamics? Would combine LNN's coordinate flexibility with HNN's symplectic-integrator compatibility. Untested.
- **Comparative cost on orbital problems.** Order-of-magnitude analysis suggests LNN's `O(d³)` is fine for `d = 6` but problematic for `d > 50`. Empirical benchmarking on orbital state spaces is open.
- **Dissipative composition (substantially resolved 2026-05-10 via D-HNN + Dissipative-SymODEN ingests).** D-HNN closes the Helmholtz-decomposition route on the Hamiltonian side; [[Dissipative-SymODEN]] / [[Port-Hamiltonian-Neural-Network]] closes the J-R-structure route at the vector-field level. The Lagrangian-side Rayleigh extension parallels the Hamiltonian-side J-R route — both give `Ḣ ≤ 0` if the damping matrix is PSD-constrained by parameterization (Cholesky). **The full Lagrangian-side equivalent of PHNN-proper ("Lagrangian + PSD-constrained Rayleigh") doesn't appear to be a named architecture in the literature — opening for a methods paper** if the orbital data favors Lagrangian formalism (which it doesn't, since orbital is canonical, but worth flagging). See § "A Fourth Axis: Dissipation Route" above for the worked-out comparison. Discrete-time exact passivity is the remaining open question (Q8b-discrete-time).
- **Cranmer thread implication.** [[LNN]] is co-authored by Cranmer; the Π-block paper isn't but Cranmer's symbolic-distillation work parallels both. Reading Cranmer's thesis (in `raw/papers/symbolic-physics/MCranmerPhDThesis.pdf`, *not yet ingested*) should clarify whether his structured-prior philosophy favors HNN-style or LNN-style for downstream symbolic distillation.

## Connections

- [[HNN]] — the Hamiltonian paper
- [[LNN]] — the Lagrangian paper
- [[D-HNN]] — Sosanya & Greydanus 2022; the Helmholtz-decomposition leg of the dissipation-route axis
- [[Hamiltonian-Neural-Network]] — Hamiltonian architecture pattern
- [[Lagrangian-Neural-Network]] — Lagrangian architecture pattern
- [[Dissipative-Hamiltonian-Neural-Network]] — Helmholtz-route dissipative extension; structurally complete but no `Ḣ ≤ 0`
- [[Hamiltonian-Mechanics]] — foundational physics, Hamiltonian side
- [[Lagrangian-Mechanics]] — foundational physics, Lagrangian side
- [[Symplectic-Gradient]] — Hamiltonian dynamics object
- [[Euler-Lagrange-Equation]] — Lagrangian dynamics object
- [[Helmholtz-Decomposition]] — math foundation of Route 1 above
- [[Rayleigh-Dissipation-Function]] — classical-mechanics primitive for both routes (Lagrangian-side)
- [[Port-Hamiltonian-Neural-Network]] — J-R-structure-route dissipative extension on Hamiltonian side; needed to close Q8b in [[CTPC-Design-Rationale]]
- [[CTPC-Design-Rationale]] — Q8a/Q8b split formalizes the dissipation-route distinction made on this page

## Sources

- `raw/papers/interpretability/HNN.pdf` — Greydanus, Dzamba, Yosinski 2019.
- `raw/papers/interpretability/LNN.pdf` — Cranmer, Greydanus, Hoyer, Battaglia, Spergel, Ho 2020. Especially Eq. 1 (Poisson bracket relations), the formal statement of the canonical-coordinate constraint that motivates LNN over HNN.
- `raw/papers/port-hamiltonian/DissipativeHNN.pdf` — Sosanya & Greydanus 2022. Source for the Helmholtz-route formalization (§3.2, Eq. 4); used to specify the fourth-axis comparison.
