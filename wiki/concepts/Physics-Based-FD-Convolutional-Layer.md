---
title: Physics-Based Finite-Difference Convolutional Layer
tags: [hard-constraint, finite-difference, convolution, pde-learning, physics-as-architecture]
sources: [raw/papers/port-hamiltonian/EncodingPhysics.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: high
hard_constraint_possible: yes
---

# Physics-Based Finite-Difference Convolutional Layer

## One-Line Intuition

A convolutional layer whose kernel is *frozen* to the finite-difference (FD) stencil of a known differential operator — making the operator exact (up to discretization order), not learned.

## Why This Was Invented

If you know your PDE has a Laplacian term `Δu`, why ask a network to *learn* one? Standard PINNs can't bake operators into the architecture, only into the loss. PeRCNN's insight (formalized as Lemma 1 in Rao et al. 2023): **any FD operator is just a Toeplitz convolution with its stencil as the kernel**, so freezing the kernel turns the conv layer into the operator itself. Trainable parameters are reserved for *unknown* physics (residuals, scalar coefficients) — the known parts contribute zero degrees of freedom.

This is the structural realization of `physics-as-architecture` for spatial differential operators.

## The Math

For a bivariate differential operator `ℒ(·)` discretized with stencil `K[k₁, k₂]`, Taylor expansion gives:

```
ℒ(u)(x,y) = Σ_{k₁,k₂ = -(p-1)/2}^{(p-1)/2} K[k₁,k₂] · u(x + k₁ δx, y + k₂ δy)
            + 𝒪(|δx|^(p-1) + |δy|^(p-1))
          = K ⊛ u + 𝒪(...)
```

So a `p × p` conv with the right entries reproduces `ℒ` to order `p−1`. Common stencils used as frozen kernels:

| Operator | Stencil (2D, δx = 1) |
|---|---|
| `∂/∂x` (central, 2nd order) | `[[0,0,0],[-½,0,½],[0,0,0]]` |
| `∂/∂y` | `[[0,-½,0],[0,0,0],[0,½,0]]` |
| `Δ` (5-point Laplacian) | `[[0,1,0],[1,-4,1],[0,1,0]]` |
| `Δ` (9-point, 4th order) | weighted 3×3 |

In a learning model, you wrap this in a short-cut connection alongside a learnable scalar coefficient:

```
ℱ̂(u) = μ · (K_Δ ⊛ u)  +  Π-block(u)
        └── frozen ──┘    └── learned residual ──┘
```

### Code Correspondence

```python
import torch, torch.nn as nn

def make_frozen_laplacian(in_ch, dx=1.0):
    """5-point Laplacian as a non-trainable conv layer. Δ ≈ K ⊛ u."""
    K = torch.tensor([[0., 1., 0.],
                      [1.,-4., 1.],
                      [0., 1., 0.]]) / (dx*dx)
    layer = nn.Conv2d(in_ch, in_ch, 3, padding=1, bias=False, groups=in_ch)
    layer.weight.data = K[None, None].repeat(in_ch, 1, 1, 1)
    for p in layer.parameters():
        p.requires_grad = False                 # ← the operator is fixed, not learned
    return layer
```

## Toy Example

```python
import torch
u = torch.zeros(1, 1, 32, 32)
u[0, 0, 16, 16] = 1.0                            # delta function
lap = make_frozen_laplacian(1)
print(lap(u)[0, 0, 15:18, 15:18])
# tensor([[ 0.,  1.,  0.],
#         [ 1., -4.,  1.],
#         [ 0.,  1.,  0.]])  ← exactly the stencil, no training needed
```

Compare to the analytical Laplacian of a Gaussian bump on a 64×64 grid: the frozen layer recovers it to discretization-error precision; an *unfrozen* random-init conv would need thousands of gradient steps to even approximate it.

## Connection to SiS / CTPC

This is the canonical tool for **hard-encoding spatial differential operators** in any SiS architecture that operates on a discretized field.

- For **CTPC's current orbital state** representation (point-mass in equinoctial elements), this layer is not directly applicable — there's no spatial domain. The analogous "frozen operator" is SGP4 itself: a fixed propagator inside which trainable variables are forbidden.
- For **future SiS components** that work over spatial fields — atmospheric drag density grids, magnetic field maps, conjunction probability fields — this is the right primitive for known operators (∇, Δ, ∇·, ∇×).
- The principle generalizes: **freeze what you know, learn only what you don't**. This minimizes degrees of freedom and makes optimization well-posed in scarce-data regimes.

## Connections

- [[PeRCNN]] — original paper that introduced this short-cut connection
- [[Pi-Block-Polynomial-Approximator]] — the complementary component that learns the *unknown* residual physics
- [[Physics-Based-Padding]] — sister technique for hard-encoding BCs

## Open Questions

- **Irregular meshes.** Standard conv assumes regular Cartesian grids. For irregular geometries, graph convolutions with FD-equivalent stencils on neighbor sets are the natural extension — paper flags this as future work.
- **High-order stencils vs. depth.** Is a single 9×9 4th-order Laplacian filter better than two stacked 3×3 2nd-order ones? Stability and accuracy trade-offs are not systematically explored in the paper.
- **Mixed-known/unknown operators.** When `ℱ` contains `u · ∇u` (advection) — `∇u` is known structure, `u·` is a state-dependent multiplication — how do you cleanly separate the frozen and learned parts? Paper handles this by composing: frozen `∇` then element-wise product in the Π-block (Eq. 6 example for Burgers).

## Sources

- `raw/papers/port-hamiltonian/EncodingPhysics.pdf` — Lemma 1 (proof that conv approximates any FD operator), Methods § "Network architecture", Fig. 1a (the short-cut connection schematic).
