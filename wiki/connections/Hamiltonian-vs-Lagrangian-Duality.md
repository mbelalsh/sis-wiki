---
title: Hamiltonian vs Lagrangian Duality (CTPC Design Decision)
tags: [synthesis, ctpc-design, hamiltonian, lagrangian, conservation-laws, design-decision]
sources: [raw/papers/interpretability/HNN.pdf, raw/papers/interpretability/LNN.pdf]
created: 2026-05-08
updated: 2026-05-08
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

Both enforce energy conservation as a *structural property* of the architecture (no soft loss penalty). Both have the same dissipation gap (need Port-Hamiltonian / Rayleigh-dissipation extensions for drag/SRP). The choice between them is determined by:

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

**CTPC's working assumption (provisional):**
- **Conservative core:** **HNN** if working in equinoctial elements (canonical), **LNN** if working in raw RTN/RSW position-velocity. Default to LNN for the corrector since RTN is the natural decomposition frame and isn't canonical.
- **Dissipative residual:** PHNN (or Rayleigh-dissipation Lagrangian extension once that's ingested).
- **Hard physics layer:** SGP4 (frozen, like a [[Physics-Based-FD-Convolutional-Layer]] but for orbital ODEs).

## What Each Formulation Hides

**HNN hides the kinetic energy structure.** `H = T(p) + V(q)` for separable mechanical systems, but `T` is a function of momentum, not velocity. For relativistic systems where `p ≠ m q̇`, defining `T(p)` requires inverting the Legendre transform — exactly the step LNN avoids.

**LNN hides the symplectic structure.** Hamilton's equations have a beautiful symplectic-2-form structure that symplectic integrators exploit for *exact* discrete energy conservation. LNN's variational-integrator analogue exists (Marsden-West) but is less developed in mainstream ML codebases.

**Both hide the action interpretation.** Both architectures conserve energy along trajectories, but neither gives you the *action* `S = ∫L dt` — which is the more fundamental physical quantity. For interpretability ("Why does the system take *this* trajectory?"), the action is the answer. Cranmer's broader thread (symbolic regression on networks; see [[Pi-Block-Polynomial-Approximator]] open question) is partly about recovering action-like interpretability post-training.

## Open Questions

- **Hybrid HNN-LNN architectures.** Could one parameterize `L_θ`, compute `p_θ = ∂L_θ/∂q̇`, then use HNN-style symplectic-gradient on the derived `(q, p_θ)` for forward dynamics? Would combine LNN's coordinate flexibility with HNN's symplectic-integrator compatibility. Untested.
- **Comparative cost on orbital problems.** Order-of-magnitude analysis suggests LNN's `O(d³)` is fine for `d = 6` but problematic for `d > 50`. Empirical benchmarking on orbital state spaces is open.
- **Dissipative composition.** Both architectures need a non-conservative extension for drag/SRP. The Hamiltonian path goes through Port-Hamiltonian (PHNN — *not yet ingested*). The Lagrangian path goes through Rayleigh dissipation `D(q̇)`. Whether the resulting CTPC architectures are equivalent or have different practical trade-offs is the next synthesis to write (after PHNN ingest).
- **Cranmer thread implication.** [[LNN]] is co-authored by Cranmer; the Π-block paper isn't but Cranmer's symbolic-distillation work parallels both. Reading Cranmer's thesis (in `raw/papers/port-hamiltonian/MCranmerPhDThesis.pdf`, *not yet ingested*) should clarify whether his structured-prior philosophy favors HNN-style or LNN-style for downstream symbolic distillation.

## Connections

- [[HNN]] — the Hamiltonian paper
- [[LNN]] — the Lagrangian paper
- [[Hamiltonian-Neural-Network]] — Hamiltonian architecture pattern
- [[Lagrangian-Neural-Network]] — Lagrangian architecture pattern
- [[Hamiltonian-Mechanics]] — foundational physics, Hamiltonian side
- [[Lagrangian-Mechanics]] — foundational physics, Lagrangian side
- [[Symplectic-Gradient]] — Hamiltonian dynamics object
- [[Euler-Lagrange-Equation]] — Lagrangian dynamics object
- [[Port-Hamiltonian-Neural-Networks]] *(not yet ingested)* — dissipative extension on Hamiltonian side; needed to close the dissipation gap

## Sources

- `raw/papers/interpretability/HNN.pdf` — Greydanus, Dzamba, Yosinski 2019.
- `raw/papers/interpretability/LNN.pdf` — Cranmer, Greydanus, Hoyer, Battaglia, Spergel, Ho 2020. Especially Eq. 1 (Poisson bracket relations), the formal statement of the canonical-coordinate constraint that motivates LNN over HNN.
