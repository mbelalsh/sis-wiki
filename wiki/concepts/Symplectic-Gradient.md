---
title: Symplectic Gradient
tags: [hamiltonian-mechanics, symplectic-geometry, vector-fields, foundational]
sources: [raw/papers/interpretability/HNN.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: high
hard_constraint_possible: yes
---

# Symplectic Gradient

## One-Line Intuition

For a scalar function `H(q,p)` on phase space, the symplectic gradient is the vector field `S_H = (∂H/∂p, −∂H/∂q)`. Where the ordinary gradient `∇H` points "uphill" (steepest ascent of `H`), the symplectic gradient points along level sets — moving along `S_H` keeps `H` exactly constant.

## Why This Was Invented

The natural derivative of a scalar function on a Riemannian manifold is the gradient, and following it changes the function value as fast as possible. But many physical systems have a *symplectic* (rather than Riemannian) structure on their state space, with a built-in "twisting" that pairs `q` and `p` coordinates. The symplectic gradient is the corresponding natural derivative: instead of changing `H`, it preserves it.

This is what makes Hamilton's equations conservative *by construction*. The symplectic gradient is the abstract object behind "energy-conserving dynamics" — and the operation [[Hamiltonian-Neural-Network]] computes via autograd.

## The Math

For phase space `ℝ^{2N}` with coordinates `x = (q,p)`, the canonical symplectic form is the matrix:

```
J = [  0    I_N ]
    [ −I_N   0  ]
```

The **symplectic gradient** of a scalar `H : ℝ^{2N} → ℝ` is:

```
S_H(x) = J ∇H(x) = (∂H/∂p, −∂H/∂q)
```

Compare to the ordinary (Riemannian) gradient `∇H = (∂H/∂q, ∂H/∂p)`.

**Key property: orthogonality.** `S_H · ∇H = (∂H/∂p)(∂H/∂q) − (∂H/∂q)(∂H/∂p) = 0`. The symplectic gradient is *always perpendicular* to the ordinary gradient — moving along `S_H` cannot change `H`.

**Hamilton's equations.** The flow `dx/dt = S_H(x)` is exactly Hamilton's equations.

**Riemann gradient (paper's terminology).** The HNN paper also uses `R_H = (∂H/∂q, ∂H/∂p)`, which they call the "Riemann gradient" (just the ordinary gradient, with a label to distinguish from `S_H`). Following `R_H` *increases* `H` — useful as a counterfactual operator ("add 1 J of energy").

### Code Correspondence

```python
import torch

def symplectic_gradient(H_fn, x):
    """S_H = J ∇H = (∂H/∂p, −∂H/∂q) via autograd."""
    x = x.requires_grad_(True)
    H = H_fn(x).sum()
    grad = torch.autograd.grad(H, x, create_graph=True)[0]
    dq, dp = grad.chunk(2, dim=-1)
    return torch.cat([dp, -dq], dim=-1)

def riemann_gradient(H_fn, x):
    x = x.requires_grad_(True)
    H = H_fn(x).sum()
    return torch.autograd.grad(H, x, create_graph=True)[0]

# Verify orthogonality on a quadratic Hamiltonian
x = torch.randn(1, 4)                        # 2 DoF system
H = lambda x: (x ** 2).sum(dim=-1)
S = symplectic_gradient(H, x)
R = riemann_gradient(H, x)
print((S * R).sum().item())                  # ≈ 0  (orthogonal)
```

## Toy Example

**Mass-spring: `H = ½q² + ½p²`.**

- `∇H = (q, p)` — points radially outward in the `(q,p)` plane (toward higher energy)
- `S_H = (p, −q)` — points tangent to circles of constant energy (rotation)

Integrating `dx/dt = S_H` produces circular orbits, the classic harmonic-oscillator phase portrait. The dynamics rotate around the energy minimum without ever climbing or descending.

## Connection to SiS / CTPC

The symplectic gradient is the **mathematical object that makes [[Hamiltonian-Neural-Network]] possible**:

- HNN training = supervise the symplectic gradient (computed via autograd) against observed time derivatives
- The orthogonality property is *why* HNNs conserve their learned Hamiltonian
- The Riemann-gradient counterfactual (Sec. 6 of HNN paper, Fig. 5) — flipping from `S_H` to `R_H` to inject energy — is a clean primitive for **maneuver simulation in CTPC** without needing a separately-trained controller. "What does the orbit look like if we add Δv?" becomes "integrate `R_H` for a short pulse, then resume `S_H`."
- For symplectic integrators (leapfrog, Yoshida), the symplectic gradient is the discrete analogue: the integrator preserves the symplectic 2-form to machine precision, so a *modified* `H̃` is conserved exactly (modulo a small bounded oscillation around true `H`).

## Connections

- [[Hamiltonian-Mechanics]] — the framework where the symplectic gradient lives
- [[Hamiltonian-Neural-Network]] — the NN architecture that uses it
- [[HNN]] — paper that ties it to ML
- [[Lagrangian-Mechanics]] *(not yet ingested)* — alternative formulation related by Legendre transform; the analogue of `S_H` is the Euler-Lagrange operator

## Open Questions

- **Non-canonical symplectic forms.** On general symplectic manifolds (not `ℝ^{2N}`), the symplectic form `ω` is more general than the canonical `J`. The symplectic gradient becomes `S_H = ω^{-1}(dH)`. Relevant for orbital mechanics in non-canonical coordinates (Delaunay variables, modified equinoctial elements).
- **Symplectic structure on Lie groups.** For attitude dynamics (rotation matrices, quaternions), phase space is `T*SO(3)` — a cotangent bundle with its own symplectic structure. Generalizing HNN to this setting is an open architectural question.
- **Discrete symplectic gradients.** For training, you typically work with discrete time steps. Discrete symplectic structure (Marsden–West discrete mechanics) gives rigorously volume-preserving integrators — pairing with HNN should improve long-horizon stability further.

## Sources

- `raw/papers/interpretability/HNN.pdf` — Section 2 Theory, Section 6 ("Adding and removing energy"), Fig. 5 (Riemann vs. symplectic gradient visualization in pixel pendulum latent space).
