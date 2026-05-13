---
title: Neural Ordinary Differential Equations (NODE)
tags: [neural-odes, continuous-depth, adjoint-method, normalizing-flows, foundational]
sources:
  - raw/papers/neural-odes/NODE.pdf   # Chen, Rubanova, Bettencourt, Duvenaud 2018 (NeurIPS, arXiv:1806.07366v5)
created: 2026-05-12
updated: 2026-05-12
sis_relevance: critical
hard_constraint_possible: yes
---

# NODE (Chen et al. 2018)

## One-Line Intuition

Take the limit `Δt → 0` of a ResNet hidden-state update `h_{t+1} = h_t + f(h_t, θ_t)` and
you get an ODE `dh/dt = f(h, t, θ)` whose solution at time `T` is the output — and you can
train the parameters with the adjoint method using `O(1)` memory in network depth.

## Why This Was Invented

ResNets (`h_{t+1} = h_t + f(h_t, θ_t)`) are Euler discretizations of a continuous ODE. The
discrete-step version pays `O(L)` memory in depth (must store all activations for
backprop), can only use a fixed number of layers, and offers no principled way to choose
"depth." Chen et al. asked: can we treat the *limit* (a continuous ODE) as the modeling
primitive and inherit (i) constant memory, (ii) adaptive evaluation, (iii) explicit
speed-vs-accuracy tradeoff at inference, (iv) natural handling of irregular time series?
Answer: yes, via the **adjoint sensitivity method** (Pontryagin 1962) — gradient
computation through any black-box ODE solver, without storing the forward pass.

## The Math

**Forward pass** — define the output as the solution of an IVP:
```
h(0) = input
dh/dt = f(h(t), t, θ)
output = h(T) = ODESolve(h(0), f, 0, T, θ)
```
The model IS the vector field `f`. "Depth" becomes integration interval `[0, T]`; "layers"
become solver evaluations. The solver can choose adaptive step sizes.

**Backward pass — the adjoint method** (Algorithm 1; derivation in Appendix B). Define the
adjoint `a(t) = ∂L/∂h(t)`. Its dynamics are the "instantaneous chain rule":
```
da/dt = −a(t)ᵀ · ∂f(h(t), t, θ)/∂h
```
Gradients with respect to parameters are then a third integral:
```
dL/dθ = −∫_{t_1}^{t_0} a(t)ᵀ · ∂f/∂θ dt
```
All three quantities (`h`, `a`, `dL/dθ`) are recovered by **one call to a reverse-time ODE
solver** on an augmented state `[h, a, ∂L/∂θ]`. No forward-pass activations stored —
`O(1)` memory in depth.

**Surprising empirical result** (Figure 3c): the number of function evaluations in the
backward pass is **roughly half** the forward pass. So the adjoint method is not only more
memory-efficient than direct backprop-through-solver, it's also more *compute*-efficient.

**Continuous Normalizing Flows (CNF)** — §4. The *instantaneous change of variables*
theorem:
```
∂log p(h(t))/∂t = −tr(df/dh)
```
Trace, not determinant. Cost is `O(D)`, not `O(D³)` like discrete normalizing flows. **And
`f` need not be bijective** — uniqueness of the ODE solution (Picard's theorem; requires
Lipschitz `f`) suffices to make the transformation invertible automatically.

**Latent ODE generative model** — §5. RNN encoder over irregular observations → posterior
`q(z_{t_0})` → ODE solve `z_{t_1}, ..., z_{t_N}` → likelihood `p(x | z_t)`. Predictions at
arbitrary `t` (forward and backward) by extrapolating the latent ODE.

### Code Correspondence

The PyTorch implementation (released as `torchdiffeq`) makes the adjoint a one-liner:

```python
import torch, torch.nn as nn
from torchdiffeq import odeint_adjoint as odeint

# The model IS the vector field
class ODEFunc(nn.Module):
    def __init__(self):
        super().__init__()
        self.net = nn.Sequential(nn.Linear(2, 64), nn.Tanh(), nn.Linear(64, 2))
    def forward(self, t, h):
        return self.net(h)   # f(h, t, θ); ignores t in this example

f = ODEFunc()
h0 = torch.randn(1, 2, requires_grad=True)
t = torch.linspace(0, 1, 50)
hT = odeint(f, h0, t)        # forward integration via adaptive solver (DOPRI5 default)
loss = ((hT[-1] - target) ** 2).sum()
loss.backward()              # adjoint method computes ∇θ in O(1) memory
```

The user picks the ODE solver and tolerance; the framework picks the number of evaluations
adaptively. Training in `O(1)` memory in depth is a free side-effect.

## Toy Example — Latent ODE on irregular spirals

| Architecture | RMSE @ 30 obs | @ 50 obs | @ 100 obs |
|---|---|---|---|
| RNN (with time-deltas concat) | 0.3937 | 0.3202 | 0.1813 |
| **Latent ODE** | **0.1642** | **0.1502** | **0.1346** |

(Table 2 of the paper.) The latent ODE wins by 2× at sparse observation regimes. Crucially,
**reconstruction quality is consistent regardless of observation density** — the ODE
extrapolates to unobserved points naturally. This is exactly the property
[[Latent-NCDE-Corrector]] inherits.

## Connection to SiS / CTPC

**NODE is the substrate underneath CTPC.** Specifically:

1. **[[Neural-Controlled-Differential-Equation]] is the direct extension.** NCDEs (Kidger
   et al. 2020) replace the autonomous `dh/dt = f(h, θ)` with a control-path-driven
   `dh = f(h, θ) dX(t)` where `X` is an observation path. Same machinery (ODE solver +
   adjoint backprop) plus integration against a control. The NCDE concept page lives in the
   wiki; this paper is its primary technical ancestor.

2. **[[Latent-NCDE-Corrector]] is the direct architectural descendant of NODE §5's Latent
   ODE.** Same VAE-style encoder-decoder pattern (RNN encoder → posterior → continuous-time
   latent → emission likelihood), upgraded to NCDE for control-path support. The
   2× RMSE win at sparse observations in Table 2 of this paper is the empirical
   precedent for [[CTPC-KDD-Submission]]'s 64% MSE reduction at 4-day horizon.

3. **Hamiltonian CNF (Appendix A.1) connects directly to [[Hamiltonian-Neural-Network]].**
   The paper notes that a *symplectic split* `(dx_{1:d}/dt, dx_{d+1:D}/dt) = (f(x_{d+1:D}),
   g(x_{1:d}))` is volume-preserving because the Jacobian has zero diagonal, so the trace
   vanishes — `∂log p/∂t = 0`. This is the same volume-preservation property that makes
   [[Symplectic-Gradient]] conserve `H` in [[Hamiltonian-Mechanics]]. NODE's CNF and HNN
   are siblings: both exploit zero-trace Jacobians from physical structure.

4. **Picard uniqueness (Appendix C remarks, §6 Scope) is the foundational existence
   guarantee for all NCDE-based correctors in CTPC.** A NODE/NCDE has a unique solution
   iff `f` is uniformly Lipschitz in `h` (and continuous in `t`). Tanh/ReLU nonlinearities
   with finite weights satisfy this. *This is the regularity condition the
   [[Geometric-Priors]] curse-of-dimensionality discussion sits on top of.*

5. **Adjoint method ≠ structure-preserving integrator.** Important caveat for
   [[CTPC-Design-Rationale]] Q8b-discrete-time: the adjoint method handles *gradient
   computation*; it doesn't make the *forward integrator* structure-preserving. NODE's
   forward solver is whatever the user picks (paper defaults to implicit Adams). For a
   PHNN-style block inside a NODE-trained pipeline, the Q8b-discrete-time question
   (discrete gradient method, AVF, Gauss-collocation) is *separate* from the adjoint
   question and must still be answered.

**Hard vs soft constraint.** NODE's continuous-time framing is itself architectural — by
defining the model as a vector field, certain mathematical structure (e.g. volume
preservation via zero-trace Jacobians) is enforceable as a parameterization constraint, not
a loss penalty. The CNF Hamiltonian split is a worked example of hard volume preservation.

## Connections

- [[Neural-Controlled-Differential-Equation]] — Kidger et al. 2020, the control-path
  extension; the SiS Corrector substrate.
- [[Latent-NCDE-Corrector]] — direct descendant of NODE §5's Latent ODE; used in CTPC.
- [[CTPC-KDD-Submission]] — Bilal's CTPC paper; inherits the NCDE machinery NODE pioneered.
- [[Adjoint-Sensitivity-Method]] — the `O(1)`-memory backprop technique as a stand-alone
  concept.
- [[Hamiltonian-Neural-Network]] — sibling architecture; Hamiltonian CNF (Appendix A.1) is
  the NODE-side instantiation of the same volume-preservation idea.
- [[Symplectic-Gradient]] — the symplectic split in Hamiltonian CNF is exactly the symplectic
  vector field.
- [[CTPC-Design-Rationale]] — D1 (Predictor-Corrector decomposition), D2 (continuous-time
  corrector), D7 (CTPC-CDE++); Q8b-discrete-time caveat.

## Open Questions

1. **Structure-preserving forward integrators inside NODE framework.** Q8b-discrete-time of
   [[CTPC-Design-Rationale]] is open. NODE uses implicit Adams (LSODE/VODE); does any of
   the candidate structure-preserving integrators (discrete gradient method, AVF,
   Gauss-collocation) play nicely with the adjoint? Or do we need to abandon the adjoint
   for structure-preserving cases? Practical question for SiS implementation.

2. **Hamiltonian CNF as a building block.** App A.1 sketches a Hamiltonian flow as a
   volume-preserving CNF; the literature has expanded on this (e.g. SymODEN family — see
   [[Dissipative-SymODEN]]). Whether the Hamiltonian-CNF route gives a *better* SiS
   building block than the J-R port-Hamiltonian route is an open SiS comparison.

3. **NODE for dissipative dynamics.** NODE assumes uniqueness from Lipschitz `f`, which
   is fine for dissipative systems too. But the structure of a dissipative NODE — what
   makes `Ḣ ≤ 0` an algebraic identity inside the NODE framework — is exactly the question
   [[Port-Hamiltonian-Neural-Network]] answers in the autonomous-ODE setting.
   Re-deriving inside the NCDE / Latent-NCDE setting is the SiS application.

## Sources

- Chen, R. T. Q., Rubanova, Y., Bettencourt, J., & Duvenaud, D. (2018). *Neural Ordinary
  Differential Equations.* NeurIPS 2018, Montréal. arXiv:1806.07366v5 (revision 14 Dec
  2019). All 18 pages read: paper proper (§1 Introduction through §8 Conclusion) + Appendix
  A (proof of instantaneous change of variables), Appendix B (adjoint proof), Appendix C
  (full adjoint algorithm with gradients w.r.t. `t_0`, `t_1`), Appendix D (autograd
  implementation), Appendix E (latent ODE training), Appendix F (extra figures).
