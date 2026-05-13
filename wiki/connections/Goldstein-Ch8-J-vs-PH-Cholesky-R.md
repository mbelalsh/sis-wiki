---
title: Goldstein Ch 8 J-side vs Port-Hamiltonian Cholesky R-side
tags: [synthesis, book-query, hamiltonian-mechanics, port-hamiltonian, cholesky, dissipation, j-r-structure, ctpc]
sources:
  - raw/books/classical-mechanics/GoldsteinPooleSafkoClassicalMechanics.pdf
  - wiki/architectures/Port-Hamiltonian-Neural-Network.md
  - wiki/concepts/Port-Hamiltonian-Systems.md
  - wiki/papers/Dissipative-SymODEN.md
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: yes
---

# Goldstein Ch 8 J-side vs Port-Hamiltonian Cholesky R-side

## One-Line Intuition

Goldstein Ch 8 establishes the **J-side** of the port-Hamiltonian `(J − R)∇H`
structure *exactly* (Eq. 8.39: `η̇ = J ∂H/∂η`, with skew-symmetry proven at
Eq. 8.38d). The **R-side is outside Ch 8's scope** — Ch 8 treats only
monogenic forces. The Cholesky parameterization `R = L Lᵀ` used in CTPC's
PHNN satisfies the PH R-side requirement (`R = Rᵀ ⪰ 0`) by linear algebra
alone, with the canonical citation living in van der Schaft & Jeltsema Ch 2
+ Khalil Ch 6 — not in Goldstein.

## The Question

CTPC's PHNN dissipation block uses `R = L Lᵀ` (with `L` lower-triangular,
softplus-positive diagonal) to make `Ḣ = −∇Hᵀ R ∇H ≤ 0` a structural
property — a hard architectural constraint per the SiS design hierarchy in
CLAUDE.md. The book query: **does this parameterization satisfy the
port-Hamiltonian J-R structure**, and **how much of that consistency is
citable to Goldstein Ch 8**?

## What Goldstein Ch 8 Directly Establishes (the J-side)

Goldstein sets up Hamilton's equations in symplectic / matrix notation.
Define the stacked canonical state (Eq. 8.36, p. 342):

> "$\eta_i = q_i, \quad \eta_{i+n} = p_i; \quad i \leq n$" — **Eq. (8.36)**

with gradient components (Eq. 8.37):

> "$\left(\frac{\partial H}{\partial \boldsymbol{\eta}}\right)_i = \frac{\partial H}{\partial q_i}, \quad \left(\frac{\partial H}{\partial \boldsymbol{\eta}}\right)_{i+n} = \frac{\partial H}{\partial p_i}; \quad i \leq n$" — **Eq. (8.37)**

The **symplectic structure matrix** (Eq. 8.38a, p. 342):

$$\mathbf{J} = \begin{bmatrix} \mathbf{0} & \mathbf{1} \\ -\mathbf{1} & \mathbf{0} \end{bmatrix} \quad \textbf{Eq. (8.38a)}$$

where each block is $n \times n$. Goldstein then proves four properties
(Eqs. 8.38b–8.38f, p. 343):

$$\tilde{\mathbf{J}} = \begin{bmatrix} \mathbf{0} & -\mathbf{1} \\ \mathbf{1} & \mathbf{0} \end{bmatrix} \quad \textbf{Eq. (8.38b)}$$

$$\tilde{\mathbf{J}}\mathbf{J} = \mathbf{J}\tilde{\mathbf{J}} = \mathbf{1} \quad \textbf{Eq. (8.38c)}$$

$$\boxed{\tilde{\mathbf{J}} = -\mathbf{J} = \mathbf{J}^{-1}} \quad \textbf{Eq. (8.38d)}$$

$$\mathbf{J}^2 = -\mathbf{1} \quad \textbf{Eq. (8.38e)}$$

$$|\mathbf{J}| = +1 \quad \textbf{Eq. (8.38f)}$$

Equation (8.38d) is **exactly** the skew-symmetry condition
$\mathbf{J}^\top = -\mathbf{J}$ that port-Hamiltonian theory places on its
$\mathbf{J}$ matrix.

With this, Hamilton's equations compress to (Eq. 8.39, p. 343):

$$\boxed{\dot{\boldsymbol{\eta}} = \mathbf{J} \, \frac{\partial H}{\partial \boldsymbol{\eta}}} \quad \textbf{Eq. (8.39)}$$

For two coordinate variables Goldstein expands this explicitly (Eq. 8.40,
p. 343):

$$\begin{bmatrix} \dot{q}_1 \\ \dot{q}_2 \\ \dot{p}_1 \\ \dot{p}_2 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \\ -1 & 0 & 0 & 0 \\ 0 & -1 & 0 & 0 \end{bmatrix} \begin{bmatrix} -\dot{p}_1 \\ -\dot{p}_2 \\ \dot{q}_1 \\ \dot{q}_2 \end{bmatrix} \quad \textbf{Eq. (8.40)}$$

**This is exactly the $\mathbf{J} \nabla H$ term in the port-Hamiltonian form
$\dot{\mathbf{x}} = (\mathbf{J} - \mathbf{R}) \nabla H + \mathbf{g}(\mathbf{x})\mathbf{u}$
when $\mathbf{R} = \mathbf{0}$.** On the J-side, the structures coincide
exactly.

## What Goldstein Ch 8 Does NOT Establish (the R-side)

**Ch 8 contains no dissipation matrix.** The chapter opener on p. 334
explicitly restricts scope:

> "We shall assume in the following chapters that the mechanical systems
> are holonomic and that the forces are *monogenic*, that is, derived
> either from a potential dependent upon position only, or from
> velocity-dependent generalized potentials of the type discussed in
> Section 1.5."

The Rayleigh dissipation function lives in §1.5 of Ch 1, **not** in Ch 8.
There is no equation in Ch 8 of the form $\dot{\mathbf{x}} = (\mathbf{J} - \mathbf{R}) \nabla H$;
the closest is the conservation statement (Eq. 8.41, p. 344):

$$\frac{dH}{dt} = \frac{\partial H}{\partial t} = -\frac{\partial L}{\partial t} \quad \textbf{Eq. (8.41)}$$

which says $H$ is conserved when $L$ is not explicitly time-dependent —
i.e. precisely the **lossless** case. Port-Hamiltonian theory generalizes
this to $dH/dt = -\nabla H^\top \mathbf{R} \nabla H \leq 0$ (strict
dissipation when $\mathbf{R} \succ \mathbf{0}$), which is **outside Ch 8's
scope**.

So whether the Cholesky parameterization satisfies the *R-side requirement*
(`R = Rᵀ ⪰ 0`) cannot be read off Goldstein Ch 8 — Ch 8 doesn't discuss
$\mathbf{R}$ at all.

## The Cholesky Algebra (Independent of Goldstein)

Port-Hamiltonian theory ([[Port-Hamiltonian-Systems]], van der Schaft &
Jeltsema Ch 2 — not yet ingested) requires $\mathbf{R} = \mathbf{R}^\top \succeq \mathbf{0}$
so that:

$$\dot H = -\nabla H^\top \mathbf{R} \nabla H \leq 0$$

structurally. The CTPC PHNN parameterizes $\mathbf{R}$ via Cholesky:
$\mathbf{R} = \mathbf{L} \mathbf{L}^\top$ with $\mathbf{L}$ lower-triangular
and strictly positive diagonal (typically via softplus).

Two identities hold for *any* $\mathbf{L}$:

**Symmetry:**
$$\mathbf{R}^\top = (\mathbf{L} \mathbf{L}^\top)^\top = \mathbf{L} \mathbf{L}^\top = \mathbf{R} \quad \checkmark$$

**Positive semi-definiteness:** for any $\mathbf{v} \in \mathbb{R}^n$,
$$\mathbf{v}^\top \mathbf{R} \mathbf{v} = \mathbf{v}^\top \mathbf{L} \mathbf{L}^\top \mathbf{v} = \|\mathbf{L}^\top \mathbf{v}\|^2 \geq 0 \quad \checkmark$$

If $\mathbf{L}$ has strictly positive diagonal (softplus / exp), then
$\mathbf{L}$ is invertible and $\mathbf{R}$ is **positive definite** (not
just semidefinite), giving strict dissipation $\dot H < 0$ whenever
$\nabla H \neq \mathbf{0}$.

This is an **algebraic identity at every parameter setting** — not a
learned property. It satisfies the SiS hard-constraint hierarchy (CLAUDE.md
step 1) and inhabits the same logic as [[PINN-Challenges]]'s SiS-connection
table.

### Code Correspondence

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class CholeskyDissipation(nn.Module):
    """R = L L^T with L lower-triangular, softplus-positive diagonal.
    Guarantees R = R^T (symmetric) and R > 0 (positive definite)
    as algebraic identities at every parameter setting."""

    def __init__(self, dim):
        super().__init__()
        # Strict-lower-triangular learnable entries
        self.L_strict = nn.Parameter(torch.zeros(dim, dim))
        # Pre-softplus diagonal entries
        self.L_diag_raw = nn.Parameter(torch.zeros(dim))
        self.dim = dim

    def L(self):
        L = torch.tril(self.L_strict, diagonal=-1)              # strict lower
        L = L + torch.diag(F.softplus(self.L_diag_raw))         # + positive diag
        return L

    def R(self):
        L = self.L()
        return L @ L.T                                          # R = L L^T

# Verification at random init — algebraic, not learned
diss = CholeskyDissipation(dim=4)
R = diss.R().detach()
assert torch.allclose(R, R.T)                                  # R = R^T
eigs = torch.linalg.eigvalsh(R)
assert (eigs > 0).all()                                        # R > 0
```

## Verdict

| Side of $(\mathbf{J} - \mathbf{R})$ | Goldstein Ch 8 directly verifies? | Citation source |
|---|---|---|
| $\mathbf{J}^\top = -\mathbf{J}$ (skew-symmetry) | **YES** — Eq. (8.38d), with explicit construction in Eq. (8.38a) | Goldstein Ch 8 |
| $\mathbf{R} = \mathbf{R}^\top \succeq \mathbf{0}$ (PSD) | **NO** — Ch 8 is monogenic-only; dissipation deferred to §1.5 | van der Schaft & Jeltsema Ch 2 + Khalil Ch 6 (both not yet ingested) |
| Full PH form $\dot{\mathbf{x}} = (\mathbf{J} - \mathbf{R})\nabla H + \mathbf{g}(\mathbf{x})\mathbf{u}$ | **NO** — Ch 8 has no $\mathbf{R}$ and no input port $\mathbf{g}(\mathbf{x})\mathbf{u}$ | PH-book Ch 2 |

**Bottom line:** the Cholesky parameterization is correct (algebraic
identity guarantees $\mathbf{R} = \mathbf{R}^\top \succeq \mathbf{0}$ from
linear algebra alone) AND composes with Goldstein's $\mathbf{J}$
(Eq. 8.38a + 8.38d) cleanly. But **Goldstein Ch 8 is not the citation for
the R-side** — it's the citation for the J-side only. The R-side citation
lives in van der Schaft & Jeltsema Ch 2 (Priority 1 of the proposed first
ingest wave in [[book-routing-map]]) and Khalil Ch 6 (Priority 2). This is
one concrete reason those two chapters were ranked highest in the routing
map.

## Connection to SiS / CTPC

This synthesis confirms that the CTPC PHNN's Cholesky parameterization
satisfies the port-Hamiltonian R-side requirement at every parameter
setting — a hard architectural constraint per CLAUDE.md SiS design
hierarchy step 1. The J-side is grounded in Goldstein's canonical
symplectic Eq. (8.39). Together these close **half of D6 of
[[CTPC-Design-Rationale]]** (PHNN passivity at the vector-field level);
the remaining half (Q8b-discrete-time) is **OPEN** — integrator
preservation is a separate question requiring Goldstein Ch 9 (Liouville's
theorem §9.9 on phase-space-volume preservation) or Arnold Ch 8
(symplectic flow).

## Connections

- [[Port-Hamiltonian-Neural-Network]] — the SiS architecture using `R = L Lᵀ`
- [[Port-Hamiltonian-Systems]] — the canonical concept page
- [[Hamiltonian-Mechanics]] — the J-side substrate
- [[Dissipative-SymODEN]] — the J-R-route paper using this exact Cholesky parameterization
- [[Rayleigh-Dissipation-Function]] — Goldstein §1.5, the dissipation primitive Ch 8 defers to
- [[CTPC-Design-Rationale]] — D6 closure (vector-field passivity)
- [[book-routing-map]] — Goldstein Ch 8 is HIGH-priority for the Hamiltonian-NN agent

## Open Questions

1. **Q8b-discrete-time remains open.** Even with continuous-time
   $\mathbf{R} = \mathbf{L}\mathbf{L}^\top$ guaranteeing $\dot H \leq 0$,
   RK4 / dopri5 do **not** preserve discrete-time passivity. Goldstein
   Ch 8 doesn't address integrators — that's a separate question requiring
   Ch 9 (Liouville's theorem §9.9 on phase-space-volume preservation) or
   Arnold Ch 8 (symplectic flow). Candidate structure-preserving
   integrators: discrete gradient method (Gonzalez 1996), AVF
   (Quispel-McLaren 2008), Gauss-collocation.

2. **The input port $\mathbf{g}(\mathbf{x})\mathbf{u}$** in the full PH
   form is also outside Goldstein Ch 8. SiS architecturally banks this —
   see [[Port-Hamiltonian-Neural-Network]] commitment #7.

3. **Channel-decomposed dissipation $\mathbf{D}(q) = \sum_i \alpha_i \mathbf{D}_i(q)$
   with each $\mathbf{D}_i = \mathbf{L}_i \mathbf{L}_i^\top$** preserves
   the PSD guarantee (sum of PSD is PSD) and adds D-HNN-style
   $\alpha$-scaling counterfactual hooks for dissipation-side
   concept-closure. The algebraic verification of the sum's PSD property
   is also independent of Goldstein Ch 8.

## Sources

- **Goldstein, Poole, Safko, *Classical Mechanics 3rd ed*** —
  Ch 8 "The Hamilton Equations of Motion" (pp. 334–368), specifically
  Eqs. (8.36), (8.37), (8.38a)–(8.38f), (8.39), (8.40), (8.41). Chapter
  opener (p. 334) restricts Ch 8 scope to monogenic forces, with
  velocity-dependent dissipation deferred to §1.5.
- **van der Schaft & Jeltsema, *Port-Hamiltonian Systems Theory* Ch 2** —
  canonical $(\mathbf{J} - \mathbf{R})\nabla H + \mathbf{g}(\mathbf{x})\mathbf{u}$
  definition. **Cited via [[Port-Hamiltonian-Systems]] — book not yet ingested.**
- **Khalil, *Nonlinear Systems 3rd ed* Ch 6** — passivity (positive-real,
  L₂-Lyapunov). **Cited via wiki — book not yet ingested.**
- `wiki/architectures/Port-Hamiltonian-Neural-Network.md` — SiS Cholesky
  parameterization details, Integrator Caveat for Q8b-discrete-time.
- `wiki/papers/Dissipative-SymODEN.md` — Zhong et al. 2020 ICLR Workshop,
  the J-R-route paper this Cholesky design originates from.
