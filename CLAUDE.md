# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this is

A personal alignment-research workspace. It wraps the `jacobian-lens`
interpretability tool as a submodule and maintains an LLM-maintained
knowledge wiki alongside it: for gathering research on relevant topics and
for tracking experiments run in this project.

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
- `llm_wiki_instructions.md` — the pattern the wiki below follows: raw
  sources (immutable) → LLM-maintained wiki (summaries, entity/concept
  pages, `index.md`, `log.md`) → this schema file. Read it for the *why*;
  this CLAUDE.md section is the concrete instantiation for *this* project.
- `wiki/` — the knowledge base. See "The wiki" below.
- `raw/` — immutable raw sources the wiki is built from. See
  [`raw/README.md`](raw/README.md).
- `README.md` — project overview and setup (submodule clone instructions).
- `LICENSE` — Apache License 2.0.
- `pyproject.toml` / `.venv/` — the sandbox. See "Sandbox" below.

## The wiki

Two uses: (1) general research on topics relevant to this project
(interpretability, alignment, global-workspace-style theories, etc.), and
(2) tracking experiments run here (lens fits, applications, evaluations).
Layout:

```
wiki/
  index.md          # content-oriented catalog — read this first on any query
  log.md            # chronological, append-only — grep "^## \[" wiki/log.md
  sources/          # one page per ingested paper/article/transcript/repo
  concepts/         # synthesis pages spanning multiple sources
  experiments/       # our own experiment runs — the lab-notebook layer
  entities/         # tools, models, people, orgs, papers-as-artifacts
raw/                # immutable raw source files, referenced by wiki/sources/
```

Each subdirectory has its own `README.md` with the frontmatter schema and
filename convention for that page type — read the relevant one before
creating a page there (`wiki/sources/README.md`, `wiki/concepts/README.md`,
`wiki/experiments/README.md`, `wiki/entities/README.md`).

Workflows, per `llm_wiki_instructions.md`:

- **Ingest** a source: read it, discuss key takeaways with the user, write/
  update its `sources/` page (and any `concepts/`/`entities/` pages it
  touches), update `index.md`, append to `log.md`.
- **Log an experiment**: create/update its `experiments/` page as it
  progresses (`status: planned → running → done`), link findings into the
  relevant `concepts/` pages so they compound rather than staying siloed,
  update `index.md`, append to `log.md`.
- **Query**: check `index.md` first to find relevant pages before searching
  broadly. A substantive answer (comparison, analysis, synthesis) is worth
  filing back into the wiki as a new/updated page rather than left in chat.
- **Lint** (periodically, on request): look for contradictions between
  pages, claims superseded by newer sources, orphan pages, concepts
  mentioned but lacking their own page, missing cross-references.

Cross-reference pages with `[[slug]]` (matching the target file's basename
without `.md`) even if the target doesn't exist yet — that marks it as
worth writing later, not an error.

Currently seeded: one entity page ([[jacobian-lens-tool]]) and one source
stub for its companion paper ([[verbalizable-representations-global-workspace]]),
both derived only from `jacobian-lens/README.md` so far — the paper itself
is still unread. No experiments logged yet.

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

## Sandbox

Managed with `uv`, system Python (3.13.5 via `/home/kelmoir/miniconda3/bin/python3`,
satisfies `jlens`'s `>=3.10` requirement). Root `pyproject.toml` declares
`jlens` as an editable path dependency on `jacobian-lens/`, plus
`jupyter`/`ipykernel` for notebook work; `dev` extra adds `pytest`, `ruff`,
`datasets` (mirrors `jacobian-lens`'s own dev extra).

```bash
uv sync --extra dev      # create/update .venv from pyproject.toml
uv run python ...        # run a script in the sandbox
uv run pytest jacobian-lens/tests  # run jlens's test suite
uv run jupyter lab        # open the walkthrough notebook, or new ones
```

`torch` resolves to a CUDA build (`2.13.0+cu130`); a GPU is present and
`torch.cuda.is_available()` is `True` in this environment. `.venv/` is
gitignored — rerun `uv sync --extra dev` after a fresh clone, or whenever
`jacobian-lens` is bumped to a commit with different dependencies.

## Conventions

- License for all first-party content in this repo: Apache 2.0.
- This CLAUDE.md is meant to co-evolve with the wiki — update it when the
  schema or workflows change, not just when the user asks for it verbatim.
