---
title: Rayleigh Dissipation Function
tags: [classical-mechanics, dissipation, lagrangian, hamiltonian, non-conservative, friction]
sources: [raw/papers/port-hamiltonian/DissipativeHNN.pdf]
created: 2026-05-10
updated: 2026-05-10
sis_relevance: high
hard_constraint_possible: partial
---

# Rayleigh Dissipation Function

## One-Line Intuition

A scalar function `D(q̇)` whose gradient w.r.t. velocity gives the dissipative force — the standard way classical mechanics extends [[Hamiltonian-Mechanics]] / [[Lagrangian-Mechanics]] to handle friction, drag, and other non-conservative effects without losing the scalar-potential structure.

## Why This Was Invented

Lagrange-Hamilton mechanics built around a single scalar (`L` or `H`) is beautiful when energy is conserved. Real physical systems — pendula in air, springs with friction, spacecraft in atmospheric drag — *aren't* conservative. Rayleigh (1873) introduced a second scalar function `D` so that dissipative forces still come from a *potential* (not from arbitrary non-conservative force terms), preserving the analytical-mechanics machinery.

For SiS, the function matters because **dissipation is a hard requirement** for orbital correctors: drag, SRP, atmospheric heating are all dissipative. A scalar parameterization of dissipation is the natural target for a neural network — same trick as parameterizing `H` with an MLP, but for the dissipation side.

## The Math

**Standard form (Rayleigh's original).** A scalar function `D : ℝᴺ → ℝ` of the generalized velocities `q̇`. The dissipative generalized force on coordinate `i` is

```
F_i^{dis} = − ∂D/∂q̇_i                            (1)
```

For Lagrangian dynamics with dissipation, the [[Euler-Lagrange-Equation]] is modified:

```
d/dt (∂L/∂q̇_i) − ∂L/∂q_i + ∂D/∂q̇_i = 0          (2)
```

**Quadratic Rayleigh function (linear damping).**

```
D(q̇) = ½ q̇ᵀ R q̇                                  (3)
```

with `R ∈ ℝ^{N×N}` positive semi-definite. Then `F^{dis} = −Rq̇` — linear damping with damping matrix `R`. Energy dissipation rate:

```
dE/dt = −2D(q̇) ≤ 0   if R is PSD                  (4)
```

This is *passivity* — the energy can only decrease. For PSD `R`, dissipation is structurally guaranteed.

**Hamiltonian-side equivalent.** Translating via Legendre transform: dissipation modifies Hamilton's equations to

```
dq/dt = ∂H/∂p,        dp/dt = −∂H/∂q − ∂D/∂q̇    (5)
```

with `q̇` recovered from `p` via `p = ∂L/∂q̇`. For separable mechanical Hamiltonians `H = T(p) + V(q)`, `q̇ = ∂H/∂p` and the modification simplifies.

**Helmholtz / D-HNN generalization.** Sosanya & Greydanus (2022, [[D-HNN]]) generalize `D` from `D(q̇)` to `D(q, p)` — a scalar field over phase space — and take *ordinary* gradients in both `q` and `p`. This is *not* the standard Rayleigh formulation. It's the formulation that maps naturally to the [[Helmholtz-Decomposition]]:

```
(dq/dt, dp/dt) = (∂H/∂p, −∂H/∂q) + (∂D/∂q, ∂D/∂p)
                  └─ symplectic ─┘   └── ordinary ──┘
                  rotational         irrotational
```

Standard Rayleigh is a *special case* of this generalization with `D` velocity-only and the gradient w.r.t. velocity giving the dissipative force. The D-HNN paper does not flag the generalization explicitly — easy to miss when reading.

### Code Correspondence

```python
import torch

def rayleigh_quadratic(q_dot, R):
    """D(q̇) = ½ q̇ᵀ R q̇.   F_dis = −R q̇.   dE/dt = −q̇ᵀ R q̇ ≤ 0  if R is PSD."""
    return 0.5 * q_dot @ R @ q_dot

def dissipative_force(D_fn, q_dot):
    """F_i^dis = −∂D/∂q̇_i  via autograd."""
    q_dot = q_dot.requires_grad_(True)
    D = D_fn(q_dot).sum()
    return -torch.autograd.grad(D, q_dot, create_graph=True)[0]
```

## Toy Example

**Damped harmonic oscillator, 1 DoF.** Mass `m`, spring `k`, damping `γ`.

| Quantity | Formula |
|---|---|
| Lagrangian | `L = ½m q̇² − ½k q²` |
| Rayleigh function | `D = ½γ q̇²` |
| Modified E-L equation | `m q̈ + k q + γ q̇ = 0` |
| Energy `E = T + V` | decays as `dE/dt = −γ q̇²` |

`γ ≥ 0` is exactly the PSD condition that gives `dE/dt ≤ 0`. Set `γ = 0` to recover undamped SHO.

**Damped pendulum.** Same `D = ½γ q̇²` added to the standard pendulum Lagrangian; gives the standard physically-realistic damped pendulum dynamics. Used in [[D-HNN]] Task 2 (real damped pendulum experiment).

## Connection to SiS / CTPC

**Where dissipation hits SiS.** The orbital corrector needs to handle:

- **Atmospheric drag** (LEO): velocity-dependent dissipation, fundamentally Rayleigh-style — `F_drag ∝ −ρv|v|·v̂`. Quadratic Rayleigh form is a coarse linear approximation; full drag is a *cubic* Rayleigh form.
- **Solar radiation pressure** (above ~600km): not a Rayleigh dissipation — it's a *non-conservative external force* that depends on solar geometry, not on velocity. Doesn't fit the Rayleigh template.
- **Atmospheric heating** (re-entry): coupled energy-dissipation — Rayleigh-style but involves thermodynamic coupling.

**Hard-constraint status.**

| Dissipation type | Rayleigh form? | `Ḣ ≤ 0` structural? |
|---|---|---|
| Linear drag (PSD `R`) | ✓ quadratic | ✓ |
| Cubic drag | ✓ cubic | ✓ if signs are right |
| SRP | ✗ (not velocity-dependent) | depends on geometry |
| External torque (e.g., reaction wheel) | ✗ (control input) | requires port-Hamiltonian framing with `g(x)u` |

**For drag (the dominant LEO dissipation), quadratic Rayleigh `D = ½q̇ᵀ R q̇` with `R` PSD gives `Ḣ ≤ 0` structurally — this is the SiS hard-constraint primitive for dissipation.** Critically, this requires the `R` matrix to be *constrained PSD by parameterization* (e.g., Cholesky `R = LLᵀ`), not learned freely. A free MLP `D_θ` does *not* give the PSD guarantee — see [[Dissipative-Hamiltonian-Neural-Network]] for the analysis.

**For SRP and other non-Rayleigh dissipation**, the Rayleigh framing alone is insufficient. The full [[Port-Hamiltonian-Systems|port-Hamiltonian]] formulation `dx/dt = (J − R)∇H + g(x)u` adds a control-input term `g(x)u` that handles non-Rayleigh non-conservative forces while preserving the `Ḣ ≤ 0` guarantee on the dissipative part. This is why CTPC needs PHNN-proper, not just Rayleigh-extended Lagrangian.

## Connections

- [[Lagrangian-Mechanics]] — the host formalism; modified Euler-Lagrange equation includes the Rayleigh term
- [[Hamiltonian-Mechanics]] — the dual formalism; Hamilton's equations modified to include `−∂D/∂q̇` in `dp/dt`
- [[Helmholtz-Decomposition]] — generalized framing where `D(q, p)` provides the irrotational component of the dynamics
- [[Dissipative-Hamiltonian-Neural-Network]] — D-HNN generalizes Rayleigh to a phase-space scalar `D(q, p)`
- [[D-HNN]] — Sosanya & Greydanus 2022 paper
- [[Port-Hamiltonian-Systems]] — alternative dissipation framing with structural `Ḣ ≤ 0` *and* control-input handling
- [[Hamiltonian-vs-Lagrangian-Duality]] — Rayleigh is the Lagrangian-side dissipation primitive; PHNN is the Hamiltonian-side primitive

## Open Questions

- **PSD-constrained learned dissipation.** Standard Rayleigh quadratic `D = ½q̇ᵀ R q̇` with PSD `R` gives `Ḣ ≤ 0` structurally. Parameterizing `R` via Cholesky decomposition is well-known, but full-rank Cholesky has `O(d²)` parameters per state. Whether sparse / structured `R` (block-diagonal, low-rank-plus-diagonal) is sufficient for orbital drag is open.
- **Cubic Rayleigh for full drag.** True drag `F = −½ρ C_D A v|v|` is *quadratic in velocity magnitude* → cubic in `q̇` for the force, so the dissipation function is *quartic* `D ∝ |q̇|⁴`. Whether this preserves the `Ḣ ≤ 0` guarantee depends on sign structure; worth working out carefully.
- **Mixed Rayleigh + SRP.** SRP is non-Rayleigh; combining it with Rayleigh drag in a single architecture is a port-Hamiltonian construction (`g(x)u` term). The composition is straightforward in PHNN but requires care in the D-HNN / Helmholtz framing where the irrotational component is a single scalar.
- **D-HNN's generalization to `D(q, p)`.** [[D-HNN]] uses `D(q, p)` not `D(q̇)`. Whether this is equivalent to standard Rayleigh under the Legendre transform, or strictly more general, is a notational question worth working out for theoretical clarity.

## Sources

- `raw/papers/port-hamiltonian/DissipativeHNN.pdf` — Sosanya & Greydanus 2022, Section 3.1 "Hamiltonian mechanics" Eqs. 2–3 (Rayleigh dissipation in the standard mechanics setup); Section 4 generalizes to phase-space `D(q, p)` for the architecture.
- Cline, D. (2017). *Variational Principles in Classical Mechanics.* Univ. of Rochester Library. (Standard textbook reference for Rayleigh dissipation; cited via DissipativeHNN.pdf.)
- Minguzzi, E. (2015). *Rayleigh's dissipation function at work.* European Journal of Physics 36(3):035014. (Modern treatment with non-quadratic generalizations; cited via DissipativeHNN.pdf.)
