---
title: Adjoint Sensitivity Method
tags: [neural-odes, gradients, backprop, optimal-control, foundational]
sources:
  - raw/papers/neural-odes/NODE.pdf   # Chen et al. 2018, §2 + Appendix B; original Pontryagin et al. 1962
created: 2026-05-12
updated: 2026-05-12
sis_relevance: high
hard_constraint_possible: yes
---

# Adjoint Sensitivity Method

## One-Line Intuition

Compute gradients of an ODE solver's output by running a *second*, augmented ODE backwards
in time — `O(1)` memory in depth instead of `O(L)` for direct backprop, and (surprisingly)
*fewer* function evaluations on the backward pass than the forward.

## Why This Was Invented

Original setting: optimal control (Pontryagin et al. 1962). You want to optimize a
trajectory by tuning controls; storing the full trajectory and differentiating through
every step is expensive. The adjoint trick: introduce a *costate variable* `a(t) = ∂L/∂h(t)`
that itself obeys an ODE running *backwards* from the final loss; integrate it once, get
all gradients for free.

For deep learning (Chen et al. 2018): a [[NODE|Neural ODE]] has effectively unbounded
"depth" via adaptive solver evaluations. Storing every solver step for backprop kills the
memory advantage of the continuous-time framing. The adjoint method recovers it: train an
arbitrarily-deep continuous model in constant memory.

## The Math

Setup: a parameterized ODE
```
dh/dt = f(h(t), t, θ),    h(t_0) = h_0
```
with output `h(t_1)` fed into a scalar loss `L(h(t_1))`.

**The adjoint `a(t) = ∂L/∂h(t)` obeys its own ODE** (the "instantaneous chain rule"):
```
da/dt = −a(t)ᵀ · ∂f/∂h(h(t), t, θ)
```
Boundary condition: `a(t_1) = ∂L/∂h(t_1)` (the gradient of the loss w.r.t. the final state,
which is known). Integrate this ODE *backward* from `t_1` to `t_0` to recover `a(t_0) = ∂L/∂h_0`.

**Parameter gradients** are a third integral that runs alongside `a(t)`:
```
dL/dθ = −∫_{t_1}^{t_0} a(t)ᵀ · ∂f/∂θ dt
```

**The trick: all three (`h`, `a`, `dL/dθ`) by ONE call to a reverse-time ODE solver** on the
augmented state `[h(t), a(t), ∂L/∂θ]`. The augmented dynamics:
```
d/dt [h, a, ∂L/∂θ] = [f, −aᵀ ∂f/∂h, −aᵀ ∂f/∂θ]
```
Solver runs from `t_1` to `t_0`, returns all gradients at the boundary.

### Code Correspondence

The vector-Jacobian products `aᵀ ∂f/∂h` and `aᵀ ∂f/∂θ` are exactly what `autograd.grad` /
`torch.autograd.grad` computes natively — no symbolic differentiation needed. Pseudo-code
mirrors the paper's Algorithm 1:

```python
def adjoint_backward(f_theta, h_t1, dL_dh_t1, t0, t1, theta):
    """One ODE-solver call returns gradients w.r.t. h(t_0) AND theta — constant memory."""
    # Augmented initial state at time t_1 (the END of the forward pass)
    s_1 = (h_t1, dL_dh_t1, torch.zeros_like(theta))
    
    def aug_dynamics(t, s):
        h, a, dL_dtheta = s
        # f forward, then VJPs via autograd
        f_eval = f_theta(t, h)
        # vjp_h = a · ∂f/∂h     vjp_theta = a · ∂f/∂theta
        vjp_h, vjp_theta = torch.autograd.grad(
            f_eval, [h, theta], grad_outputs=a, retain_graph=True)
        return (f_eval, -vjp_h, -vjp_theta)
    
    # Integrate BACKWARD from t_1 to t_0
    h_t0, a_t0, dL_dtheta = ode_solve(aug_dynamics, s_1, t1, t0)
    return a_t0, dL_dtheta   # gradients w.r.t. initial state and parameters
```

The `retain_graph=True` is for repeated calls within the solver; `ode_solve` is any
black-box adaptive solver (DOPRI5, implicit Adams, etc.).

## Toy Example — gradient through an ODE

```python
import torch
from torchdiffeq import odeint_adjoint as odeint   # uses adjoint method automatically

class Linear(torch.nn.Module):
    def __init__(self): 
        super().__init__()
        self.A = torch.nn.Parameter(torch.tensor([[-1.0, 0.5], [-0.5, -1.0]]))
    def forward(self, t, h): 
        return h @ self.A.T   # f(h) = A h

f = Linear()
h0 = torch.tensor([1.0, 0.0], requires_grad=True)
t = torch.linspace(0, 2, 50)
hT = odeint(f, h0, t)[-1]
loss = (hT ** 2).sum()
loss.backward()   # adjoint method — no forward activations stored
print(f.A.grad)   # gradient through 50+ adaptive solver steps in O(1) memory
```

For a `Linear` field this is verifiable in closed form: `h(T) = exp(AT) h_0`, so
`∂h(T)/∂A` is an exponential-matrix derivative. The adjoint method gives the same answer
without ever materializing `exp(AT)` or storing any intermediate solver step.

## Connection to SiS / CTPC

**Adjoint method is the gradient backbone of every continuous-time CTPC architecture.**

1. **[[Latent-NCDE-Corrector]] training relies on the adjoint** for memory-efficient
   backprop through the NCDE encoder + NCDE decoder. Without it, `O(L)`-memory cost in the
   number of solver steps would dominate at long horizons.

2. **Trains an arbitrarily-long horizon** without memory growth. Operational asset for
   SDA: long-horizon trajectory training (multi-day forecasts in [[CTPC-KDD-Submission]])
   is only practical because the adjoint decouples memory from horizon length.

3. **The adjoint is NOT a structure-preserving integrator.** Important caveat for Q8b of
   [[CTPC-Design-Rationale]]: the adjoint method is for *gradient computation*. The
   *forward integrator* that determines whether `Ḣ ≤ 0` is preserved discretely is a
   *separate* choice. The adjoint composes with any forward solver — including
   structure-preserving ones like discrete gradient method / AVF / Gauss-collocation — but
   does not itself confer any structure-preservation guarantee.

4. **Connection to optimal control.** The adjoint method is *literally* the costate
   variable from Pontryagin's maximum principle. For the future `g(q)u` maneuver-planning
   asset in [[Port-Hamiltonian-Neural-Network]] (the SiS-canonical dissipative block), the
   adjoint method is already the right machinery — same equations as Pontryagin, just
   applied to gradient-of-loss rather than gradient-of-cost-to-go.

**Hard vs soft constraint.** Not a constraint, a *gradient computation algorithm*. But it
is a hard *correctness* property: it computes the exact gradient of the ODE solver's output
w.r.t. inputs/parameters (modulo solver tolerance), as an algebraic identity from the
chain rule. No approximation involved.

## Caveat — adjoint is error-prone for stiff / high-Lyapunov systems

Per [[PIML-Survey|Watson et al. 2025]] §3.1: "the gradient estimation based on the adjoint
method is error-prone due to the numerical inaccuracies of solving the backward ODE, due
to the typical high Lyapunov characteristic exponent of highly nonlinear ODEs." Citing
Gholaminejad et al. 2019, Onken & Ruthotto 2020, Zhuang et al. 2020/2021a, Krishnapriyan
et al. 2023.

**For SiS this matters.** Orbital dynamics has high Lyapunov exponents in chaotic /
resonance / close-approach regimes — exactly where SDA accuracy demands are highest.
Mitigations cited in the survey: (a) checkpointing forward states and re-integrating to
reconstruct rather than running the augmented backward ODE blindly; (b) improved backward
integrators; (c) solver-tolerance constraints. Empirically the issue often doesn't bite in
practice (per [[NODE]] §6 Scope and Limitations) but should be checked at the chaotic /
close-approach regime for CTPC operationally.

## Generalized Adjoint (Massaroli et al. 2020)

[[Dissecting-NODE|Massaroli et al. 2020]] extends the adjoint method to **losses
distributed along the depth domain**: `ℓ = L(z(S)) + ∫_𝒮 l(τ, z(τ)) dτ`. The backward
ODE for the adjoint picks up an extra source term `−∂l/∂z` wherever the integral loss is
non-zero:
```
ȧᵀ(s) = −aᵀ(s) ∂f_θ/∂z − ∂l/∂z,    aᵀ(S) = ∂L/∂z(S)
dℓ/dθ = ∫_𝒮 aᵀ(τ) ∂f_θ/∂θ dτ
```
Standard adjoint is the special case `l ≡ 0`. Same `O(1)`-memory property. Relevant to
CTPC if a trajectory-distributed loss replaces the current terminal-only NLL+CRPS+KL.

## Connections

- [[NODE]] — Chen et al. 2018, the paper that introduced the adjoint method to deep
  learning.
- [[Dissecting-NODE]] — Massaroli et al. 2020, generalized adjoint for path-distributed losses.
- [[Neural-Controlled-Differential-Equation]] — uses the adjoint for backprop through
  control-path-driven ODEs.
- [[Latent-NCDE-Corrector]] — operational dependency on the adjoint for long-horizon
  training.
- [[CTPC-Design-Rationale]] — Q8b-discrete-time caveat: adjoint ≠ structure-preserving.

## Open Questions

1. **Numerical accuracy of the adjoint vs direct backprop.** Reverse-time integration can
   introduce extra numerical error if the reconstructed forward trajectory diverges from
   the original. Mitigation: checkpointing intermediate states on the forward pass and
   re-integrating from those (NODE §6). Operationally, the paper reports this isn't a
   problem in practice; worth verifying at SDA-scale horizons.

2. **Adjoint method + structure-preserving integrators.** Open question whether the
   combination preserves both the structure-preservation of the forward solver AND the
   `O(1)` memory of the adjoint backward. Theoretically yes (they're independent); needs
   empirical verification for the specific PHNN-in-NCDE composition SiS targets.

## Sources

- Chen, R. T. Q., Rubanova, Y., Bettencourt, J., & Duvenaud, D. (2018). *Neural ODE.*
  NeurIPS 2018, §2 + Appendix B (modern proof of adjoint method) + Appendix C (full
  algorithm with gradients w.r.t. `t_0`, `t_1`). arXiv:1806.07366v5.
- Pontryagin, L. S., Mishchenko, E. F., Boltyanskii, V. G., Gamkrelidze, R. V. (1962). *The
  Mathematical Theory of Optimal Processes.* Original costate / adjoint formulation in
  continuous control.
