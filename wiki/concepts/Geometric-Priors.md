---
title: Geometric Priors (Symmetry + Scale Separation)
tags: [geometric-dl, equivariance, symmetry, scale-separation, inductive-bias]
sources:
  - raw/papers/geometric-dl/GeometricDL.pdf   # §3, "Geometric Priors"
created: 2026-05-12
updated: 2026-05-12
sis_relevance: critical
hard_constraint_possible: yes
---

# Geometric Priors

## One-Line Intuition

Two assumptions about the world — *symmetries exist* and *coarse approximations preserve
information* — that together turn an exponentially-hard high-dimensional learning problem
into a tractable one.

## Why This Was Invented

The curse of dimensionality is brutal: estimating a generic 1-Lipschitz function on
`[0,1]^d` to `ε` accuracy requires `~ε⁻ᵈ` samples (the Lipschitz class grows too quickly,
per Sobolev-class minimax rates). For `d = 100` and `ε = 0.01` that's more atoms than the
universe. But *most real-world targets aren't generic Lipschitz* — they live on
low-dimensional geometric structure, and their dependence on input variables respects
physical symmetries.

Geometric priors formalize this: instead of assuming the target `f : ℝᵈ → ℝ` is just
Lipschitz, assume it is **equivariant** under a known symmetry group and **scale-separable**
across a hierarchy of resolutions. These two priors are the source of every architecture in
the [[Geometric-Deep-Learning|Bronstein blueprint]].

## The Math

For a signal `x : Ω → 𝒞` on a domain `Ω` with symmetry group `𝔊`:

**Prior 1 — Equivariance** (Symmetry).
A function `f : 𝒳(Ω) → 𝒴` is `𝔊`-equivariant if there exist representations `ρ` on input
and `ρ'` on output such that:
```
f(ρ(g) x) = ρ'(g) f(x)   for all g ∈ 𝔊, x ∈ 𝒳(Ω)
```
Invariance is the special case `ρ' = id`. Equivariance is the algebraic identity that turns
`|𝔊|` orbits into a single training example — sample complexity scales with `|𝒳(Ω) / 𝔊|`,
not `|𝒳(Ω)|`.

**Prior 2 — Scale separation.**
There exists a domain coarsening `Ω → Ω₁ → Ω₂ → ...` such that the target factors as a
composition of local operators across scales. Formally: `f ≈ f_K ∘ P_K ∘ ... ∘ f_1 ∘ P_1`
where each `P_k` coarsens and each `f_k` is local on the coarsened domain. Multigrid solvers
and CNN pooling are the canonical examples.

### Code Correspondence

```python
# Equivariance is verifiable as an algebraic identity at any (even random) parameter setting:
def assert_equivariant(f, x, group_action_in, group_action_out, g):
    # f(ρ(g) x) == ρ'(g) f(x)
    lhs = f(group_action_in(g, x))
    rhs = group_action_out(g, f(x))
    assert torch.allclose(lhs, rhs, atol=1e-6)

# Scale separation is realized via pooling / coarsening in the forward pass:
def multiscale_layer(x, local_op, coarsen):
    fine_features = local_op(x)
    coarse_x = coarsen(x)
    return fine_features, coarse_x   # downstream layers operate on coarse_x
```

## Toy Example

Translation equivariance on a 1D signal:

```python
import numpy as np
# A 1D moving-average filter (kernel size 3) commutes with circular shifts
def conv1d(x, kernel):
    return np.array([sum(x[(i+k) % len(x)] * kernel[k] for k in range(len(kernel))) for i in range(len(x))])

x = np.array([0.0, 1.0, 0.0, 2.0, 0.0, 3.0])
kernel = np.array([1/3, 1/3, 1/3])
shifted_x = np.roll(x, 2)
assert np.allclose(conv1d(shifted_x, kernel), np.roll(conv1d(x, kernel), 2))
# Algebraic identity — holds at any kernel.
```

For scale separation: stack the conv with a 2-stride downsample, and global structure
emerges from local + coarsen composition. A fully-connected MLP cannot do either.

## Connection to SiS / CTPC

**Hard constraint, by construction.** Both priors are baked into the architecture (the
`local_op` is equivariant by parameterization; the `coarsen` is a fixed deterministic
operator). This is exactly the SiS design hierarchy step 1 — physical knowledge as
architecture, not loss penalty.

For CTPC the priors apply as follows:

- **Symmetry prior.** The orbital state `(r, v) ∈ ℝ⁶` has `SO(2)` symmetry around Earth's
  pole (J2-broken `SO(3)`). The Predictor (GMAT) respects this exactly; the Corrector should
  inherit it. PhyArch's parity-split assembly is the engineering realization — see
  [[PhyArch]] and Q1 of [[CTPC-Design-Rationale]]. For the variance head, the right
  equivariance is `Σ ↦ R Σ Rᵀ` (conjugation), which is what
  [[Analytic-Sigma-CTPC-Composition]] makes exact.

- **Scale-separation prior.** Operationally less obvious for orbital dynamics. Candidates:
  orbital period vs. J2 secular drift vs. atmospheric-drag decay constant — these are
  different physical timescales that could in principle be the "scales" for a multi-scale
  corrector. This is currently an **open question** (flagged in
  [[Geometric-Deep-Learning]]) — using scale separation principledly in CTPC is a future
  design opening.

## Connections

- [[Geometric-Deep-Learning]] — the framework that elevates these two priors as the
  organizing principle of all deep architectures.
- [[PhyArch]] — symmetry prior instantiation for ODE state spaces.
- [[Symplectic-Gradient]], [[Port-Hamiltonian-Systems]] — symmetry-respecting *dynamics*
  via energy conservation / dissipation structure.
- [[Actionable-Interpretability-Symmetries]] — Barbiero's "inference equivariance" is the
  same algebraic identity as Prior 1.
- [[Physics-Based-FD-Convolutional-Layer]] — a worked example of hard-encoding a known
  differential operator; the spatial analog of the symmetry prior.

## Open Questions

1. **What is the right scale-separation hierarchy for orbital trajectories?** Period /
   J2 / drag timescales are candidates; selecting the "pooling" operator is an open SiS
   design decision.

2. **Equivariance for stochastic NCDEs.** Bronstein's framework is for deterministic
   functions on signals. CTPC is probabilistic — the equivariance constraint on the
   *predictive distribution* is `P_{Y|X}(y | ρ(g)x) = P_{Y|X}(ρ'(g)⁻¹ y | x)`, a
   distribution-level identity. Test 4 of [[Analytic-Sigma-CTPC-Composition]] is the
   empirical verification target.

3. **Gauge-equivariance for J2-broken dynamics.** When the symmetry group varies across
   the domain (J2 breaks `SO(3) → SO(2)` differently at different latitudes — actually it
   doesn't, but more general perturbations can break global symmetries), the framework
   becomes gauge-equivariance (Bronstein §4.5). Operational relevance for SiS unclear.

## Sources

- Bronstein, Bruna, Cohen, Veličković (2021). *Geometric Deep Learning.* arXiv:2104.13478v2,
  §2.2 (Curse of Dimensionality), §3 (Geometric Priors). Pages 8–15 read for this ingest.
