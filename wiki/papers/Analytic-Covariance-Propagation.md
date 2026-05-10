---
title: Analytic Covariance Propagation in Neural Networks (Wright et al. AISTATS 2024)
tags: [uncertainty-quantification, moment-propagation, bayesian-neural-network, analytic-covariance, information-invariance]
sources: [raw/papers/uncertainty-propagation/AnalyticUncertaintyProp.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: high
hard_constraint_possible: yes
---

# Analytic Covariance Propagation in Neural Networks (Wright et al. AISTATS 2024)

> Wright, Nakahira, Moura. *An Analytic Solution to Covariance Propagation in Neural Networks*. AISTATS 2024. arXiv:2403.16163.

> **Operational relevance:** this is the technical machinery for verifying *information invariance* on the *full predictive distribution* — the remaining Barbiero symmetry currently flagged as "partial (mean only)" in [[CTPC-Design-Rationale]] Part III. See [[Analytic-Sigma-CTPC-Composition]] for the concrete Year 2 milestone applying it to the Latent NCDE Corrector with TLE uncertainty.

## One-Line Intuition

Sample-free moment propagation through neural networks: layer-by-layer propagation of **mean vectors AND full covariance matrices** through arbitrary architectures, with a closed-form analytic expression for the covariance of element-wise nonlinear activations of Gaussian inputs. The covariance is a power series in the Pearson correlation coefficient, with each term involving k-th derivatives of `E[g(y)]`. Generalizes prior approximate methods (Wu et al. DVI, Bibi et al. PL-DNN) to arbitrary precision and arbitrary activation function.

## Why This Was Invented

Uncertainty quantification through neural networks has three main approaches:

- **Sampling-based** (MCVI, dropout-as-Bayesian, deep ensembles) — Monte Carlo over weights or activations. `O(s · n²)` per layer. Cost grows linearly in sample count `s`; with finite `s`, sampling noise contaminates the variance estimate.
- **Variance-only propagation** [Gast & Roth 2018, Loquercio et al. 2020] — propagate the diagonal of the covariance matrix only. Loses inter-dimension correlation; the resulting predictions can't represent oriented uncertainty ellipsoids.
- **Approximate full-covariance propagation** [Wu et al. 2019 DVI, Bibi et al. 2018 PL-DNN] — propagate the full covariance matrix but use *activation-specific approximations* (closed-form-only for zero-mean Gaussian inputs in PL-DNN; ReLU/Heaviside-only approximations in DVI). Loses precision and generality.

**Wright's contribution: closed-form covariance for arbitrary activation functions, to arbitrary precision.** The technique is sample-free (cheap per inference), full-covariance (preserves orientation), exact in the limit (no approximation error if Taylor series is taken to infinity), and activation-agnostic (works for Heaviside, ReLU, GELU, sigmoid, and any activation with closed-form mean and derivatives).

For **safety-critical applications** (the paper's stated motivation, citing autonomous vehicles, industrial robots, medicine — the same SDA-relevant safety frame as [[CTPC-KDD-Submission]] and [[PhyArch-Double-Pendulum-Benchmark]]) this matters because point estimates without rigorous uncertainty quantification fail under input perturbations and adversarial attacks. Calibrated, sample-free, full-covariance UQ is the structural property safety-critical deployment needs.

## The Math

### Theorem 1 — analytic activation covariance

Suppose `y ∈ ℝⁿ` is multivariate Gaussian, `y ~ N(μ, Σ)`, and `z = g(y)` is element-wise independent and identical (`g(y) = [g(y_1), …, g(y_n)]^T` with scalar `g`). With Pearson correlation `ρ_ij = E[(y_i − μ_i)(y_j − μ_j)] / (σ_i σ_j)`:

```
Cov(z_i, z_j) = Σ_{k=1}^∞ (ρ_ij^k / k!) · (σ_i^k · ∂^k E[z_i]/∂μ_i^k) · (σ_j^k · ∂^k E[z_j]/∂μ_j^k)
```

**Variance corollary** (set `i = j`, `ρ_ii = 1`):

```
Var(z) = Σ_{k=1}^∞ (1/k!) · (σ^k · ∂^k E[z]/∂μ^k)²
```

The variance is a sum of strictly positive squared terms — *monotonically additive* in Taylor order, so no oscillatory cancellation between terms. Numerically stable.

**Truncation accuracy.** Truncating at order K gives a controllable approximation. For ReLU at K=4, max absolute error is `3.74 × 10^-4` over `μ_1, μ_2 ∈ [-5, 5]` with `σ_1 = σ_2 = 1, ρ = 0.5`. For comparison, Wu et al. 2019's prior approximation has max error `1.16 × 10^-1` (~300× worse). K=2 is usually sufficient; K=4 is essentially exact for practical purposes.

**Runtime.** `O(K · n²)` per layer (covariance matrix is `n × n`).

**Proof** (App. A) is a clean Fourier-analysis derivation: treat the activation-covariance integral as a 2D convolution in `(μ_i, μ_j)`, transform to Fourier space where convolutions become products and the bivariate Gaussian density has form `exp{-2π²[ξ_i² σ_i² + 2ρ ξ_i ξ_j σ_i σ_j + ξ_j² σ_j²]}`, expand the cross-term `exp{-4π²ρξ_iξ_jσ_iσ_j} - 1` as a Taylor series in ρ, apply the Fourier differentiation property (`ξ^k G(ξ) ↔ ∂^k g/∂μ^k`), inverse-transform.

### Closed-form derivatives for common activations

The k-th derivatives `∂^k E[z]/∂μ^k` have clean closed forms via Hermite polynomials. Let `φ` be the standard normal PDF, `Φ` the CDF, and `He_k` the probabilist's Hermite polynomial of degree k.

**Heaviside** (App. B.1):
```
σ^k · ∂^k E[z]/∂μ^k = (-1)^(k-1) · He_{k-1}(μ/σ) · φ(μ/σ)
```

**ReLU** (App. B.2): for `k > 1`,
```
σ^k · ∂^k E[z]/∂μ^k = σ · (-1)^k · He_{k-2}(μ/σ) · φ(μ/σ)
```

**GELU** (App. B.3): the *mean* doesn't appear in prior literature — Wright derives it:
```
E[GELU(y)] = μ Φ(μ/√(1+σ²)) + (σ²/√(1+σ²)) · φ(μ/√(1+σ²))
```
Letting `α = σ/√(1+σ²)`, the k-th derivative for `k > 1`:
```
σ^k · ∂^k E[z]/∂μ^k = α^(k-1) σ (-1)^k [He_{k-2}(αμ/σ) - (1-α²) He_k(αμ/σ)] φ(αμ/σ)
```
As `σ → 0`, `α → 1`, the GELU formula reduces to the ReLU formula — consistent limiting behavior.

**Sigmoid** (App. B.4): no closed-form mean, but [Daunizeau 2017]'s approximation `E[s(y)] ≈ s(μ/√(1+ασ²))` with `α = 0.368` (or `α = π/8` for some ranges) admits closed-form derivatives. Approximate, not exact.

### Algorithm 1 — layer-by-layer propagation

```
Input: input mean μ_x, covariance Σ_x
For each layer ℓ:
    Affine:     μ_y = W μ_x + b              (exact, Eq. 3)
                Σ_y = W Σ_x W^T              (exact, Eq. 4)
    Activation: μ_z, Σ_z from y ~ N(μ_y, Σ_y)
                  μ_z: closed-form for activation (Eq. 5–7 for Heaviside/ReLU/GELU)
                  Σ_z: Theorem 1
    (μ_x, Σ_x) ← (μ_z, Σ_z)
Return output (μ, Σ)
```

The Gaussian assumption on activation inputs is the standard moment-propagation assumption (CLT-justified for high-dimensional inputs; also used by Wu et al. 2019, Bibi et al. 2018, Hernández-Lobato & Adams 2015, Gast & Roth 2018). Wright observes that empirical results justify the assumption in practice; the authors leave a more thorough theory of when Gaussian approximation is sufficient as future work (Sec. 5).

**Specialized layers.** Pooling, batch normalization, and softmax don't have closed-form moment propagation. Two routes:
- **Refactoring** [Shriver 2022]: such functions can be re-expressed as compositions of linear + ReLU operations (which Wright's framework covers).
- **Replacement** [Springenberg et al. 2015]: pooling layers can be replaced by strided convolutions without loss of inference accuracy.

### Code Correspondence

Reference implementation: <https://github.com/omwright/cov-prop-nn>. Sketch:

```python
import torch
from scipy.special import hermite_e         # or use a Hermite polynomial library

def relu_kth_derivative(mu, sigma, k):
    """k-th derivative of E[ReLU(y)] w.r.t. mu, multiplied by sigma^k."""
    standardized = mu / sigma
    if k == 0:
        return mu * Phi(standardized) + sigma * phi(standardized)
    elif k == 1:
        return sigma * Phi(standardized)
    else:
        return sigma * (-1)**k * hermite_e(k-2)(standardized) * phi(standardized)

def activation_covariance(mu, Sigma, activation_kth_derivative, K=4):
    """Theorem 1: covariance of element-wise activation given Gaussian input."""
    n = len(mu)
    sigma = torch.sqrt(torch.diag(Sigma))
    # Pearson correlation matrix
    rho = Sigma / (sigma.unsqueeze(0) * sigma.unsqueeze(1))
    # Per-element kth derivatives, shape (K, n)
    derivs = torch.stack([
        torch.tensor([activation_kth_derivative(mu[i], sigma[i], k) for i in range(n)])
        for k in range(1, K+1)
    ])
    # Theorem 1 sum
    cov = torch.zeros((n, n))
    for k in range(1, K+1):
        cov += (rho**k / factorial(k)) * (derivs[k-1].unsqueeze(0) * derivs[k-1].unsqueeze(1))
    return cov

def propagate_layer(mu_x, Sigma_x, W, b, activation_fn):
    """One layer of Algorithm 1: affine + activation."""
    mu_y = W @ mu_x + b
    Sigma_y = W @ Sigma_x @ W.T
    mu_z = activation_mean(mu_y, torch.diag(Sigma_y), activation_fn)
    Sigma_z = activation_covariance(mu_y, Sigma_y, activation_fn.kth_derivative, K=4)
    return mu_z, Sigma_z
```

## Toy Example — Bivariate ReLU Covariance

The paper's Figure 2 plots `Cov(z_1, z_2)` for `(y_1, y_2) ~ N(0, Σ)` with `σ_1 = σ_2 = 1, ρ = 0.5` over `μ_1, μ_2 ∈ [-5, 5]`. The numerical-integration ground truth has a smooth "saddle"-like structure peaking near `(0, 0)`.

| Approximation | Max abs error |
|---|---|
| Wu et al. 2019 (PL-DNN/DVI) | 1.160 × 10⁻¹ |
| Theorem 1 truncated at K=1 | 2.034 × 10⁻² (~6× better) |
| Theorem 1 truncated at K=4 | 3.740 × 10⁻⁴ (~300× better) |

K=4 essentially eliminates the error.

## Empirical Results

### Trained-network analysis (Sec. 4.2, Tables 1–2)

Synthetic FC and CNN networks, depths 4 and 8, ReLU activations. Tightness measure: `ratio of MC estimate to analytic estimate` should be `1 ± 0`.

For both fully-connected and convolutional networks, Wright achieves `1.000 ± 0.001` to `1.000 ± 0.021` for both mean and variance — essentially perfect tracking of MC ground truth. PL-DNN baseline: `0.997 ± 0.643` for FC-4, `1.222 ± 0.240` for CNN-4 (large standard deviations indicate sensitivity to input distribution).

MNIST-trained CNN (Table 2): same story per output logit. Wright `1.000 ± 0.000` to `1.001 ± 0.015`; PL-DNN `0.971 ± 0.185` to `1.068 ± 0.684`.

### BNN training (Sec. 4.3, Table 3)

UCI regression datasets, comparing exact-covariance DVI (Wright) vs. approximate DVI (Wu et al. 2019) vs. baseline MCVI. **Mixed results.**

| Dataset | Wright | DVI (Wu) | MCVI |
|---|---|---|---|
| boston | **−2.31 ± 0.19** | −2.33 ± 0.19 | −2.40 ± 0.23 |
| concrete | **−2.98 ± 0.11** | −2.99 ± 0.11 | −3.04 ± 0.10 |
| energy | **−1.27 ± 0.21** | −1.30 ± 0.22 | −1.40 ± 0.18 |
| kin8nm | 1.11 ± 0.03 | 1.11 ± 0.03 | **1.20 ± 0.03** |
| naval | 5.81 ± 0.16 | 5.73 ± 0.22 | **5.92 ± 0.24** |
| protein | **−2.97 ± 0.01** | −2.97 ± 0.02 | −3.05 ± 0.02 |

Higher test log-likelihood is better. Wright wins on 6 of 9 datasets but loses on `kin8nm` and `naval`. **Honest negative reporting**: the authors conjecture that when the variational posterior approximation (factorized Gaussian) is the bottleneck, exact covariance doesn't help. **Important caveat for the orbital application: exact covariance isn't always the limiting factor.**

## Connection to SiS / CTPC

Wright's analytic propagation is the **technical machinery for closing the information-invariance gap** in [[CTPC-Design-Rationale]] Part III — the symmetry currently flagged as "partial (mean only)."

For [[CTPC-KDD-Submission]] specifically, the application has three pieces:

1. **Replace the K-sample Monte Carlo aggregation** (CTPC paper Algorithm 1, lines 8–15) with analytic propagation of `Σ_L` through the decoder NCDE → exact between-sample variance, no MC noise.
2. **Add TLE input uncertainty propagation** through the encoder NCDE → enables propagation of `Σ_TLE` (the input measurement noise) all the way to the output uncertainty ellipsoid. Currently not propagated at all.
3. **Make information invariance verifiable**: under exact analytic propagation, the predictive covariance `Σ_t` should transform exactly correctly under input frame transformations (e.g., ECI ↔ RTN). This is the empirical test for Barbiero's information invariance on the full predictive distribution.

Concrete design: [[Analytic-Sigma-CTPC-Composition]] — Year 2 milestone of the PhyArch-CTPC roadmap.

The composition with [[CBM-CTPC-Composition]] (Year 1, concept-closure invariance) is sequential: Year 1 promotes concept closure from asserted to verified; Year 2 promotes information invariance from "partial (mean only)" to "verified for full distribution." Together they close the remaining Barbiero gaps.

## Connections

- [[Analytic-Sigma-CTPC-Composition]] — concrete Year 2 milestone design applying Wright's propagation to the Latent NCDE Corrector with TLE uncertainty
- [[CTPC-Design-Rationale]] Part III — where the information-invariance gap is identified
- [[Latent-NCDE-Corrector]] — the architecture into which analytic propagation gets composed
- [[CTPC-KDD-Submission]] — the deployed framework whose K-sample inference algorithm gets replaced by analytic propagation
- [[Concept-Bottleneck-Models]] — Year 1 sibling: concept-closure verification mechanism. Year 1 + Year 2 compose to close the remaining Barbiero gaps.
- [[Actionable-Interpretability-Symmetries]] — Barbiero et al. 2026 four-symmetry framework being instantiated by SiS. **Reframing note (2026-05-10):** this paper's analytic propagation was originally framed as closing Barbiero's *information invariance* on the full distribution; after canonical Barbiero ingest, the corrected reading is that it refines *inference equivariance* from mean to full distribution — information invariance is closed by the CBM bottleneck (Year 1), not by analytic Σ propagation (Year 2). See [[CTPC-Design-Rationale]] § Corrections from Barbiero Ingest.
- [[Neural-Controlled-Differential-Equation]] — NCDEs are the substrate; Wright's propagation needs to be extended for RK4-integrated NCDEs (open question, see [[Analytic-Sigma-CTPC-Composition]])

## Open Questions

- **NCDE-specific propagation theorem.** Wright covers MLPs. NCDE has a continuous-time integral solved by RK4. Each RK4 step is a fixed sequence of MLP evaluations + linear combinations — Wright applies to each MLP, then linear-propagates through the RK4 step. Explicit derivation of "Analytic-Σ-NCDE theorem" is an open contribution. *Flagged as a potential publishable methods paper in its own right* — see [[Analytic-Sigma-CTPC-Composition]] open questions.
- **Tanh covariance.** Activation-specific derivation. Tanh derivatives have a polynomial-in-tanh structure; the k-th derivative formula likely has a Hermite-polynomial-style expression but Wright doesn't include it.
- **Heavy-tailed inputs.** Wright assumes Gaussian throughout. For Student-t inputs (CTPC's prediction head; orbital error distributions per Mashiku 2012), the framework needs extension. Not addressed.
- **Variational posterior bottleneck** (Sec. 4.3). For BNN training, the factorized-Gaussian variational posterior often dominates the error budget — exact covariance through the network doesn't help if the posterior approximation is the limiting factor. Worth tracking when applying to CTPC.
- **Specialized-layer extension.** Pooling and batch norm need refactoring or replacement. NCDE doesn't have these, but if future variants do, the refactoring step adds engineering complexity.
- **Theory for Gaussian approximation.** Wright observes empirically that the assumed-Gaussian-input approximation works well for high-dimensional networks (CLT), but a more thorough theoretical analysis is open. For low-dimensional CTPC (state-space `~ ℝ⁶`, hidden dim `~ 100`), the CLT argument is weaker — empirical validation needed.
- **Covariance of correlated activations after a non-element-wise function.** Theorem 1 requires element-wise independent and identical activations. For attention layers, batch normalization, or other non-element-wise operations, separate analysis is needed.

## Sources

- `raw/papers/uncertainty-propagation/AnalyticUncertaintyProp.pdf` — Wright, Nakahira, Moura 2024 (AISTATS). Sec. 3 (problem formulation + Theorem 1), Sec. 4 (experiments: synthetic networks, MNIST, UCI BNN training), Appendix A (full Fourier-analysis proof), Appendix B (closed-form mean + k-th derivative formulae for Heaviside, ReLU, GELU, sigmoid).
- Code: <https://github.com/omwright/cov-prop-nn>
