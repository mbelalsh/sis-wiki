---
title: Hamiltonian Neural Network
tags: [hamiltonian-mechanics, architecture-pattern, scalar-parameterization, conservation-laws, physics-as-architecture]
sources: [raw/papers/interpretability/HNN.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: high
hard_constraint_possible: yes
---

# Hamiltonian Neural Network

## One-Line Intuition

An architecture pattern for learning conservative dynamical systems: parameterize a scalar Hamiltonian `H_θ(q,p)` with an MLP, then compute the dynamics `(dq/dt, dp/dt)` by autograd of `H_θ` through Hamilton's equations. Conservation of `H_θ` along learned trajectories is structural, not penalized.

## Why This Was Invented

A vanilla NN dynamics model `(q,p) → (dq/dt, dp/dt)` has no inductive bias toward conservation laws: it must learn that the system has a conserved quantity, and it gets this slightly wrong, which compounds over time. The Hamiltonian Neural Network bakes conservation in by construction.

Introduced in [[HNN]]; the pattern generalizes to any system whose dynamics can be expressed as a [[Symplectic-Gradient]] of a scalar function. This page describes the architectural pattern itself, separable from the experimental claims of any specific paper.

## The Math

The architecture in three lines:

1. **Network**: `H_θ : ℝ^{2N} → ℝ` (any scalar-output net — MLP, ResNet, equivariant net)
2. **Dynamics**: autograd `(∂H_θ/∂q, ∂H_θ/∂p)`, then symplectic-flip → `(dq/dt, dp/dt) = (∂H_θ/∂p, −∂H_θ/∂q)`
3. **Loss**: supervise the autograd outputs against observed time derivatives:
   ```
   L = ‖∂H_θ/∂p − dq/dt‖² + ‖∂H_θ/∂q + dp/dt‖²
   ```

Integration of the resulting field uses any ODE solver (RK4 in the original paper; symplectic integrators like leapfrog or Yoshida give exact `H` conservation to a specified order).

**Design parameters:**

| Knob | Typical value | Trade-off |
|---|---|---|
| Activation | `tanh` | Smooth, twice-differentiable; ReLU works but kinks the Hamiltonian |
| Depth × width | 3 × 200 | Bigger nets fit complex `H` but harder to optimize |
| Loss type | gradient-MSE (Eq. 3) | Could also supervise integrated trajectories; more expensive |
| Integrator | RK4, `rtol=1e-9` | Symplectic integrators (leapfrog) preserve `H` exactly |
| Coordinate frame | canonical `(q,p)` | Non-canonical inputs need an auxiliary loss to enforce Poisson-bracket structure |

### Code Correspondence

```python
import torch, torch.nn as nn

class HNN(nn.Module):
    """Generic Hamiltonian Neural Network.

    H_θ : ℝ^{2N} → ℝ ;  dynamics via autograd of H_θ.
    """
    def __init__(self, dim, hidden=200, depth=3, activation=nn.Tanh):
        super().__init__()
        layers, in_d = [], 2 * dim
        for _ in range(depth - 1):
            layers += [nn.Linear(in_d, hidden), activation()]
            in_d = hidden
        layers += [nn.Linear(in_d, 1)]
        self.H = nn.Sequential(*layers)

    def hamiltonian(self, x):
        return self.H(x)

    def time_derivative(self, x):
        x = x.requires_grad_(True)
        H = self.H(x).sum()
        dH = torch.autograd.grad(H, x, create_graph=True)[0]
        dq, dp = dH.chunk(2, dim=-1)
        return torch.cat([dp, -dq], dim=-1)
```

## Toy Example

For a 1-D mass-spring with true `H = ½q² + ½p²`:

```python
hnn = HNN(dim=1)
x = torch.tensor([[1.0, 0.0]])           # q=1, p=0  → max PE, zero KE
dxdt = hnn.time_derivative(x)            # before training: random
# After training on (q,p) trajectories, dxdt → (0, -1) at this point
```

After ~2000 gradient steps on 25 noisy trajectories, the contour map of `H_θ(q,p)` is concentric circles (level sets of true energy), differing from true `H` only by an additive constant.

## Connection to SiS / CTPC

This is the canonical pattern for **conservative-system corrector architectures** in CTPC.

- **Direct fit for Keplerian dynamics.** The two-body problem is Hamiltonian; the reduced-mass formulation `H = |p|²/(2μ) + V(|q|)` is well-posed. An HNN trained on TLE-derived state-rate pairs would produce a corrector that conserves orbital energy by construction.
- **Composition with [[Physics-Based-FD-Convolutional-Layer]]-style frozen physics.** SGP4 = frozen Keplerian + perturbation predictor; HNN-corrector = learned residual on the conservative part. Decomposition: dynamics = SGP4 (frozen, hard physics) + HNN (learned conservative residual) + dissipation term (PHNN for drag/SRP).
- **Reversibility for orbit determination.** Liouville's theorem gives bijective propagation → backward-integration is mathematically exact, useful for state estimation problems.

**Hard limitation: pure HNN cannot model non-conservative effects.** Drag, SRP, atmospheric heating, third-body resonances with non-Hamiltonian dissipation — none fit into a pure HNN. The natural extension is [[Port-Hamiltonian-Neural-Networks]], which adds an explicit dissipation operator. The SiS dissipation constraint `Ḣ ≤ 0` requires PHNN, not HNN.

## Connections

- [[HNN]] — the introducing paper
- [[Hamiltonian-Mechanics]] — foundational physics
- [[Symplectic-Gradient]] — the math object that produces Hamilton's equations
- [[Port-Hamiltonian-Neural-Networks]] *(not yet ingested)* — dissipative extension
- [[Lagrangian-Neural-Network]] — Legendre-dual architecture (works with non-canonical coords)
- [[Hamiltonian-vs-Lagrangian-Duality]] — synthesis: when to pick HNN vs LNN for CTPC
- [[PeRCNN]] — physics-as-architecture sibling for spatial PDEs

## Open Questions

- **Coordinate choice matters.** HNN assumes inputs are canonical `(q,p)` pairs. For orbital state in equinoctial elements, you'd need to verify the resulting Hamiltonian transformation produces a canonical pair (it does for standard canonical transformations, but worth checking for non-singular orbital element sets).
- **Symplectic integrators vs. RK4.** Paper uses RK4 with very tight tolerance (`1e-9`). Symplectic integrators (leapfrog, Yoshida) preserve a *modified* Hamiltonian exactly to a specified order — pairing them with HNN should improve long-horizon stability further. Worth experimenting.
- **High-dimensional state.** Paper goes up to 8-D (two-body, 4 `(q,p)` pairs). For full N-body or perturbation-rich orbital state, scaling behavior is unstudied here.
- **Latent-space `(q,p)` learning.** Paper's pixel pendulum needs an auxiliary canonical-coordinate loss. Whether equivariant encoders or learned symplectic-by-construction coordinate transformations can eliminate this is open.
- **First entry in `wiki/architectures/`.** As more dynamics-learning architectures are ingested (LNN, PHNN, Symplectic ResNet, Hamiltonian Generative Networks), this page will become a hub. The pattern of separating "the architecture" from "the paper that introduced it" should hold.

## Sources

- `raw/papers/interpretability/HNN.pdf` — Greydanus, Dzamba, Yosinski 2019 (NeurIPS); paper that introduced this pattern.
