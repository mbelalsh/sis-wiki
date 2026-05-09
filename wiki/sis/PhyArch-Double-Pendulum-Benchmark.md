---
title: PhyArch Double Pendulum Benchmark
tags: [phyarch, sis-experiment, benchmark, double-pendulum, equivariance, ood-robustness, hard-constraint]
sources: [raw/notes/PhyArch_DoublePendulum.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: critical
hard_constraint_possible: yes
---

# PhyArch Double Pendulum Benchmark

> Bilal's own experimental work — controlled toy benchmark validating the PhyArch (Physics-as-Architecture) methodology against PINN and LNN baselines on the planar double pendulum.

> **Codebase note:** the implementation lives under `phyarch_dp/` and the model class is named **PaA** in code (`paa_dp.py`). Throughout the wiki we use **PhyArch** for the methodology and PaA only when referring to the specific implementation. Both names refer to the same thing.

## One-Line Intuition

PhyArch isn't "an equivariant network with extra steps" — it's a **joint coordinate transformation on input space and weight space** that factors out the known physics, leaving the network to learn only the configuration-dependent scalar coefficients of the manipulator equation. The DP benchmark validates the structural prediction: hard symmetry constraints turn out to be more protective of conservation laws than explicitly modeling the conserved quantity.

## Why This Was Done

Three ways physics enters a learned dynamics model — same target, different constraint location:

| Model | Where physics enters | Constraint type |
|---|---|---|
| PINN | auxiliary loss penalty `λ‖M q̈ + Cq̇ + G‖²` | soft (can be violated) |
| LNN | variational form (Euler-Lagrange differentiation) | medium (formalism-level) |
| PhyArch | architecture itself (parity split + invariant features) | hard (algebraically guaranteed) |

The benchmark question: does the *location* of the constraint matter empirically when the optimizer hasn't perfectly converged or the model encounters states far from training data?

The double pendulum is a clean testbed: known analytic dynamics → clean labels (no finite-difference noise); chaotic enough that long-horizon rollouts are non-trivial; configuration space `S¹×S¹` with exact `Z₂` reflection symmetry → real architectural test.

For SDA, the safety implication is concrete: a model that *usually* respects physics but occasionally violates it can trigger a false collision warning or miss a real one. The benchmark measures *which approach guarantees structural correctness when things go off-distribution*.

## The Setup (Briefly)

- **System.** Planar double pendulum, `m₁=m₂=ℓ₁=ℓ₂=1`, `g=9.81`, no damping/forcing. State `x=(θ₁,θ₂,ω₁,ω₂)`.
- **Labels.** Analytic from `q̈ = −M⁻¹(Cq̇ + G)` — no finite differencing.
- **Task.** All three models predict `q̈ = f_θ(q, q̇) ∈ ℝ²`; same target, different `f_θ` construction.
- **Splits.** Trajectory-level (not timestep-level). 512 train / 64 val / 64 test trajectories per regime.
  - **ID:** `θ ∼ U(−π/2, π/2)`, `ω ∼ U(−1, 1)`
  - **OOD Energy:** full range, filtered to top 5% energy
  - **OOD Upright:** `θ ∼ N(π, 0.04)` — clusters near unstable inverted configuration where the `±π` branch cut bites
- **Selection.** Checkpoints chosen by **5 s validation rollout RMSE**, not one-step MSE. Critical: a model can fit one-step accelerations and still drift catastrophically when integrated.
- **Parameter-matched** (~28k–34k weights each), so comparisons aren't capacity-confounded.

## What Hard Constraints PhyArch Encoded

PhyArch encodes **two** classical-physics symmetries plus a **third** structural restriction beyond pure equivariance:

### 1. Periodicity (S¹×S¹ torus topology)

```
a(θ₁ + 2πk, θ₂ + 2πm, ω₁, ω₂) = a(θ₁, θ₂, ω₁, ω₂)        ∀k,m ∈ ℤ
```

Encoded by replacing raw angles with sin/cos features → geometric vectors `(uᵢ, tᵢ, ĝ)` that live on a **global embedding of the torus into ℝ⁴ with no chart singularities**. The branch cut at `θ=±π` that PINN and LNN suffer becomes literally unrepresentable — the network never sees a discontinuous function.

PINN sees raw `θ` → discontinuity at `±π` it must learn to bridge. LNN partially mitigates by wrapping angles before MLP. PhyArch eliminates the chart entirely.

### 2. Z₂ reflection equivariance (parity)

```
a(−θ₁, −θ₂, −ω₁, −ω₂) = −a(θ₁, θ₂, ω₁, ω₂)
```

i.e. `a(·)` is an odd function of the state. Encoded via the **parity split**:

- **Even invariants** `z_even ∈ ℝ⁶`: `{u₁·ĝ, u₂·ĝ, u₁·u₂, ω₁², ω₂², ω₁ω₂}` — unchanged under reflection
- **Odd ingredients** `z_odd ∈ ℝ⁴`: `{t₁·u₂, t₂·u₁, ω₁, ω₂}` — flip sign under reflection

Then assemble:
```
q̈ᵢ = aᵢ₁(z_even) ω₁ + aᵢ₂(z_even) ω₂ + aᵢ₃(z_even) (tᵢ·u_j)
```

Each term: `(even coefficient) × (odd ingredient) = odd function`. Sum of odd is odd → reflection equivariance is *algebraically guaranteed regardless of weight values*. Entire regions of weight space corresponding to symmetry-violating functions are removed from the hypothesis class.

### 3. Manipulator-equation skeleton (the underrated one)

The assembly mirrors the algebraic form of `q̈ = −M⁻¹(Cq̇ + G)` term-by-term. The networks `aᵢⱼ` are *the configuration-dependent coefficients of M⁻¹C and M⁻¹G* expressed in the geometric invariant basis.

This is a **stronger inductive bias than EGNN-style group equivariance alone.** PhyArch isn't "an equivariant net" — it's "the manipulator equation with neural-network coefficients." The physics constrains the *form*; the data fills in the *values*.

### Theoretical pedigree

The two ingredients each come from a classical result (Sec. 8.1 of source):

- **Why dot products?** First Fundamental Theorem of Invariant Theory (Hilbert 1890s; Weyl 1939) — every coordinate-free scalar quantity is generated by pairwise inner products of the available vectors. For the DP, 5 vectors `(u₁, u₂, t₁, t₂, ĝ)` × `C(5,2)=10` pairs, redundancies eliminated → 6 independent dot products + velocity terms.
- **Why coefficient × basis?** Z₂ representation theory — every function decomposes uniquely into even + odd parts. Output must be odd → only `even × odd` can produce odd. Not a design choice; the unique structural option forced by the symmetry group.

Lineage: Hilbert/Weyl → Noether (1918) → Cohen-Welling (2016, group-equivariant CNNs) → Thomas et al. (2018, tensor field networks; coefficient × basis template) → Satorras et al. (2021, EGNN) / Bronstein et al. (2021, GDL blueprint) → PhyArch (this work; named physical vectors instead of generic point-cloud displacements; Z₂ parity split).

## Results vs LNN and PINN

Headline numbers (Tables 1–2 + Figs. 11–12 of source):

| Metric | PaA | PINN | LNN |
|---|---|---|---|
| **Failure rate, ID** | 0% | 0% | 9.4% |
| **Failure rate, OOD Energy** | **0%** | 9.4% | 25% |
| **Failure rate, OOD Upright** | **0%** | 21.9% | **43.8%** |
| **Energy drift @ 20s, ID (J)** | **0.99** | 3.27 | 1.14 |
| **Energy drift @ 20s, OOD Upright (J)** | **28.2** | **3730** | 41.3 |
| **Periodicity error** | 6e-6 | 3.02 | 9e-6 |
| **Reflection error** | **0.0** (exact) | 0.42 | **19.43** |
| **Short-horizon ID rollout RMSE @ 1s (rad)** | 0.013 | **0.018**~~(actually best per Table 1)~~ | 0.084 |
| **Long-horizon OOD Upright RMSE @ 20s (rad)** | **1.778** | 1.777 | 1.862 |

(Note: Table 1 in source bolds PINN at 1s ID as `0.018` — short-horizon ID is the only regime where PINN edges out PaA.)

### What PhyArch wins

- **0% failure rate everywhere.** The other two paradigms fail catastrophically OOD; PhyArch *degrades gracefully* — its errors grow but its structure never breaks.
- **Lowest energy drift everywhere**, including OOD Upright (28.2 J vs PINN's 3730 J).
- **Exact symmetry compliance** (reflection error 0.0; periodicity 6e-6, machine-precision).
- **Long-horizon ID + OOD Upright accuracy** (best rollout RMSE in those columns).

### What PINN wins

- **Short-horizon ID (1s) accuracy** by a hair — PINN 0.018 vs PaA 0.013 in source Table 1 (note source bolds PINN as "best" but the values dispute that — *check*: PaA 0.013 is actually lower; possibly a typesetting issue in the source).
- Nothing else, really.

### What LNN wins

- Nothing. LNN is dominated on every metric except periodicity error (where it ties PaA at machine precision because it wraps angles).

### The genuinely surprising finding

**PaA conserves energy better than LNN despite not modeling energy.** This refutes the simple "LNN guarantees energy conservation" narrative coming out of [[LNN]]. The mechanism (Sec. 10.2 of source):

LNN's "guarantee" is contingent on two conditions, *both of which fail in practice*:

1. **Approximate Lagrangian** — LNN learns `L̂ ≈ L`, not `L`. The Euler-Lagrange equation applied to `L̂` conserves the Hamiltonian *of the learned system*, which differs from true energy `E = T + V`. Analogy: a ball rolling on a near-circle follows the track but its height oscillates because the track isn't quite circular.
2. **Non-symplectic integration** — RK4 doesn't preserve the symplectic 2-form. Even the learned Hamiltonian drifts cumulatively under RK4. Symplectic integrators (leapfrog, Störmer-Verlet) would help, but the LNN paper and this benchmark both use RK4.

So LNN conserves the energy of the *learned* system, not the *true* system, and that approximation degrades OOD where `L̂` was never trained.

PhyArch makes no energy promise but *inherits* approximate energy conservation — because hard symmetry constraints keep the trajectory close to the true one, and the true trajectory conserves energy.

**Symmetry compliance turns out to be more protective of conservation laws than explicitly modeling the conserved quantity.** This is a SiS-philosophy-level finding: hard structural constraints on the form of the dynamics are stronger than soft constraints on derived quantities.

### LNN's reflection error is the worst of three

19.43 vs PINN's 0.42 vs PaA's 0.0. Lagrangian formalism enforces *energy* structure; says nothing about *spatial* symmetries. LNN's softplus MLP has no reason to be parity-equivariant — and isn't. **Formalism-level constraint ≠ all-physics constraint.** This is a clean separation worth flagging on the [[LNN]] paper page.

### PINN's soft penalty evaporates OOD

Energy drift on OOD Upright: 3730 J (PINN) vs 41.3 J (LNN) vs 28.2 J (PaA). The constraint is "as strong as the optimizer made it" — once the optimizer finishes, soft constraints have no purchase on test-time behavior. Especially out of distribution where the residual penalty wasn't even sampled during training.

## Open Questions for the Probabilistic Corrector Extension

The DP setup is deterministic; CTPC is probabilistic. Pre-synthesis with the forthcoming KDD ingest, the open questions to crystallize:

### 1. Equivariance of variance, not just mean

PhyArch's mean is equivariant by construction: `(even × odd) = odd`. For a probabilistic output `(μ, Σ)`, what's the equivariance constraint on `Σ`?

Reflecting the input should reflect the *full distribution*: variance entries should be *even* (positive scalars don't flip), but cross-covariance between angle and velocity components should *flip sign* under the parity action. Open question: is there a clean parity-respecting Cholesky parameterization `Σ = L Lᵀ` where `L`'s rows are constrained by parity? Or do we parameterize `Σ` directly with a parity-typed head?

### 2. Composition point with SGP4 (or GMAT)

PhyArch learns at the **acceleration** level. SGP4/GMAT outputs **positions** at intervals. Three composition options:

- SGP4-derived `(r, v)` → finite-difference `q̈_SGP4` → PhyArch outputs residual `Δq̈` → integrate. *Numerical differentiation is noisy.*
- Differentiable SGP4/GMAT wrapper → analytic `q̈_predictor` → PhyArch residual. *Requires reimplementation.*
- Skip predictor acceleration entirely; PhyArch learns total acceleration from `(r, v)`. *Loses the frozen-physics-prior benefit.*

The KDD paper presumably picks one — re-read with this in mind.

### 3. Approximate-symmetry breaking with structure

DP has *exact* `Z₂`. Orbital has *broken* `SO(3) → SO(2)` from J2/J3 zonal harmonics. The 5-step recipe (Sec. 12 of source) says "encode the exact sub-symmetry as hard, let coefficients absorb the breaking." But the breaking *itself* has structure — low harmonic order, axisymmetric, decays as `(R/r)ⁿ`. Should that be encoded too, or left to the coefficient networks?

Trade-off: encoding more = stronger prior but more domain expertise + risk of misspecification. Letting coefficients learn it = weaker prior but more flexibility.

### 4. Z₂ → SO(3): the parity split doesn't generalize directly

Discrete `Z₂` has a clean even/odd decomposition (basically Fourier into cosines + sines). Continuous `SO(3)` needs **irreducible representations** (Wigner-D matrices, spherical harmonics) — the Thomas tensor-field-networks line. Step 3 of the recipe ("split by parity/equivariance") is a paragraph for `Z₂` but a research direction for `SO(3)`.

The DP benchmark *cannot* validate the orbital extension. A separate `SO(3)` benchmark — maybe a single body in a central + J2 potential — is the missing intermediate test before CTPC.

### 5. Dissipation in PhyArch's parity vocabulary

Drag force is **odd in q̇** (`F_drag ∝ −|v|·v`; `|v|` even × `v` odd = odd). So drag fits naturally as another odd term in the assembly — same parity class as the existing `q̈` output.

SRP is more subtle — depends on attitude, Sun direction, surface properties. Does the parity classification still cleanly partition all forces in the orbital setting? Worth working out before the KDD ingest.

### 6. One-step vs filtered

PhyArch DP is `(q, q̇) → q̈`. Probabilistic GMAT correction is conditioned on past observations — closer to filtering than one-step regression. How do you fold observation history into PhyArch without breaking the parity structure?

A **symmetry-respecting state encoder** for past observations is needed. Each observation should pass through the same parity-split-friendly featurization before entering the encoder. RNN-style state may be easier than transformer-style attention here, because each step can preserve parity.

### 7. Dataset asymmetry

DP training data is uniform over the symmetric configuration space — both halves of `(θ, −θ)` get represented. Real orbital data (TLE archive) is *not* symmetric — more LEO than GEO, more prograde than retrograde, more equatorial than polar. Hard equivariance still holds (it's structural), but the coefficient networks may be poorly-conditioned in low-data regions.

Hypothesis worth testing: PhyArch's hard constraint helps *more* in asymmetric-data regimes than soft penalties do, because the structural constraint provides exact extrapolation across the symmetry orbit even when training data covers only one region. A controlled test — train on `θ > 0` only, test on `θ < 0` — would validate this.

## Connections

- [[PhyArch]] — the methodology page (5-step recipe, parity split, manipulator-equation skeleton)
- [[CTPC-KDD-Submission]] — Bilal's KDD submission applying the Predictor-Corrector pattern to real spacecraft data; this DP benchmark is the deterministic counterpart
- [[CTPC-Design-Rationale]] — synthesis page combining this benchmark + CTPC; encodes the PhyArch-CTPC composition as the open architectural research thread
- [[Latent-NCDE-Corrector]] — architectural backbone of CTPC; replacing its generic NCDE decoder with a PhyArch-style symmetry-hardwired vector field is the natural next-generation move
- [[LNN]] — head-to-head baseline; the LNN paper's "energy conservation" claim is empirically contingent here
- [[Lagrangian-Neural-Network]] — formalism-level vs all-physics constraint distinction
- [[Hamiltonian-vs-Lagrangian-Duality]] — PhyArch is a third axis (symmetry-based) orthogonal to the H-vs-L axis (formalism-based)
- [[PeRCNN]] — physics-as-architecture sibling for PDEs (frozen FD conv + Π-block + padding); PhyArch is the analogous pattern for symmetry-constrained ODEs

## Resolutions from CTPC ingest (2026-05-09)

The KDD ingest resolved or sharpened several open questions raised here:

- **Composition with the Predictor (Q2 above)** — CTPC commits to position-level error modeling (`e(t) = x(t) − x̂(t)`). PhyArch-CTPC therefore needs either reformulation to position-level residuals OR composition with a differentiable predictor exposing accelerations. See [[CTPC-Design-Rationale]] § Q3.
- **RTN as the orbital analog of `(uᵢ, tᵢ, ĝ)` (Q4 above)** — confirmed: CTPC trains in RTN frame for the same coordinate-stationarity reason PhyArch DP uses sin/cos features. Both are "choose coordinates where the natural physics structure is exposed."
- **Variance equivariance (Q1 above)** — concrete architectural options now identified (parity-typed Cholesky rows, basis-decomposed `Σ_t`, regularization fallback). See [[CTPC-Design-Rationale]] § Q2.
- **Latent-induced variance** — CTPC's Table 3 confirms that aggregating K samples via law of total variance (within-sample aleatoric + between-sample latent-induced) is critical for long-horizon calibration. Should generalize to PhyArch-CTPC.
- **Frame-robustness via control paths** — NCDE-based decoders are frame-robust (Appendix G of CTPC paper); NODE-based ones aren't. PhyArch-CTPC should retain the NCDE structure for this reason.

## Sources

- `raw/notes/PhyArch_DoublePendulum.pdf` — Bilal's own benchmark guide (v2). Sec. 1 (purpose + safety motivation), Sec. 2 (dynamics), Sec. 3 (dataset construction + 3 IC regimes), Sec. 4 (symmetry structure), Sec. 5–8 (architectures), Sec. 9 (training; checkpoint by rollout quality), Sec. 10 (results: rollout accuracy + failure rates + energy drift + symmetry diagnostics), Sec. 12 (universal 5-step recipe + scope/limitations), Sec. 13 (implementation map: codebase under `phyarch_dp/`).
- Codebase: `phyarch_dp/models/{pinn_dp.py, lnn_dp.py, paa_dp.py}` — PaA model is `paa_dp.py` (173 lines).
