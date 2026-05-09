---
title: Neural Controlled Differential Equation
tags: [ncde, neural-ode-adjacent, irregular-time-series, control-path, foundational, continuous-time]
sources: [raw/papers/my-papers/KDD_submission.pdf]
created: 2026-05-09
updated: 2026-05-09
sis_relevance: high
hard_constraint_possible: partial
---

# Neural Controlled Differential Equation

## One-Line Intuition

A continuous-time analog of an RNN: hidden state `z(t)` evolves continuously in time, but driven by an *input control path* `X(t)` rather than autonomously. Where a Neural ODE evolves `dz/dt = f_θ(z)` (uncontrolled), an NCDE evolves `dz = f_θ(z) dX(t)` — the control path injects information at every infinitesimal moment, making the model handle irregular sampling and missing features natively.

## Why This Was Invented

[Kidger, Morrill, Foster, Lyons 2020] introduced NCDEs to address a structural limitation of Neural ODEs (Chen et al. 2018): NODEs are *uncontrolled* — once integrated forward from an initial state, they evolve autonomously with no way to incorporate streaming observations except by re-initializing or using awkward discrete-time updates (as in Latent ODE / GRU-ODE-Bayes / ODE-RNN).

**The control path framing fixes this.** Controlled differential equations (Lyons' rough-path theory) are the standard mathematical object for ODEs *driven by* an input signal. NCDEs apply this to neural networks: the input time series becomes a continuous control path (via spline interpolation), and the network's vector field is multiplied by the control path's differential. The result:

- Hidden dynamics are continuous everywhere (not just between observations as in ODE-RNN)
- Irregular sampling is handled natively — the spline is defined for any `t`
- Missing features can be encoded as additional channels in the control path (missingness indicators)
- Long-term dependencies are modeled as well as RNNs (with explicit-control conditioning)

For the [[CTPC-KDD-Submission]] application, NCDEs solved a real practical problem: NASA CDDIS spacecraft observations arrive at irregular times with occasional missing channels. NCDEs absorb both characteristics without hacks.

## The Math

### Definition

Given an input time series `(t_0, y_0), ..., (t_n, y_n)` with `y_i ∈ ℝ^v`, construct a continuous control path `X : [t_0, t_n] → ℝ^{v+1}` (state + time) by interpolation (typically natural cubic spline). Then the NCDE:

```
z(t) = z(t_0) + ∫_{t_0}^t f_θ(z(s)) dX(s)
```

The integral is a Riemann-Stieltjes integral. The vector field `f_θ : ℝ^h → ℝ^{h × (v+1)}` is parameterized by an MLP. Its *output is a matrix*, not a vector — `f_θ(z(s)) ∈ ℝ^{h × (v+1)}` and `dX(s) ∈ ℝ^{v+1}`, so their matrix-vector product gives `dz ∈ ℝ^h`. This shape mismatch with NODEs (where `f_θ(z) ∈ ℝ^h`) is what allows the control path to inject information.

### Evaluation as a Standard ODE

Since the control path `X(t)` is differentiable (cubic spline → continuous derivative), the NCDE can be re-written as a standard ODE:

```
dz/dt = f_θ(z(t)) · dX/dt
```

where `dX/dt` is computed analytically from the spline. This means **NCDEs can be solved with any standard ODE solver** (Euler, RK4, Dormand-Prince, etc.) — no specialized rough-path machinery needed for the smooth-path case.

### Universal approximation

NCDEs subsume RNNs in expressivity (Kidger et al. 2020 Theorem 4.1): any continuous function on a sequence space can be approximated by an NCDE to arbitrary precision. The control path matters here: it provides the conditioning signal that lets the hidden state distinguish different input trajectories.

### Code Correspondence

```python
import torch
import torchcde      # or use diffrax in JAX

class NCDEFunc(nn.Module):
    """The vector field f_θ(z) for a CDE: outputs a (h × control_dim) matrix."""
    def __init__(self, hidden_dim, control_dim, mlp_hidden=128):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(hidden_dim, mlp_hidden), nn.Tanh(),
            nn.Linear(mlp_hidden, mlp_hidden), nn.Tanh(),
            nn.Linear(mlp_hidden, hidden_dim * control_dim),
        )
        self.hidden_dim, self.control_dim = hidden_dim, control_dim

    def forward(self, t, z):
        return self.net(z).view(z.shape[0], self.hidden_dim, self.control_dim)

class NCDEModel(nn.Module):
    def __init__(self, input_dim, hidden_dim, output_dim):
        super().__init__()
        self.func = NCDEFunc(hidden_dim, input_dim + 1)        # +1 for time
        self.initial = nn.Linear(input_dim + 1, hidden_dim)
        self.readout = nn.Linear(hidden_dim, output_dim)

    def forward(self, coeffs, times):
        X = torchcde.NaturalCubicSpline(coeffs)
        z0 = self.initial(X.evaluate(times[0]))
        z = torchcde.cdeint(X=X, func=self.func, z0=z0, t=times)
        return self.readout(z[:, -1])
```

## Toy Example

**Predicting the next value of an irregularly-sampled time series.** Generate a trajectory by sampling a smooth function (e.g., `sin(2πt) + cos(3πt)`) at random times, training an NCDE to predict subsequent values. Comparison:

- A standard RNN with timestep deltas as features: works but has discrete-time artifacts at the sampling boundaries
- ODE-RNN: continuous between samples but discrete updates at observations; loses information about the *shape* of the trajectory between samples
- **NCDE**: smooth conditioning everywhere; absorbs irregular timing and shape information naturally through the spline

For the CTPC application, the toy example is the GRU-ODE-Bayes / Latent ODE comparison in Table 2 of the source paper: NCDE-based variants (CTPC-CDE, CTPC-CDE++) consistently outperform Latent ODE baselines on irregular-sampling spacecraft data.

## Connection to SiS / CTPC

NCDEs are the **temporal substrate** for the [[Latent-NCDE-Corrector]] used in [[CTPC]]:

- **Encoder NCDE**: ingests irregular spacecraft observations + their missingness indicators. The control path *is* the observed error history.
- **Decoder NCDE**: drives the hidden state forward over the forecast horizon. The control path *is* the deterministic Predictor's (GMAT's) forecasts. This is the key architectural insight: the decoder's evolution is conditioned on what the physics model predicts at every infinitesimal moment.

For SiS architectures more broadly:

- NCDEs handle the **irregular-time observation** problem that any real-world SDA pipeline faces (TLE update intervals, ground-station passes, etc.)
- The control-path formulation is **coordinate-frame-agnostic**: a transformation on the input trajectory is automatically a transformation on the control path. This is why NCDE-based CTPC variants are robust to the ECI ↔ RTN coordinate-frame change while NODE-based variants are not (Appendix G of source paper).
- Composing an NCDE decoder with [[PhyArch]]-style symmetry-hardwiring inside `f_θ` is the open architectural question for PhyArch-CTPC. See [[CTPC-Design-Rationale]].

## Connections

- [[CTPC-KDD-Submission]] — uses NCDEs in both encoder and decoder of the Latent NCDE Corrector
- [[Latent-NCDE-Corrector]] — the architecture pattern built on NCDEs
- [[Hamiltonian-Neural-Network]] — orthogonal continuous-time pattern: HNN parameterizes a *scalar* + uses autograd; NCDE parameterizes a *vector field matrix* + uses control paths
- [[Lagrangian-Neural-Network]] — analogous orthogonality

## Open Questions

- **Linear NCDE variants (SLiCE).** [Walker et al. 2025] introduced Structured Linear CDEs for parallelizable training. Source paper notes SLiCE was insufficient for capturing the nonlinear, trajectory-dependent error dynamics in spacecraft forecasting — but for shorter-horizon, less-nonlinear settings, the parallelism could matter. Open whether SLiCE-style linear CDE blocks can be combined with nonlinear NCDE blocks in a hybrid corrector.
- **Streaming / online NCDEs.** [Morrill et al. 2021] adapt NCDEs for online prediction. Operationally, real SDA pipelines need online updates as new TLE data arrives. The CTPC framework currently does encoder-then-decoder; a streaming variant would interleave them.
- **Rough-path NCDEs for non-smooth controls.** Cubic splines smooth out the control path, but real observations may contain genuine discontinuities (maneuvers, anomalies). The general rough-path theory handles non-smooth controls; whether CTPC benefits from the more general formulation is open.
- **Adjoint vs. discretize-then-optimize gradients.** NODEs originally used adjoint sensitivity; modern practice often discretizes and backprops through the solver. Source paper (Kidger 2022) discusses this trade-off; CTPC source paper uses discretize-then-optimize for stability.
- **Compositionality with hard-constrained vector fields.** Replacing the generic `f_θ` with a [[PhyArch]]-style symmetry-hardwired vector field is the path to PhyArch-CTPC. Training stability with second-order autograd through such a vector field is unstudied.

## Sources

- `raw/papers/my-papers/KDD_submission.pdf` — Shahid, Jiang, Sarkar, Fleming 2026. Eq. 1 (encoder NCDE), Eq. 3 (decoder NCDE), Sec. 4.1 (Latent NCDE architecture). The paper uses NCDEs as the foundational continuous-time substrate.
- **Kidger, Morrill, Foster, Lyons 2020.** *Neural Controlled Differential Equations for Irregular Time Series.* arXiv:2005.08926. The foundational NCDE paper. *Not yet ingested as a separate paper page* — flagged for future ingest if NCDE-specific deep dive is needed.
- **Kidger 2022.** *On Neural Differential Equations.* arXiv:2202.02435. Comprehensive thesis-level reference on NDEs (NODEs, NCDEs, Neural SDEs). *Not yet ingested.*
- **Morrill et al. 2021.** Online NCDE variants. *Not yet ingested.*
- **Walker, Yang, Muca Cirone, Salvi, Lyons 2025.** Structured Linear CDEs. *Not yet ingested.*
