---
title: Concept Bottleneck Models (Koh et al. ICML 2020)
tags: [interpretability, concept-bottleneck, intervention, actionable-interpretability, concept-closure]
sources: [raw/papers/interpretability/ConceptBottleneckModels.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: high
hard_constraint_possible: yes
---

# Concept Bottleneck Models (Koh et al. ICML 2020)

> Koh, Nguyen, Tang, Mussmann, Pierson, Kim, Liang. *Concept Bottleneck Models*. ICML 2020. arXiv:2007.04612.

> **Operational relevance:** this is the mechanism cited by [[Actionable-Interpretability-Symmetries|Barbiero et al. 2026]] §2.3.1 as the architectural enforcer of *concept-closure invariance* (Symmetry 3). It also closes *information invariance* (Symmetry 2) when the bottleneck is small enough — the k-dim concept layer IS the compressed sufficient statistic Z. See [[CBM-CTPC-Composition]] for the concrete plan to apply CBM to the Latent NCDE Corrector, closing both symmetries with one architectural mechanism.

## One-Line Intuition

Architecturally split a model into `f ∘ g` where `g : x → ĉ` predicts human-specified concepts and `f : ĉ → ŷ` predicts the target — with **no side channel from `x` to `y`**. The prediction is by construction a function of the predicted concepts only, so editing concept values at test time and propagating through `f` produces a counterfactual prediction the model is forced to respect. This is *concept closure by architecture*, not by post-hoc fitting.

## Why This Was Invented

The interpretability literature pre-CBM had two main camps:

- **Post-hoc explainers** (TCAV, Network Dissection, SHAP, LIME) — train a black-box model, then *fit* an explanation to it. Problem: the explanation may not match what the model actually does internally; you can't intervene on the explanation to change the prediction in any guaranteed way.
- **Self-interpretable models** (decision lists, GAMs, Rudin-style sparse models) — interpretable by construction, but accuracy ceiling is often lower and they don't naturally support *concept-level* manipulation.

Earlier concept-bottleneck-style models (Kumar 2009, Lampert 2009) existed but were beaten on accuracy by end-to-end NNs, leading to a perceived accuracy↔interpretability tradeoff. **The KOH paper's contribution is showing that this tradeoff largely vanishes when you use modern NN backbones**: train a deep net, but resize one layer to `k` units (one per concept) and add a concept-prediction loss. Task accuracy stays competitive; you gain concept interventions for free.

The paper validates this on knee X-ray grading (OAI dataset) and bird species identification (CUB) — both domains where practitioners reason in terms of concepts ("bone spurs," "wing color") that should support intervention queries.

## The Math

### Setup

Training data: `(x⁽ⁱ⁾, c⁽ⁱ⁾, y⁽ⁱ⁾)` triples — input `x ∈ ℝᵈ`, concept vector `c ∈ ℝᵏ` (or `{0,1}ᵏ` for binary concepts), target `y`. Bottleneck model:

```
ĉ = g(x)        g : ℝᵈ → ℝᵏ            (concept predictor)
ŷ = f(ĉ)        f : ℝᵏ → ℝ             (target predictor)
```

The architectural commitment: **`f` takes only `ĉ` as input. There is no side channel from `x` to `y`.** This is what makes interventions valid.

### Three Training Schemes

| Scheme | `g` trained on | `f` trained on | Intervention behavior |
|---|---|---|---|
| **Independent** | `(x, c)` | `(c, y)` — true `c` | **Best** — `f` was trained on true concept distribution, so replacing `ĉ ← c_true` puts inputs exactly in-distribution |
| **Sequential** | `(x, c)` first, then frozen | `(ĝ(x), y)` — predicted `ĉ` | Intermediate — `f` adapts to the predicted-concept distribution but loses the in-distribution intervention property |
| **Joint** | jointly minimize `L_Y(f(g(x)), y) + λ · L_C(g(x), c)` | same | Weakest intervention if `λ` too small (concepts can drift away from human meaning); best pre-intervention task accuracy |

**Joint training with low `λ` can produce a model where intervention HURTS task accuracy** — the "control" model in Figure 4-Left. This is the most important caveat: high task accuracy + high concept accuracy do *not* guarantee good intervention behavior. Training scheme is a third independent axis.

### Intervention Mechanism

At test time, given `x`:

1. Compute `ĉ = g(x)` (predicted concepts)
2. For concepts the operator wants to test, replace `ĉⱼ ← c_specified` (true value, modified value, or counterfactual)
3. Propagate: `ŷ_intervened = f(ĉ_modified)`
4. Compare with `ŷ = f(ĉ)` — the difference is the counterfactual response to the concept edit

For binary classification with logits, intervention requires picking a logit value; the paper sets logits to the 5th/95th percentile of training distribution depending on whether the true concept is 0 or 1. This works but is awkward — see "Linear vs nonlinear `c → y`" below for a related issue.

### Code Correspondence

```python
import torch.nn as nn

class CBMIndependent(nn.Module):
    """Independent CBM: g and f trained separately."""
    def __init__(self, backbone, n_concepts, n_outputs):
        super().__init__()
        self.g = nn.Sequential(backbone, nn.Linear(backbone.out_dim, n_concepts))
        self.f = nn.Sequential(
            nn.Linear(n_concepts, 64), nn.ReLU(),
            nn.Linear(64, 64), nn.ReLU(),
            nn.Linear(64, n_outputs),
        )

    def forward(self, x):
        c_hat = self.g(x)
        y_hat = self.f(c_hat)
        return c_hat, y_hat                    # both used during training

    def intervene(self, x, intervention_dict):
        """intervention_dict: {concept_idx: new_value}."""
        c_hat = self.g(x)
        for idx, val in intervention_dict.items():
            c_hat[..., idx] = val              # substitute
        return self.f(c_hat)                   # propagate

# Independent training: train g on (x, c), then train f on (c, y) using TRUE c
# This is what gives independent CBM its strong intervention behavior
```

## Toy Example — and the Empirical Findings

### OAI (knee X-ray grading)

`k=10` clinical concepts (osteophytes, sclerosis, joint-space narrowing, etc.) → 4-level KLG severity score.

| Model | Task RMSE | Concept RMSE |
|---|---|---|
| Independent | 0.435 | 0.529 |
| Sequential | 0.418 | 0.527 |
| Joint | 0.418 | 0.543 |
| Standard end-to-end | 0.441 | (not modeled) |
| Linear probe on standard model | — | **0.680** |

Two takeaways: (1) bottleneck models match or beat the end-to-end model on task accuracy; (2) **post-hoc linear probes on the end-to-end model recover concepts much worse than the bottleneck does** (0.68 RMSE vs 0.53). You cannot just train a black-box and read concepts out — you have to train *for* concepts.

### CUB (bird species ID)

`k=112` binary attributes (wing color, beak shape, etc.) → 200-way species classification.

Bottleneck models attain task error 0.20–0.24 vs. standard 0.175. Concept error 0.03 vs. linear probe 0.09.

### Test-time intervention

Querying just **2 concepts** (out of 10) on OAI reduces task RMSE from >0.4 to ≈0.3. With all 10 concepts substituted, independent CBM gets to ~0.16. **Independent > sequential ≈ joint** in intervention performance — consistent with the in-distribution argument above.

### Robustness to spurious correlations (TravelingBirds, Section 7)

CBM trained on CUB with bird-class-specific backgrounds, tested on shuffled backgrounds:

| Model | Task error |
|---|---|
| Standard end-to-end | 0.627 |
| Independent CBM | 0.482 |
| Joint CBM | 0.482 |
| Sequential CBM | 0.496 |

**CBMs cut task error by ~25%** on the shifted distribution because the per-concept features (e.g., wing color) appear across multiple classes, breaking the spurious background-label correlation. **This is a generalization story, not just an interpretability story.**

### Theoretical bound (Appendix C)

For well-specified linear regression, asymptotic relative excess error of independent CBM vs. standard model:

```
lim_{n→∞} excess_error(CBM) / excess_error(standard) ≤ (k/d) · (σ²_Y + σ²_C) / (σ²_Y + σ²_C)
```

CBM has lower excess error when **`k/d` is small** (few concepts relative to input dimension) AND **`σ²_C ≪ σ²_Y`** (concepts are less noisy than the target). For CTPC orbital application, both are true: `k ~ 9` concepts, `d` = state-history dimensionality (much larger), and well-defined physics concepts (J2, drag, SRP) have lower noise than the total residual error.

## Connection to SiS / CTPC

CBM is the **architectural mechanism for verifying TWO of the four [[Actionable-Interpretability-Symmetries|Barbiero]] symmetries** in PhyArch-CTPC — concept-closure invariance (Symmetry 3, paper §2.3.1) AND information invariance (Symmetry 2, because the k-dim bottleneck IS the compressed sufficient statistic Z). One architectural mechanism, two symmetry guarantees.

The composition is concrete: insert a CBM layer between the Latent NCDE decoder hidden state `z_d(t)` and the prediction head, with named orbital-physics concepts (J2, drag, SRP, third-body, RTN-frame components). See [[CBM-CTPC-Composition]] for the full design.

Three properties transfer cleanly to the orbital application:

1. **Concept closure by construction.** Predictions flow through named physics concepts; counterfactuals like "what if drag were zero?" are well-defined operations on the model.
2. **Robustness to spurious correlations.** Each physics concept (J2, drag, SRP) appears across all spacecraft in the training set, breaking per-spacecraft training-distribution quirks.
3. **Theoretical excess-error bound favors CBM** for the orbital regime (small `k`, low concept noise from analytic perturbation models).

The mechanism complements rather than competes with [[PhyArch]]'s symmetry hardwiring:
- **PhyArch** enforces *inference equivariance* + *structural invariance* (hard architectural symmetries on the dynamics).
- **CBM** enforces *concept-closure invariance* (verifiable counterfactual semantics on the residual).
- Together they hit 3 of 4 Barbiero symmetries with concrete verification protocols. Information invariance for the full predictive distribution is the remaining gap (Year 2 milestone — analytic covariance propagation).

## Connections

- [[CBM-CTPC-Composition]] — concrete design plan for applying CBM to the Latent NCDE Corrector with orbital-physics concepts (Year 1 milestone of the PhyArch-CTPC roadmap)
- [[CTPC-Design-Rationale]] Part III — where the concept-closure-invariance gap is identified
- [[Latent-NCDE-Corrector]] — the architecture into which CBM gets inserted
- [[PhyArch]] — orthogonal interpretability mechanism (symmetry hardwiring); CBM provides the verifiable-concept-closure piece PhyArch alone doesn't
- [[Pi-Block-Polynomial-Approximator]] — alternative interpretability mechanism: Π-block does *post-hoc* symbolic readout from a polynomial-form-restricted network; CBM does *up-front* concept enforcement via architectural bottleneck. Different points in the interpretability pipeline; both alternatives to SHAP-style explainers.

## Open Questions

- **Linear vs nonlinear `c → y` is unexplained.** Paper finds linear `f` performs *worse* under intervention than a nonlinear MLP `f`, with comparable pre-intervention accuracy. Counterintuitive — linear seems "more interpretable" — but operationally it's the wrong choice. For CBM-CTPC, this implies the post-bottleneck head should be a small MLP, not a linear layer.
- **Intervention with classification (logit setting).** The paper's 5th/95th percentile heuristic for binary logit intervention is awkward. For regression-style outputs (CBM-CTPC's continuous orbital error correction), this issue is sidestepped, but for binary classification CBMs it remains an open design question.
- **Concept-set design is hand-engineered.** Paper assumes the concept set `{c_1, ..., c_k}` is given by domain experts. For OAI/CUB this is well-posed; for orbital it's also reasonable (J2, drag, SRP are standard). But for novel domains, *learning* the concept set rather than specifying it is open work (Cheng & Bernstein 2015, Duan 2012 cited as related).
- **Side channels.** Paper notes that adding a direct `x → y` connection (using `c` as auxiliary features) breaks intervention. For multi-stage architectures like CTPC's encoder-decoder, the analogous question is whether the latent-state path constitutes a side channel. Empirical verification needed.
- **Concept whitening alternative** (Chen, Bei, Rudin 2020 — cited by the paper). Replaces the bottleneck with a whitening layer that decorrelates concept axes. Different trade-offs; worth ablating against CBM in CBM-CTPC.

## Sources

- `raw/papers/interpretability/ConceptBottleneckModels.pdf` — Koh, Nguyen, Tang, Mussmann, Pierson, Kim, Liang 2020 (ICML). Sec. 3 (setup + three training schemes), Sec. 4 (OAI/CUB benchmarks), Sec. 5 (linear-probe comparison vs post-hoc), Sec. 6 (test-time intervention experiments), Sec. 7 (TravelingBirds robustness experiment), Appendix A–B (dataset + training details), Appendix C (excess-error bound for independent CBM vs standard model in linear regression).
- Code: <https://github.com/yewsiang/ConceptBottleneck>
