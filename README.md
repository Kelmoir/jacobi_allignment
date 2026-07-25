# Jacobi Alignment Experiment

Personal alignment research workspace built around the [Jacobian
lens](https://github.com/anthropics/jacobian-lens) interpretability tool,
paired with a self-maintaining LLM wiki for general knowledge work.

## Contents

- **[`jacobian-lens/`](jacobian-lens/)** — git submodule, companion code for
  [*Verbalizable Representations Form a Global Workspace in Language
  Models*](https://transformer-circuits.pub/2026/workspace/index.html). Reads
  out what an internal activation is disposed to make a model say, by
  transporting a residual-stream vector to the final-layer basis via the
  average input-output Jacobian and decoding it with the model's own
  unembedding. See its own README for install/usage.
- **`llm_wiki_instructions.md`** — the pattern doc this project's knowledge
  base (once built) will follow: an LLM-maintained, interlinked markdown wiki
  layered over curated raw sources, rather than query-time RAG. See that file
  for the full idea.
- **Wiki** (not yet created) — will hold the general-knowledge-work
  companion wiki for this project, plus a `CLAUDE.md`-driven schema once
  instantiated.

## Setup

Clone with submodules:

```bash
git clone --recurse-submodules <this-repo-url>
```

Or, if already cloned:

```bash
git submodule update --init --recursive
```

To work on `jacobian-lens` itself, see its
[README](jacobian-lens/README.md) and `pyproject.toml` (Python >= 3.10, `uv`
for dependency management).

## License

Apache License 2.0 — see [LICENSE](LICENSE). `jacobian-lens` carries its own
Apache-2.0 license as a submodule.
