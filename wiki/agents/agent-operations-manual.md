---
title: Agent Operations Manual — Modes, When-to-Use, and Invocation
tags:
  - agents
  - operations
  - modes
  - usage
  - reference
  - routing
sources:
  - wiki/agents/
created: 2026-05-22
updated: 2026-05-22
sis_relevance: high
hard_constraint_possible: n/a
---

# Agent Operations Manual

## Calling an agent

The 5 `active` agents are wired as Claude Code subagents in `.claude/agents/`. Invoke any of them with `@agent-<name>`:

| `@agent-…` | Domain (and modes, if any) |
|---|---|
| `@agent-generic-writing-agent` | sentence / paragraph / math-exposition craft — DIAGNOSE · REVISE · EXPOSIT |
| `@agent-paper-writing-agent` | research-paper structure & contribution framing — DRAFT · REVISE · AUDIT · RESPOND |
| `@agent-proposal-writing-agent` | grant/proposal structure & Specific Aims — DRAFT · REVISE · AUDIT |
| `@agent-classical-mechanics-agent` | first-principles classical mechanics (Goldstein, Arnold) |
| `@agent-orbital-mechanics-agent` | astrodynamics (Wakker, Curtis) |

**Other invocation methods.** Natural language — *"use the orbital-mechanics-agent to…"* routes too. Session-wide — `claude --agent <name>` starts a whole session as that agent.

> **The 7 still-`ready` agents** ([[Gap-Agent]] + 6 domain experts) have no `@agent-` handle yet — call them by name in plain language and the main session loads the persona inline.

> ⚠️ Subagent files are loaded at **session start** — after editing `.claude/agents/`, restart Claude Code for changes to take effect.

---

## Purpose

A single front-door reference for **every agent in `wiki/agents/`**: what it is, its
modes (if any), when to use each mode, what input it needs, and how to invoke it.

This is a **derived doc.** Each agent's own file in `wiki/agents/` is the source of
truth; this manual aggregates them. The `## Agents` section of [[index]] is the
one-line registry; [[book-routing-map]] is the book-query routing table. This manual
is the operational how-to that sits between them.

---

## How agents work (read first)

- **Agent files are retrieval-persona definitions, not pre-wired bots.** Each file is a
  system prompt + corpus pointers + standing questions + book-query routing +
  cross-claims. There is no separate runtime.
- **To run an agent**, its definition file is loaded and the persona is adopted — either
  **inline** (interactive, in the main thread, so you can iterate turn-by-turn) or as an
  **isolated subagent** (a clean-room run). Multi-agent "council" sessions are filed to
  `wiki/agent_outputs/` and referenced from [[log]].
- **Status legend** (the `status:` frontmatter field):
  - `draft` — under construction; corpus incomplete.
  - `ready` — corpus complete, persona + commitments defined; **not yet wired** (`tools: []`).
  - `active` — wired with tools and in use.
  - **5 agents are `active`** — the three writing agents, [[Classical-Mechanics-Agent]],
    and [[Orbital-Mechanics-Agent]] — wired as Claude Code subagents in `.claude/agents/`
    (invoke with `@agent-<name>`, e.g. `@agent-orbital-mechanics-agent`). The other
    **7 remain `ready`**. Each `.claude/agents/` file is a thin loader that reads the
    agent's full `wiki/agents/` definition at run time, so the wiki file stays the single
    source of truth.
- **Two-tier corpus** (every agent has both):
  - **Tier 1** — wiki summary pages (`wiki/papers/`, `wiki/concepts/`, etc.), retrieved at
    query time. Answers "what exists / how does X relate to CTPC."
  - **Tier 2** — raw books/guides (`raw/books/`, `raw/writing/.../guides/`), read directly
    via the agent's `# Book query protocol`. Answers "derive / prove / verify from first
    principles." A Tier-1 summary is **not** a substitute for a Tier-2 derivation.
- **Modes are not universal.** Only the **3 writing agents** have explicit modes.
  **Gap-Agent** is generation-only. The **8 domain agents** are query-answering personas
  with no mode switch — you pose a claim, they audit it.

---

## Quick reference — all 12 agents

| Agent                         | Family             | Layer | Modes / Operations               | Status | Domain                                        |
| ----------------------------- | ------------------ | ----- | -------------------------------- | ------ | --------------------------------------------- |
| [[Generic-Writing-Agent]]     | Writing            | 0     | DIAGNOSE · REVISE · EXPOSIT      | active | universal prose & math-exposition craft       |
| [[Paper-Writing-Agent]]       | Writing            | 1a    | DRAFT · REVISE · AUDIT · RESPOND | active | research-paper authoring (venue-agnostic)     |
| [[Proposal-Writing-Agent]]    | Writing            | 1b    | DRAFT · REVISE · AUDIT           | active | grant/proposal authoring (funder-agnostic)    |
| [[Gap-Agent]]                 | Generation         | —     | generation-only (no modes)       | ready  | candidate research-direction generation       |
| [[Classical-Mechanics-Agent]] | Domain (mechanics) | 0     | — query persona                  | active | the three formalisms of classical mechanics   |
| [[Orbital-Mechanics-Agent]]   | Domain (mechanics) | 1     | — query persona                  | active | astrodynamics / orbital mechanics             |
| [[Hamiltonian-NN-Agent]]      | Domain             | —     | — query persona                  | ready  | HNN/LNN/PHNN/D-HNN energy architectures       |
| [[Neural-ODE-Agent]]          | Domain             | —     | — query persona                  | ready  | Neural ODE / NCDE continuous-time substrate   |
| [[Geometric-DL-Agent]]        | Domain             | —     | — query persona                  | ready  | equivariant / group-convolution architectures |
| [[ML-Theory-Agent]]           | Domain             | —     | — query persona                  | ready  | learning theory & uncertainty quantification  |
| [[Physics-Informed-Agent]]    | Domain             | —     | — query persona                  | ready  | soft-vs-hard physics-informed ML              |
| [[Interpretability-Agent]]    | Domain             | —     | — query persona                  | ready  | Barbiero symmetries / CBM interpretability    |

---

## Family 1 — Writing agents (3)

A three-layer stack. **Layer 0 ([[Generic-Writing-Agent]])** owns universal prose craft;
**Layer 1a ([[Paper-Writing-Agent]])** and **Layer 1b ([[Proposal-Writing-Agent]])** own
genre structure and **delegate every sentence-, paragraph-, and math-exposition-level
question down to Layer 0**. Both Layer-1 agents refuse venue/funder-specific format
questions (page limits, NSF PAPPG, BAA rules) — those are a deferred "Layer 2."

### Generic-Writing-Agent — Layer 0

| Operation    | When to use                              | Input it needs                               | What it returns                                                                                |
| ------------ | ---------------------------------------- | -------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **DIAGNOSE** | You want failures *named*, not yet fixed | a prose or math passage                      | a list of clarity / cohesion / concision / exposition failures, each grounded in a corpus page |
| **REVISE**   | You want the passage *rewritten*         | a passage                                    | a clarity rewrite, with any meaning-changing edit explicitly flagged                           |
| **EXPOSIT**  | The passage is *mathematical*            | mathematical prose (notation, theorem/proof) | notation discipline, symbol-introduction, quantifier precision, equation-embedding fixes       |

Owns sentence/paragraph construction, clarity, cohesion, concision, narrative flow, math
exposition. Does **not** own genre structure. Refuses inventing style rules and fixing
surface grammar while a structural clarity failure stands.

### Paper-Writing-Agent — Layer 1a

| Mode        | When to use                           | Input it needs              | What it returns                                                                                                       |
| ----------- | ------------------------------------- | --------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **DRAFT**   | Starting a new paper                  | topic + rough contributions | section skeleton + one-line intent per section + refutable-contribution list (confirm, then prose section-by-section) |
| **REVISE**  | A draft exists, needs structural work | a draft or outline          | structural revision — section order, contribution alignment, narrative arc, figure-first design                       |
| **AUDIT**   | Near-final, pre-submission            | a near-final draft          | [[Ramdas-Paper-Checklist]] + [[Langley-Crafting-ML-Papers]] checks; the predicted dominant objection                  |
| **RESPOND** | Reviews are back                      | reviews + the draft         | a response-to-reviewers letter (5 opening moves + per-comment sub-moves) + the revision plan                          |

Owns paper structure, section anatomy, contribution framing, narrative arc, evaluation
*presentation*, the pre-submission audit. `priority_doc` = the active manuscript
([[CTPC-KDD-Submission]]). Delegates prose craft to [[Generic-Writing-Agent]]; refuses
chronological organization, non-refutable contributions, bake-off-only evaluation,
hostages to fortune, and venue-specific questions.

### Proposal-Writing-Agent — Layer 1b

| Mode | When to use | Input it needs | What it returns |
|---|---|---|---|
| **DRAFT** | Starting a proposal | a proposal idea + the funder | the one-page Specific Aims / problem statement *first*; confirmed, then built outward |
| **REVISE** | A proposal draft exists | a proposal draft | structural revision — persuasive arc, problem-before-solution order, significance framing |
| **AUDIT** | Pre-submission | a proposal draft | the three-reviewer-questions check + weakness-preemption check + academic-habit scan |

Owns proposal structure, Specific-Aims framing, the persuasive case, significance,
weakness-preemption, funder-and-audience alignment. Delegates prose craft to
[[Generic-Writing-Agent]]; refuses the Bizzwidget Problem (solution before problem),
academic-mode defaults, vague methodology, hidden weaknesses, and funder-specific
questions.

> Both Layer-1 agents seed their DRAFT mode from [[Draft-Section-Scaffolds]] — the
> consolidated paper (IMRD) and grant (Kraicer-spine) section skeletons.

---

## Family 2 — Generation agent (1)

### Gap-Agent

**No modes — generation-only.** You point it at the SiS design space; it emits candidate
research directions and never verifies them. Every output uses a **mandatory format**:

```
HYPOTHESIS · COMBINES · ADDRESSES · EXPERIMENT · VERIFY WITH · CONFIDENCE (low | medium)
```

Three standing task categories:

1. **Hard-constraint generation** — for `hard_constraint_possible: partial` pages, propose
   the missing architectural mechanism.
2. **Q-item discriminating experiments** — for each open Q-item in [[CTPC-Design-Rationale]],
   propose the *smallest* test that resolves it.
3. **Synthesis-gap hypotheses** — for each gap flagged in `wiki/connections/`, propose one
   concept-combining hypothesis.

**When to use:** you want candidate directions or next experiments — not answers.
**Hard refusals:** never `CONFIDENCE: high`, never a hypothesis without an `EXPERIMENT`,
never an unflagged out-of-corpus combination, never closes a Q-item. Routes every
hypothesis's verification to a domain agent via the `VERIFY WITH` field.

> The task counts inside `Gap-Agent.md` are a **2026-05-13 snapshot** — re-scan the
> `partial` pages and open Q-items before relying on them.

---

## Family 3 — Domain expert agents (8)

### How to use a domain agent

Domain agents **have no modes.** Usage is uniform:

1. **Pose a methodology claim or a domain question** — e.g. "we enforce dissipation with
   a soft loss penalty" or "derive the J2 secular nodal-regression rate."
2. The agent audits it against its **5 Standing Questions** and its **core commitments**,
   grounding the answer in **Tier-1** wiki pages.
3. When the question needs first-principles derivation, it triggers a **Book query**
   (Tier-2) — reading raw textbook chapters directly and citing verified equation numbers.

Each agent has a `priority_doc` — its "home" section of the SiS design space — and a set
of explicit refusals and deferrals.

### The mechanics sub-hierarchy

Two of the eight form a parent/child pair: **[[Classical-Mechanics-Agent]] (Layer 0)**
owns the three formalisms as physics; **[[Orbital-Mechanics-Agent]] (Layer 1)** owns the
astrodynamics *application* and defers every first-principles formalism derivation up to
the parent — the same parent/child pattern as the writing stack.

### Domain agent reference

| Agent | Owns | "Home" in CTPC design space | Tier-2 books | Defers to |
|---|---|---|---|---|
| [[Classical-Mechanics-Agent]] | the three formalisms (Newton/Lagrange/Hamilton), Legendre transform, Noether, symplectic structure, Rayleigh dissipation; physics-correctness of every conservation claim wiki-wide | Core Design Philosophy + D1 | Goldstein, Arnold | Orbital-Mechanics (application), Hamiltonian-NN (architecture), Geometric-DL (NN symmetry), Neural-ODE (integrators) |
| [[Orbital-Mechanics-Agent]] | astrodynamics — two-body/Kepler, orbital elements, perturbations (J2/J3/drag/SRP/3-body), reference frames, orbit determination, relative motion, attitude, conjunction analysis, SGP4/GMAT/TLE physics | D6 / Q6 / Q7 | Wakker, Curtis (+ orbital chapters of Goldstein/Arnold) | up to Classical-Mechanics (formalism); Hamiltonian-NN & Neural-ODE (architecture); ML-Theory (UQ) |
| [[Hamiltonian-NN-Agent]] | HNN / LNN / PHNN / D-HNN energy-based architectures, Cholesky-PSD dissipation, discrete-time passivity | Q8 (Q8a & Q8b-vector-field closed; Q8b-discrete-time open) | van der Schaft & Jeltsema (Port-Hamiltonian), Khalil | Classical-Mechanics (formalism) |
| [[Neural-ODE-Agent]] | Neural ODE / NCDE, adjoint sensitivity method, continuous-time substrate, integrator choice | D1 / D2 / D7 / Q4 / Q8b-discrete-time | Khalil, Tu, Goldstein Ch 11, GP App B | Hamiltonian-NN (symplectic side of Q8b) |
| [[Geometric-DL-Agent]] | equivariant NN layers, group convolution, irreps-as-architecture, PhyArch | Q1 / Q2 | Tu (Intro to Manifolds), Port-Hamiltonian Ch 3, GP Ch 4 | Classical-Mechanics (physics of symmetry) |
| [[ML-Theory-Agent]] | statistical learning theory + principled UQ — calibration, generalization, PAC-Bayes, the Student-t head | D3 / D4 / D5 | Rasmussen-Williams GP, Shalev-Shwartz, Goodfellow, Murphy Vol 1 & 2 | empirical formal-verification to the GAINS / SafeDNN paper corpus |
| [[Physics-Informed-Agent]] | soft-vs-hard PIML axis, PINN failure modes, PeRCNN physics-encoding | Core Design Philosophy (hard / soft / residual hierarchy) | Khalil Ch 4 (rarely — paper-served) | Hamiltonian-NN (dissipative stability & passivity) |
| [[Interpretability-Agent]] | Barbiero's four symmetries, CBM, concept-closure, structural invariance | Part III of [[CTPC-Design-Rationale]] | mostly paper-served; GP Ch 2 | Hamiltonian-NN (Khalil Lyapunov/passivity), Classical-Mechanics (mechanics) |

---

## Routing — which agent for which question

| If the question is about… | Route to |
|---|---|
| sentence / paragraph / math-exposition craft | [[Generic-Writing-Agent]] |
| research-paper structure, contribution framing, reviewer response | [[Paper-Writing-Agent]] |
| grant/proposal structure, Specific Aims, funder fit | [[Proposal-Writing-Agent]] |
| a new research direction or the next experiment | [[Gap-Agent]] |
| Newton/Lagrange/Hamilton first principles, Noether, symplectic structure, canonical transforms | [[Classical-Mechanics-Agent]] |
| orbital elements, perturbations, reference frames, conjunction analysis, SGP4/TLE | [[Orbital-Mechanics-Agent]] |
| HNN/LNN/PHNN/D-HNN, Cholesky-PSD dissipation, integrator-discretization passivity | [[Hamiltonian-NN-Agent]] |
| NODE/NCDE substrate, adjoint method, Latent NCDE Corrector | [[Neural-ODE-Agent]] |
| equivariance, SO(3)/SE(3), group convolution, PhyArch | [[Geometric-DL-Agent]] |
| UQ, calibration, Student-t, generalization bounds, OOD | [[ML-Theory-Agent]] |
| PINN failure modes, soft-vs-hard hierarchy, PeRCNN | [[Physics-Informed-Agent]] |
| Barbiero symmetries, CBM, concept-closure, "is this interpretable?" | [[Interpretability-Agent]] |

A question that spans domains goes to a **council** — convene all the relevant agents;
file the session output to `wiki/agent_outputs/`.

---

## Delegation graph

- **Writing:** [[Paper-Writing-Agent]] and [[Proposal-Writing-Agent]] delegate all
  prose / math-exposition craft *down* to [[Generic-Writing-Agent]].
- **Mechanics:** [[Orbital-Mechanics-Agent]] defers every first-principles formalism
  derivation *up* to [[Classical-Mechanics-Agent]].
- **Architecture vs physics:** [[Hamiltonian-NN-Agent]], [[Geometric-DL-Agent]], and
  [[Neural-ODE-Agent]] own the *architecture*; [[Classical-Mechanics-Agent]] owns whether
  the underlying *physics* is stated correctly.
- **Dynamics vs uncertainty:** [[Orbital-Mechanics-Agent]] supplies the dynamics;
  [[ML-Theory-Agent]] owns the uncertainty calculus propagated through them.
- **Generation vs verification:** [[Gap-Agent]] generates; a domain agent named in its
  `VERIFY WITH` field verifies. Gap-Agent never self-verifies.

---

## Activation — promoting `ready` → `active`

A `ready` agent has a complete corpus and a defined persona but is not wired. To
activate one:

1. Fill the `tools:` frontmatter list (`[]` on every not-yet-wired agent).
2. Point `priority_doc` at the live artifact it should serve (e.g. an active manuscript
   for [[Paper-Writing-Agent]], an active solicitation for [[Proposal-Writing-Agent]]).
3. Set `status: active`.

Until then, an agent is exercised by **loading its persona and running it on real input**
— that is the test step, and it is how you confirm a `ready` agent behaves before
committing it to `active`.

---

## Maintenance

This manual is **derived**. When an agent file's modes, status, corpus, `priority_doc`,
or delegation change, update this manual and the `## Agents` section of [[index]]
together. The per-agent file in `wiki/agents/` is always the source of truth.

## Sources

Derived from the 12 agent definition files in `wiki/agents/`
([[Generic-Writing-Agent]], [[Paper-Writing-Agent]], [[Proposal-Writing-Agent]],
[[Gap-Agent]], [[Classical-Mechanics-Agent]], [[Orbital-Mechanics-Agent]],
[[Hamiltonian-NN-Agent]], [[Neural-ODE-Agent]], [[Geometric-DL-Agent]],
[[ML-Theory-Agent]], [[Physics-Informed-Agent]], [[Interpretability-Agent]]), plus
[[book-routing-map]] and the `## Agents` section of [[index]].
