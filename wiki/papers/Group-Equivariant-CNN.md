---
title: Group Equivariant Convolutional Networks (G-CNN)
tags: [geometric-dl, equivariance, group-convolution, foundational]
sources:
  - raw/papers/geometric-dl/GroupEquiCNN.pdf   # Cohen & Welling 2016 (ICML, arXiv:1602.07576v3)
created: 2026-05-12
updated: 2026-05-12
sis_relevance: high
hard_constraint_possible: yes
---

# G-CNN (Cohen & Welling 2016)

## One-Line Intuition

Replace `[f ★ ψ](x) = Σ_y f(y)ψ(x − y)` with `[f ★ ψ](g) = Σ_y f(y)ψ(g⁻¹y)` — same shape,
general group action instead of just translations — and the entire CNN machinery (pooling,
nonlinearities, batchnorm, residual blocks) becomes equivariant to that group "for free."

## Why This Was Invented

By 2016 the field knew CNNs are translation-equivariant but had no principled way to bake
in *other* symmetries like rotations and reflections. The state of the art for rotated
MNIST involved hand-engineered rotation-aware features (Schmidt & Roth 2012, 3.98% error).
Cohen & Welling asked: can the convolution operation itself be generalized so that
equivariance to a larger group is structural, not learned-from-augmentation?

Answer: yes, for any discrete group `G` acting on the sampling lattice. Plain CNNs are the
special case `G = ℤ²` (translations only). The paper introduces `p4` (translations + 90°
rotations, a 4-rotation subgroup of the wallpaper group) and `p4m` (translations +
rotations + mirror reflections, 8 elements per spatial location).

## The Math

**Symmetry group.** A symmetry of a domain `Ω` is a transformation that leaves the domain
invariant. For `ℤ²`, the group `p4` consists of all compositions of translations and 90°
rotations about any grid center; `p4m` adds mirror reflections. `p4` has 4 rotations × all
translations; `p4m` has 8 rotation-flips × all translations.

**G-equivariance** (eq. 1 of the paper):
```
Φ(T_g x) = T_g' Φ(x)   for all g ∈ G
```
The operator `T'` on the output need not equal `T` on the input — only `T(gh) = T(g)T(h)`
(group homomorphism).

**G-convolution** (eqs. 10, 11):
- **First layer** (input is a function on `ℤ²`):
  `[f ★ ψ](g) = Σ_{y ∈ ℤ²} Σ_k f_k(y) ψ_k(g⁻¹y)`
- **Subsequent layers** (inputs are functions on `G`):
  `[f ★ ψ](g) = Σ_{h ∈ G} Σ_k f_k(h) ψ_k(g⁻¹h)`

After the first G-conv, feature maps live on `G`, not on `ℤ²`. For `p4` each spatial
location carries 4 rotation "channels"; for `p4m`, 8.

**Equivariance proof sketch.** The substitution `h → uh` and the group axiom `(uh)⁻¹g =
h⁻¹u⁻¹g` make `[L_u f ★ ψ](g) = [L_u (f ★ ψ)](g)` an algebraic identity. (Full derivation
in Appendix A.)

**Three G-CNN layer types, all G-equivariant:**

1. G-convolution / G-correlation (as above).
2. Pointwise nonlinearity `ν : ℝ → ℝ` — equivariant by post-composition: `(ν ∘ L_h f) = L_h (ν ∘ f)`.
3. G-pooling — max over a neighborhood `gU`; commutes with `L_h`.

For *invariance* to a subgroup `H ⊂ G` (e.g. invariance to rotation but not translation),
use **coset pooling**: pool over each coset `gH` so the result lives on `G/H`. Premature
coset pooling kills equivariance in deeper layers — the paper shows P4CNN (G-equivariance
throughout, pool at end) beats P4CNNRotationPooling (pool to invariance early) substantially
on rotated MNIST.

### Code Correspondence

The implementation insight (§7) is that p4 / p4m G-convolution reduces to a *filter
transformation* (rotate / flip the kernel) followed by a standard planar convolution with
an `|G|`-times-larger filter bank:

```python
import torch, torch.nn as nn, torch.nn.functional as F

# p4 G-convolution as filter transformation + planar conv
class P4Conv2d(nn.Module):
    def __init__(self, in_channels, out_channels, kernel_size):
        super().__init__()
        # Standard planar filters; we'll rotate them to build the augmented bank
        self.weight = nn.Parameter(torch.randn(out_channels, in_channels, kernel_size, kernel_size))

    def forward(self, x):
        # Build augmented filter bank by rotating the kernel 4 times (90° each)
        filters = torch.stack([torch.rot90(self.weight, k=i, dims=(-2, -1)) for i in range(4)])
        # filters: [4, out_c, in_c, k, k]   →   reshape to [4*out_c, in_c, k, k]
        filters = filters.view(-1, x.shape[1], *filters.shape[-2:])
        y = F.conv2d(x, filters, padding=filters.shape[-1] // 2)
        # y: [B, 4*out_c, H, W]   →   [B, out_c, 4, H, W]  (4 = rotation channel)
        B, _, H, W = y.shape
        return y.view(B, -1, 4, H, W)
```

For deeper layers the input already has a rotation-channel axis; the filter transformation
becomes a permutation over that axis as well. The point: the cost is *roughly the same as a
plain CNN with an `|G|`-times-wider filter bank* — `O(|G|)` overhead, not `O(|G|²)`.

## Toy Example — Rotated MNIST

| Architecture | Test error (%) |
|---|---|
| Schmidt & Roth 2012 (rotation-aware hand features) | 3.98 |
| Z²CNN baseline | 5.03 |
| P4CNNRotationPooling (pool to invariance after layer 1) | 3.21 |
| **P4CNN (G-equivariance throughout, coset-pool at end)** | **2.28** |

P4CNN halves the error rate of the planar baseline at the same parameter budget. The gap
between P4CNN (2.28%) and P4CNNRotationPooling (3.21%) is the empirical case for
*equivariance throughout vs. premature invariance*.

## Connection to SiS / CTPC

**G-CNN is the canonical worked example of [[Geometric-Priors]] for non-trivial groups.**
It demonstrates that the "hard constraint via equivariant parameterization" principle is
not just theoretical — it gives drop-in replacements for plain conv layers that consistently
improve generalization without additional tuning.

For SiS:

1. **PhyArch's parity-split assembly is the SO(2)-equivariant analog of G-CNN for ODE state
   spaces.** Where Cohen-Welling discretizes `SO(2)` to `p4` (4 rotations) for image grids,
   PhyArch works in continuous `SO(2)` on configuration manifolds. The principle —
   equivariance baked into the layer, not added by augmentation — is identical. See
   [[PhyArch]].

2. **"Equivariance throughout, invariance only at the end" applies to CTPC.** The
   prediction head is the only place where invariance (or partial invariance) should be
   built in; intermediate Latent NCDE Corrector layers should preserve full equivariance.
   This bears on Q2 of [[CTPC-Design-Rationale]] (variance head equivariance) — premature
   invariance in the variance head would lose orientation information.

3. **The discrete-group restriction matters.** G-CNN is for discrete groups; continuous
   `SO(2)` (orbital) requires either approximation (e.g. discretize to enough rotations and
   accept residual error) or moving to the steerable / irrep-based framework (Cohen &
   Welling 2017+, Tensor Field Networks 2018). SiS needs the continuous case; G-CNN is the
   conceptual entry point but not the operational solution.

4. **Coset pooling for invariance to physically irrelevant symmetries.** If a CTPC
   prediction should be invariant to a physically-meaningless choice (e.g. ECI frame
   orientation gauge), coset pooling over the gauge group at the end of the corrector is
   the canonical mechanism.

**Hard vs soft constraint.** **Hard.** Equivariance is an algebraic identity at random init
(Appendix A is the derivation). This is exactly the SiS step-1 design philosophy.

## Connections

- [[Geometric-Deep-Learning]] — Bronstein et al. 2021, where G-CNN appears as §5.2 in the
  unified blueprint.
- [[Geometric-Priors]] — the symmetry prior G-CNN realizes for discrete groups.
- [[Group-Convolution]] — the foundational mathematical operation as a stand-alone concept.
- [[PhyArch]] — SiS continuous-`SO(2)` analog for ODE state spaces.
- [[CTPC-Design-Rationale]] — Q1 (where does PhyArch symmetry go) and Q2 (variance head
  equivariance) are the SiS counterparts of G-CNN's design choices.

## Open Questions

1. **Continuous-group equivariance for orbital `SO(2)`.** G-CNN is discrete; orbital is
   continuous. Candidates: (a) discrete approximation with enough rotations + Lie-group
   interpolation; (b) steerable kernels / irreps (Cohen-Welling 2017, Weiler et al. 2018);
   (c) gauge-equivariant continuous kernels (Cohen-Geiger-Weiler 2019). To be settled by
   the geometric-dl agent in dialogue with the Hamiltonian-NN agent on continuous-time
   dynamics.

2. **`p4m`-style mirror symmetry for orbital.** Mirror symmetry is a `Z₂` element. Orbital
   dynamics under J2 has time-reversal `Z₂` but not spatial mirror. Whether the `p4m`
   pattern (translation × rotation × mirror) maps to anything operationally useful for
   orbital is unclear.

3. **G-CNN for graphs / sets.** The paper's framework is sampling-lattice specific. The
   `[f ★ ψ](g) = Σ_h f(h)ψ(g⁻¹h)` formula generalizes to any group, but for graphs the
   natural symmetry is permutation; this leads to GNNs and DeepSets, not G-CNN. Relevant
   for if/when SDA models a constellation as a graph.

## Sources

- Cohen, T. S. & Welling, M. (2016). *Group Equivariant Convolutional Networks.* ICML 2016.
  arXiv:1602.07576v3. All 12 pages read for this ingest (paper proper + Appendix A
  equivariance derivation + Appendix B gradient + Appendix C G-conv calculus).
