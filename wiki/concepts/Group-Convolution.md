---
title: Group Convolution (G-convolution)
tags: [geometric-dl, equivariance, group-action, foundational]
sources:
  - raw/papers/geometric-dl/GroupEquiCNN.pdf   # Cohen & Welling 2016, §4-6
created: 2026-05-12
updated: 2026-05-12
sis_relevance: high
hard_constraint_possible: yes
---

# Group Convolution

## One-Line Intuition

Ordinary convolution sums shifted copies of a filter; group convolution sums
group-transformed copies of a filter — same formula, general group action replaces the
shift.

## Why This Was Invented

Ordinary 2D convolution `[f ★ ψ](x) = Σ_y f(y)ψ(x − y)` is equivariant to translations
because `x − y` reduces to `x + t − y` after shifting by `t` — the shift commutes with the
sum. That commutativity is the structural reason CNNs generalize. **Anything that
commutes with a more general group action would give equivariance to that group.** Group
convolution is the principled generalization.

## The Math

For a discrete group `G` acting on its underlying set:

**G-correlation** (first layer, where input is a signal on `ℤ²`):
```
[f ★ ψ](g) = Σ_{y ∈ ℤ²} Σ_k f_k(y) ψ_k(g⁻¹ y)
```

**G-correlation** (subsequent layers, where input is a function on `G` itself):
```
[f ★ ψ](g) = Σ_{h ∈ G} Σ_k f_k(h) ψ_k(g⁻¹ h)
```

**Equivariance property** — the central algebraic identity:
```
L_u (f ★ ψ) = (L_u f) ★ ψ      for all u ∈ G
```
where `L_u f(g) := f(u⁻¹ g)` is the left-action of `u` on functions. Proof: substitute
`h → uh` in the sum and use group associativity. Holds **at random init** — equivariance
is a property of the operation, not of the weights.

**Special cases:**
- `G = ℤ²` (translations): recovers plain 2D convolution.
- `G = p4` (translations + 90° rotations): the layer is translation- *and* rotation-equivariant.
- `G = p4m` (translations + rotations + mirror): adds reflection equivariance.
- `G = SO(3)` (continuous 3D rotations): requires steerable kernels or spherical
  harmonics; the discrete-G formula does not apply directly.

### Code Correspondence

```python
import torch, torch.nn.functional as F

def p4_group_conv_first_layer(x, filters):
    """G-correlation for G=p4, first layer (input is a function on ℤ²).
    x: [B, C_in, H, W]
    filters: [C_out, C_in, k, k]  (learnable; will be rotated 4 times)
    returns: [B, C_out, 4, H, W]  — 4 = rotation channel
    """
    # Build the augmented filter bank by rotating the kernel through the 4 elements of p4
    bank = torch.stack([torch.rot90(filters, k=r, dims=(-2, -1)) for r in range(4)], dim=1)
    # bank: [C_out, 4, C_in, k, k]  →  [C_out * 4, C_in, k, k]
    bank = bank.reshape(-1, *bank.shape[2:])
    y = F.conv2d(x, bank, padding=filters.shape[-1] // 2)
    B, _, H, W = y.shape
    return y.view(B, -1, 4, H, W)
```

The forward-pass cost is `O(|G|)` multiplied over a plain convolution, not `O(|G|²)` —
because the filter transformation is just a precomputed permutation/rotation of one set of
learnable parameters.

## Toy Example

Verify that `p4`-correlation is equivariant under a 90° rotation:

```python
import torch
x = torch.randn(1, 3, 8, 8)
filters = torch.randn(4, 3, 3, 3)
y = p4_group_conv_first_layer(x, filters)            # [1, 4, 4, 8, 8]

# Rotate the input 90° and rerun
x_rot = torch.rot90(x, k=1, dims=(-2, -1))
y_rot = p4_group_conv_first_layer(x_rot, filters)    # [1, 4, 4, 8, 8]

# Equivariance: rotating the input cyclically permutes the rotation-channel of the output
# AND rotates the spatial features. Check the cyclic permutation + spatial rotation:
y_expected = torch.roll(torch.rot90(y, k=1, dims=(-2, -1)), shifts=1, dims=2)
print(torch.allclose(y_rot, y_expected, atol=1e-5))   # True — algebraic identity
```

## Connection to SiS / CTPC

Group convolution is the operational realization of [[Geometric-Priors]]' symmetry prior.
For SiS:

- **Discrete-group case:** any time the SDA data has a discrete symmetry (e.g. mirror
  symmetry of a sensor geometry, rotation symmetry of an antenna array), G-convolution
  gives a drop-in hard-constraint layer.
- **Continuous-group case (orbital `SO(2)`/`SO(3)`):** the formula `Σ_h f(h)ψ(g⁻¹h)` becomes
  an integral, and the kernel must be steerable (decomposable in irreducible
  representations). This is the technique used in Tensor Field Networks, equivariant point
  cloud networks, and the orbital-rotation-equivariant variants in
  [[Geometric-Deep-Learning]] §5.2.
- **For ODE state spaces:** [[PhyArch]] is the continuous-`SO(2)` analog. Group convolution
  is the "translation step" that PhyArch's parity-split assembly performs in continuous
  configuration space.

**Hard constraint.** The equivariance identity holds at random init. This is exactly the
SiS step-1 design philosophy — physical knowledge as architecture, not loss penalty.

## Connections

- [[Group-Equivariant-CNN]] — Cohen-Welling 2016, the deep-learning paper that introduced
  the discrete-G case operationally.
- [[Geometric-Deep-Learning]] — Bronstein blueprint where G-conv is the canonical
  equivariant local operator.
- [[Geometric-Priors]] — the symmetry prior G-convolution realizes.
- [[PhyArch]] — continuous-`SO(2)` analog for ODE state spaces.

## Open Questions

1. **Continuous-group convolution for `SO(2)`/`SO(3)`.** The discrete formula doesn't
   extend trivially. Steerable kernels (Cohen-Welling 2017), spherical CNNs (Cohen et al.
   2018), Tensor Field Networks (Thomas et al. 2018), e3nn (Geiger-Smidt 2022) are the
   continuous-group machinery. Need to ingest one of these to anchor the operational
   solution for orbital `SO(2)`.

2. **Group convolution on Lie-group state spaces.** Orbital configuration space is a Lie
   group `SE(3)` (or `SO(3) × ℝ³` for some parameterizations). The natural convolution is
   over `SE(3)`. Implementation candidates: B-spline CNN on Lie groups (Bekkers 2020,
   already in `raw/papers/geometric-dl/B-Spline_CNN_Lie_Groups.pdf`).

## Sources

- Cohen, T. S. & Welling, M. (2016). *Group Equivariant Convolutional Networks.* §4
  Mathematical Framework, §6 Group Equivariant Networks. arXiv:1602.07576v3.
