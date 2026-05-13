---
title: Geometric Deep Learning — Grids, Groups, Graphs, Geodesics, and Gauges
tags: [geometric-dl, equivariance, symmetry, scale-separation, erlangen-programme, foundational]
sources:
  - raw/papers/geometric-dl/GeometricDL.pdf   # Bronstein, Bruna, Cohen, Veličković 2021 (arXiv:2104.13478v2, May 2021)
created: 2026-05-12
updated: 2026-05-12
sis_relevance: critical
hard_constraint_possible: yes
---

# Geometric Deep Learning (Bronstein Proto-Book, 2021)

## One-Line Intuition

The Erlangen Programme of machine learning: every successful neural architecture is unified
as a *symmetry + scale-separation* prior on a domain (a "5 Gs" zoo of Grids, Groups, Graphs,
Geodesics, Gauges), so when you pick an architecture you're really picking a geometric
domain and its symmetry group.

## Why This Was Invented

The "zoo problem." By 2020 deep learning had CNNs, RNNs, GNNs, Transformers, DeepSets, Mesh
CNNs, Equivariant Message Passing, Intrinsic Mesh CNNs, LSTMs — all invented somewhat
independently, with few unifying principles. The book's diagnosis (Preface): *"this makes it
difficult to understand the relations between various methods, inevitably resulting in the
reinvention and re-branding of the same concepts in different application domains."* Plus
the curse of dimensionality — generic Lipschitz learning requires `~ε⁻ᵈ` samples (the
`Lipschitz class grows too quickly`, §2.2), so any tractable high-dimensional learning needs
geometric structure to dodge the exponential. The remedy: borrow Felix Klein's 1872
*Erlangen Programme* — define each geometry by its invariants under a symmetry group — and
apply it to deep architectures.

## The Math

**Geometric prior = signals + symmetry + scale separation.** A learning problem is specified
by:

- **Domain** `Ω` (a set, grid, graph, manifold — the underlying structure)
- **Signal space** `𝒳(Ω, 𝒞) = {x : Ω → 𝒞}` — `𝒞`-valued functions on `Ω`, a Hilbert space
  under the natural inner product `⟨x, y⟩ = ∫_Ω ⟨x(u), y(u)⟩_𝒞 dμ(u)`
- **Symmetry group** `𝔊` acting on `Ω` (e.g. translations on `ℤ²`, rotations on `S²`,
  permutations on graphs)
- **Function-on-signals** `f : 𝒳(Ω) → 𝒴` that is **𝔊-equivariant** (commutes with the
  group action) and admits **scale separation** (composes coarse approximations with local
  refinements)

The two geometric priors:

1. **Symmetry / equivariance.** If `g ∈ 𝔊` acts on the input as `ρ(g)x`, then
   `f(ρ(g)x) = ρ'(g)f(x)` for some output action `ρ'` (this is *Symmetry 1* in the modern
   Barbiero framing — see [[Actionable-Interpretability-Symmetries]]).

2. **Scale separation.** Coarsening the domain (pooling, downsampling, message passing
   between levels) preserves task-relevant information — global features = composition of
   local features. This is what CNN pooling, multigrid solvers, and U-nets all do.

### Code Correspondence

The "Geometric Blueprint" (§3.5) reduces to:

```python
# Generic GDL layer: equivariant local op + (optional) scale-separation pooling
def gdl_layer(x, group_action, local_op, pooling=None):
    # x: signal on domain Ω, shape [|Ω|, C]
    # local_op is 𝔊-equivariant by construction (the "hard constraint")
    y = local_op(x)                       # f(ρ(g)x) = ρ'(g)f(x) — algebraic identity
    if pooling is not None:
        y = pooling(y)                    # coarsen the domain; scale separation
    return y
```

Specific architectures are *instances*:

- **CNN** — Ω = grid; 𝔊 = translations; local_op = convolution; pooling = subsampling.
- **GNN** — Ω = graph; 𝔊 = permutations of nodes; local_op = message passing; pooling = graph coarsening.
- **DeepSets** — Ω = unordered set; 𝔊 = full permutation group; local_op = sum-pooled embedding.
- **Transformer** — Ω = set with learned positional embeddings; 𝔊 = permutations; local_op = attention.
- **Group-equivariant CNN** — Ω = group `G`; 𝔊 = `G` acting by left-multiplication;
  local_op = group convolution. See [[Group-Equivariant-CNN]] (forward ref to ingest #2).

## Toy Example

Generic equivariance is checkable as an algebraic identity, not a learned property:

```python
import torch, torch.nn as nn

# 2D CNN is translation-equivariant by construction
conv = nn.Conv2d(1, 1, kernel_size=3, padding=1, bias=False)
x = torch.randn(1, 1, 8, 8)

# Equivariance: shift then conv = conv then shift
shifted = torch.roll(x, shifts=(2, 3), dims=(2, 3))
assert torch.allclose(conv(shifted), torch.roll(conv(x), shifts=(2, 3), dims=(2, 3)))
# Holds at random init — it's an architectural property, not a learned one.
```

The same check on a fully-connected MLP fails: MLPs are not translation-equivariant. That
failure is exactly the curse-of-dimensionality cost a generic MLP pays.

## Connection to SiS / CTPC

GDL is **the foundational framework** for the symmetry-hardwiring side of the SiS design
hierarchy. Specifically:

1. **[[PhyArch]] is a GDL instantiation.** PhyArch's parity-split assembly + geometric-feature
   embedding is the Bronstein blueprint applied to ODE state spaces: domain = configuration
   manifold, group = the relevant physical symmetry (`Z₂` for double pendulum reflection,
   `SO(2)` for orbital rotation around Earth's pole — `SO(3)` broken to `SO(2)` by J2).
   Validated empirically in [[PhyArch-Double-Pendulum-Benchmark]] (0% failure on OOD,
   lowest energy drift).

2. **CTPC-Design-Rationale Q1 lives here.** "Where does PhyArch's symmetry hardwiring go
   inside CTPC?" is a direct application of the Bronstein blueprint — what is `Ω`, what is
   `𝔊`, where in the Latent NCDE Corrector does the equivariant local op fit? See Q1 of
   [[CTPC-Design-Rationale]].

3. **CTPC-Design-Rationale Q2 (variance head equivariance) is the same question for `𝒴 = Σ`.**
   If the predictive covariance lives on a different output representation, the equivariance
   constraint becomes `f(ρ(g)x) = ρ'(g) Σ ρ'(g)ᵀ` — the right-hand side is the conjugation
   action on covariance matrices. Implemented analytically in
   [[Analytic-Sigma-CTPC-Composition]].

4. **The 5 Gs map onto SDA data.** Orbital state lives on `SE(3)` (a group); TLE catalogs
   form a relational dataset (set-or-graph structure); spacecraft constellation could be
   modeled as a graph; surface-normal-aware drag terms live on `S²` (a manifold). The
   framework provides a principled vocabulary for choosing architectures here.

**Hard vs. soft constraint classification.** GDL is **the canonical hard-constraint
framework**. Equivariance is enforced *by construction of the local op*, not via loss
penalty. This is exactly what CLAUDE.md SiS design hierarchy step 1 means by "baked into
architecture." The book's framing makes this explicit (§3): equivariance is an algebraic
identity at every random init, not a learning objective.

## Connections

- [[Geometric-Priors]] — the symmetry + scale-separation pair as a stand-alone concept.
- [[PhyArch]] — SiS instantiation for ODE state spaces.
- [[CTPC-Design-Rationale]] — Q1, Q2 are direct applications.
- [[Group-Equivariant-CNN]] — Cohen-Welling 2016, the first deep-learning realization of a
  non-trivial group action beyond translations (ingest #2 of this batch).
- [[Actionable-Interpretability-Symmetries]] — Barbiero et al. 2026's "inference
  equivariance" is the same construct.
- [[Symplectic-Gradient]], [[Port-Hamiltonian-Systems]] — symmetry-respecting *dynamics*
  via Hamiltonian / port-Hamiltonian structure; complementary to GDL's static-signal
  framing.

## Open Questions

1. **GDL for continuous-time dynamics.** Bronstein's framework is for static signals
   `x: Ω → 𝒞`. CTPC's Latent NCDE Corrector is a *dynamical* system on a control path.
   Section 5.7 (RNNs) and 5.8 (LSTMs) cover discrete-time recurrence but not NCDEs. **What
   is the right GDL framing for an SO(2)-equivariant NCDE driven by a TLE control path?**
   This is the rigor target for [[Analytic-Sigma-CTPC-Composition]] Test 4 and connects to
   Q2 of [[CTPC-Design-Rationale]].

2. **Gauge theory for J2-broken orbital dynamics.** Section 4.5 (Gauges and Bundles) covers
   the framework for symmetries that vary across the domain — e.g. orbital dynamics under
   J2 is no longer globally `SO(3)`-invariant but is locally so. The proper GDL framing is
   gauge-equivariant; whether this is operationally useful for SDA is an open SiS question.

3. **Scale separation in orbital trajectories.** "Pooling" for a CTPC corrector is unclear —
   are there meaningful scales (e.g. orbital period, J2 secular timescale, drag decay scale)
   to coarsen across? If so, a multi-scale corrector could be principled rather than ad hoc.

## Sources

- Bronstein, Bruna, Cohen, Veličković (2021). *Geometric Deep Learning: Grids, Groups,
  Graphs, Geodesics, and Gauges.* arXiv:2104.13478v2. Pages 1–15 read for this ingest
  (Preface, Notation, §1 Introduction, §2 Learning in High Dimensions, §3 Geometric Priors
  intro). Remaining chapters (§3.5 Blueprint, §4 the 5 Gs, §5 Models, §6 Applications, §7
  Historic Perspective) deferred for future deeper ingest passes when specific GDL
  techniques become operationally relevant to a SiS milestone.
