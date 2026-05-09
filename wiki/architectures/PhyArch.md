---
title: PhyArch (Physics-as-Architecture via Symmetry)
tags: [phyarch, physics-as-architecture, equivariance, parity-split, hard-constraint, manipulator-equation, sis-methodology, architecture-pattern]
sources: [raw/notes/PhyArch_DoublePendulum.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: critical
hard_constraint_possible: yes
---

# PhyArch (Physics-as-Architecture via Symmetry)

> **Codebase note:** the implementation under `phyarch_dp/` names the model class **PaA** (in `paa_dp.py`). Throughout the wiki we use **PhyArch** for the methodology and PaA only when referring to the specific implementation. Both names refer to the same thing.

## One-Line Intuition

A neural-network architecture pattern that performs a **joint coordinate transformation on input space and weight space**: replace coordinate-chart inputs (angles, positions) with coordinate-free invariant features, then restrict the representable functions to the algebraic skeleton of the system's equation of motion. The network learns only the configuration-dependent scalar coefficients — the geometric symmetries are baked into the architecture and cannot be violated by any choice of weights.

## Why This Was Invented

Three locations where physics can enter a learned dynamics model:

| Approach | Where physics enters | Constraint type | Failure mode |
|---|---|---|---|
| PINN | auxiliary loss penalty | soft (can be violated) | optimizer ignores penalty OOD |
| [[Lagrangian-Neural-Network]] | variational form (Euler-Lagrange) | medium (formalism-level) | enforces energy structure but not spatial symmetry |
| **PhyArch** | architecture itself | **hard (algebraic)** | structurally cannot violate constraints |

PhyArch was designed to push the constraint as far up the stack as possible — from "hope the optimizer learns it" (PINN) past "use a formalism that implies it" (LNN) to "make it impossible to violate by any choice of weights." This matters most in safety-critical applications where the difference between *the model gets worse* and *the model breaks* is operationally significant.

The pattern was validated on the [[PhyArch-Double-Pendulum-Benchmark]] against PINN and LNN baselines; the broader PhyArch project targets [[CTPC]]-style spacecraft dynamics where SO(3) equivariance with J2/J3-broken symmetry is the operational case.

## The Math

PhyArch is a procedural recipe, not a single architecture. Five steps (Sec. 12 of source):

### Step 1 — Identify the symmetry group

This is the only step requiring human domain expertise; the rest is mechanical once Step 1 is done.

| System | Symmetry group |
|---|---|
| Double pendulum | `S¹ × S¹ ⋊ Z₂` (torus + reflection) |
| Two-body orbit | `SO(3)` (full rotation) |
| Earth-perturbed orbit | `SO(2)` (axial — broken from `SO(3)` by J2 zonal harmonic) |
| Rigid body attitude | `SO(3) × SO(3)` (frame and body-fixed) |

For approximate symmetries (most real cases): encode the exact sub-symmetry as hard architectural constraint, let the coefficient networks (Step 4) absorb the symmetry-breaking residual from data.

### Step 2 — Build geometric features (embed the manifold)

Embed the configuration manifold into Euclidean space without chart singularities. Replace coordinate parameterizations with invariant geometric vectors.

| System | Geometric features |
|---|---|
| DP | `uᵢ = (sin θᵢ, −cos θᵢ)`, `tᵢ = (cos θᵢ, sin θᵢ)`, `ĝ = (0, −1)` |
| Orbital | RTN basis `(r̂, t̂, n̂)`, `‖r‖`, `r·v`, `‖h‖` |

The branch cut at `θ=±π` for the DP isn't a property of the physical configuration space — it's an artifact of the half-open interval `(−π, π]` chart. The sin/cos embedding moves from a chart with singularities to a global embedding without them.

### Step 3 — Split features by parity / equivariance type

Classify into **invariants** (input to coefficient networks; unchanged by the symmetry action) and **basis terms** (carriers of directional information; transform predictably).

For Z₂ (DP): even (cosines, products of velocities) vs. odd (sines, bare velocities).
For SO(3) (orbital): scalar invariants vs. components in the `(r̂, t̂, n̂)` frame.

This step is a paragraph for discrete groups (Z₂) but a research problem for continuous groups (SO(3) needs irreducible representations / Wigner-D matrices / spherical harmonics — the Thomas tensor-field-networks / EGNN line).

### Step 4 — Small networks learn invariant scalar coefficients

Each coefficient network `aᵢⱼ : ℝ^|invariants| → ℝ` takes only invariants as input. Output is automatically invariant *regardless of weight values* — this is the weight-space restriction.

For DP: 6 small networks `aᵢⱼ(z_even)` for `i ∈ {1,2}`, `j ∈ {1,2,3}`.
For orbital: scalar coefficients `s_R(I), s_T(I), s_N(I)` for the RTN components.

### Step 5 — Assemble: coefficient × basis = equivariant output

Final output:
```
output = Σⱼ (invariant coefficient aⱼ) × (basis term zⱼ)
```

The product `invariant × equivariant = equivariant` holds by representation theory. Sum of equivariant terms is equivariant. The output transformation behavior matches the physics by construction.

For DP:
```
q̈ᵢ = aᵢ₁(z_even) · ω₁  +  aᵢ₂(z_even) · ω₂  +  aᵢ₃(z_even) · (tᵢ·u_j)
       └─── even × odd = odd (reflection-equivariant) ───┘
```

### What's beyond pure equivariance: the manipulator-equation skeleton

PhyArch's assembly formula isn't an arbitrary equivariant template — it mirrors the algebraic form of the *true* analytic acceleration. For DP:

```
q̈ = −M(q)⁻¹ (C(q,q̇)q̇ + G(q))
```

Expanding component-wise and re-expressing in the geometric invariant basis (using `sin(θ₁−θ₂) = t₁·u₂`, `sin θᵢ = −uᵢ·ĝ`, etc.) yields an expression of the form `Σⱼ fⱼ(invariants) × (odd basis term)` — exactly PhyArch's assembly, with `fⱼ` being analytic functions of physical constants `(mᵢ, ℓᵢ, g)`.

PhyArch replaces those analytic `fⱼ` with learnable networks `aᵢⱼ(z_even)`. So:

> **The physics constrains the *form*; the data fills in the *values*.**

This makes PhyArch a strictly stronger inductive bias than EGNN-style group equivariance alone. The hypothesis class is "the manipulator equation with neural-network coefficients," not "any equivariant function."

### Theoretical foundations

Two classical results justify the construction:

- **Why dot products?** First Fundamental Theorem of Invariant Theory (Hilbert 1890s; Weyl 1939). Every coordinate-free scalar quantity is generated by pairwise inner products of the available vectors. PhyArch applies this to the named physical vectors of the system.
- **Why coefficient × basis?** Representation theory of the symmetry group. For `Z₂`: every function decomposes uniquely into even + odd parts; if output must be odd, only `even × odd` produces odd. Algebraically forced, not a design choice.

Lineage: Hilbert/Weyl invariant theory → Noether (1918, symmetry ↔ conservation) → Cohen-Welling (2016, group-equivariant CNNs) → Thomas et al. (2018, tensor field networks; coefficient × basis template for SO(3)) → Satorras et al. (2021, EGNN) / Bronstein et al. (2021, GDL blueprint) → PhyArch (named physical vectors instead of generic point-cloud displacements).

### Code Correspondence

```python
# Sketch of the PhyArch pattern for a generic mechanical system
class PhyArch(nn.Module):
    def __init__(self, n_invariants, n_basis_terms, hidden=64):
        super().__init__()
        # One small network per (output_dim × basis_term) coefficient
        self.coeffs = nn.ModuleList([
            mlp(n_invariants, hidden, 1)
            for _ in range(output_dim * n_basis_terms)
        ])

    def forward(self, q, q_dot):
        z_inv, z_basis = geometric_features(q, q_dot)   # Steps 2-3
        out = torch.zeros(output_dim)
        for i in range(output_dim):
            for j in range(n_basis_terms):
                out[i] += self.coeffs[i*n_basis_terms + j](z_inv) * z_basis[j]
        return out                                       # equivariant by construction

# DP-specific instance: see paa_dp.py in the codebase (173 lines)
```

## Toy Example

The full DP instantiation lives in [[PhyArch-Double-Pendulum-Benchmark]]. Headline:

- **Inputs:** `(θ₁, θ₂, ω₁, ω₂)`. Symmetry: `S¹×S¹ ⋊ Z₂`.
- **Geometric features:** `uᵢ, tᵢ, ĝ` → 6 even invariants `z_even`, 4 odd ingredients `z_odd`.
- **Networks:** 6 small MLPs `aᵢⱼ(z_even)` (Tanh, hidden=64, depth=2; ~28k total params).
- **Assembly:** `q̈ᵢ = aᵢ₁ω₁ + aᵢ₂ω₂ + aᵢ₃(tᵢ·u_j)`.
- **Result:** 0% failure rate on all OOD splits (vs PINN 21.9%, LNN 43.8% on OOD Upright); reflection error exactly 0; lowest energy drift everywhere despite not modeling energy.

## Connection to SiS / CTPC

PhyArch is the **operational definition of physics-as-architecture for ODE systems** in the SiS philosophy. It maps directly onto the SiS design hierarchy from `CLAUDE.md`:

| SiS layer | PhyArch realization |
|---|---|
| Hard constraint → architecture | Geometric features (Step 2) + parity-split assembly (Steps 3–5) — symmetries algebraically enforced |
| Soft constraint → loss | None — the architecture removes the need |
| Learned residual → ML corrector | Coefficient networks `aᵢⱼ(invariants)` learn the configuration-dependent scalars |

Compared to the existing physics-as-architecture options:

- [[PeRCNN]] — physics-as-architecture for **PDEs** via frozen FD conv + Π-block + BC padding. PhyArch is the analogous pattern for **symmetry-constrained ODEs**.
- [[Hamiltonian-Neural-Network]] — physics-as-architecture for **canonical-coordinate conservative ODEs** via scalar `H_θ` + symplectic gradient. Hardwires *energy structure*; says nothing about spatial symmetries.
- [[Lagrangian-Neural-Network]] — physics-as-architecture for **arbitrary-coordinate conservative ODEs** via scalar `L_θ` + Euler-Lagrange. Same — formalism-level constraint, not spatial.
- **PhyArch** — physics-as-architecture for **symmetry-constrained ODEs** (any ODE with an identifiable symmetry group + manipulator-equation form). Hardwires *spatial symmetries* + *equation form*; says nothing about energy.

These are not redundant. The empirical finding from the DP benchmark — PhyArch conserves energy *better than LNN* despite not modeling energy — suggests **spatial-symmetry hardwiring is downstream-protective for conservation laws** that other paradigms model explicitly. They occupy different axes of the physics-as-architecture design space; see [[Hamiltonian-vs-Lagrangian-Duality]] for the H-vs-L axis and how PhyArch is orthogonal to it.

For [[CTPC]] specifically (the broader PhyArch target):

- **Step 1 for CTPC.** SO(3) for two-body Keplerian; broken to SO(2) by J2/J3 zonal harmonics; further broken (slowly, time-dependent) by lunisolar perturbations.
- **Step 2 for CTPC.** RTN basis `(r̂, t̂, n̂)` is the orbital analog of DP's `(uᵢ, tᵢ)`. Scalars: `‖r‖`, `r·v`, `‖h‖ = ‖r×v‖` (specific angular momentum).
- **Composition with SGP4/GMAT.** Open question — does PhyArch correct at the acceleration level (cleanest, but requires differentiable predictor) or position level (loses manipulator structure)?
- **Probabilistic extension.** PhyArch's mean is equivariant by `(invariant × equivariant = equivariant)`. The variance head needs analogous parity typing — open architectural question; see open questions on [[PhyArch-Double-Pendulum-Benchmark]].

## Connections

- [[PhyArch-Double-Pendulum-Benchmark]] — the validation experiment with full results
- [[CTPC-KDD-Submission]] — Bilal's deployed framework using a generic Latent NCDE Corrector; PhyArch-CTPC is the natural symmetry-hardwired upgrade
- [[CTPC-Design-Rationale]] — synthesis page; Part II § Q1–Q3 lays out the PhyArch-CTPC composition architectural details
- [[Latent-NCDE-Corrector]] — composition target: replace the generic NCDE decoder vector field `f_θ_d` with a PhyArch-style symmetry-hardwired one
- [[PeRCNN]] — physics-as-architecture sibling for spatial PDEs
- [[Hamiltonian-Neural-Network]] — sibling architecture; energy-formalism-based hard constraint
- [[Lagrangian-Neural-Network]] — sibling architecture; coordinate-free formalism-based hard constraint
- [[Hamiltonian-vs-Lagrangian-Duality]] — H-vs-L axis (formalism-based); PhyArch is the symmetry-based orthogonal axis
- [[Pi-Block-Polynomial-Approximator]] — different inductive-bias choice (polynomial form) at the architectural level

## PhyArch-CTPC: The Open Composition

Now that [[CTPC-KDD-Submission]] is ingested, the concrete next-paper architecture is **PhyArch-CTPC**: replace the [[Latent-NCDE-Corrector]]'s generic decoder vector field with a PhyArch-style symmetry-hardwired one. The 5-step recipe applied to orbital residuals (see [[CTPC-Design-Rationale]] § Q1 for full architectural details):

1. **Symmetry group** = SO(3) for two-body, broken to SO(2) for J2-perturbed
2. **Geometric features** = RTN basis `(r̂, t̂, n̂)` + scalar invariants `‖r‖, r·v, ‖h‖, e`
3. **Equivariance split** = scalar invariants vs. vector basis components
4. **Coefficient networks** = `s_R(I), s_T(I), s_N(I)` from invariants only
5. **Assembly** = `f_θ_d(z_d) = s_R · r̂ + s_T · t̂ + s_N · n̂` (or higher-order)

Plus the variance-head equivariance question: parity-typed Cholesky rows or basis-decomposed `Σ_t` (see [[CTPC-Design-Rationale]] § Q2).

**This is the deterministic-toy → real-data → next-paper pipeline:** PhyArch DP validated symmetry hardwiring on a controlled benchmark; CTPC validated probabilistic Predictor-Corrector on real CDDIS data; PhyArch-CTPC composes them.

## Open Questions

- **Step 3 for continuous groups.** Z₂ parity split is a paragraph; SO(3) needs irreducible representations and Wigner-D / spherical harmonics machinery. The Thomas et al. tensor-field-networks line is the natural template, but adapting it to "named physical vectors" (PhyArch's concrete-ness advantage) is open.
- **Approximate-symmetry encoding.** Recipe currently says "encode exact sub-symmetry, let coefficients absorb breaking." Whether the breaking *itself* (J2 zonal structure, low harmonic order) should be hardwired vs. learned is unstudied.
- **Probabilistic outputs.** Mean is equivariant by construction; variance needs analogous parity typing. Cholesky parameterization with parity-typed rows is one candidate; direct `Σ`-head with parity constraints is another.
- **Composition with frozen physics layers.** PhyArch outputs accelerations; SGP4/GMAT outputs positions. Composition point unresolved.
- **Dataset asymmetry.** Hard equivariance protects extrapolation across symmetry orbits even when training data is asymmetric. Unstudied empirically — controlled test (train on `θ > 0`, test on `θ < 0`) would validate.
- **Higher-order forces.** SRP, lunisolar, relativistic perturbations — does the parity / equivariance classification cleanly partition them in the orbital setting? Worth working out before composing with the probabilistic corrector.

## Sources

- `raw/notes/PhyArch_DoublePendulum.pdf` — Bilal's own benchmark documentation. Sec. 8 (architecture details), Sec. 8.1 (theoretical foundations + intellectual lineage), Sec. 8.4 (connection to manipulator equation), Sec. 12 (universal 5-step recipe + scope/limitations: applies to any system whose symmetry group can be identified, fails for systems with no known symmetries).
- Codebase: `phyarch_dp/models/paa_dp.py` (the PaA implementation, 173 lines).
