# SiS Wiki Index
Last updated: 2026-05-08 | Total pages: 13

## Concepts (7)
- [[Hamiltonian-Mechanics]] — Foundational physics: scalar `H(q,p)` + Hamilton's equations gives conservation by construction; basis for HNN, LNN, PHNN | sis_relevance: critical | sources: 1
- [[Lagrangian-Mechanics]] — Action principle + scalar `L(q,q̇) = T − V`; coordinate-free conservation; basis for LNN | sis_relevance: critical | sources: 1
- [[Symplectic-Gradient]] — Vector field `S_H = (∂H/∂p, −∂H/∂q)`; orthogonal to `∇H`, so flow along `S_H` conserves `H` | sis_relevance: high | sources: 1
- [[Euler-Lagrange-Equation]] — `d/dt(∂L/∂q̇) = ∂L/∂q`; how a Lagrangian becomes dynamics; Lagrangian analog of symplectic gradient | sis_relevance: high | sources: 1
- [[Physics-Based-FD-Convolutional-Layer]] — Frozen-stencil conv that hard-encodes a known differential operator; reserves trainable parameters for unknown physics | sis_relevance: high | sources: 1
- [[Pi-Block-Polynomial-Approximator]] — Element-wise products of parallel convs make a learnable multivariate polynomial; SymPy-extractable closed form | sis_relevance: high | sources: 1
- [[Physics-Based-Padding]] — Encode Dirichlet/Neumann/Robin/periodic BCs as deterministic ghost-cell padding; hard constraint by construction | sis_relevance: medium | sources: 1

## Architectures (2)
- [[Hamiltonian-Neural-Network]] — Parameterize scalar `H_θ(q,p)` with an MLP; derive dynamics by autograd through Hamilton's equations; conservation is structural | sis_relevance: high | sources: 1
- [[Lagrangian-Neural-Network]] — Parameterize scalar `L_θ(q,q̇)` with an MLP; derive `q̈` via Hessian inversion in Euler-Lagrange; works with non-canonical coords | sis_relevance: high | sources: 1

## Papers (3)
- [[HNN]] — Greydanus et al. 2019 (NeurIPS); first NN parameterization of a Hamiltonian, validated on mass-spring, pendulum, two-body, pixel pendulum | sis_relevance: high | sources: 1
- [[LNN]] — Cranmer et al. 2020 (ICLR Workshop); Lagrangian counterpart, works with non-canonical coords, validated on double pendulum + relativistic particle + 1D wave equation | sis_relevance: high | sources: 1
- [[PeRCNN]] — Rao et al. 2023 (Nat. Mach. Intell.); physics-as-architecture for PDE learning: frozen FD conv + Π-block + physics-based padding, no soft physics loss | sis_relevance: high | sources: 1

## Books (0)
_None yet._

## Connections / Synthesis (1)
- [[Hamiltonian-vs-Lagrangian-Duality]] — CTPC design-decision matrix: when to pick HNN (canonical coords, `O(d²)`) vs LNN (arbitrary coords, `O(d³)`); both share the dissipation gap | sis_relevance: critical | sources: 2

## SiS Design Decisions (0)
_None yet._
