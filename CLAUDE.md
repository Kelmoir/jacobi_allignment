# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this is

A personal alignment-research workspace. It wraps the `jacobian-lens`
interpretability tool as a submodule and will grow an LLM-maintained
knowledge wiki alongside it for general knowledge work in this domain.

## Structure

- `jacobian-lens/` — **git submodule** (`git@github.com:anthropics/jacobian-lens.git`).
  Companion code for *Verbalizable Representations Form a Global Workspace in
  Language Models*. Provides the `jlens` package: fits and applies a
  "Jacobian lens" that linearly transports a residual-stream activation to
  the final-layer basis and decodes it with the model's own unembedding, to
  read out what a hidden activation is disposed to make the model say. It is
  a reference implementation upstream ("not maintained and not accepting
  contributions") — treat it as read-only vendored code. Do not edit files
  inside it; if changes are needed, do them as a fork/patch discussion with
  the user first, since submodule edits diverge from upstream and need
  deliberate handling (new remote, patch branch, etc.).
- `llm_wiki_instructions.md` — the pattern this project's wiki will follow
  once built: raw sources (immutable) → LLM-maintained wiki (summaries,
  entity/concept pages, `index.md`, `log.md`) → this schema file, which
  should be updated with wiki-specific conventions (directory layout,
  frontmatter conventions, ingest/query/lint workflows) as soon as the wiki
  is instantiated. Until that happens, this section of CLAUDE.md is a stub.
- `README.md` — project overview and setup (submodule clone instructions).
- `LICENSE` — Apache License 2.0.

## Working with the submodule

```bash
# initial clone
git clone --recurse-submodules <repo-url>

# if already cloned without submodules
git submodule update --init --recursive

# pull upstream updates into jacobian-lens
cd jacobian-lens && git fetch origin && git checkout origin/main
cd .. && git add jacobian-lens && git commit -m "Update jacobian-lens submodule"
```

The outer repo pins `jacobian-lens` to a specific commit (recorded as a
gitlink in the index, tracked via `.gitmodules`) — bumping it is a deliberate
action, not automatic.

## Conventions

- License for all first-party content in this repo: Apache 2.0.
- No wiki exists yet. When the user asks to start it, follow
  `llm_wiki_instructions.md` and update this file with the concrete
  directory layout and workflows agreed on — this file is meant to co-evolve
  with the wiki, not stay static.
