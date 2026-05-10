---
title: Port-Hamiltonian Neural Network (PHNN-proper / Dissipative SymODEN)
tags: [port-hamiltonian, hamiltonian-mechanics, dissipation, passivity, cholesky-psd, architecture-pattern, sis-canonical, physics-as-architecture]
sources: [raw/papers/port-hamiltonian/DissipativeSymODEN-Zhong2020.pdf]
created: 2026-05-10
updated: 2026-05-10
sis_relevance: critical
hard_constraint_possible: yes
---

# Port-Hamiltonian Neural Network (PHNN-proper / Dissipative SymODEN)

> **The canonical SiS dissipative block.** This is the architecture that closes Q8b-vector-field of [[CTPC-Design-Rationale]] (the J-R-structure route to `Ḣ ≤ 0` as a hard architectural constraint). The Dissipative SymODEN paper ([[Dissipative-SymODEN]], Zhong, Dey, Chakraborty 2020) provides the realization; this page is the architecture pattern, separable from the paper's specific experiments.

## One-Line Intuition

Encode [[Port-Hamiltonian-Systems|port-Hamiltonian]] structure as the computation graph: parameterize `H = ½pᵀM⁻¹(q)p + V(q)` and Cholesky-parameterize the dissipation matrix `D = LLᵀ` so it's PSD-by-construction. Dynamics `dx/dt = (J − D)∇H + g(q)u` then satisfy `dH/dt = −∇HᵀD∇H ≤ 0` *structurally* — passivity is built into the architecture, not learned.

## Why This Was Invented

[[HNN]] enforces conservation; [[D-HNN]] adds dissipation via [[Helmholtz-Decomposition]] but **fails the SiS hard-constraint requirement** — its energy rate `dH/dt = ∇H · ∇D` has indeterminate sign because the two scalars `(H_θ, D_θ)` are unconstrained relative to each other. See [[Dissipative-Hamiltonian-Neural-Network]] § "Why D-HNN is not enough for SiS" for the full analysis.

Port-Hamiltonian Neural Network (PHNN-proper) takes a different architectural strategy: encode the *port-Hamiltonian form* directly, with PSD-by-parameterization of the dissipation matrix. The result is a model that *cannot* produce non-passive dynamics — `Ḣ ≤ 0` is forced by the algebraic form, not learned from data.

## The Math

The architecture in five lines (per [[Dissipative-SymODEN]] §3.2, Eq. 5):

1. **Four neural networks** parameterizing the port-Hamiltonian components separately:
   - `M⁻¹_θ1(q)` — inverse mass matrix (Cholesky-PSD-parameterized; see below)
   - `V_θ2(q)` — scalar potential energy
   - `D_θ4(q)` — dissipation matrix (Cholesky-PSD-parameterized)
   - `g_θ3(q)` — input matrix (full column rank assumed; not PSD-constrained)

2. **Restricted Hamiltonian form** (Eq. 6):
   ```
   H_θ(q, p) = ½ pᵀ M⁻¹_θ1(q) p + V_θ2(q)
   ```
   Mechanical form: kinetic-quadratic-in-`p` plus position-only potential. Not generic `H_θ(q,p)` like HNN/D-HNN.

3. **Dynamics** (Eq. 5):
   ```
                                                          ┌──────┐
   (dq/dt, dp/dt) = J ∇H − D_θ4(q) ∇H + ┌    0    ┐ u    where  J = │ 0  I │
                                          └ g_θ3(q) ┘                │ −I 0 │
                                                                     └──────┘
   ```

4. **Cholesky parameterization for PSD**:
   ```
   D_θ4 = L_D Lᵀ_D       ← lower-triangular L_D ;   guarantees D_θ4 PSD for ANY L_D
   M⁻¹_θ1 = L_M Lᵀ_M + εI ← positive-definite (small ε for numerical stability)
   ```
   These constraints are *forced by parameterization*. Any `L_D` produces a valid PSD `D_θ4`; no additional loss term is required, no projection step is needed during training. **This is the SiS hard-constraint primitive.**

5. **Loss**: standard MSE on observed trajectories, integrated through Neural ODE:
   ```
   L = ‖x_observed − ODESolve(x_init, f_θ, t_span)‖²₂
   ```
   No physics-informed loss penalty. The physics is in the architecture.

### Energy balance — passivity is structural

Compute `dH/dt` along the dynamics:

```
dH/dt = ∇Hᵀ ẋ
       = ∇Hᵀ [J − D_θ4(q)] ∇H + ∇Hᵀ [0; g_θ3(q)] u
       = ∇Hᵀ J ∇H − ∇Hᵀ D_θ4 ∇H + ∇Hᵀ [0; g_θ3] u
       = 0           − ∇Hᵀ D_θ4 ∇H + ∇Hᵀ [0; g_θ3] u                     (J skew → ∇HᵀJ∇H = 0)
       ≤ ∇Hᵀ [0; g_θ3] u                                                  (D_θ4 PSD → −∇HᵀD∇H ≤ 0)
```

For the autonomous case (`u = 0`), `dH/dt = −∇Hᵀ D_θ4 ∇H ≤ 0`. **The PSD constraint on `D_θ4` is the load-bearing assumption.** Cholesky parameterization makes that PSD constraint structural.

### Code Correspondence

```python
import torch, torch.nn as nn

class PortHamiltonianNN(nn.Module):
    """Port-Hamiltonian NN with Cholesky-PSD dissipation matrix.
    Closes Q8b-vector-field of CTPC-Design-Rationale: dH/dt ≤ 0 by construction.
    """
    def __init__(self, q_dim, u_dim=0, hidden=128, eps=1e-3):
        super().__init__()
        self.q_dim, self.u_dim, self.eps = q_dim, u_dim, eps
        # Each "scalar" or "matrix-output" network is its own MLP
        self.V       = self._mlp(q_dim, hidden, 1)                 # potential V(q)
        self.L_M     = self._mlp(q_dim, hidden, q_dim*(q_dim+1)//2)  # Cholesky factor of M⁻¹
        self.L_D_2nq = self._mlp(q_dim, hidden, (2*q_dim)*(2*q_dim+1)//2)  # Cholesky of D (full 2n×2n)
        if u_dim > 0:
            self.g   = self._mlp(q_dim, hidden, q_dim*u_dim)       # input matrix g(q)

    @staticmethod
    def _mlp(in_d, h, out_d):
        return nn.Sequential(nn.Linear(in_d, h), nn.Tanh(),
                             nn.Linear(h, h),    nn.Tanh(),
                             nn.Linear(h, out_d))

    def _cholesky_psd(self, flat_lower, n):
        """Build PSD matrix from lower-triangular flat parameters (n*(n+1)/2 entries)."""
        L = torch.zeros(*flat_lower.shape[:-1], n, n, device=flat_lower.device)
        tril_idx = torch.tril_indices(n, n)
        L[..., tril_idx[0], tril_idx[1]] = flat_lower
        return L @ L.transpose(-1, -2)

    def hamiltonian(self, q, p):
        M_inv = self._cholesky_psd(self.L_M(q), self.q_dim) + self.eps * torch.eye(self.q_dim)
        return 0.5 * (p @ M_inv @ p.unsqueeze(-1)).squeeze(-1) + self.V(q).squeeze(-1)

    def time_derivative(self, q, p, u=None):
        x = torch.cat([q, p], dim=-1).requires_grad_(True)
        q_, p_ = x.chunk(2, dim=-1)
        H = self.hamiltonian(q_, p_).sum()
        gradH = torch.autograd.grad(H, x, create_graph=True)[0]
        # Symplectic part J∇H = (∂H/∂p, -∂H/∂q)
        dH_q, dH_p = gradH.chunk(2, dim=-1)
        sym = torch.cat([dH_p, -dH_q], dim=-1)
        # Dissipative part -D∇H, with D PSD via Cholesky
        D = self._cholesky_psd(self.L_D_2nq(q_), 2*self.q_dim)
        dis = -(D @ gradH.unsqueeze(-1)).squeeze(-1)
        # Input part [0; g·u]
        if u is not None and self.u_dim > 0:
            g = self.g(q_).reshape(*q_.shape[:-1], self.q_dim, self.u_dim)
            inp = torch.cat([torch.zeros_like(q_), (g @ u.unsqueeze(-1)).squeeze(-1)], dim=-1)
            return sym + dis + inp
        return sym + dis
```

## Toy Example

**Damped pendulum, generalized coordinates** (Task 1 of [[Dissipative-SymODEN]]). True dynamics: `q̇ = 3p`, `ṗ = −5 sin q − 0.3 p + u`. True Hamiltonian: `H = 1.5 p² + 5(1 − cos q)`.

After training, the four learned networks recover the physical components separately:

| Component | Recovery |
|---|---|
| `M⁻¹(q)` | recovered to within tight tolerance |
| `V(q)` | recovered up to additive constant (only derivatives matter) |
| `g(q)` | recovered well |
| `D(q)` | the trickiest — paper notes it doesn't fit ground truth perfectly with embedded data; better with explicit `(q, p)` data |

Quantitative: prediction error 34 vs naive baseline 33 (similar; baseline reached origin too fast which artificially flattered MSE), with **0.15M parameters vs naive 0.36M** — *structural prior gives a 2.4× parameter reduction at parity accuracy.*

For more challenging tasks (CartPole, Acrobot), the structural prior wins decisively: prediction error 12.7 vs 590 for unstructured-baseline on Acrobot (45× improvement), with smaller networks.

## Architectural Comparison: PHNN-proper vs D-HNN

| Aspect | [[Dissipative-Hamiltonian-Neural-Network|D-HNN]] (Helmholtz route) | **PHNN-proper (J-R route — this page)** |
|---|---|---|
| Hamiltonian form | Generic scalar `H_θ(q, p)` | Restricted: `½ pᵀ M⁻¹(q) p + V(q)` |
| Dissipation | Free scalar `D_θ(q,p)`, *ordinary* gradient | PSD matrix `D = L Lᵀ` via Cholesky, applied to `∇H` |
| Energy rate | `dH/dt = ∇H · ∇D` (indeterminate sign) | `dH/dt = −∇HᵀD∇H ≤ 0` (structural) |
| Control input | Not in formulation | Native via `g(q) u` |
| Networks | 2 scalars (`H, D`) | 4 networks (`M⁻¹, V, D, g`) |
| Param efficiency | Larger nets needed | 2.4× parameter reduction at parity accuracy |
| Coordinate handling | Generic phase space | (q, p), embedded angles `(cos q, sin q, q̇)`, hybrid `Rⁿ × Tᵐ` |
| Counterfactual scaling | Free via `α · D_θ` | Requires reparameterization of `D` (e.g., channel-decomposition — see § Future Extensions) |
| Decomposition guarantee | ✓ Helmholtz uniqueness | ✗ Restricted form (this is the *good* restriction for SiS) |
| **For SiS Ḣ ≤ 0 deployment-safety** | **Insufficient** — empirical only | **Sufficient at vector-field level** (RK4 caveat — see below) |

**The architectural choice for SiS is PHNN-proper.** D-HNN remains useful as a *diagnostic* tool (post-hoc Helmholtz decomposition of a learned vector field) but is not the deployment primitive.

## Why "Restricted Hamiltonian Form" Is Not a Compromise for Orbital

The mechanical-form restriction `H = ½ pᵀ M⁻¹(q) p + V(q)` excludes relativistic Hamiltonians, position-dependent non-quadratic kinetic terms, and gauge-theory Hamiltonians. **For orbital dynamics, this is exactly the form physics already provides:**

```
H_orbital = |p|²/(2m) + V(r)         ← two-body Newtonian
H_orbital = |p|²/(2m) + V_J2(r) + V_3body(r) + V_SRP(r) + ...  ← perturbed
```

The kinetic energy is exactly quadratic in `p`; perturbations enter through `V(q)` (potential) or `D(q)` (drag dissipation) — never through non-quadratic `T(p)` terms. **The architectural restriction is a structural match for orbital, not a compromise.** This is a confirmation of the J-R-route choice for CTPC, not a caveat.

## Integrator Caveat: RK4 ≠ Symplectic Preservation

[[Dissipative-SymODEN]] (the paper realizing this architecture) uses **RK4 as its numerical integrator**, *not* a structure-preserving integrator. Despite the name "SymODEN", the **"Sym" refers to the symplectic structure of the *vector field*, not the *integrator*.** The `Ḣ ≤ 0` guarantee derived above holds at the level of the continuous-time port-Hamiltonian ODE; the discrete RK4 trajectory only satisfies it *approximately* — accurate to RK4's `O(Δt⁴)` truncation error per step.

For SiS deployment-safety claims, this matters: the architectural `Ḣ ≤ 0` guarantee is real *up to integrator error*. For a structurally-exact discrete passivity guarantee, three candidate structure-preserving integrators are worth investigating:

- **Discrete gradient method** (Gonzalez 1996; McLachlan, Quispel, Robidoux 1999) — replaces `∇H` with a *discrete gradient* operator `∇̄H` satisfying `H(x_{k+1}) − H(x_k) = ∇̄H · (x_{k+1} − x_k)`. By construction the energy / dissipation balance is preserved exactly at the discrete level: `H(x_{k+1}) = H(x_k) − Δt · ∇̄Hᵀ D ∇̄H ≤ H(x_k)`.
- **Average-Vector-Field (AVF) method** (Quispel, McLaren 2008; Celledoni, Grimm, McLachlan, McLaren, O'Neale, Owren, Quispel) — second-order energy-preserving integrator that integrates the *time-averaged* vector field `(1/Δt) ∫ f(x(τ)) dτ`. AVF preserves first integrals of conservative systems exactly; extended versions handle dissipation while preserving the dissipative-energy-balance.
- **Gauss-collocation methods** (s-stage Gauss-Legendre) — implicit Runge-Kutta methods that are *symplectic* for purely Hamiltonian systems and have substantially smaller energy drift than explicit methods like RK4 when applied to dissipative perturbations. Higher computational cost per step (implicit equations to solve) but much larger usable timestep.

**These are flagged as concrete targets for the Q8b-discrete-time follow-up ingest.** When that ingest happens, this section becomes the resolution point. Until then: PHNN-proper closes Q8b at the vector-field level, and discrete-time exact passivity is a separate, well-defined open question.

## Connection to SiS / CTPC

**This is the canonical SiS dissipative block.** The CTPC corrector's vector-field decomposition becomes:

```
f_θ_d(z_d) = f_PHNN-conservative(z_d) + f_PHNN-dissipative(z_d) + f_PHNN-input(z_d, u)
              └────── J ∇H ───────┘     └──── −D(q) ∇H ────┘   └── g(q) u ──┘
```

with `H = ½ pᵀ M⁻¹(q) p + V(q)`, `D(q) = L L^T` (Cholesky-PSD), `g(q)` learned freely. **In current CTPC the input channel is unused** (`u = 0` — no actuator commands during forecast); the architecture handles this by setting `g·u = 0`. The architecture is ready for future maneuver-planning extensions where `u ≠ 0` (see § Future Extensions).

**Composition with PhyArch + Latent NCDE.** PHNN-proper plugs into the same composition gateway as Q1 of [[CTPC-Design-Rationale]] — replace the Latent NCDE decoder's vector field `f_θ_d(z_d)` with a PHNN-proper block. PhyArch's symmetry hardwiring (parity-split, geometric features) operates *inside* the H/V/D/g networks, not at the composition level. The principled triple `(LNN-or-HNN, PhyArch, J-R)` from [[Hamiltonian-vs-Lagrangian-Duality]] specializes here as: HNN-side conservative core (since orbital is canonical), PhyArch-symmetry-respecting network designs for `M⁻¹/V/D`, and J-R port-Hamiltonian structure for the dissipation.

**Hard-constraint classification.** Yes at the *vector-field level*; partial at the *integrated trajectory level* due to RK4 (see § Integrator Caveat).

## Future Extensions

**Native control-input channel `g(q)u` — forward-looking asset.** The architecture supports an external control input via the `g_θ3(q) u` term — currently unused in CTPC because spacecraft-state forecasting has no actuator commands during the prediction window. **For a future SiS extension to integrated maneuver planning** (e.g., joint state forecasting + Δv recommendation for collision avoidance, or model-predictive control of a propulsive spacecraft), the `u` channel is exactly the right architectural slot for representing thrust / Δv commands. This is a free asset, not a current need — but worth banking architecturally rather than rebuilding later.

**Channel-decomposed dissipation matrix for per-concept counterfactuals.** [[Dissipative-SymODEN]] uses a single `D_θ4(q)` Cholesky-parameterized matrix. For [[CBM-CTPC-Composition]] (Year 1 milestone), the design insight from [[CTPC-Design-Rationale]] § "Architectural Hook for Dissipation-Side Concept-Closure" calls for *channel-decomposed* dissipation:

```
D(q) = α_drag · D_drag(q) + α_SRP · D_SRP(q) + α_thermal · D_thermal(q) + ...
```

with each `D_i(q) = L_i Lᵢᵀ` Cholesky-PSD and each `α_i` a learnable scalar. **Sum of PSD matrices is PSD**, so `dH/dt ≤ 0` survives the decomposition. This extension is straightforward in PHNN-proper but requires an additional architectural commitment beyond what [[Dissipative-SymODEN]] delivers. Flagged as Year 1 implementation work; the decomposition is what closes both Q8b *and* the dissipation-side concept-closure compliance simultaneously.

**Discrete-time exact passivity (Q8b-discrete-time).** See § Integrator Caveat. Discrete gradient method is the most direct candidate; AVF is the most studied for second-order accuracy; Gauss-collocation is the highest-accuracy implicit option. None are in the current paper.

**Lagrangian-side analog (named-architecture gap in literature).** PHNN-proper is the Hamiltonian-side J-R-route architecture. The Lagrangian-side equivalent — Lagrangian + PSD-constrained Rayleigh dissipation — does not appear to be a named architecture in the literature. Opening for a methods paper if the orbital data turns out to favor the Lagrangian formalism (which it doesn't, since orbital is canonical, but worth flagging).

## Connections

- [[Dissipative-SymODEN]] — the introducing paper (Zhong, Dey, Chakraborty 2020 ICLR Workshop)
- [[Port-Hamiltonian-Systems]] — the math/physics foundation; provides the `(J − R)∇H + gu` formalism this architecture realizes
- [[Hamiltonian-Mechanics]] — the conservative special case; PH generalizes
- [[Hamiltonian-Neural-Network]] — conservative-only predecessor without dissipation
- [[Dissipative-Hamiltonian-Neural-Network]] — the *alternative* (Helmholtz-route) NN realization of dissipative HNN; insufficient for SiS — see § "Why D-HNN is not enough for SiS" on that page
- [[D-HNN]] — Sosanya & Greydanus 2022; the paper introducing the alternative route
- [[Helmholtz-Decomposition]] — math foundation of the alternative route
- [[Rayleigh-Dissipation-Function]] — classical-mechanics primitive that both routes generalize
- [[Hamiltonian-vs-Lagrangian-Duality]] § "A Fourth Axis: Dissipation Route" — the design-axis treatment of Helmholtz vs J-R
- [[CTPC-Design-Rationale]] Q8b-vector-field — closed by this architecture
- [[CBM-CTPC-Composition]] — Year 1 milestone; the channel-decomposed extension above is part of its open design decisions
- [[PhyArch]] — symmetry-hardwiring methodology; composes orthogonally with this dissipation architecture
- [[Latent-NCDE-Corrector]] — the CTPC backbone that this architecture's vector field plugs into

## Open Questions

- **Channel decomposition of `D` for per-concept counterfactuals.** Concrete extension flagged above. Implementation question for Year 1.
- **Position- and time-dependent `R(q, t)`.** Drag varies with altitude / atmospheric density / solar activity. The architecture allows `D(q)` directly; extending to `D(q, t)` requires the time channel as input. Worth empirical investigation.
- **Discrete-time passivity preservation.** Three integrator candidates flagged in § Integrator Caveat. Q8b-discrete-time follow-up.
- **Sparse / structured `D` for high-dimensional state.** Full-rank Cholesky has `O(d²)` parameters per state. Block-diagonal or low-rank-plus-diagonal `D` may suffice for orbital drag (which couples primarily through velocity-dependent terms). Open whether sparse `D` retains useful expressivity.
- **Activation choice for `M⁻¹` Cholesky and `V`.** Standard `tanh` is fine empirically. Whether equivariant or symplectic-by-construction parameterizations of these networks (interfacing with PhyArch) compose cleanly is open.
- **Composability with [[Lagrangian-Neural-Network|LNN]].** PHNN-proper assumes `(q, p)` canonical coordinates. For non-canonical observed data (TLE-derived `(r, v)` under perturbed Hamiltonian), the LNN-side has a Rayleigh-extended Euler-Lagrange analog. Bridging the two — using LNN's coordinate flexibility with PHNN-proper's J-R passivity — is conceptually clean but doesn't appear in any single paper.

## Sources

- `raw/papers/port-hamiltonian/DissipativeSymODEN-Zhong2020.pdf` — Zhong, Dey, Chakraborty 2020. §3.2 (Eq. 5: full architecture); §3.5 (Cholesky parameterization for both `D` and `M⁻¹`); §4 (experiments and Table 1 quantitative results showing parameter efficiency).
- `raw/books/dynamical-systems/Port_Hamiltonian_Book.pdf` — *not yet ingested.* Will substantially expand this page on next ingest, especially the Dirac-structure / constrained PH discussion relevant to spacecraft-attitude.
