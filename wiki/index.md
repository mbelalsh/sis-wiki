# SiS Wiki Index
Last updated: 2026-05-08 | Total pages: 8

## Concepts (5)
- [[Hamiltonian-Mechanics]] — Foundational physics: scalar `H(q,p)` + Hamilton's equations gives conservation by construction; basis for HNN, LNN, PHNN | sis_relevance: critical | sources: 1
- [[Symplectic-Gradient]] — Vector field `S_H = (∂H/∂p, −∂H/∂q)`; orthogonal to `∇H`, so flow along `S_H` conserves `H` | sis_relevance: high | sources: 1
- [[Physics-Based-FD-Convolutional-Layer]] — Frozen-stencil conv that hard-encodes a known differential operator; reserves trainable parameters for unknown physics | sis_relevance: high | sources: 1
- [[Pi-Block-Polynomial-Approximator]] — Element-wise products of parallel convs make a learnable multivariate polynomial; SymPy-extractable closed form | sis_relevance: high | sources: 1
- [[Physics-Based-Padding]] — Encode Dirichlet/Neumann/Robin/periodic BCs as deterministic ghost-cell padding; hard constraint by construction | sis_relevance: medium | sources: 1

## Architectures (1)
- [[Hamiltonian-Neural-Network]] — Parameterize scalar `H_θ(q,p)` with an MLP; derive dynamics by autograd through Hamilton's equations; conservation is structural | sis_relevance: high | sources: 1

## Papers (2)
- [[HNN]] — NeurIPS 2019; first NN parameterization of a Hamiltonian, validated on mass-spring, pendulum, two-body, pixel pendulum | sis_relevance: high | sources: 1
- [[PeRCNN]] — Rao et al. 2023 (Nat. Mach. Intell.); physics-as-architecture for PDE learning: frozen FD conv + Π-block + physics-based padding, no soft physics loss | sis_relevance: high | sources: 1

## Books (0)
_None yet._

## Connections / Synthesis (0)
_None yet._

## SiS Design Decisions (0)
_None yet._
