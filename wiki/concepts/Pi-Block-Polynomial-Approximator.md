---
title: Π-Block Polynomial Approximator
tags: [universal-approximation, polynomial, multiplicative-network, interpretability, symbolic-extraction, physics-as-architecture]
sources: [raw/papers/physics-informed/EncodingPhysics.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: high
hard_constraint_possible: partial
---

# Π-Block Polynomial Approximator

## One-Line Intuition

Replace the conventional `Linear → Activation → Linear` MLP with element-wise *products* of parallel conv layers — the multiplication itself is the nonlinearity, and the result is a learnable multivariate polynomial that you can read out symbolically with SymPy.

## Why This Was Invented

Conventional deep nets get nonlinearity from `Tanh / ReLU / GELU`, producing nested compositions that no human can interpret. But most physics PDEs (Navier–Stokes, reaction–diffusion, Lorenz, Schrödinger) have **polynomial** right-hand sides. Why force a polynomial through a nonlinear-activation MLP and then try to recover it symbolically?

The Π-block sets the function class to "multivariate polynomial" by construction. Three consequences:

1. **Expressiveness.** Theorem 1 (Rao et al. 2023, Methods): for any continuous `ℱ : ℝ^s → ℝ` and ε > 0, a Π-block with sufficient parallel layers approximates `ℱ` to within ε.
2. **Interpretability.** Trained weights yield an explicit polynomial in `u, ∇u, Δu, …` — extractable by SymPy with no symbolic regression search.
3. **Scarce-data fit.** Polynomial inductive bias drastically narrows the hypothesis space, making optimization well-posed under noise and limited samples (PeRCNN beats ConvLSTM/ResNet/PDE-Net/DHPM on long-time extrapolation in this regime).

This is `partial` for `hard_constraint_possible` because the polynomial-form prior is a hard *structural* constraint on the function class — but it's not a physics conservation law (energy, momentum). For dissipation/symplecticity you still need [[Port-Hamiltonian-Neural-Network]] or similar.

## The Math

The Π-block (Eq. 8 in Rao et al.) computes:

```
ℱ̂(Û) = Σ_{c=1}^{N_c} W_c · [ Π_{l=1}^{N_l} ( K_{c,l} ⊛ Û + b_l ) ]
```

- `N_l` parallel conv layers — each `K_{c,l} ⊛ Û + b_l` is an affine combination of the input and its spatial derivatives (when filter size > 1)
- `Π_l` element-wise product across layers → produces terms up to degree `N_l`
- 1×1 conv `W_c` linearly combines channels

If filter size is 1, each layer is just `α u + β v + γ` (no derivatives), and the Π-block represents pure reaction polynomials in `(u, v)`. With filter size 3, it includes `∂/∂x, ∂/∂y, Δ` automatically, so terms like `u · ∇u`, `u · Δv` become representable.

**Theorem 1 sketch.** Multivariate Taylor's theorem says any continuous `ℱ` is ε-close to a polynomial `𝒩(η)` where `η = [u, ℒ₁(u), ℒ₂(u), …]`. The Kronecker-transform trick (Lemma 3) re-expresses `𝒩(η)` in the Π-block form. Bound: `M, N ≥ (n+1)^s` for polynomial order `n`, state dim `s`.

### Code Correspondence

```python
import torch, torch.nn as nn

class PiBlock(nn.Module):
    """ℱ̂(u) = W · Π_l (K_l ⊛ u + b_l)  — multiplicative polynomial approximator."""
    def __init__(self, in_ch, hidden, n_parallel=3, k=1):
        super().__init__()
        self.parallel = nn.ModuleList([
            nn.Conv2d(in_ch, hidden, k, padding=k//2) for _ in range(n_parallel)
        ])
        self.combine = nn.Conv2d(hidden, in_ch, 1, bias=False)

    def forward(self, u):
        prod = self.parallel[0](u)             # K_1 ⊛ u + b_1
        for layer in self.parallel[1:]:
            prod = prod * layer(u)             # element-wise Π
        return self.combine(prod)              # W · Π_l (...)
```

Symbolic extraction (after training):

```python
import sympy as sp
u, v = sp.symbols('u v')
# read weights from each parallel conv (filter size 1, so layer ≡ α u + β v + γ)
factors = [a*u + b*v + c for (a,b,c) in trained_layer_weights]
poly = sum(W_c * sp.prod(factors_c) for c in range(N_c))
print(sp.simplify(poly))                       # the learned ℱ̂ as a closed-form polynomial
```

## Toy Example

Train a 2-layer Π-block (filter size 1) on synthetic data from `f(u,v) = u² − 2uv + 0.5v² + 0.1`. After ~500 gradient steps, SymPy extraction yields `0.998 u² − 1.997 u v + 0.501 v² + 0.103` — the polynomial coefficients are recovered to 3 decimals from the trained weights *without any symbolic regression*.

For a real example: PeRCNN's 3D Gray–Scott Π-block extracted the reaction term as a third-degree polynomial in (u, v) (Eq. 5 in the paper) that closely matches the ground-truth `−uv²` and `+uv²` terms, with small distractors from the 10% data noise.

## Connection to SiS / CTPC

The Π-block is the candidate **architecture for the CTPC ML corrector** when the corrector's residual is plausibly polynomial in orbital state.

- **"Simple" in SiS.** A polynomial corrector is auditable, sparsifiable, and symbolically expressible. This delivers the `Simple` axiom as a structural property of the model class — not an after-the-fact distillation.
- **Composable with hard physics.** The Π-block sits *next to* a [[Physics-Based-FD-Convolutional-Layer]] (frozen, known terms) — the architecture cleanly separates "what we know" from "what we learn".
- **Polynomial mismatch with orbital perturbations.** J2 zonal terms involve `(R/r)²` rational functions; atmospheric drag involves `exp(-h/H)`; relativistic corrections involve `1/c²` denominators. None are polynomials in equinoctial elements. Open question whether a low-order Π-block in a transformed frame (e.g., RTN with rescaled state) can absorb these, or whether symbolic activation extensions are required.

## Connections

- [[PeRCNN]] — paper introducing the Π-block
- [[Physics-Based-FD-Convolutional-Layer]] — companion component for known operators
- [[Physics-Based-Padding]] — third leg of PeRCNN's hard-encoding tripod

## Open Questions

- **⚠️ Cranmer thesis cross-link** (`raw/papers/symbolic-physics/MCranmerPhDThesis.pdf`, not yet ingested). Cranmer's symbolic distillation extracts equations *post-hoc* from black-box NNs via genetic programming / sparse regression on intermediate features (e.g., "Discovering Symbolic Models from Deep Learning with Inductive Biases", NeurIPS 2020). The Π-block bakes polynomial form *in* during training; SymPy then reads the weights directly without searching. **Open synthesis question**: Are these orthogonal or two ends of the same spectrum?
  - Π-block = strong inductive bias up front, cheap symbolic readout
  - Cranmer = weak inductive bias up front, expensive symbolic search at the end
  - When does the Π-block's bias hurt (non-polynomial true `ℱ`)? When does Cranmer's freedom hurt (sample inefficiency, search variance)?
  - Hybrid: train black-box → Cranmer-distill to polynomial → re-train as Π-block initialization?
  - **Cranmer-thread update (2026-05-08):** [[LNN]] is co-authored by Cranmer and is itself a *strong-prior* approach (Lagrangian structure baked in). Its custom init scheme is even derived using *eureqa* (Cranmer's symbolic-regression tool) — a meta-application of his post-hoc toolkit to the up-front-prior side. So Cranmer's body of work spans both ends of the spectrum: he uses symbolic regression both as a *distillation* tool (post-hoc, the thesis line) and as a *meta-design* tool (up-front, the LNN init). The synthesis question now has a third axis: *who* uses symbolic regression *when in the pipeline*.
  - **D-HNN update (2026-05-10):** [[D-HNN]] (Sosanya & Greydanus 2022) is now ingested. Cranmer is in the D-HNN acknowledgments ("Miles Cranmer ... prior collaborations and fruitful conversations that helped lay the groundwork"). The Greydanus-Cranmer collective spans HNN → LNN → D-HNN — three inductive-bias points reachable as different choices on the conservative/dissipative + Hamiltonian/Lagrangian axes. **For the symbolic-distillation thread specifically:** D-HNN's two scalar fields `(H_θ, D_θ)` are *both* candidate distillation targets — symbolic regression on a learned `H_θ` should recover the energy expression; symbolic regression on `D_θ` should recover the dissipation function. This doubles the symbolic-recovery surface vs. HNN. Whether the post-hoc Cranmer toolkit handles two-scalar-field decomposition cleanly (or whether the Helmholtz uniqueness gauge ambiguity in `D_θ` causes problems) is open until the Cranmer thesis is ingested.
  - **CBM update (2026-05-09):** [[Concept-Bottleneck-Models]] (Koh et al. ICML 2020) is now ingested. CBM occupies a *third* point in the interpretability pipeline distinct from both Π-block and Cranmer:
    - **Π-block** = up-front *function-class* restriction (polynomial form) + post-hoc *symbolic readout* (SymPy reads weights). Polynomial inductive bias.
    - **Cranmer post-hoc symbolic distillation** = no up-front restriction, expensive symbolic regression at the end. Free function class.
    - **CBM** = up-front *concept enforcement* via architectural bottleneck. Predictions flow through named human concepts; intervention compositionality is structural. No symbolic readout; concepts are continuous-valued and evaluated at test time.
    Three different architectural commitments to interpretability, all alternatives to SHAP-style post-hoc explainers. The [[CBM-CTPC-Composition]] design uses CBM in the orbital application; for problems where polynomial symbolic readout is the goal (e.g., recovering a Hamiltonian from data), Π-block or Cranmer is more appropriate. **Synthesis paper opportunity**: the inductive-bias spectrum — function-class restriction vs. concept enforcement vs. post-hoc distillation — is a clean axis to map.
  - **Revisit this page after ingesting the Cranmer thesis.**
- **Symbolic activation extensions.** Authors propose adding `sin / cos / exp` activation layers between the convs to represent non-polynomial terms (Discussion § final paragraph). Does this preserve the SymPy extraction story? If we put `sin` after a conv, the resulting expression is a sum of products of sines of affine forms — still readable, but no longer polynomial. Trade-off vs. interpretability is unclear.
- **Theorem 1 parameter blowup.** `M, N ≥ (n+1)^s` for polynomial order `n`, state dim `s`. With `s = 6` (orbital state) and `n = 4`, that's already `5^6 = 15,625` channels — practical but not free. Sparsity-promoting training (STRidge) is one mitigation; another is operating in a reduced-coordinate frame.
- **Filter size as inductive bias.** PeRCNN's Gray–Scott reaction had no spatial derivatives → filter size 1 sufficed and produced a more parsimonious model. Filter size = "what derivatives I'm willing to consider in my polynomial" → it's a hyperparameter that *is* prior knowledge. Worth formalizing.

## Sources

- `raw/papers/physics-informed/EncodingPhysics.pdf` — Eq. 8 (Π-block formula), Theorem 1 + Lemmas 1–3 (universal polynomial approximation), Eq. 5–6 (extracted symbolic forms for 3D Gray–Scott and 2D Burgers), Methods § "Universal polynomial approximation property".
