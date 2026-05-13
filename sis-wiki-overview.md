# sis-wiki — High-Level Overview for claude.ai Agentic Workflow

## What sis-wiki/ is

A **personal research wiki** maintained by an LLM (per `CLAUDE.md`), not a static
knowledge base. It runs on a three-layer pattern adapted from Karpathy's LLM-wiki idea:

- **`raw/`** — immutable canonical sources (PDFs of papers, books, Bilal's drafts)
- **`wiki/`** — the artifact the agent compounds over time — markdown pages with YAML
  frontmatter, wikilinks, and a parseable index/log
- **`CLAUDE.md`** — the operational schema (~20KB) defining every workflow

`AGENTS.md` is a symlink to `CLAUDE.md` (Codex/OpenCode compatibility).

---

## Top-level layout

```
sis-wiki/
├── CLAUDE.md                          ← agent schema (the contract)
├── AGENTS.md → CLAUDE.md              ← symlink
├── llm-wiki_Karpathy_instructions.md  ← meta-doc / philosophical north star
├── SETUP.md
├── raw/                               ← sources (gitignored, local-only)
│   ├── _inbox/arxiv-sweep-YYYY-MM-DD/ ← transient weekly arXiv drops (19 PDFs + digest.md right now)
│   ├── books/{classical-mechanics, differential-geometry, dynamical-systems, ml-theory}/
│   ├── papers/{neural-odes, geometric-dl, port-hamiltonian, physics-informed,
│   │           neural-operators, symbolic-physics, interpretability,
│   │           uncertainty-propagation, formal-verification, my-papers, sda}/
│   ├── notes/
│   └── Proposals/{AFRL_Proposal, CTPC_verify, DiffSWM, Multi_Agent, NOAA_Proposal, review_papers}/
├── wiki/
│   ├── index.md                       ← auto-updated master index (31 pages today)
│   ├── log.md                         ← append-only parseable operation log
│   ├── concepts/      (11 pages)      ← math / physics primitives
│   ├── architectures/ (6 pages)       ← NN architecture patterns
│   ├── papers/        (8 pages)       ← one summary per ingested paper
│   ├── books/         (0 pages, empty)
│   ├── connections/   (1 page)        ← cross-cutting synthesis
│   ├── sis/           (5 pages)       ← SiS / CTPC design decisions
│   └── .obsidian/                     ← Obsidian vault config (wiki is an Obsidian vault)
└── assets/figures/
```

Totals: **173 source files** under `raw/`, **31 wiki pages**.

---

## What the agent is optimized to do

Four named workflows in `CLAUDE.md`, each invokable by phrase:

| Trigger | Workflow |
|---|---|
| `ingest: <path>` | Read a `raw/` source → write summary + concept pages → inject wikilinks → update `index.md` → append to `log.md` |
| Question | Read `index.md` → relevant pages → answer with citations to wiki pages, not raw sources; offer to file a synthesis page |
| `lint` | Health check: orphans, missing frontmatter, contradictions, stale pages, `hard_constraint_possible: unknown`, missing cross-links |
| Triage | `raw/_inbox/` PDFs only get ingested *after* human moves them to canonical `raw/papers/<topic>/` |

---

## The thematic spine (for retrieval / context-picking)

The wiki is a focused research thread, not a general KB. Everything orbits these axes:

- **CTPC** (Continuous-Time Probabilistic Corrector) — Bilal's core framework (KDD '26 submission)
- **Physics-as-Architecture** — hard constraints baked into nets, never soft loss penalties
- **Dissipative HNN routes** — Helmholtz (D-HNN, closed Q8a) vs J-R structure
  (Dissipative SymODEN / PHNN, closed Q8b-vector-field; Q8b-discrete-time open)
- **Barbiero four-symmetry interpretability** — the framework
  `CTPC-Design-Rationale.md` Part III is built on
- **SDA / orbital mechanics** as the application domain (SGP4, TLE, GMAT, RTN frame)

The `sis/CTPC-Design-Rationale.md` page is the load-bearing synthesis doc
(sources: 7) — most ingests update it.

---

## Things to wire into an agentic workflow

1. **`wiki/index.md` is the entrypoint** — line-per-page, machine-parseable, includes
   `sis_relevance` + source count. Cheap to load into context.
2. **`wiki/log.md` is greppable** — `## [YYYY-MM-DD] <op> | <title> | <summary>` format.
   Good for "what's been ingested" / "what changed when" queries.
3. **Frontmatter is structured**:
   `title, tags, sources, created, updated, sis_relevance (none|low|medium|high|critical),
   hard_constraint_possible (yes|no|partial|unknown)` — usable as filter keys.
4. **Wikilinks use hyphenated filenames** (`[[Pi-Block-Polynomial-Approximator]]`),
   not YAML titles — critical for any agent that resolves links.
5. **`_inbox/` is human-triage-only** — agents must refuse `ingest: raw/_inbox/...`.
6. **`raw/` is gitignored**; the wiki travels via git, sources stay local.
