---
title: D-HNN — Dissipative Hamiltonian Neural Networks (Sosanya & Greydanus 2022)
tags: [hamiltonian-mechanics, dissipation, helmholtz-decomposition, physics-as-architecture, oceanography, counterfactual-generalization]
sources: [raw/papers/port-hamiltonian/DissipativeHNN.pdf]
created: 2026-05-10
updated: 2026-05-10
sis_relevance: high
hard_constraint_possible: partial
---

# D-HNN — Dissipative Hamiltonian Neural Networks (Sosanya & Greydanus 2022)

> Sosanya, A., Greydanus, S. *Dissipative Hamiltonian Neural Networks: Learning Dissipative and Conservative Dynamics Separately.* arXiv:2201.10085, Jan 2022.

## One-Line Intuition

[[HNN]] but with two scalar subnetworks: `H_θ(q,p)` for the conservative dynamics + `D_θ(q,p)` for the dissipative. Sum the symplectic gradient of `H` and the ordinary gradient of `D` to get the full velocity field. By [[Helmholtz-Decomposition]], this representation is *complete* (any smooth field decomposes this way) and *forced* (the conservative/dissipative split is structural, not emergent).

## Why This Was Invented

[[HNN]] (Greydanus et al. 2019) hardwires energy conservation, which is exactly wrong for any real-world dynamic system with friction, drag, or external dissipation. The HNN paper itself flagged this: real damped pendulum data made HNN's energy-conservation prior strictly *worse* than a vanilla MLP (Task 3 of the original HNN paper).

Sosanya & Greydanus's question: how do you keep HNN's structural advantages on the *conservative* part while letting the network *also* model dissipation? Their answer: parameterize a *second* scalar `D_θ` and combine via [[Helmholtz-Decomposition]]. Two scalars instead of one; cleanly separated by Helmholtz uniqueness; structural completeness preserved.

## The Math

The architecture in three lines (Eq. 5 of the paper):

1. **Two scalar networks**: `H_θ : ℝ^{2N} → ℝ` (Hamiltonian) and `D_θ : ℝ^{2N} → ℝ` (Rayleigh-style dissipation function generalized to phase space).
2. **Combine via Helmholtz form**:
   ```
   (dq/dt, dp/dt) = (∂H/∂p, −∂H/∂q) + (∂D/∂q, ∂D/∂p)
                     └ symplectic ─┘   └── ordinary ─┘
                     conservative      dissipative
   ```
3. **Loss** (Eq. 5):
   ```
   L_DHNN = ‖(∂H/∂p + ∂D/∂q) − dq/dt‖² + ‖(−∂H/∂q + ∂D/∂p) − dp/dt‖²
   ```

**Helmholtz uniqueness.** [[Helmholtz-Decomposition]] (paper Section 3.2): any smooth vector field with appropriate decay decomposes uniquely as `V = V_irr + V_rot` (irrotational gradient + solenoidal curl). The architecture *cannot* produce dynamics outside this form because it has only two scalar outputs. Two scalars; complete vector field.

**Generalization of `D` from `D(q̇)` to `D(q, p)`.** Standard Rayleigh dissipation (Section 3.1, Eq. 2) has `D = D(q̇)` and `F^{dis}_i = −∂D/∂q̇_i`. The paper *generalizes* without flagging it explicitly: `D_θ(q, p)` is a scalar field over the *full phase space*, not just velocity, and the architecture takes ordinary gradients in *both* `q` and `p`. This is the move that makes Rayleigh dissipation map cleanly to Helmholtz framing — standard Rayleigh becomes a special case. Easy to miss when reading. Worth flagging because it changes the semantics of `D_θ` from "dissipation function" (Rayleigh) to "irrotational potential" (Helmholtz).

## Toy Example

**Damped mass-spring** (Task 1, Section 5): `H = ½kq² + p²/(2m)`, dissipation `∝ p²` with friction coefficient `ρ`. Train with `ρ = 2` for 5000 gradient steps; test in-distribution + counterfactual `ρ ∈ {1, 4}`.

| Model | Test MSE (×10⁻⁵) | Energy MSE (×10⁻⁵) |
|---|---|---|
| MLP | 9.9 | 0.6 |
| HNN | 63,000 | 29,000 |
| **D-HNN** | **5.8** | **0.36** |
| Numerical (Gauss-Seidel Helmholtz) | 730 | — (interpolation error dominates) |

D-HNN beats HNN by ~4 orders of magnitude on test MSE — HNN structurally over-conserves; baseline MLP fits but can't decompose; D-HNN does both. **The MLP's Test MSE is competitive but misleading** — it can't separate conservative and dissipative components, so it can't generalize to unseen friction coefficients.

**Counterfactual generalization to unseen friction** (Fig. 3). Multiply trained `D_θ` by `α = 0.5` or `α = 2.0` at inference → predict dynamics under `ρ = 1` or `ρ = 4` (factor-of-2 changes in friction). The model only saw `ρ = 2` during training. The MLP cannot do this — it has no "friction knob." This is the architectural hook for an *intervention class* over dissipation parameters; **see § "Counterfactual generalization mechanism" in [[Dissipative-Hamiltonian-Neural-Network]]** for the SiS-relevant framing.

**Real damped pendulum** (Task 2, Section 5; data from Schmidt & Lipson 2009). Hamiltonian `H = 2mgl(1 − cos q) + l²p²/(2m)` with `l = m = 1`, `g = 3`. D-HNN beats both HNN and MLP on coordinate MSE and energy MSE (Table 1). HNN explicitly fails because it forces energy conservation in a system that *isn't* conservative.

**OSCAR ocean currents** (Task 3, Section 5). Real satellite data from Earth and Space Research, 69 time frames × 50×50 grid of surface currents in the Atlantic. Trained for 24,000 steps. D-HNN learns scalar fields `H` and `D` over the ocean surface; their level sets correspond to known oceanographic features:

- Vortex centers (eddies) — visible as bright/dark spots in `H`'s scalar field. To maintain geostrophic balance in a vortex, water moves up- or down-ward.
- Upwelling / downwelling — visible in `D`'s scalar field. Cold, nutrient-rich waters rise to the surface in upwelling regions.

The decomposition matches non-ML Helmholtz analyses (Zhang et al. 2019). D-HNN beats the baselines on test MSE (Table 1: 0.143 vs. 0.146 HNN vs. 0.181 MLP) but the gap is small — most ocean dynamics are conservative-dominated, so the dissipation modeling gain is modest. The interesting result is the *interpretability*: the learned scalar fields surface oceanographic features that ML models without the decomposition cannot.

## Connection to SiS / CTPC

D-HNN is **partially** relevant for SiS — it closes one route to dissipative HNN (the Helmholtz-decomposition route) but not the route SiS actually needs (the J-R-structured port-Hamiltonian route).

### Where it helps SiS

**Counterfactual generalization template.** The `α · D_θ` knob for unseen friction coefficients is a clean architectural mechanism for *intervention compositionality* on dissipation parameters. In the [[Actionable-Interpretability-Symmetries]] framework, this is concept-closure compliance on the dissipation-coefficient concept space — the model supports the intervention class "what if friction were different?" by construction. This is a template that should transfer to whatever dissipation architecture CTPC ends up with: parameterize individual dissipation channels (drag, SRP, atmospheric heating) so each can be scaled independently for counterfactual reasoning.

**Diagnostic for learned dynamics.** Even when the deployed CTPC architecture is PHNN-proper (not D-HNN), the Helmholtz decomposition can be applied *post-hoc* to inspect a learned vector field. Useful for debugging and interpretability.

### Where it fails SiS

**`Ḣ ≤ 0` is not satisfied structurally.** Compute `dH/dt` along D-HNN dynamics:

```
dH/dt = ∇H · grad(D)                                                  (∗)
```

The conservative cross-terms vanish (correctly), but the dissipative term is `∇H · ∇D` — the inner product between the gradients of two *learned* scalars. This sign is *indeterminate*. The architecture imposes no constraint forcing `∇H · ∇D ≤ 0`. By contrast, [[Port-Hamiltonian-Systems|port-Hamiltonian]] dynamics `(J − R)∇H` give `dH/dt = −∇Hᵀ R ∇H ≤ 0` *structurally* whenever `R` is constrained PSD. **For CTPC's deployment-safety requirement, the structural guarantee is required, not just empirical satisfaction.** See [[Dissipative-Hamiltonian-Neural-Network]] § "Why D-HNN is not enough for SiS" for the full analysis.

### Q8 closure status (updated 2026-05-10 after Dissipative-SymODEN ingest)

[[CTPC-Design-Rationale]] Part II Q8 (HNN/LNN/PHNN integration) now splits three ways:
- **Q8a — Helmholtz-decomposition route:** *closed* by D-HNN. The architecture exists, scales (in principle), generalizes counterfactually via `α · D_θ`.
- **Q8b-vector-field — J-R-structured route at continuous-time:** *closed* by [[Dissipative-SymODEN]] (ingested 2026-05-10). Cholesky-PSD parameterization of the dissipation matrix gives `dH/dt = −∇HᵀD∇H ≤ 0` structurally at the level of the continuous-time port-Hamiltonian ODE.
- **Q8b-discrete-time — exact discrete passivity:** *open*. Dissipative SymODEN uses RK4, not a structure-preserving integrator. Three candidate integrators (discrete gradient method, AVF, Gauss-collocation) flagged in [[Port-Hamiltonian-Neural-Network]] § "Integrator Caveat" for future ingest.

## Connections

- [[Dissipative-Hamiltonian-Neural-Network]] — the architecture pattern this paper introduces, separated from this paper's specific experiments. **Detailed treatment of "Why D-HNN is not enough for SiS" lives there.**
- [[Helmholtz-Decomposition]] — the math foundation
- [[Rayleigh-Dissipation-Function]] — the classical-mechanics primitive that D-HNN generalizes
- [[HNN]] — the conservative-only predecessor; D-HNN's first author (Sosanya) collaborated with HNN's first author (Greydanus) directly on this extension
- [[Hamiltonian-Neural-Network]] — the architectural pattern D-HNN extends
- [[Lagrangian-Neural-Network]] — Lagrangian-side analog; the corresponding dissipative extension goes through Rayleigh's modified Euler-Lagrange equation (Eq. 2 of [[Rayleigh-Dissipation-Function]])
- [[Hamiltonian-vs-Lagrangian-Duality]] — D-HNN occupies the *Helmholtz-decomposition leg* of the third axis (dissipation-route axis: Helmholtz vs J-R)
- [[Port-Hamiltonian-Systems]] — the alternative, stronger dissipation framing that SiS uses for the deployment primitive
- [[Port-Hamiltonian-Neural-Network]] — the SiS-canonical dissipative block; the J-R-route NN realization
- [[Dissipative-SymODEN]] — Zhong, Dey, Chakraborty 2020; closes Q8b-vector-field
- [[CTPC-Design-Rationale]] — Q8a closed by this paper; Q8b-vector-field closed by Dissipative SymODEN; Q8b-discrete-time still open
- [[Pi-Block-Polynomial-Approximator]] — Cranmer thread: Miles Cranmer is in the D-HNN acknowledgments ("prior collaborations and fruitful conversations that helped lay the groundwork"), reinforcing the Greydanus-Cranmer collective as the central thread of conservative-dynamics ML

## Open Questions

- **`Ḣ ≤ 0` constraint via parameterization.** Could a constrained parameterization of `D_θ` (e.g., monotonic-decreasing in `‖p‖²` for separable systems) recover the passivity guarantee? In general no — the constraint depends on the unknown `H_θ`. But for separable systems with separable dissipation it may decouple. Open theoretical question.
- **Symplectic-integrator compatibility.** [[HNN]] pairs naturally with leapfrog/Yoshida for exact discrete `H`-conservation. D-HNN's dissipative term breaks symplecticity; the right discrete analog is variational dissipation (Marsden-West with Rayleigh) but isn't in standard ML codebases. Implementation gap.
- **High-DoF scaling.** Paper tests on 1-DoF mechanical systems and 2-D ocean fields. Whether the architecture scales cleanly to `d > 10` (orbital state, full N-body) is unclear — Helmholtz decomposition still works mathematically, but the optimization landscape may degrade.
- **Decomposition gauge.** `D_θ` is determined only up to a harmonic function. The training objective doesn't pin this gauge. Whether the optimization finds a "natural" gauge or an arbitrary one affects interpretability of the learned `D_θ`. Worth empirical investigation.
- **OSCAR result is interpretability-driven, not accuracy-driven.** D-HNN beats baselines on test MSE by a small margin on the ocean-current task; the value-add is the decomposition for upwelling/downwelling localization. Whether SDA has analogous interpretability tasks where decomposition-for-feature-detection matters operationally (e.g., separating gravitational from atmospheric perturbations) is worth thinking about.

## Sources

- `raw/papers/port-hamiltonian/DissipativeHNN.pdf` — full paper. Section 3.1 (Hamiltonian + Rayleigh review, Eqs. 1–3); Section 3.2 (Helmholtz decomposition, Eq. 4); Section 4 (architecture + loss, Eq. 5; tasks defined); Section 5 (results, Fig. 2 = damped spring decomposition, Fig. 3 = counterfactual friction, Fig. 4 = real pendulum trajectories, Fig. 5 = OSCAR ocean current decomposition; Table 1 quantitative comparison); Section 6 (discussion).
- Code: <https://github.com/greydanus/dissipative_hnns>
