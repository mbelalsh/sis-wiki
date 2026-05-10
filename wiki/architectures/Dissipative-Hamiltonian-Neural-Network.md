---
title: Dissipative Hamiltonian Neural Network (D-HNN)
tags: [hamiltonian-mechanics, dissipation, helmholtz-decomposition, architecture-pattern, scalar-parameterization, physics-as-architecture]
sources: [raw/papers/port-hamiltonian/DissipativeHNN.pdf]
created: 2026-05-10
updated: 2026-05-10
sis_relevance: high
hard_constraint_possible: partial
---

# Dissipative Hamiltonian Neural Network (D-HNN)

## One-Line Intuition

Two scalar subnetworks `H_θ(q,p)` and `D_θ(q,p)`. Symplectic-flip the gradient of `H` to get the conservative dynamics; take the ordinary gradient of `D` to get the dissipative dynamics; sum. By [[Helmholtz-Decomposition]] the architecture can represent *any* smooth vector field with the conservative/dissipative split forced rather than emergent.

## Why This Was Invented

[[Hamiltonian-Neural-Network]] enforces energy conservation by construction — but real systems aren't conservative. Friction, drag, and SRP add dissipative forces that pure HNN cannot represent. The architectural question: how do you *add dissipation* to HNN without losing the structural conservation guarantee on the conservative part?

[[D-HNN]] (Sosanya & Greydanus 2022) answers via the [[Helmholtz-Decomposition]]: parameterize the dissipative dynamics as the ordinary gradient of a *second* scalar `D_θ`. The conservative part stays clean (only `H_θ` produces conservative flow); the dissipative part is structurally separate (only `D_θ` produces irrotational flow). Two scalars, complete vector field, forced split.

This is one of two architectural strategies for adding dissipation to HNN. The other — [[Port-Hamiltonian-Systems|port-Hamiltonian]] structure with `dx/dt = (J − R)∇H` — gives a *stronger* guarantee (`Ḣ ≤ 0`) but a different architectural form. See § "Why D-HNN is not enough for SiS" below for why CTPC needs the second strategy, not this one.

## The Math

The architecture in three lines:

1. **Two scalar networks**: `H_θ : ℝ^{2N} → ℝ` and `D_θ : ℝ^{2N} → ℝ`. Both take `(q, p)` as input.
2. **Dynamics**: autograd both, combine via Helmholtz form:
   ```
   (dq/dt, dp/dt) = (∂H/∂p,  −∂H/∂q)  +  (∂D/∂q,  ∂D/∂p)
                     └─ symplectic ──┘    └── ordinary ──┘
                     conservative          dissipative
   ```
3. **Loss** ([[D-HNN]] Eq. 5):
   ```
   L_DHNN = ‖(∂H/∂p + ∂D/∂q) − dq/dt‖² + ‖(−∂H/∂q + ∂D/∂p) − dp/dt‖²
   ```

**Note on `D`'s arguments.** Standard [[Rayleigh-Dissipation-Function]] is `D(q̇)`, velocity-only. [[D-HNN]] generalizes to `D(q, p)` — a scalar field over phase space — and takes ordinary gradients in both `q` *and* `p`. This is what makes the Helmholtz interpretation work but is *not* the textbook Rayleigh setup.

**Helmholtz uniqueness.** Given a vector field `V` and appropriate boundary/decay conditions, the Helmholtz decomposition `V = ∇D + S∇H` is unique up to a harmonic gauge. The architecture *cannot* produce a vector field outside this form because it has only two scalar outputs combined this specific way. This is the *structural completeness* claim: any smooth dynamics is representable.

**Counterfactual generalization.** Once trained, multiplying the learned `D_θ` by a constant `α` simulates dynamics under a scaled friction coefficient — without retraining. The conservative `H_θ` is unaffected. See § "Counterfactual generalization mechanism" below.

### Code Correspondence

```python
import torch, torch.nn as nn

class DHNN(nn.Module):
    """Two scalar subnetworks: H_θ(q,p) for conservative, D_θ(q,p) for dissipative.
    Dynamics = symplectic_grad(H) + grad(D).
    """
    def __init__(self, dim, hidden=256, depth=2):
        super().__init__()
        self.H = self._scalar_net(2*dim, hidden, depth)
        self.D = self._scalar_net(2*dim, hidden, depth)

    @staticmethod
    def _scalar_net(in_d, hidden, depth):
        layers = []
        for _ in range(depth):
            layers += [nn.Linear(in_d, hidden), nn.Tanh()]
            in_d = hidden
        layers += [nn.Linear(in_d, 1)]
        return nn.Sequential(*layers)

    def time_derivative(self, x, dissipation_scale=1.0):
        x = x.requires_grad_(True)
        H = self.H(x).sum()
        D = self.D(x).sum()
        dH = torch.autograd.grad(H, x, create_graph=True)[0]
        dD = torch.autograd.grad(D, x, create_graph=True)[0]
        dH_q, dH_p = dH.chunk(2, dim=-1)
        dD_q, dD_p = dD.chunk(2, dim=-1)
        # Helmholtz form: V = symplectic_grad(H) + dissipation_scale * grad(D)
        dq = dH_p + dissipation_scale * dD_q
        dp = -dH_q + dissipation_scale * dD_p
        return torch.cat([dq, dp], dim=-1)
```

The `dissipation_scale=α` knob at inference time is the counterfactual generalization mechanism (multiply learned `D_θ` by `α` to predict under a different friction coefficient).

## Toy Example

**Damped mass-spring** (Task 1 of [[D-HNN]]). True dynamics: `H = ½kq² + p²/(2m)`, dissipation `D = ½ρq̇²` (standard Rayleigh, but reformulated as `D(p) ∝ p²` in the paper since `q̇ = p/m`).

After training on one friction coefficient (`ρ = 2`), the model decomposes the field cleanly:

| Model | Test MSE (×10⁻⁵) | Energy MSE (×10⁻⁵) | Decomposition? |
|---|---|---|---|
| MLP | 9.9 | 0.6 | No |
| HNN | 63,000 | 29,000 | No (HNN over-conserves) |
| **D-HNN** | **5.8** | **0.36** | **Yes — clean H/D split** |
| Numerical (Gauss-Seidel) | — | — | Yes but with grid-interpolation error ≫ NN error |

D-HNN beats HNN by ~4 orders of magnitude on this task — HNN is structurally unable to represent the energy decay; baseline MLP fits but can't decompose; D-HNN does both.

**Counterfactual generalization to unseen friction**. Multiply learned `D_θ` by `α = 0.5` or `α = 2.0` → predict dynamics under different friction coefficients. The model was trained on one `ρ`; it generalizes to others without retraining. ([[D-HNN]] Fig. 3.)

## Counterfactual Generalization Mechanism

This is the architectural feature most relevant for SiS interpretability:

**Mechanism.** The trained model has two scalar fields `(H_θ, D_θ)`. At inference, multiplying the dissipative gradient by a scalar `α`:

```
(dq/dt, dp/dt) = symplectic_grad(H_θ) + α · grad(D_θ)
```

simulates dynamics under a counterfactual friction coefficient `α · ρ_train`. The model only ever saw one `ρ` during training but recovers the full one-parameter family.

**Why this works.** The Helmholtz decomposition is *complete* (covers all smooth vector fields) and *separates* the two components. Once `D_θ` is fixed (it represents the *shape* of the dissipative gradient field), scaling it linearly scales the dissipative force — exactly what a different friction coefficient would do. The conservative `H_θ` is untouched, preserving the conservative dynamics correctly under the counterfactual.

**Connection to Barbiero concept-closure compliance.** This is a clean architectural mechanism for an *intervention class*: "what if friction were halved/doubled?" The architecture supports the intervention by construction — no auxiliary mechanism required. In the [[Actionable-Interpretability-Symmetries]] framework, sound translation between concept vocabularies (`τ_C : T → T'`) is what concept-closure invariance demands; the `α`-scaling of `D_θ` is a concrete instantiation in the dissipation-coefficient concept space. This is the kind of *structural support for counterfactual reasoning* that [[CBM-CTPC-Composition]] is also reaching for, but here it falls out of the Helmholtz decomposition for free, without needing an explicit concept layer.

For [[CTPC-Design-Rationale]] this is the design-level insight worth elevating: **architectural decomposition of dynamics into scalar fields gives a free hook for counterfactual interventions on the parameters of those scalar fields.** The same logic should apply to drag-coefficient counterfactuals in CTPC.

## Why D-HNN Is Not Enough for SiS

This section exists because the architectural choice between D-HNN (Helmholtz route) and PHNN-proper (J-R structure route) is *not obvious from the paper alone* — D-HNN looks like the simpler approach, but it fails the SiS hard-constraint requirement. An AFRL reviewer asking "why didn't you use the simpler architecture?" needs a precise answer.

### The SiS hard-constraint requirement

CTPC requires `dH/dt ≤ 0` along learned trajectories — the dissipation rate of the system's Hamiltonian must be *non-positive* (passivity). This is the formal expression of the physical fact that orbital dissipation (drag, atmospheric heating) can only *remove* energy from the orbit, never add it. From `CLAUDE.md`:

> "CTPC framework: ... Key constraint: dissipation enforced via `Ḣ ≤ 0` as a hard architectural constraint"

A model that *could* learn to add energy to the orbit — even if it usually doesn't, given training data — fails this requirement. Hard architectural constraints mean *the model cannot violate the constraint*, not *the model has been trained to usually satisfy it*.

### Why D-HNN does not satisfy `Ḣ ≤ 0` structurally

Compute the energy rate along D-HNN dynamics. Let the dynamics be `(dq/dt, dp/dt) = (∂H/∂p + ∂D/∂q, −∂H/∂q + ∂D/∂p)`. Then:

```
dH/dt = (∂H/∂q)(dq/dt) + (∂H/∂p)(dp/dt)
      = (∂H/∂q)(∂H/∂p + ∂D/∂q) + (∂H/∂p)(−∂H/∂q + ∂D/∂p)
      = (∂H/∂q)(∂H/∂p) − (∂H/∂p)(∂H/∂q) + (∂H/∂q)(∂D/∂q) + (∂H/∂p)(∂D/∂p)
      = ∇H · ∇D                                                       (∗)
```

The conservative cross-term vanishes (as it should — that's why HNN conserves energy). The dissipative term is `∇H · ∇D` — **the inner product between the gradients of the two learned scalar fields**.

This sign is *indeterminate*. `∇H · ∇D` can be positive or negative depending on the angle between the two gradients at any phase-space point. If `∇D` happens to align with `∇H`, the model *adds* energy (`dH/dt > 0`); if anti-aligned, it removes energy. The architecture imposes no constraint that forces alignment in any particular direction.

**The Helmholtz decomposition is a function-decomposition technique, not a passivity-preserving structure.** It guarantees the vector field decomposes into two components — but says nothing about whether the dissipative component dissipates *energy*.

### Why PHNN-proper does satisfy `Ḣ ≤ 0` structurally

[[Port-Hamiltonian-Systems|Port-Hamiltonian]] dynamics have the form

```
dx/dt = [J(x) − R(x)] ∇H(x) + g(x)u                                 (PH)
```

with `J(x)` skew-symmetric and `R(x)` positive semi-definite. Energy rate:

```
dH/dt = ∇Hᵀ [J − R] ∇H + ∇Hᵀ g u
      = −∇Hᵀ R ∇H + ∇Hᵀ g u                          (J skew → ∇HᵀJ∇H = 0)
      ≤ ∇Hᵀ g u                                       (R PSD → −∇HᵀR∇H ≤ 0)
```

For the unforced system (`u = 0`), `dH/dt = −∇Hᵀ R ∇H ≤ 0` *structurally* — passivity is built into the algebraic form, not learned. As long as `R(x)` is constrained PSD by parameterization (e.g., Cholesky `R = L Lᵀ`), the architecture cannot produce non-passive dynamics.

### The architectural difference, side-by-side

| | D-HNN (Helmholtz route) | PHNN-proper (J-R route) |
|---|---|---|
| Form | `dx/dt = symplectic_grad(H) + grad(D)` | `dx/dt = (J − R) ∇H + g u` |
| Two scalars? | Yes: `H, D` | Just `H`; `J`, `R`, `g` are *operators* |
| Decomposition guarantee | ✓ Helmholtz: any field decomposes uniquely into two scalar fields | ✗ Restricted form: not every field is `(J−R)∇H` |
| Passivity guarantee `dH/dt ≤ 0` | ✗ Indeterminate sign of `∇H · ∇D` | ✓ Structural via `−∇HᵀR∇H ≤ 0` for PSD `R` |
| Counterfactual `α · D` | ✓ Multiply `D_θ` by α | Needs reparameterization of `R` |
| Control input `u` | Not in the formulation | ✓ Native via `g(x)u` |
| Use case | Helmholtz-decomposing arbitrary dynamics; interpretability of conservative vs dissipative components | Hard-constraint-required dissipative dynamics with control |

### What D-HNN is good for

This is *not* a critique of the paper — D-HNN is well-suited for its target use case (decomposing observed vector fields into rotational + irrotational components for *scientific interpretability*, e.g., the OSCAR ocean-current task in the paper). The Helmholtz framing is the right tool for that job.

But the SiS use case is *different*. CTPC needs `Ḣ ≤ 0` as a deployment-safety guarantee — a constraint that holds on *all* inputs, not just training distributions. For that, PHNN-proper's J-R structure is the architecturally correct choice.

### Practical conclusion for CTPC

**D-HNN partially closes Q8 of [[CTPC-Design-Rationale]] Part II — specifically Q8a, the Helmholtz-decomposition route to dissipative HNN.** It does *not* close Q8b, the J-R-structured route required by SiS's `Ḣ ≤ 0` constraint. **Q8b-vector-field is now closed by [[Dissipative-SymODEN]]** (Zhong, Dey, Chakraborty 2020, ingested 2026-05-10) via Cholesky-PSD parameterization of the dissipation matrix; see [[Port-Hamiltonian-Neural-Network]] for the SiS-canonical architectural treatment. **Q8b-discrete-time** (exact passivity at the integrated-trajectory level) remains open — Dissipative SymODEN uses RK4, not a structure-preserving integrator; three candidate integrators are flagged in [[Port-Hamiltonian-Neural-Network]] § "Integrator Caveat".

## Connection to SiS / CTPC

**Where D-HNN fits in the CTPC architecture.** D-HNN is the *Helmholtz-decomposition leg* of the duality between conservative and dissipative dynamics — see the third axis added to [[Hamiltonian-vs-Lagrangian-Duality]]. It is *not* the right architectural primitive for the dissipation layer of CTPC (see § "Why D-HNN is not enough for SiS"). It is, however, useful as:

- **A diagnostic tool.** Given a learned dynamics model, the Helmholtz decomposition can be applied post-hoc to inspect what's conservative vs. dissipative. Useful for interpretability even when the architecture itself is PHNN-proper.
- **A counterfactual-generalization template.** The `α`-scaling of `D_θ` for unseen friction coefficients is an architectural idea that should transfer to PHNN-proper: parameterize `R(x)` so individual dissipation channels can be intervened on (drag coefficient counterfactual, SRP counterfactual). This is the template the [[CTPC-Design-Rationale]] should adopt for the dissipation-side counterfactual reasoning.

**Hard-constraint classification.** Partial. The Helmholtz form is hard-constrained (architecture cannot produce non-Helmholtz dynamics), but `Ḣ ≤ 0` is not.

## Connections

- [[D-HNN]] — the introducing paper (Sosanya & Greydanus 2022)
- [[Hamiltonian-Neural-Network]] — the conservative-only predecessor that D-HNN extends
- [[Helmholtz-Decomposition]] — the math foundation
- [[Rayleigh-Dissipation-Function]] — the classical-mechanics primitive D-HNN generalizes
- [[Hamiltonian-Mechanics]] — foundational physics
- [[Symplectic-Gradient]] — produces the rotational/conservative component
- [[Port-Hamiltonian-Systems]] — the alternative, stronger dissipation framing CTPC needs
- [[Hamiltonian-vs-Lagrangian-Duality]] — D-HNN occupies the Helmholtz-decomposition leg of the third axis (dissipation-route axis)
- [[CTPC-Design-Rationale]] — Q8a closure via this architecture; Q8b still requires PHNN-proper
- [[CBM-CTPC-Composition]] — analogous architectural support for counterfactual reasoning on the concept-vocabulary side; D-HNN's `α`-scaling is the dissipation-side analog

## Open Questions

- **PSD-constrained `D_θ` for partial passivity.** If `D_θ` is constrained so that `∇H · ∇D ≤ 0` everywhere, does the architecture recover the passivity guarantee? In general no (the constraint depends on `H_θ` too, which isn't fixed). But in special cases (separable `H = T(p) + V(q)`, separable `D = D_T(p) + D_V(q)`), the constraint may decouple. Worth working out.
- **Higher-DoF generalization.** Paper tests on 1-DoF (mass-spring, pendulum) and 2-D fields (ocean currents). Whether the architecture scales cleanly to `d > 10` (orbital state) is unclear — Helmholtz decomposition still works, but the optimization landscape may degrade.
- **Compatibility with symplectic integrators.** Pure HNN pairs naturally with leapfrog/Yoshida (preserve `H` exactly to a specified order). For D-HNN, symplectic integrators don't preserve the discrete dissipation structure correctly — the dissipative term breaks symplecticity. Whether discrete-time variational dissipation integrators (Marsden-West with Rayleigh) close this gap is an open implementation question.
- **The `D(q, p)` vs `D(q̇)` semantics.** Standard Rayleigh has `D(q̇)` and the gradient w.r.t. velocity gives the dissipative force. D-HNN has `D(q, p)` and ordinary gradients in both — equivalent to standard Rayleigh after Legendre transform, or strictly more general? Worth resolving for theoretical clarity.

## Sources

- `raw/papers/port-hamiltonian/DissipativeHNN.pdf` — Sosanya & Greydanus 2022, arXiv:2201.10085. Section 3 (theory: Hamiltonian mechanics review + Helmholtz decomposition); Section 4 (methods: architecture + Eq. 5 loss); Section 5 (results: damped spring, real pendulum, OSCAR ocean currents); Fig. 1 (architecture diagram); Fig. 3 (counterfactual generalization to unseen friction coefficients via `α · D` scaling).
