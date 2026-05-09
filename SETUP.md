# SiS Knowledge Base — Mac Setup Guide

## What You're Building

```
Your Mac
├── ~/sis-wiki/
│   ├── CLAUDE.md          ← the brain (agent schema)
│   ├── AGENTS.md          ← symlink (for Codex/OpenCode compatibility)
│   ├── raw/               ← your books and papers (immutable)
│   └── wiki/              ← LLM-generated, compounds over time
│       ├── index.md
│       ├── log.md
│       ├── concepts/
│       ├── architectures/
│       ├── papers/
│       ├── books/
│       ├── connections/
│       └── sis/
└── Obsidian Vault
    └── (points to ~/sis-wiki/wiki/)   ← browse the wiki visually
```

Claude Code is the programmer. Obsidian is the IDE. The wiki is the codebase.

---

## Step 1 — Install Claude Code

```bash
# Requires Node.js >= 18. Check your version:
node --version

# If you don't have Node, install via Homebrew:
brew install node

# Install Claude Code globally:
npm install -g @anthropic/claude-code

# Verify:
claude --version
```

Log in when prompted — it uses your Anthropic account (same as claude.ai).

---

## Step 2 — Create the Wiki Directory

```bash
# Create the wiki root
mkdir -p ~/sis-wiki/raw/books/differential-geometry
mkdir -p ~/sis-wiki/raw/books/classical-mechanics
mkdir -p ~/sis-wiki/raw/books/dynamical-systems
mkdir -p ~/sis-wiki/raw/books/ml-theory
mkdir -p ~/sis-wiki/raw/papers/neural-odes
mkdir -p ~/sis-wiki/raw/papers/geometric-dl
mkdir -p ~/sis-wiki/raw/papers/port-hamiltonian
mkdir -p ~/sis-wiki/raw/papers/sda
mkdir -p ~/sis-wiki/raw/notes
mkdir -p ~/sis-wiki/wiki/concepts
mkdir -p ~/sis-wiki/wiki/architectures
mkdir -p ~/sis-wiki/wiki/papers
mkdir -p ~/sis-wiki/wiki/books
mkdir -p ~/sis-wiki/wiki/connections
mkdir -p ~/sis-wiki/wiki/sis
mkdir -p ~/sis-wiki/assets/figures

# Initialize git (optional but strongly recommended — free version history)
cd ~/sis-wiki
git init
echo "assets/figures/*.png" >> .gitignore
git add .
git commit -m "init: wiki scaffold"
```

---

## Step 3 — Place CLAUDE.md

Copy the CLAUDE.md file (provided separately) into `~/sis-wiki/`:

```bash
cp /path/to/CLAUDE.md ~/sis-wiki/CLAUDE.md

# Create the symlink for Codex/OpenCode compatibility
ln -s CLAUDE.md ~/sis-wiki/AGENTS.md
```

---

## Step 4 — Set Up Obsidian (the Visual Interface)

1. Download Obsidian from https://obsidian.md (free)
2. Open Obsidian → "Open folder as vault"
3. Select `~/sis-wiki/wiki/` as the vault root
4. Recommended plugins to install (Settings → Community plugins):
   - **Dataview** — query your wiki pages like a database
   - **Graph view** (built-in) — see connections between pages visually
   - **Marp** — generate slide decks from wiki markdown
   - **Obsidian Web Clipper** (browser extension) — clip web articles to raw/

In Obsidian Settings → Files and links:
- Set "Attachment folder path" to `../assets/figures`
- Enable "Automatically update internal links"

---

## Step 5 — First Claude Code Session

```bash
cd ~/sis-wiki
claude
```

Claude Code opens in your terminal. It reads CLAUDE.md automatically because it's
in the working directory. You should see it acknowledge the schema.

**Bootstrap the wiki:**

```
> bootstrap the wiki
```

Claude Code will create `wiki/index.md` and `wiki/log.md` with the correct structure.

---

## Step 6 — Add Your First Source

Place a PDF or markdown file into the appropriate `raw/` subdirectory:

```bash
# Example: place Arnol'd chapter into raw/
cp ~/Downloads/arnold-classical-mechanics-ch1.pdf \
   ~/sis-wiki/raw/books/classical-mechanics/
```

Then in Claude Code:

```
> ingest: raw/books/classical-mechanics/arnold-classical-mechanics-ch1.pdf
```

Claude Code will:
1. Read the chapter
2. Discuss key ideas with you
3. Create wiki pages in `wiki/books/` and `wiki/concepts/`
4. Update `wiki/index.md` and `wiki/log.md`

You will see the new pages appear in Obsidian in real time.

---

## Daily Workflow

### Adding a new source
```
> ingest: raw/papers/geometric-dl/bronstein-gdl-ch3.pdf
```

### Asking a question (answer gets filed back into wiki)
```
> How does SO(3) equivariance relate to our RTN decomposition in CTPC?
```

### Health check
```
> lint
```

### Searching the wiki
```
> search: Port-Hamiltonian
```

### Generating a summary for a meeting
```
> synthesize everything we know about uncertainty propagation into a 
  one-page summary for an AFRL audience
```

---

## Tips Specific to Your Setup

### ISU Nova HPC integration
If you want Claude Code to also work with code on Nova, you can run it locally
and point it at SSH-mounted directories, or run it directly on Nova via SSH.
Your existing ControlMaster config means this is low-friction.

### Obsidian Web Clipper for papers
Install the browser extension. When you find a paper on arXiv or a relevant
blog post, clip it directly to `raw/papers/` with one click, then ingest it.

### Keeping raw/ lean
Don't put full textbooks in raw/ if you only need specific chapters.
Clip the relevant chapters as PDFs. Claude Code's context window handles
20-40 pages comfortably per session.

### Git commits after each session
```bash
cd ~/sis-wiki
git add wiki/
git commit -m "ingest: Arnol'd Ch.1 — symplectic geometry"
```
This gives you a timeline of your knowledge base's evolution.

### Token budgeting
For long books, tell Claude Code explicitly:
```
> ingest: raw/books/lee-smooth-manifolds-ch2.pdf
> Focus on: manifolds, tangent spaces, vector fields. 
> Skip: algebraic topology prerequisites.
```
This keeps sessions focused and prevents context overflow.

---

## Recommended First 5 Ingests (in order)

1. **Arnol'd Ch. 1-2** — Lagrangian mechanics, variational principles
   → seeds: Lagrangian NN page, CTPC physics predictor foundation

2. **Bronstein et al. GDL Ch. 3** — Symmetry and equivariance
   → seeds: SO(3) equivariance page, Schur's Lemma page

3. **Greydanus et al. HNN paper** — Hamiltonian Neural Networks (NeurIPS 2019)
   → seeds: HNN architecture page, energy conservation as hard constraint

4. **Kidger et al. Neural CDE paper** — Neural CDEs
   → seeds: CDE page, connection to CTPC corrector

5. **Tu — Introduction to Manifolds Ch. 1** — smooth manifolds gently
   → seeds: manifold page, tangent bundle page

Each of these will cross-link heavily and the wiki will start feeling alive
after just these five sessions.

---

## Troubleshooting

**Claude Code can't read my PDF**
```bash
# Install pymupdf for PDF text extraction
pip install pymupdf --break-system-packages
```

**Context overflow on large chapters**
Split the chapter: "ingest pages 1-30 of this PDF only"

**Obsidian not showing new pages**
Force refresh: Cmd+R in Obsidian

**Claude Code not finding CLAUDE.md**
Make sure you launched it from ~/sis-wiki/:
```bash
cd ~/sis-wiki && claude
```
