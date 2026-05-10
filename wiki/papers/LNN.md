---
title: LNN — Lagrangian Neural Networks (Cranmer et al. 2020)
tags: [lagrangian-mechanics, conservation-laws, physics-as-architecture, neural-ode-adjacent, energy-conservation, jax, hessian-inversion]
sources: [raw/papers/interpretability/LNN.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: high
hard_constraint_possible: yes
---

# LNN — Lagrangian Neural Networks (Cranmer et al. 2020)

> Cranmer, Greydanus, Hoyer, Battaglia, Spergel, Ho. *Lagrangian Neural Networks*. ICLR 2020 Workshop on Deep Differential Equations. arXiv:2003.04630.

## One-Line Intuition

Like [[HNN]] but parameterize the **Lagrangian** `L(q, q̇)` instead of the Hamiltonian. Derive accelerations `q̈` by inverting the velocity-Hessian of `L` from the Euler-Lagrange equation. Same conservation guarantee — but no requirement that inputs be canonical coordinates. Works directly with generalized `(q, q̇)`.

## Why This Was Invented

[[HNN]] requires inputs to be canonical `(q, p)` pairs satisfying the Poisson-bracket relations: `{q_i, q_j} = 0`, `{p_i, p_j} = 0`, `{q_i, p_j} = δ_{ij}` (Eq. 1 of paper). In practice, observed coordinates are rarely canonical:

- Relativistic particle: `p = q̇(1−q̇²)^(−3/2)`, not `m·q̇`
- Charged particle in magnetic field: `p = m q̇ + q A(r)`
- Most real datasets give `(position, velocity)`, not `(position, canonical momentum)`

Constructing canonical momenta requires knowing the Lagrangian — circular if your goal is to *learn* the dynamics from raw observables.

LNN sidesteps this. The Lagrangian formalism enforces conservation just as well as Hamiltonian, but works in arbitrary generalized coordinates `(q, q̇)`. So the authors parameterize `L_θ(q, q̇)` with an MLP, derive `q̈` by solving the Euler-Lagrange equation, and supervise on `q̈`.

DeLaN (Lutter et al. 2019) tried something similar but assumed `T = q̇^T M(q) q̇` (kinetic energy quadratic in `q̇`). Works for rigid-body dynamics; *fails* for relativistic systems, charged particles, anything with velocity-dependent potentials. LNN puts no functional constraint on `L`.

## The Math

Lagrangian formalism: action `S = ∫ (T − V) dt` is stationary on physical trajectories → [[Euler-Lagrange-Equation]]:

```
d/dt (∂L/∂q̇_j) = ∂L/∂q_j
```

Vectorized: `d/dt ∇_q̇ L = ∇_q L`. Expanding the time derivative by chain rule:

```
(∇_q̇ ∇_q̇^T L) q̈ + (∇_q ∇_q̇^T L) q̇ = ∇_q L
```

Solve for `q̈`:

```
q̈ = (∇_q̇ ∇_q̇^T L)^(-1) [ ∇_q L − (∇_q ∇_q̇^T L) q̇ ]                    (Eq. 6)
```

The matrix `∇_q̇ ∇_q̇^T L` is the velocity-Hessian (the "mass matrix" for mechanical systems). Inverted at every forward pass — `O(d³)` cost. Paper uses `pinv` (pseudoinverse) for numerical safety against singular cases.

Loss: `MSE(q̈_predicted − q̈_true)` (analogous to HNN's gradient-MSE loss).

### Code Correspondence (JAX, App. A)

```python
import jax, jax.numpy as jnp

def acceleration(L_fn, params, q, q_t):
    """q̈ = M^(-1) [∇_q L − (∇_q ∇_q̇^T L) q̇]   where M = ∇_q̇ ∇_q̇^T L."""
    L = lambda q, q_t: L_fn(params, q, q_t)
    M     = jax.hessian(L, 1)(q, q_t)                    # ∇_q̇ ∇_q̇^T L
    grad_q = jax.grad(L, 0)(q, q_t)                       # ∇_q L
    cross = jax.jacobian(jax.jacobian(L, 1), 0)(q, q_t)   # ∇_q ∇_q̇^T L
    return jnp.linalg.pinv(M) @ (grad_q - cross @ q_t)
```

Four lines for the entire forward dynamics. Equivalent torch is much messier (`create_graph=True` cascades).

**Activation choice matters:** the forward pass takes the Hessian of network output, so activation second derivatives must be non-zero. ReLU is broken (`∂²/∂x² = 0`). Hyperparameter search over ReLU², ReLU³, tanh, sigmoid, softplus → **softplus wins**.

**Custom initialization** (App. C): standard Kaiming/Xavier insufficient — loss is highly nonlinear in params due to Hessian inversion. They use Cranmer's *eureqa* symbolic regression to fit:
```
σ = (1/√n) × { 2.2 first layer, 0.58·i hidden layer i, n output layer }
```

## Toy Example

**Double pendulum** (Sec. 5). Masses + lengths set to 1. 600,000 random initial conditions, learn instantaneous accelerations.

| Metric | LNN | Baseline |
|---|---|---|
| L2 loss | 7.3e-2 | 7.4e-2 |
| Mean energy discrepancy (% of max PE) | **0.4%** | 8% |
| Long-horizon energy drift (`t=0..1000`) | ≈ 0 | ~6% loss |

Phase-space dynamics tracked tightly for both at short horizons; energy drift exposes the difference (Fig. 2b).

**Relativistic particle** (Sec. 5). `L = ((1−q̇²)^(−1/2) − 1) + g·q`. Canonical momentum `p = q̇(1−q̇²)^(−3/2)`. Given non-canonical observables `(q, q̇)`:

| Approach | Result |
|---|---|
| HNN, arbitrary coords | **fails** (Fig. 3a) |
| HNN, canonical coords | succeeds (Fig. 3b) |
| LNN, arbitrary coords | succeeds (Fig. 3c) |

This is the existence proof for "LNN works where HNN can't."

**1D wave equation** with Lagrangian Graph Network (Sec. 5). Per-node Lagrangian density summed over grid; sparse Hessian (nearest-neighbor only) → linear-time inversion. Learns wave dynamics + conserves total energy across the grid (Fig. 4).

## Connection to SiS / CTPC

LNN closes the **Hamiltonian↔Lagrangian duality** for SiS architectures. See [[Hamiltonian-vs-Lagrangian-Duality]] for the full design-decision treatment.

Quick summary:

| Feature | HNN | LNN |
|---|---|---|
| Coordinates | canonical `(q,p)` only | arbitrary `(q, q̇)` |
| Cost / step | `O(d²)` (autograd of scalar) | `O(d³)` (Hessian + matrix inversion) |
| Relativistic / non-quadratic KE | fails without canonical transform | works directly |
| Compatible integrator | symplectic (leapfrog, Yoshida) | variational (Marsden-West) |
| Dissipation | none — needs PHNN | none — needs Rayleigh-dissipation |

For CTPC specifically:
- **Keplerian dynamics in equinoctial elements** (already canonical, ~quadratic KE): HNN may be cheaper.
- **Relativistic GEO/cislunar precision** or unusual perturbations: LNN handles cases HNN can't.
- **Position-velocity straight from TLE data**: LNN avoids the canonical-momentum construction step (TLE-derived `(r, v)` are *not* canonical under perturbed Hamiltonians).
- **Dissipation gap is identical to HNN; same two-route choice.** The Lagrangian dissipative extension uses a [[Rayleigh-Dissipation-Function]] `D(q̇)` modifying Euler-Lagrange to `d/dt(∂L/∂q̇) − ∂L/∂q + ∂D/∂q̇ = 0`. **The Hamiltonian-side now has [[D-HNN]] (Helmholtz route) and [[Port-Hamiltonian-Neural-Network]] (J-R route) as the two architectural realizations** — see [[Hamiltonian-vs-Lagrangian-Duality]] § "A Fourth Axis: Dissipation Route". The Lagrangian-side equivalent of D-HNN doesn't appear named in the literature; the equivalent of PHNN-proper is "Lagrangian + PSD-constrained Rayleigh" but isn't a named architecture either. Open territory.

## Connections

- [[Lagrangian-Neural-Network]] — the architecture pattern, separable from this paper's experiments
- [[Lagrangian-Mechanics]] — foundational physics
- [[Euler-Lagrange-Equation]] — math object behind the forward dynamics
- [[Rayleigh-Dissipation-Function]] — the Lagrangian-side dissipation primitive (modified Euler-Lagrange `d/dt(∂L/∂q̇) − ∂L/∂q + ∂D/∂q̇ = 0`)
- [[Hamiltonian-vs-Lagrangian-Duality]] — synthesis: when to use HNN vs LNN for CTPC; also Helmholtz-vs-J-R dissipation routes
- [[HNN]] — Hamiltonian counterpart; co-authored by Greydanus (also on this paper)
- [[D-HNN]] — Sosanya & Greydanus 2022; Hamiltonian-side Helmholtz-route dissipative extension. Cranmer is in D-HNN's acknowledgments — same Greydanus-Cranmer collective
- [[Hamiltonian-Neural-Network]] — sibling architecture
- [[Dissipative-Hamiltonian-Neural-Network]] — D-HNN architecture pattern; the Lagrangian-side analog isn't yet in the literature
- [[Port-Hamiltonian-Neural-Network]] — J-R-route dissipative extension on Hamiltonian side
- [[PeRCNN]] — physics-as-architecture sibling for spatial PDEs (LGN here is the Lagrangian PDE analogue)
- [[Pi-Block-Polynomial-Approximator]] — Cranmer thread: LNN is Cranmer-authored *structured-prior* example; the *eureqa*-based init scheme here is a meta-application of his symbolic regression toolkit

## Empirical Counterpoint (PhyArch DP Benchmark, 2026-05)

The [[PhyArch-Double-Pendulum-Benchmark]] (Bilal's own work) tested LNN head-to-head against PINN and a symmetry-hardwired PhyArch (PaA) model on the planar double pendulum. Results push back on the standard "LNN guarantees energy conservation" narrative:

- **LNN failure rate: 9.4% / 25% / 43.8%** on ID / OOD-Energy / OOD-Upright. Highest of the three models on every split.
- **LNN reflection error: 19.43**, the *worst* of the three (PaA: 0.0 exact; PINN: 0.42). Lagrangian formalism enforces *energy* structure but says nothing about *spatial* symmetries; the softplus MLP has no reason to be parity-equivariant — and isn't.
- **Energy drift @ 20s OOD Upright: 41.3 J for LNN vs 28.2 J for PaA.** PhyArch conserves energy *better* than LNN despite not modeling energy explicitly.

The mechanism (Sec. 10.2 of the PhyArch source): LNN's "guarantee" is contingent on (1) having the *exact* Lagrangian — but LNN learns `L̂ ≈ L` and the Euler-Lagrange equation applied to `L̂` conserves a Hamiltonian *of the learned system*, slightly different from true `E = T + V`; and (2) using a symplectic integrator — but RK4 isn't, so even the learned Hamiltonian drifts cumulatively.

**Both conditions fail in practice.** LNN's structural guarantee is therefore formalism-level, not all-physics-level. This benchmark validates the distinction empirically.

## Open Questions

- **Computational cost at SDA scale.** `O(d³)` Hessian inversion at every step. For `d ~ 6` (orbital state) free; for `d ~ 100+` (N-body), infeasible without graph sparsity. Whether this is a hard blocker for production CTPC depends on the actual `d`.
- **Activation expressiveness.** Softplus's second derivative is bell-shaped and vanishes at extremes. May limit ability to represent Lagrangians with sharp features (e.g., relativistic kinetic energy near `q̇ → c`). Open whether other twice-differentiable activations (Mish, GELU) help.
- **Variational integrator pairing.** Symplectic integrators preserve `H` for HNN; the Lagrangian analogue (Marsden-West discrete mechanics) preserves the discrete Euler-Lagrange equation exactly. Pairing LNN with variational integrators is unstudied here.
- **Cranmer thread.** Same author on `MCranmerPhDThesis.pdf` (in `raw/papers/symbolic-physics/`, *not yet ingested*) and the symbolic-distillation work flagged on the [[Pi-Block-Polynomial-Approximator]] page. LNN sits at the *structured-prior* end of Cranmer's philosophy; the thesis covers the *post-hoc symbolic distillation* end. Reading both should close the inductive-bias-spectrum synthesis.
- **Init scheme empirical.** Custom σ values found by *eureqa*; no theoretical derivation. Robustness to architecture changes (depth/width beyond tested ranges) untested.
- **Gauge equivalence.** `L` and `L + df/dt` give identical dynamics. The learned `L_θ` from LNN isn't unique — only the equivalence class matters. Open whether this gauge freedom helps or hurts optimization.

## Sources

- `raw/papers/interpretability/LNN.pdf` — Eq. 1 (Poisson bracket relations defining canonical), Eq. 2–3 (action + Euler-Lagrange), Eq. 5–6 (vectorized + solved-for-q̈), Sec. 5 experiments (double pendulum, relativistic particle, 1D wave eq.), App. A (4-line JAX implementation), App. B (worked ball-in-gravity example), App. C (custom init scheme).
- Code: <https://github.com/MilesCranmer/lagrangian_nns>
