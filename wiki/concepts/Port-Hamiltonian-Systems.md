---
title: Port-Hamiltonian Systems
tags: [classical-mechanics, control-theory, port-hamiltonian, passivity, dissipation, foundational]
sources: [raw/papers/port-hamiltonian/DissipativeSymODEN-Zhong2020.pdf]
created: 2026-05-10
updated: 2026-05-10
sis_relevance: critical
hard_constraint_possible: yes
---

# Port-Hamiltonian Systems

## One-Line Intuition

A generalization of [[Hamiltonian-Mechanics]] that *adds* dissipation and external inputs while keeping the energy `H` as a structural anchor: `dx/dt = (J − R)∇H + g(x)u`. The PSD matrix `R` encodes dissipation; the energy balance `dH/dt = −∇Hᵀ R ∇H + ∇Hᵀ g u` makes passivity (`Ḣ ≤ 0` for `u = 0`) *structural*, not learned.

## Why This Was Invented

Hamiltonian mechanics is beautiful for conservative systems but fails when energy isn't conserved — friction, drag, resistive losses break the formalism. Two classical fixes existed:

1. Add a [[Rayleigh-Dissipation-Function]] `D(q̇)` to Lagrangian mechanics (works, but Lagrangian-side only).
2. Add ad-hoc dissipative terms to Hamilton's equations (loses the algebraic clarity of the formalism).

The port-Hamiltonian framework (Maschke, van der Schaft, Ortega, in the 1990s–2000s) found a third path: keep `H` as the energy anchor, but generalize the *structural matrix* on Hamilton's equations from the symplectic `J = [[0, I], [−I, 0]]` (skew-symmetric, conservative) to `J − R` where `R` is positive semi-definite (dissipative). Add a separate input channel `g(x)u` for external forcing. The resulting form *unifies* mechanical, electrical, and electromechanical systems in a single language built around energy flow.

For SiS this matters because port-Hamiltonian dynamics is the **structural primitive** for `Ḣ ≤ 0` as a hard architectural constraint — the SiS deployment-safety requirement. PSD-by-parameterization of `R` (e.g., via Cholesky decomposition, see [[Port-Hamiltonian-Neural-Network]]) gives the constraint structurally, not through a soft loss penalty.

## The Math

**The port-Hamiltonian form.** State `x = (q, p) ∈ ℝ^{2n}`, scalar Hamiltonian `H(x) : ℝ^{2n} → ℝ` (typically energy). The dynamics:

```
dx/dt = [J(x) − R(x)] ∇H(x) + g(x) u                                   (PH)
```

with:

| Object | Property | Role |
|---|---|---|
| `J(x) ∈ ℝ^{2n × 2n}` | skew-symmetric: `Jᵀ = −J` | interconnection / conservative coupling |
| `R(x) ∈ ℝ^{2n × 2n}` | symmetric, PSD | dissipation / resistance |
| `g(x) ∈ ℝ^{2n × m}` | full column rank | input map |
| `u ∈ ℝ^m` | — | external input / control |

For mechanical systems with canonical coordinates, `J` is the standard symplectic block matrix `[[0, I], [−I, 0]]` and is constant. `R` and `g` are typically position-dependent; `R` may also depend on `p` for velocity-dependent damping.

**Energy balance.** Time derivative of `H` along (PH) trajectories:

```
dH/dt = ∇Hᵀ ẋ
       = ∇Hᵀ (J − R) ∇H + ∇Hᵀ g u
       = ∇Hᵀ J ∇H − ∇Hᵀ R ∇H + ∇Hᵀ g u
       = 0           − ∇Hᵀ R ∇H + ∇Hᵀ g u                                  (∗)
```

The first term vanishes because `J` is skew-symmetric (`xᵀ J x = 0` for any skew `J`). The second is non-positive when `R` is PSD. The third represents power input from external sources.

**Passivity.** For the autonomous (`u = 0`) system with `R` PSD:

```
dH/dt = −∇Hᵀ R ∇H ≤ 0                                                   (passivity)
```

Energy can only decrease. This is the *structural* passivity guarantee — built into the algebraic form, not learned. **For SiS this is the Ḣ ≤ 0 hard constraint** that distinguishes Q8b (closed by [[Port-Hamiltonian-Neural-Network]]) from Q8a (the [[Helmholtz-Decomposition]] route, where `dH/dt = ∇H · ∇D` has indeterminate sign — see [[Dissipative-Hamiltonian-Neural-Network]] § "Why D-HNN is not enough for SiS").

**Stability via passivity.** Passive systems with positive-definite `H` (energy bounded below, `H(x) → ∞` as `‖x‖ → ∞`) are *Lyapunov-stable* with `H` as the Lyapunov function. If `R` is positive definite (not just PSD) on a neighborhood of the equilibrium, the system is *asymptotically* stable. This connects port-Hamiltonian structure to control-theoretic stability guarantees automatically.

**Examples of the (J, R, g) decomposition.**

| System | `H` | `J` | `R` | `g` |
|---|---|---|---|---|
| Damped mass-spring | `½kq² + p²/(2m)` | `[[0,1],[−1,0]]` | `[[0,0],[0,γ/m]]` | `[[0],[1]]` if forced |
| RLC circuit (charge `q`, flux `p`) | `½q²/C + ½p²/L` | symplectic | `[[0,0],[0,R]]` (resistor) | source-dependent |
| Robotic arm with friction | `½q̇ᵀM(q)q̇ + V(q)` (in `(q, p)` form) | symplectic | velocity-dependent friction | actuator coupling |
| **Two-body orbit + drag** | `|p|²/(2m) + V(r)` (Newtonian gravity) | symplectic | drag-velocity coupling (small) | thrust input (when present) |

The orbital row matters most for SiS — the orbital Hamiltonian is *exactly* the mechanical form, drag is approximately a `pᵀRp` quadratic dissipation, and future maneuver-planning extensions slot into the `g·u` channel.

### Code Correspondence

```python
import numpy as np
from scipy.integrate import solve_ivp

def port_hamiltonian_flow(grad_H, J, R, g, u, x0, t_span):
    """Integrate ẋ = (J − R) ∇H(x) + g(x) u from x0 over t_span.

    Args:
      grad_H : callable x → ∇H(x), shape (2n,)
      J, R   : callables x → matrix, shape (2n, 2n).  J skew, R PSD.
      g      : callable x → matrix, shape (2n, m); set to None if no input.
      u      : callable t → input, shape (m,); set to None if no input.
    """
    def rhs(t, x):
        gradH = grad_H(x)
        out   = (J(x) - R(x)) @ gradH
        if g is not None and u is not None:
            out = out + g(x) @ u(t)
        return out
    return solve_ivp(rhs, t_span, x0, rtol=1e-9)

# Verify energy balance: dH/dt should equal -∇H^T R ∇H + ∇H^T g u along trajectory
```

For the *learned* version, `H, M⁻¹, V, R, g` are replaced by neural networks with PSD constraints on `R` and `M⁻¹` enforced via Cholesky parameterization — see [[Port-Hamiltonian-Neural-Network]].

## Toy Example

**Damped 1-DoF harmonic oscillator.** `H = ½kq² + p²/(2m)`, `J = [[0,1],[−1,0]]`, `R = [[0,0],[0,γ/m]]`, no input.

Dynamics from (PH):
```
(dq/dt, dp/dt) = (p/m, −kq − γp/m)
```
which is exactly `m q̈ + γ q̇ + k q = 0` (the standard damped oscillator).

Energy balance:
```
dH/dt = −∇Hᵀ R ∇H = −(p/m)² · γ = −γ q̇² ≤ 0  ✓
```
Energy decays at rate proportional to dissipated power — exactly the physical expectation.

## Connection to SiS / CTPC

**The structural primitive for `Ḣ ≤ 0`.** Port-Hamiltonian form is *the* architectural template SiS needs for the corrector's dissipative block. The PSD-by-parameterization of `R` makes `dH/dt ≤ 0` structural, not learned — the architecture *cannot* produce non-passive dynamics, regardless of training data or input distribution.

**Direct mapping to the orbital corrector.**

| PH component | Orbital realization |
|---|---|
| `H` | Total orbital energy = kinetic + Newtonian gravity + perturbation potentials |
| `J` | Symplectic `[[0, I], [−I, 0]]` for `(r, p)` canonical coordinates |
| `R` | Drag matrix (dominant in LEO); thermal coupling for re-entry; structural-mode damping |
| `g` | Maneuver input matrix — empty for current CTPC (no actuator commands during forecast); active for future maneuver-planning extension |
| `u` | `Δv` thrust commands when present |

**Why this is a structural match for orbital, not a compromise.** The Dissipative SymODEN paper restricts the Hamiltonian to mechanical form `H = ½ pᵀ M⁻¹(q) p + V(q)`. **The orbital Hamiltonian *is* this form** — `|p|²/(2m) + V(r)` in the two-body case, with `V(r)` extended to include J2/J3 zonal harmonics + lunisolar perturbations + SRP potential. So the architectural restriction is exactly what orbital physics already provides; expressivity isn't compromised.

**Hard-constraint classification.** Yes. Both the energy structure (`H` as anchor) and the passivity guarantee (`R` PSD) are baked into the architecture, not penalized via loss.

## Connections

- [[Hamiltonian-Mechanics]] — the conservative special case (`R = 0`, `u = 0`); port-Hamiltonian generalizes
- [[Symplectic-Gradient]] — the `J ∇H` part of port-Hamiltonian dynamics
- [[Rayleigh-Dissipation-Function]] — the Lagrangian-side dissipation primitive; equivalent to the `R ∇H` part for quadratic `R`
- [[Helmholtz-Decomposition]] — alternative dissipation framing; lacks the `Ḣ ≤ 0` guarantee that PH has
- [[Port-Hamiltonian-Neural-Network]] — the SiS-canonical NN realization (Cholesky-parameterized `R`)
- [[Dissipative-SymODEN]] — Zhong, Dey, Chakraborty 2020; the introducing paper for the NN realization
- [[Dissipative-Hamiltonian-Neural-Network]] — the *alternative* (Helmholtz-route) NN realization; insufficient for SiS — see § "Why D-HNN is not enough for SiS"
- [[Hamiltonian-vs-Lagrangian-Duality]] § "A Fourth Axis: Dissipation Route" — the J-R route this concept underpins
- [[CTPC-Design-Rationale]] Q8b — closed by this concept's NN realization

## Open Questions

- **Position-dependent `R(x)`.** Drag varies with altitude (atmospheric density), aerodynamic regime, and attitude. A position-dependent `R(q)` is straightforward in port-Hamiltonian form. The neural-net realization makes this a learned function with PSD constraint at every point. Whether the resulting `R(q)` interpolates physically across atmospheric regimes is open empirically.
- **Time-varying `R(x, t)`.** Solar activity affects atmospheric density on hour-to-day timescales. Time-varying `R` is allowed in the formalism but requires the time channel as an additional input. Worth flagging when extending to multi-spacecraft / space-weather conditioning.
- **Discrete-time passivity preservation.** The continuous-time `Ḣ ≤ 0` guarantee is structural, but standard ODE integrators (RK4 in the Dissipative-SymODEN paper) only preserve it approximately. For exact discrete-time passivity, structure-preserving integrators (discrete gradient method, AVF, Gauss-collocation) are the candidate next-stage tools — see [[Port-Hamiltonian-Neural-Network]] § "Integrator Caveat".
- **Constrained port-Hamiltonian systems.** Holonomic / non-holonomic constraints (e.g., spacecraft attitude on the unit-quaternion manifold) generalize via implicit port-Hamiltonian form `Eẋ = (J − R)∇H + gu` with `E` allowed singular. Adds Dirac-structure machinery; relevant for spacecraft attitude but not core orbital corrector.

## Sources

- `raw/papers/port-hamiltonian/DissipativeSymODEN-Zhong2020.pdf` — Zhong, Dey, Chakraborty 2020. §2 "The Port-Hamiltonian Dynamics" (Eqs. 1–3); compactly states the formalism in the form used by the NN realization.
- van der Schaft, A., Maschke, B. (1995). *The Hamiltonian formulation of energy conserving physical systems with external ports.* Archiv für Elektronik und Übertragungstechnik 49(5/6):362–371. — Original formulation, *not yet ingested* (no PDF in raw/).
- Ortega, R., Van Der Schaft, A., Maschke, B., Escobar, G. (2002). *Interconnection and damping assignment passivity-based control of port-controlled Hamiltonian systems.* Automatica 38(4):585–596. — Cited via Dissipative-SymODEN as reference [18]; gives the IDA-PBC control technique built on PH structure.
- Port-Hamiltonian textbook (`raw/books/dynamical-systems/Port_Hamiltonian_Book.pdf`) — *not yet ingested*. Will substantially expand this page on next ingest.
