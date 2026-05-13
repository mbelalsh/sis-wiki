---
title: GAINS — Efficient Certified Training and Robustness Verification of Neural ODEs
tags: [formal-verification, neural-odes, adversarial-robustness, abstract-interpretation, controlled-adaptive-solver, certified-training]
sources:
  - raw/papers/formal-verification/FVerify_NODE.pdf   # Zeqiri, Müller, Fischer, Vechev 2023 (ICLR, arXiv:2303.05246v1)
created: 2026-05-12
updated: 2026-05-12
sis_relevance: critical
hard_constraint_possible: yes
---

# GAINS (Zeqiri, Müller, Fischer, Vechev 2023)

## One-Line Intuition

The first scalable framework that **formally verifies the robustness of a Neural ODE
including its ODE solver** — replace the adaptive solver's continuous step-size space
with a discrete exponential grid (CAS), abstract the resulting trajectory ensemble as a
finite graph, and propagate DeepPoly linear bounds through the graph. Cuts complexity from
`O(exp(d) + exp(T))` to `O(d + T² log² T)`.

## Why This Was Invented

Neural ODEs were initially marketed as inherently adversarially robust (Yan et al. 2020,
Kang et al. 2021, Rodriguez et al. 2022, Zakwan et al. 2022). Huang et al. 2020 then showed
this robustness was **gradient-obfuscation** caused by adaptive ODE solvers — when you
attack the solver trajectory itself, NODEs are *not* robust. Empirical robustness ≠
formal robustness; safety-critical deployment needs the latter.

But the NODE verification problem is hard: standard NN verifiers (Reluplex, DeepPoly,
α,β-CROWN, Beta-CROWN, PRIMA) don't handle adaptive solvers. Each input region induces a
*continuous range* of step-sizes → infinitely many possible solver trajectories →
intractable abstraction. Prior NODE verification (Lopez et al. 2022) sidestepped this by
*ignoring* the solver, analyzing only the learned dynamics — fine for `d < 10` but doesn't
scale.

GAINS solves both problems: (i) **discretize the solver's step-size space** to enable
verification, (ii) **abstract the trajectory ensemble as a finite graph** to enable
efficient bound propagation.

## The Math

**Standard NODE** (recap, from [[NODE]]):
```
z(T) = z(0) + ∫₀ᵀ g_θ(z(t), t) dt    solved by adaptive solver Γ(z₀)
```
For a *single* initial state, the adaptive solver `Γ` deterministically picks a trajectory
of `(t, h)` pairs (time, step-size). For an *input region* (e.g. an ℓ_∞ ball of radius
ε), Γ yields a continuous range of trajectories — intractable.

**CAS (Controlled Adaptive Solver, §4).** Modify the step-size update rule:
```
h_{k+1} = h · α     if δ ≤ τ_α   (error well-tolerated; grow step)
       = h         if τ_α < δ ≤ 1  (within tolerance; keep step)
       = h / α     if δ > 1     (error exceeds tolerance; shrink step)
```
with `α > 1` an integer (paper uses α=2) and `τ_α = α⁻ᵖ` derived from solver order `p`.
This forces step-sizes to live on a *discrete* exponential grid `{h₀ · α^k}` for some
integer `k`. The reachable `(t, h)` set after `N` steps is now finite.

CAS is at most `α`× slower than a standard adaptive solver in step count (Figure 4 of
the paper shows it's empirically comparable to dopri5).

**GAINS (Graph based Abstract Interpretation for NODEs, §5).** Even with CAS, exponential
many trajectories remain. Construction:

- **Nodes** = `(t, h)` states.
- **Edges** = solver steps connecting consecutive `(t, h)` states.
- **State annotation** = interval bounds on `z(t)` at that node.

Trajectories with identical `(t, h)` are *merged* — interval bounds combined via convex
hull. Result: at most `O(T_end · log(T_end))` nodes; `O(T_end² · log²(T_end))` edges. **For
solver step `(t, h) → (t', h')`, run one abstract solver step**: compute interval bounds
on the local error estimate `δ`, check which CAS step-size updates are compatible, add
edges accordingly.

**Bound propagation through the trajectory graph.** Two modes:

1. **Interval bounds**: trivial forward pass; cheap but loose.
2. **Linear bounds (DeepPoly)**: backsubstitution along all incoming edges of every node.
   But nodes can have multiple successors via different trajectories — yields multiple
   linear bounds for the same expression. Soundly merging them is the **Linear Constraint
   Aggregation Problem (LCAP)**.

**CURLS (Constraint Unification via ReLU Simplification, §5).** Rewrite `max{u^j}_j` as
nested ReLU compositions:
```
max(u^1, u^2) = u^1 + max(0, u^2 − u^1) = u^1 + ReLU(u^2 − u^1)
```
Repeat for `m` constraints. Now DeepPoly's ReLU primitives apply. The paper notes CURLS is
faster than LP solving for `d > ~75` while being only 1.02× looser (Figure 8).

### Code Correspondence

```python
# Sketch — actual implementation at https://github.com/eth-sri/GAINS

def cas_step(z_bounds, t, h, alpha, tau_alpha, g_theta):
    """Abstract CAS step: compute interval bounds on next (t, h) candidates."""
    # Interval-propagate one solver evaluation
    error_est_bounds = abstract_error_estimate(z_bounds, t, h, g_theta)
    
    # Three possible next states; include all whose error-estimate range is compatible
    successors = []
    if interval_intersects(error_est_bounds, [None, tau_alpha]):
        successors.append((t + h, h * alpha))            # grow step
    if interval_intersects(error_est_bounds, [tau_alpha, 1.0]):
        successors.append((t + h, h))                    # keep step
    if interval_intersects(error_est_bounds, [1.0, None]):
        successors.append((t + h, h / alpha))            # shrink step
    
    return successors, propagate_z_bounds(z_bounds, t, h, g_theta)

def gains_verify(node_model, input_region, t_end):
    """Build trajectory graph + propagate bounds."""
    graph = TrajectoryGraph()
    graph.add_node((0, h_0), bounds=input_region)
    
    frontier = [(0, h_0)]
    while frontier:
        (t, h) = frontier.pop()
        if t >= t_end:
            continue
        next_states, z_bounds = cas_step(graph.bounds[(t, h)], t, h,
                                          ALPHA, TAU_ALPHA, node_model.g_theta)
        for (t', h') in next_states:
            if graph.has_node((t', h')):
                graph.merge_bounds((t', h'), z_bounds)
            else:
                graph.add_node((t', h'), bounds=z_bounds)
                frontier.append((t', h'))
            graph.add_edge((t, h), (t', h'))
    
    return graph.linear_bounds_at(t_end)   # via DeepPoly + CURLS
```

## Empirical Results

**Image classification** (Table 1):

| Setting | ε | Standard accuracy | Certified accuracy |
|---|---|---|---|
| MNIST, Std training | 0.10 | 98.8% | 0% |
| MNIST, Adversarial training | 0.10 | 99.2% | 0% |
| **MNIST, GAINS certified training** | 0.10 | 95.5% | **89.0%** |
| **MNIST, GAINS, larger ε_t** | 0.10 | 91.8% | 88.5% |
| FMNIST, GAINS | 0.10 | 75.1% | **62.5%** |

**Time-series forecasting on PhysioNet** (Table 2, **most CTPC-relevant**):

| Forecast horizon | Training | ε | Std MAE × 10⁻² | Adversarial accuracy | Certified accuracy |
|---|---|---|---|---|---|
| 6h | Standard | 0.05 | 47.4 | 54.3% | 0% |
| 6h | GAINS | 0.05 | 51.1 | **97.7%** | **93.0%** |
| **24h** | GAINS, ε_t = 0.2 | 0.05 | 53.7 | **99.7%** | **99.1%** |

For 24-hour forecasts on PhysioNet, **certified accuracy of 99.1%** at ε=0.05. This is
the closest published benchmark to CTPC's setting.

**Trajectory sensitivity ablation** (Table 3) — confirms Huang et al. 2020: standard NODEs
are vulnerable to adversarial attacks on the solver trajectory (95.5% attack success at
ε=0.20). GAINS training reduces this to 65.2%-82.2%. Solver-aware verification *matters*.

## Three Things Worth Calling Out

1. **GAINS finally lets us treat the solver as part of the model.** Lopez et al. 2022's
   "ignore the solver" approach scales only to `d < 10`. GAINS handles `d = 784` (MNIST)
   and time-series `d = 35` features × 48 hours. **This is the result that makes Q5 of
   [[CTPC-Design-Rationale]] operationally addressable, not aspirational.**

2. **The PhysioNet benchmark is structurally similar to CTPC.** PhysioNet: 8000 time-series
   of up to 48 hours of 35 irregularly-sampled features → forecast last measurement using
   prior 6h/12h/24h. CTPC: NASA CDDIS multi-day spacecraft trajectories → forecast
   trajectory residual at 7-day horizon. Both use latent-ODE-style encoder-decoder
   architecture; both have probabilistic targets; both have natural ν-δ-robustness
   formulation for regression. **The 99.1% certified accuracy at 24h is the credibility
   benchmark for any future CTPC certification claim.**

3. **CRITICAL CAVEAT for SiS**: GAINS handles **NODEs, not NCDEs.** Latent NCDE Corrector
   ([[Latent-NCDE-Corrector]]) integrates against a *control path* (TLE observations) —
   the GAINS abstraction doesn't directly extend. Open SiS research question: can the
   CAS + trajectory-graph machinery generalize to NCDEs where each step depends on
   `dX(t)` from the control path? Probably yes (the path is bounded in some
   semi-norm) but the extension is non-trivial and unpublished.

## Connection to SiS / CTPC

**This paper directly addresses Q5 of [[CTPC-Design-Rationale]]** — "Formal verification +
provable safety guarantees." Before this ingest, Q5 was the most aspirational of the
deferred questions: it appeared to require new theoretical machinery for verifying ODE
solver behavior. GAINS shows the machinery exists, scales, and gives meaningful
certificates on time-series forecasting tasks at the scale CTPC operates.

**Direct integration plan for CTPC formal verification (post-ingest):**

1. **Train [[Latent-NCDE-Corrector]] with a GAINS-style certified training loop.** Sample
   trajectories from a CAS-abstracted trajectory graph during training. Adapt CURLS to
   handle the control-path dependence (NCDE-specific extension).

2. **Use the PhysioNet 24h-horizon 99.1% certified accuracy as the SiS certification
   target.** If CTPC can hit ≥95% certified accuracy at 24h on real spacecraft data,
   that's an AFRL-deployable safety claim.

3. **The CAS step-size discretization (`{h₀ · α^k}` with α=2) is a Q8b-discrete-time
   candidate.** Standard RK4 with fixed step doesn't preserve passivity for PHNN
   dynamics. CAS with α=2 keeps adaptive efficiency while making the step-size space
   finite — useful for both verification and possibly for symplectic-integrator analogs
   (though CAS is not itself symplectic; this needs verification).

4. **Couple GAINS verification with [[Analytic-Sigma-CTPC-Composition]] for full
   distributional certification.** GAINS certifies mean prediction robustness via
   ν-δ-bounds; Analytic-Σ propagates the full covariance. Together they could certify
   "the predictive distribution under any ε-perturbation of input TLEs lives within a
   bounded Wasserstein distance of the nominal." Concrete Year 3 milestone.

5. **The "Ethics Statement" framing applies directly to SiS.** The paper notes
   that ℓ_∞-bounded formal guarantees "could give practitioners a false sense of security"
   for real-world adversaries. For SDA, the relevant threat model is not adversarial
   perturbation of TLEs (no adversary) but rather *sensor noise + processing-pipeline
   error*. The right specification for SiS verification is the noise/error model from
   Space-Track's TLE generation process, not ε-balls.

**Hard vs soft constraint.** GAINS is a *verification* framework, not a constraint
mechanism. It composes naturally with SiS hard-constraint architectures: PHNN's
Cholesky-PSD parameterization gives `Ḣ ≤ 0` algebraically, GAINS can then *prove* that
this property is preserved through training and inference, not just asserted. The
combination is the AFRL-deployable end-state.

## Connections

- [[NODE]] — Chen et al. 2018, the architecture this paper verifies.
- [[Adjoint-Sensitivity-Method]] — orthogonal to GAINS (forward verification, not gradient
  computation); high-Lyapunov caveat (added 2026-05-12) is the *adjoint*-specific version
  of GAINS's "ignore the solver" problem.
- [[Latent-NCDE-Corrector]] — the SiS architecture that needs the NCDE extension of GAINS.
- [[CTPC-Design-Rationale]] — Q5 (formal verification + safety) is operationally addressed
  by this paper.
- [[Analytic-Sigma-CTPC-Composition]] — natural composition target for full-distribution
  certification.
- [[Port-Hamiltonian-Neural-Network]] — SiS hard-constraint substrate; GAINS verification
  would let us *prove* `Ḣ ≤ 0` is preserved, not just asserted.

## Open Questions

1. **Extending GAINS to NCDEs.** The CAS + trajectory-graph machinery doesn't directly
   handle control-path-driven dynamics. The path `X(t)` is bounded in some semi-norm; can
   we abstract its rough integrals as part of the trajectory graph state? Unpublished as
   of 2023; concrete SiS research opening for a methods paper.

2. **Certified training for PHNN with `Ḣ ≤ 0` as a verification target.** GAINS verifies
   `ν-δ`-robustness on outputs. Adapting it to verify a *trajectory-level* invariant like
   `Ḣ(z(t)) ≤ Ḣ(z(0)) for all t > 0` requires graph nodes annotated with bounds on `H` and
   `∇H` simultaneously. Concrete next-step research.

3. **CAS vs structure-preserving integrators (Q8b-discrete-time).** Discrete gradient
   method / AVF / Gauss-collocation give exact discrete passivity. CAS keeps adaptive
   efficiency but is not structure-preserving. Open question: can CAS be composed with a
   symplectic correction step, retaining both verifiability and discrete passivity?

4. **The right specification for SDA.** ε-ball L_∞ robustness is not the SDA threat
   model. Sensor noise on TLEs has a different statistical structure (correlated across
   orbital elements, time-dependent magnitude). Defining the right specification is a
   prerequisite to any SiS verification claim.

## Sources

- Zeqiri, M., Müller, M. N., Fischer, M., Vechev, M. (2023). *Efficient Certified Training
  and Robustness Verification of Neural ODEs.* ICLR 2023. arXiv:2303.05246v1 (9 Mar 2023).
  Pages 1–15 read for this ingest (Abstract, §1 Introduction, §2 Adversarial Robustness,
  §3 Neural ODEs, §4 Controlled Adaptive ODE Solvers, §5 Verification — CAS + trajectory
  graph + CURLS, §6 Experimental Evaluation, §7 Related Work, §8 Conclusion, §9 Ethics,
  §10 Reproducibility, References pp. 11–14, App A Latent ODEs for Time-Series Forecasting
  pp. 15). Appendices B (formal proofs), C-G (additional implementation details and
  ablations) deferred — referenced but not separately summarized.
