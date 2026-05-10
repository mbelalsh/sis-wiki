# SiS Wiki Index
Last updated: 2026-05-10 | Total pages: 24

## Concepts (8)
- [[Hamiltonian-Mechanics]] — Foundational physics: scalar `H(q,p)` + Hamilton's equations gives conservation by construction; basis for HNN, LNN, PHNN | sis_relevance: critical | sources: 1
- [[Lagrangian-Mechanics]] — Action principle + scalar `L(q,q̇) = T − V`; coordinate-free conservation; basis for LNN | sis_relevance: critical | sources: 1
- [[Symplectic-Gradient]] — Vector field `S_H = (∂H/∂p, −∂H/∂q)`; orthogonal to `∇H`, so flow along `S_H` conserves `H` | sis_relevance: high | sources: 1
- [[Euler-Lagrange-Equation]] — `d/dt(∂L/∂q̇) = ∂L/∂q`; how a Lagrangian becomes dynamics; Lagrangian analog of symplectic gradient | sis_relevance: high | sources: 1
- [[Neural-Controlled-Differential-Equation]] — Continuous-time RNN driven by an input control path; handles irregular sampling natively; foundational substrate for CTPC | sis_relevance: high | sources: 1
- [[Physics-Based-FD-Convolutional-Layer]] — Frozen-stencil conv that hard-encodes a known differential operator; reserves trainable parameters for unknown physics | sis_relevance: high | sources: 1
- [[Pi-Block-Polynomial-Approximator]] — Element-wise products of parallel convs make a learnable multivariate polynomial; SymPy-extractable closed form | sis_relevance: high | sources: 1
- [[Physics-Based-Padding]] — Encode Dirichlet/Neumann/Robin/periodic BCs as deterministic ghost-cell padding; hard constraint by construction | sis_relevance: medium | sources: 1

## Architectures (4)
- [[Hamiltonian-Neural-Network]] — Parameterize scalar `H_θ(q,p)` with an MLP; derive dynamics by autograd through Hamilton's equations; conservation is structural | sis_relevance: high | sources: 1
- [[Lagrangian-Neural-Network]] — Parameterize scalar `L_θ(q,q̇)` with an MLP; derive `q̈` via Hessian inversion in Euler-Lagrange; works with non-canonical coords | sis_relevance: high | sources: 1
- [[Latent-NCDE-Corrector]] — Encoder-decoder pattern: NCDE encoder over past errors → latent posterior → NCDE decoder driven by Predictor's forecasts → Student-t prediction head; strictly proper loss | sis_relevance: critical | sources: 1
- [[PhyArch]] — Joint coordinate transformation on input + weight space; geometric-feature embedding + parity-split assembly hardwires symmetries and manipulator-equation form; SiS methodology | sis_relevance: critical | sources: 1

## Papers (6)
- [[HNN]] — Greydanus et al. 2019 (NeurIPS); first NN parameterization of a Hamiltonian, validated on mass-spring, pendulum, two-body, pixel pendulum | sis_relevance: high | sources: 1
- [[LNN]] — Cranmer et al. 2020 (ICLR Workshop); Lagrangian counterpart, works with non-canonical coords, validated on double pendulum + relativistic particle + 1D wave equation | sis_relevance: high | sources: 1
- [[Concept-Bottleneck-Models]] — Koh et al. 2020 (ICML); architectural pattern for concept-level interventions; mechanism for verifying concept-closure invariance + information invariance in CBM-CTPC (one bottleneck, two Barbiero symmetries) | sis_relevance: high | sources: 1
- [[PeRCNN]] — Rao et al. 2023 (Nat. Mach. Intell.); physics-as-architecture for PDE learning: frozen FD conv + Π-block + physics-based padding, no soft physics loss | sis_relevance: high | sources: 1
- [[Analytic-Covariance-Propagation]] — Wright et al. 2024 (AISTATS); Theorem 1 closed-form covariance through nonlinear activations + Algorithm 1 layer-by-layer propagation; technical machinery for refining inference equivariance to full distribution in Analytic-Σ-CTPC | sis_relevance: high | sources: 1
- [[Actionable-Interpretability-Symmetries]] — Barbiero et al. 2026 (arXiv); position paper arguing interpretability must be defined via four symmetries (inference equivariance, information invariance, concept-closure invariance, structural invariance); canonical source for the four-symmetry framework operationalized in [[CTPC-Design-Rationale]] Part III | sis_relevance: critical | sources: 1

## Books (0)
_None yet._

## Connections / Synthesis (1)
- [[Hamiltonian-vs-Lagrangian-Duality]] — CTPC design-decision matrix: when to pick HNN (canonical coords, `O(d²)`) vs LNN (arbitrary coords, `O(d³)`); both share the dissipation gap; PhyArch is an orthogonal third axis | sis_relevance: critical | sources: 2

## SiS Design Decisions (5)
- [[Analytic-Sigma-CTPC-Composition]] — Year 2 milestone design: replace K-sample MC with analytic moment propagation through Latent NCDE Corrector; enable TLE uncertainty source; frame-equivariance verification protocol (`Σ_t^ECI = R^T Σ_t^RTN R` to machine precision) IS the empirical test for Barbiero information invariance on the full predictive distribution — promotes from "partial (mean only)" to "verified". Open contribution: Analytic-Σ-NCDE theorem (publishable methods paper) | sis_relevance: critical | sources: 2
- [[CBM-CTPC-Composition]] — Year 1 milestone design: insert a CBM concept bottleneck (k=9 orbital concepts: J2, J3, drag, SRP, lunar/solar 3-body + RTN components) between Latent NCDE decoder and prediction head; independent training scheme (operational use case IS intervention); four-test verification protocol promotes concept-closure invariance from "asserted" to "verified" | sis_relevance: critical | sources: 2
- [[CTPC-KDD-Submission]] — Bilal's KDD '26 submission defining CTPC: GMAT (frozen Predictor) + Latent NCDE (probabilistic Corrector) on real NASA CDDIS spacecraft data; 64% MSE reduction at 4-day horizon, d̄² ≈ 1 calibration vs Latent ODE baselines' d̄² > 20,000 | sis_relevance: critical | sources: 1
- [[CTPC-Design-Rationale]] — Synthesis page combining PhyArch DP + CTPC + Barbiero interpretability framing; Part I D1–D7 (decisions made), Part II Q1–Q8 (deferred to PhyArch-CTPC), Part III (PhyArch + CBM-CTPC + Analytic-Σ-CTPC under the Barbiero framework — substantially revised 2026-05-10 after canonical Barbiero ingest with visible audit trail of four corrections; publishable claim softened to "candidate practical instantiation contingent on orbital-mechanics-trained user"). Year 1 CBM-CTPC closes BOTH concept-closure AND information invariance (one bottleneck, two symmetries); Year 2 Analytic-Σ refines inference equivariance to full distribution | sis_relevance: critical | sources: 6
- [[PhyArch-Double-Pendulum-Benchmark]] — Bilal's own benchmark: PhyArch vs LNN vs PINN on planar double pendulum; PhyArch achieves 0% failure rate everywhere + lowest energy drift despite not modeling energy; demonstrates symmetry compliance is more protective of conservation laws than explicitly modeling the conserved quantity | sis_relevance: critical | sources: 1
