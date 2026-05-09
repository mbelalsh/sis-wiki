---
title: PeRCNN — Physics-Encoded Recurrent CNN
tags: [physics-encoded-learning, pde-learning, reaction-diffusion, polynomial-nn, equation-discovery, hard-constraint]
sources: [raw/papers/port-hamiltonian/EncodingPhysics.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: high
hard_constraint_possible: yes
---

# PeRCNN — Physics-Encoded Recurrent CNN

> Rao, Ren, Wang, Buyukozturk, Sun, Liu. *Encoding physics to learn reaction–diffusion processes*. Nature Machine Intelligence 5, 765–779 (2023).

## One-Line Intuition

Don't penalize physics violations in the loss — make the architecture *incapable* of violating them: freeze finite-difference convolutional kernels for known PDE terms, use a polynomial Π-block for unknown terms, and pad the conv input to enforce boundary conditions exactly.

## Why This Was Invented

PINNs leverage prior physics knowledge but as a **soft penalty**: the loss is `MSE(data) + λ · MSE(PDE residual)`. Three resulting pains:

1. λ tuning is trial-and-error; the optimization is ill-posed without enough labelled data
2. PINNs use FCNNs as continuous solution approximators — bad for high-resolution spatiotemporal fields
3. Cannot hard-encode an *incomplete* PDE — you need the full residual to penalize

PeRCNN replaces the soft-penalty paradigm with **physics-as-architecture** for PDE systems: the network's structure literally implements the known parts of the PDE. Unknown parts are isolated to a Π-block whose polynomial form keeps the result interpretable.

## The Math

The state update follows a forward Euler scheme:

```
û^(k+1) = û^(k) + ℱ̂(û^(k); θ) · δt
```

where `ℱ̂` is parameterized by the network. The Π-block (Eq. 8) approximates `ℱ`:

```
ℱ̂(Û^(k)) = Σ_c W_c · [ Π_l ( K_{c,l} ⊛ Û^(k) + b_l ) ]
```

`N_l` parallel conv layers feed an element-wise product → 1×1 conv combines channels. **Theorem 1** (paper, Methods): for any continuous `ℱ : ℝ^s → ℝ`, sufficient `M, N` make the Π-block ε-close. With FD-stencil convolutional filters (Lemma 1), the differential operators inside `ℱ` come along for free.

When a term is *known* (e.g. `Δu`), it bypasses the Π-block via a [[Physics-Based Finite-Difference Convolutional Layer]] short-cut connection — frozen stencil, scalar coefficient learnable. Boundary conditions are enforced by [[Physics-Based Padding for Boundary Conditions]] applied to every recurrent input.

### Code Correspondence

```python
import torch, torch.nn as nn

class PeRCNNStep(nn.Module):
    """Single forward-Euler step: û^{k+1} = û^k + ℱ̂(û^k) · δt"""
    def __init__(self, in_ch, hidden, n_parallel=3, k=1, dt=0.5):
        super().__init__()
        self.dt = dt
        self.parallel = nn.ModuleList([
            nn.Conv2d(in_ch, hidden, k, padding=k//2) for _ in range(n_parallel)
        ])
        self.combine = nn.Conv2d(hidden, in_ch, 1)   # 1x1 mixing
        self.diffusion = make_frozen_laplacian(in_ch)  # known physics short-cut
        self.mu = nn.Parameter(torch.tensor([1e-5]))   # learnable diff coeff

    def forward(self, u):
        prod = self.parallel[0](u)
        for layer in self.parallel[1:]:
            prod = prod * layer(u)            # element-wise product = nonlinearity
        f_hat = self.combine(prod) + self.mu * self.diffusion(u)
        return u + f_hat * self.dt
```

Reference implementation: <https://github.com/isds-neu/PeRCNN>.

## Toy Example

The 2D Gray–Scott reaction–diffusion system (Eq. 4):
```
u_t = μ_u Δu − u v² + F(1−u)
v_t = μ_v Δv + u v² − (F+κ) v
```
PeRCNN is given the diffusion term as known (a frozen Laplacian conv) and learns the reaction `−uv², F(1−u)` via the Π-block. With 41 noisy 26×26 snapshots over `t ∈ [0, 400 s]`, it accurately extrapolates 1700 steps beyond the training horizon and generalizes to unseen ICs (Fig. 4a, Fig. 5a) — where ConvLSTM, ResNet, PDE-Net, and DHPM all fail past the training window.

## Connection to SiS / CTPC

PeRCNN is a clean worked example of the SiS design hierarchy on PDE systems:

| SiS layer | PeRCNN realization |
|-----------|-------------------|
| Hard constraint → architecture | Frozen FD-stencil conv for known PDE terms; physics-based padding for BCs |
| Soft constraint → loss | **None** — data-driven mode has zero physics-loss term |
| Learned residual → ML corrector | Π-block (polynomial) for unknown reaction/source terms |

This maps to CTPC structurally:

- **SGP4 ≈ frozen FD layer** — fixed predictor encoding conservation laws / known dynamics
- **ML corrector in RTN frame ≈ Π-block** — learns the residual from data
- **Symbolic interpretability** — SymPy reads polynomial form out of trained Π-block weights (Eqs. 5–6); this delivers the "I" (Interpretable) in SiS as a *structural* property, not post-hoc explanation

PeRCNN does **not** enforce dissipation `Ḣ ≤ 0` — that's an energy/Lyapunov constraint, orthogonal to polynomial form. Port-Hamiltonian Neural Networks would supplement this. PeRCNN + PH composition is an open architectural question.

## Connections

- [[Physics-Based Finite-Difference Convolutional Layer]] — the frozen-stencil short-cut connection that hard-encodes known PDE terms
- [[Π-Block Polynomial Approximator]] — the multiplicative core that approximates unknown `ℱ`
- [[Physics-Based Padding for Boundary Conditions]] — encoding Dirichlet/Neumann/Robin via padding instead of loss penalty

## Open Questions

- **Polynomial-only restriction.** `ℱ̂` is a multivariate polynomial. Orbital perturbations involve trig (J2 zonal terms), exp (atmospheric density), and rational functions (drag scaling). Authors propose symbolic-activation extensions (sin/cos/exp layers) but don't validate. Does this break the SymPy extraction story?
- **No dissipation enforcement.** PeRCNN's polynomial form is structural, not energetic. Pairing with Port-Hamiltonian Neural Networks (HNN/LNN/PHNN) to compose polynomial-form + dissipative-form is an open architectural question.
- **Cartesian-only.** Standard convolutions assume regular grids. Atmospheric drag fields on irregular Earth-fixed grids would need graph convolutions (paper notes this as future work).
- **Theorem 1 parameter blowup.** `M, N ≥ (n+1)^s` where `s` is the state dimension and `n` the polynomial order. Practical for low-dim state, problematic for high-dim residual fields.
- **Forward vs. data-driven brand.** Two distinct training regimes share the "PeRCNN" name: (a) forward-solver mode with physics-residual loss and FD-customized layers; (b) data-driven mode with no physics loss and a Π-block. Worth disambiguating when citing.

## Sources

- `raw/papers/port-hamiltonian/EncodingPhysics.pdf` — Rao et al., Nat. Mach. Intell. 5, 765–779 (2023). Equations (1)–(8), Theorem 1, Lemmas 1–3, Figs. 1–6, Extended Data Tables 1–3.
- Code: <https://github.com/isds-neu/PeRCNN>
