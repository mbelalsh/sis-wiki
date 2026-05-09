---
title: Physics-Based Padding for Boundary Conditions
tags: [hard-constraint, boundary-conditions, padding, pde-learning, ghost-cells, physics-as-architecture]
sources: [raw/papers/port-hamiltonian/EncodingPhysics.pdf]
created: 2026-05-08
updated: 2026-05-08
sis_relevance: medium
hard_constraint_possible: yes
---

# Physics-Based Padding for Boundary Conditions

## One-Line Intuition

Encode boundary conditions by *deterministically computing the padding values* around the conv input — the network output literally cannot violate the BC, because the math of padding pre-determines the boundary values before the conv sees them.

## Why This Was Invented

Standard PINNs penalize BC violations in the loss: `L_BC = MSE(û|_∂Ω − g)`. This is a **soft** constraint — the network can and does violate it during training, hopefully decreasing toward zero. PeRCNN imports the **ghost-cell** technique from classical FD methods: pad the discretized field with values that make the boundary stencil compute the BC exactly. The conv kernel sliding over the padded edges sees a setup that already satisfies the BC.

This is a hard constraint by construction. No loss term, no λ to tune, no asymptotic violation.

## The Math

For an internal grid `U[1..H, 1..W]`, pad to `Ũ[0..H+1, 0..W+1]` with rules per BC type:

| BC type | Rule for ghost cell `Ũ[0, j]` (left edge) |
|---|---|
| Dirichlet `u = g` | `Ũ[0, j] = g(j)` (prescribed value) |
| Neumann `∂u/∂n = q` | `Ũ[0, j] = U[1, j] − q · δx`  (ghost = interior + flux) |
| Robin `α u + β ∂u/∂n = γ` | linear combo of Dirichlet + Neumann rules |
| Periodic | `Ũ[0, j] = U[H, j]` (wrap-around) |

A 3-point stencil `[a, b, c]` applied at `j = 1` then computes `a · Ũ[0, j] + b · U[1, j] + c · U[2, j]`, with `Ũ[0, j]` fixed by the BC rule. The result respects the BC by construction.

The same logic generalizes to 2D / 3D and to higher-order stencils (more ghost layers).

### Code Correspondence

```python
import torch.nn.functional as F

def dirichlet_pad(u, value=0.0, pad=1):
    """u = value on the boundary."""
    return F.pad(u, (pad,)*4, mode='constant', value=value)

def neumann_pad(u, flux=0.0, pad=1, dx=1.0):
    """∂u/∂n = flux on the boundary. flux=0 → zero-flux (reflect)."""
    if flux == 0.0:
        return F.pad(u, (pad,)*4, mode='replicate')   # ghost = boundary cell
    # General: u_ghost = u_boundary - flux*dx
    pads = []
    # ... per-edge construction omitted for brevity
    raise NotImplementedError("non-zero Neumann flux: see paper Supplementary B.3")

def periodic_pad(u, pad=1):
    """Wrap-around BC."""
    return F.pad(u, (pad,)*4, mode='circular')
```

After padding, the conv layer in the recurrent step is applied as usual:

```python
def rd_step(u, conv_block, dt, bc='neumann'):
    u_padded = neumann_pad(u) if bc == 'neumann' else dirichlet_pad(u)
    f_hat = conv_block(u_padded)
    return u + f_hat * dt   # BC satisfied at every step, no loss term needed
```

## Toy Example

Heat equation `u_t = α Δu` on a 32×32 grid with zero-flux Neumann BCs (insulated walls):

- **Without physics-based padding** (zero-padding default): the boundary acts as Dirichlet zero; the field artificially loses energy at the walls.
- **With Neumann padding** (`mode='replicate'`): total mass `Σ u` is conserved to discretization precision because `∂u/∂n = 0` is exact at every step.

A single recurrent unroll of 1000 steps shows the difference visibly: zero-padded simulations lose ~30% mass; replicate-padded simulations lose <0.01% (residual from finite stencil).

## Connection to SiS / CTPC

For the **current CTPC** (point-mass orbital ODEs, no spatial domain), this concept is not directly applicable — there are no spatial boundaries, only initial conditions. The analogue is the network's IC encoding (PeRCNN's `Initial State Generator` decoder), and CTPC's analogue is just feeding the SGP4 prediction as the initial state.

For **future SiS components** with spatial structure, this is critical:

- **Atmospheric drag fields.** Density `ρ(h, lat, lon, t)` on a spherical grid → periodic in longitude, Neumann at poles, Dirichlet at upper boundary (vacuum).
- **Magnetic field models.** Vector field on Earth-fixed grid with curl-free / div-free constraints — these are *internal* hard constraints that compose with the boundary padding.
- **Conjunction probability fields.** Fields over relative-motion encounter geometries with natural radiation BCs.

The general principle — **encode the constraint geometrically, don't penalize it** — generalizes beyond spatial BCs. For orbital ODEs, an analogous trick would be re-parameterizing the state so impossible values (e.g., negative semi-major axis, eccentricity > 1 for bound orbits) are unreachable by construction.

## Connections

- [[PeRCNN]] — paper introducing physics-based padding alongside the FD layer and Π-block
- [[Physics-Based-FD-Convolutional-Layer]] — sister technique that hard-encodes interior PDE structure
- [[Pi-Block-Polynomial-Approximator]] — the learned-residual companion

## Open Questions

- **Corner consistency.** When two adjacent edges have different BC types (e.g., Dirichlet on one, Neumann on the other), the corner ghost cell is over-determined. Paper handles this case-by-case in Supplementary Note B.3 — formalizing a general rule is open.
- **Irregular geometries.** Padding presumes a Cartesian grid with edges. For domains with curved or polygonal boundaries (the actual atmospheric drag case on Earth), graph-based ghost-node constructions are needed.
- **Robin and time-varying BCs.** Robin BCs (`αu + β∂u/∂n = γ`) require solving a small linear system per edge cell; computationally fine but adds complexity. Time-varying `g(t)` Dirichlet is straightforward; time-varying Neumann needs the gradient at each time step.
- **High-order stencils.** A 5-point stencil needs 1 ghost layer; a 9-point stencil needs 2. Ensuring consistency between stencil order and ghost-layer count is an easy footgun in larger architectures.

## Sources

- `raw/papers/port-hamiltonian/EncodingPhysics.pdf` — Fig. 1b (BC encoding schematic), Methods § "Network architecture" final paragraph, Supplementary Note B.3 (full padding rules for Dirichlet/Neumann/Robin/periodic), Supplementary Note G (Neumann effectiveness demonstration).
