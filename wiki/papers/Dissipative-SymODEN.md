---
title: Dissipative SymODEN — Encoding Hamiltonian Dynamics with Dissipation and Control into Deep Learning (Zhong, Dey, Chakraborty 2020)
tags: [port-hamiltonian, hamiltonian-mechanics, dissipation, passivity, cholesky-psd, neural-ode, physics-as-architecture]
sources: [raw/papers/port-hamiltonian/DissipativeSymODEN-Zhong2020.pdf]
created: 2026-05-10
updated: 2026-05-10
sis_relevance: critical
hard_constraint_possible: yes
---

# Dissipative SymODEN — Encoding Hamiltonian Dynamics with Dissipation and Control into Deep Learning (Zhong, Dey, Chakraborty 2020)

> Zhong, Y. D., Dey, B., Chakraborty, A. *Dissipative SymODEN: Encoding Hamiltonian Dynamics with Dissipation and Control into Deep Learning.* ICLR 2020 Workshop on Integration of Deep Neural Models and Differential Equations (DeepDiffEq). arXiv:2002.08860.

## One-Line Intuition

[[HNN]] but with [[Port-Hamiltonian-Systems|port-Hamiltonian]] form `dx/dt = (J − D(q))∇H + g(q)u` baked into the computation graph, with `D` and `M⁻¹` PSD-constrained via Cholesky parameterization. Energy dissipation rate `dH/dt = −∇HᵀD∇H ≤ 0` is structural — passivity by construction, not by training.

## Why This Was Invented

[[HNN]] (Greydanus 2019) and SymODEN (Zhong, Dey, Chakraborty 2020 ICLR — same authors as this paper, predecessor) hardwire energy conservation. Both fail on real systems with friction, drag, or actuator inputs. [[D-HNN]] (Sosanya & Greydanus 2022, concurrent work) tackles this via [[Helmholtz-Decomposition]] but **does not give `Ḣ ≤ 0` structurally** — see [[Dissipative-Hamiltonian-Neural-Network]] § "Why D-HNN is not enough for SiS".

This paper takes a different architectural strategy: encode the *port-Hamiltonian form* directly. Port-Hamiltonian dynamics (Maschke, van der Schaft, Ortega) generalizes Hamiltonian mechanics by adding a PSD dissipation matrix `D` and a control-input channel `g(q)u`, while preserving the energy `H` as a structural anchor. The energy-balance equation gives `dH/dt = −∇HᵀD∇H + ∇Hᵀ[0;g]u`; for the autonomous case (`u = 0`), passivity is automatic *as long as `D` is constrained PSD*. The architectural primitive: **Cholesky parameterization** of `D = LLᵀ` makes any learned `L` produce a valid PSD `D` — no soft loss, no projection step.

## The Math

**Port-Hamiltonian form** (Eq. 3 of paper):

```
                           ┌  ∂H/∂q  ┐                ┌  0   ┐
(q̇, ṗ) = J · ∇H − D(q) · │         │   +   g(q) u   │       │
                           └  ∂H/∂p  ┘                └ ←u→ ┘
              └── Eq. 3 ──────────────────────────────────┘
```

with `J = [[0, I], [−I, 0]]` (skew-symmetric), `D(q)` symmetric PSD, `g(q)` full column rank. With zero dissipation and zero input, this reduces to standard Hamilton's equations.

**Restricted Hamiltonian form** (Eq. 6):

```
H_θ1,θ2(q, p) = ½ pᵀ M⁻¹_θ1(q) p + V_θ2(q)
```

Mechanical form: kinetic-quadratic-in-momentum plus position-only potential. **NOT** a generic `H_θ(q,p)` like HNN/D-HNN. This is the same restriction DeLaN (Lutter et al. 2019) makes — and that LNN explicitly criticized. Trade-off: structural interpretability (you can read off `M, V, D, g` separately as physical components) and parameter efficiency, at the cost of expressivity. Cannot represent relativistic Hamiltonians or non-quadratic-in-`p` kinetic terms. **For orbital, this is fine — orbital `H` is exactly this form** (see [[Port-Hamiltonian-Neural-Network]] § "Why Restricted Hamiltonian Form Is Not a Compromise for Orbital").

**Cholesky parameterization for PSD** (§3.5 — the load-bearing architectural primitive):

```
D_θ4 = L_D Lᵀ_D                ← any L_D (lower-triangular) gives PSD D_θ4
M⁻¹_θ1 = L_M Lᵀ_M + εI         ← positive-definite via the +εI offset
```

For any output of the network parameterizing `L_D`, the resulting `D` is PSD. **This is what makes `dH/dt ≤ 0` structural.** Same trick is used for the mass matrix `M⁻¹` to ensure positive-definiteness (small `ε` for numerical stability and invertibility of `M`).

**Network architecture.** Four separate MLPs:
- `M⁻¹_θ1(q)` — outputs Cholesky factor `L_M`, then `M⁻¹ = L_M Lᵀ_M + εI`
- `V_θ2(q)` — scalar potential (no PSD constraint)
- `g_θ3(q)` — input matrix (no PSD constraint, full column rank assumed)
- `D_θ4(q)` — outputs Cholesky factor `L_D`, then `D = L_D Lᵀ_D`

**Loss.** Standard MSE between predicted and true trajectories, integrated through Neural ODE (Chen et al. 2018) with **RK4** as the solver — *not* a structure-preserving / symplectic integrator (see § Integrator Caveat in [[Port-Hamiltonian-Neural-Network]]).

**Coordinate handling for angles** (§3.3). For state variables on `S¹` (e.g., pendulum angle), use the embedded representation `(cos q, sin q, q̇, u)` instead of raw `q ∈ [−π, π)`. Solves the wraparound issue. Dynamics (Eq. 7):
```
ẋ₁ = −sin q ◦ q̇ = −x₂ ◦ q̇
ẋ₂ = cos q ◦ q̇  = x₁ ◦ q̇
ẋ₃ = (d/dt)(M⁻¹(x₁, x₂) p) = ...
```

**Hybrid spaces** (§3.4): for systems with both translational `r ∈ Rⁿ` and rotational `φ ∈ Tᵐ` coordinates (cartpole, acrobot, spacecraft attitude + position), combine the embedded-angle approach with raw-position channels. State becomes `(r, cos φ, sin φ, ṙ, φ̇, u)`.

## Toy Examples

Four tasks (§4):

| # | Task | State | Highlights |
|---|---|---|---|
| 1 | Pendulum, `(q, p)` data | 2-D | recovers `M⁻¹, V, g` cleanly; `D` doesn't fit perfectly with limited data |
| 2 | Pendulum, embedded angle | 3-D | with `(cos q, sin q, q̇)`, `D` recovery improves substantially |
| 3 | CartPole | 5-D | hybrid R × T |
| 4 | Acrobot | 6-D | hybrid R² × T² |

**Quantitative results** (Table 1, prediction error on 40-step rollout from training initial condition):

| Task | Naive Baseline | Geometric | Unstr. Diss. SymODEN | SymODEN | **Dissipative SymODEN** |
|---|---|---|---|---|---|
| 1: Pendulum (q,p) | 32.5 ± 36 | — | 219 ± 297 | 96.5 ± 99.6 | **34.0 ± 47.8** |
| 2: Pendulum (embedded) | 40 ± 78 | 0.81 ± 0.68 | 7.04 ± 13.7 | 73 ± 90 | **1.04 ± 1.3** |
| 3: CartPole | 268 ± 204 | 60 ± 96 | 366 ± 405 | 30 ± 34 | **8.32 ± 7.81** |
| 4: Acrobot | 36.65 ± 77 | 44.26 ± 96 | 590.77 ± 808 | 68 ± 103 | **12.72 ± 32** |

**Two key takeaways:**

1. **Dissipative SymODEN wins on all 4 tasks.** Even where SymODEN's energy-conservation prior is wrong (dissipative systems), the J-R structure recovers correctly because `D` is allowed (and Cholesky-PSD-constrained).
2. **Parameter efficiency.** Dissipative SymODEN: 0.15M–0.53M params. Naive baseline: 0.36M–1.46M params. **Smaller networks, better accuracy** — the structural prior trades parameters for inductive bias.

The "Unstructured Dissipative SymODEN" (Hamiltonian replaced by free MLP — *no* mechanical-form restriction) does *worst* of all variants except in Task 2. **The restriction `H = ½pᵀM⁻¹(q)p + V(q)` is doing real work** — it's not a constraint to be relaxed; it's the structural commitment that makes the architecture identifiable from limited data.

## Connection to SiS / CTPC

**This paper closes Q8b-vector-field of [[CTPC-Design-Rationale]] Part II** — the J-R-structure route to `Ḣ ≤ 0` as a hard architectural constraint. The Cholesky-PSD parameterization of `D` is exactly the SiS-canonical primitive for the dissipation block.

### What this paper gives CTPC

- **Cholesky parameterization → structural `Ḣ ≤ 0`.** This is the deployment-safety primitive SiS needs. PSD by construction; not learned, not penalized.
- **Restricted mechanical-form Hamiltonian → structural match for orbital.** Orbital `H = |p|²/(2m) + V(r)` is exactly this form. Architecture matches physics; no expressivity is sacrificed for the orbital domain.
- **Native control-input channel `g(q)u` → free asset for future maneuver planning.** Currently unused in CTPC (no actuator commands during forecast); slots in directly for an integrated state-forecasting + Δv-recommendation extension later.
- **Embedded-angle handling.** For spacecraft attitude (orientation on `SO(3)`), the embedded approach (`cos φ, sin φ, ω`) translates the same way the paper handles pendulum angles. Makes attitude integration with PHNN-proper concrete.
- **Parameter efficiency.** Smaller networks → easier to train on limited spacecraft data, faster inference for real-time CTPC corrector evaluation.

### What this paper does NOT give CTPC (the open caveats)

- **Discrete-time `Ḣ ≤ 0` is approximate, not exact.** The paper uses RK4. The continuous-time guarantee is structural; the integrated-trajectory guarantee is RK4-truncation-error-bounded. This is the **Q8b-discrete-time** open question — see [[Port-Hamiltonian-Neural-Network]] § "Integrator Caveat" for the three candidate structure-preserving integrators (discrete gradient method, AVF, Gauss-collocation).
- **Single dissipation matrix `D(q)`, not channel-decomposed.** [[CBM-CTPC-Composition]] (Year 1 milestone) needs `D = Σᵢ αᵢ Dᵢ(q)` for per-concept counterfactual interventions on individual dissipation channels (drag, SRP, thermal). The extension is straightforward but isn't in this paper.
- **No symmetry-hardwiring.** [[PhyArch]] composes orthogonally inside the `M⁻¹/V/D` networks but the paper doesn't address this composition.

### Q8b closure status (post-ingest 2026-05-10)

| Sub-question | Status | Closing mechanism |
|---|---|---|
| Q8b-vector-field — `Ḣ ≤ 0` structural at the continuous-time level | **✓ closed** | This paper's Cholesky-PSD parameterization of `D` |
| Q8b-discrete-time — `Ḣ ≤ 0` exact at the integrated-trajectory level | open | Future ingest of structure-preserving integrators (discrete gradient / AVF / Gauss-collocation) |

## Connections

- [[Port-Hamiltonian-Neural-Network]] — the architecture pattern this paper introduces, separated from the paper's specific experiments. **Detailed treatment of the SiS deployment story lives there.**
- [[Port-Hamiltonian-Systems]] — the math/physics foundation
- [[HNN]] — the conservative-only predecessor in the broader Greydanus family
- [[Hamiltonian-Neural-Network]] — the corresponding architecture pattern
- [[D-HNN]] — Sosanya & Greydanus 2022; the *alternative* (Helmholtz-route) dissipative HNN; concurrent with this paper
- [[Dissipative-Hamiltonian-Neural-Network]] — D-HNN architecture pattern; § "Why D-HNN is not enough for SiS" gives the precise reason this paper is the SiS-canonical choice
- [[Helmholtz-Decomposition]] — math foundation of the alternative route
- [[Hamiltonian-Mechanics]] — the conservative special case
- [[Rayleigh-Dissipation-Function]] — the classical-mechanics primitive both routes generalize
- [[Hamiltonian-vs-Lagrangian-Duality]] § "A Fourth Axis: Dissipation Route" — the Helmholtz-vs-J-R design-axis treatment
- [[CTPC-Design-Rationale]] Q8b — closed (vector-field level) by this paper
- [[CBM-CTPC-Composition]] — Year 1 milestone; channel-decomposed extension flagged
- [[LNN]] — Lagrangian-side counterpart; the Lagrangian PHNN-equivalent (Lagrangian + PSD-constrained Rayleigh) is unnamed in literature

## Open Questions

- **Discrete-time exact passivity (Q8b-discrete-time).** Paper uses RK4. Three candidate integrators flagged in [[Port-Hamiltonian-Neural-Network]] § "Integrator Caveat": discrete gradient method, AVF, Gauss-collocation. Future ingest target.
- **Channel-decomposed `D = Σᵢ αᵢ Dᵢ(q)`.** Per-concept counterfactual extension for [[CBM-CTPC-Composition]]; sum of PSD matrices is PSD so structural guarantee survives, but isn't in this paper.
- **Position- and time-dependent `D`.** Atmospheric density varies with altitude, time, solar activity. Architecture allows `D(q)` directly; `D(q, t)` requires time channel; whether this scales to multi-spacecraft / space-weather conditioning is open.
- **Composition with PhyArch.** Symmetry-hardwiring of the `M⁻¹/V/D/g` networks (parity-split, equivariant features). Conceptually clean but the paper doesn't address it.
- **Unstructured Dissipative SymODEN's failure mode.** Replacing the mechanical-form Hamiltonian with a free MLP — generic `H_θ(q,p)` — *worsens* performance significantly (Table 1). Why? Because the four-network decomposition `(M⁻¹, V, D, g)` is *over-determined* without the form constraint — many `(M⁻¹, V)` pairs produce the same `H`, so the decomposition is non-identifiable. The mechanical-form restriction *enforces identifiability*. Worth flagging — restriction is doing more than constraining expressivity; it's making the inverse problem well-posed.
- **Acknowledgments + funding.** Yaofeng Zhong's work supported by ONR grant #N00014-18-1-2873; mostly carried out at Siemens Corporate Technology during an internship. Worth noting: the same N. E. Leonard / A. Majumdar / Princeton group has been deeply involved in port-Hamiltonian and stability-guaranteed control over decades — this paper is part of a long line.

## Sources

- `raw/papers/port-hamiltonian/DissipativeSymODEN-Zhong2020.pdf` — full paper. §1 (motivation + contribution); §2 (port-Hamiltonian dynamics review, Eqs. 1–3); §3 (the architecture: §3.1 training Neural ODE with constant forcing, §3.2 generalized coords + momentum + Eqs. 5–6, §3.3 embedded angle data + Eq. 7, §3.4 hybrid spaces, **§3.5 the Cholesky parameterization for PSD `D` and `M⁻¹`** — the load-bearing architectural primitive); §4 (experiments + Fig. 1–3 learned components + Table 1 quantitative comparison); §4.1 specifies RK4 as the integrator (the integrator caveat).
- Code: <https://github.com/d-biswa/Symplectic-ODENet> (predecessor SymODEN); Dissipative SymODEN extends it.
