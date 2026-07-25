---
updated: 2026-07-25
---

# Wiki Index

Catalog of everything in the wiki. Read this first when answering a query,
then drill into the linked pages. Updated on every ingest.

## Entities

Tools, models, papers-as-artifacts, people, orgs.

- [jacobian-lens (tool)](entities/jacobian-lens-tool.md) — the `jlens`
  Python package/repo (submodule at [`../jacobian-lens/`](../jacobian-lens/)):
  fits and applies a Jacobian lens to decoder transformers, reading out and
  manipulating the "J-space" described in
  [global-workspace-hypothesis](concepts/global-workspace-hypothesis.md).

## Sources

One page per ingested paper, article, transcript, etc.

- [Verbalizable Representations Form a Global Workspace in Language Models](sources/verbalizable-representations-global-workspace.md) —
  Anthropic, 2026-07-06. The paper `jacobian-lens` is companion code for.
  **Read.** Central thesis and evidence synthesized into
  [global-workspace-hypothesis](concepts/global-workspace-hypothesis.md);
  Alignment Auditing, Assistant's Perspective, and Counterfactual
  Reflection Training sections still deferred (see the source page).

## Concepts

Synthesis pages for ideas/topics that span multiple sources.

- [Global workspace hypothesis (in language models)](concepts/global-workspace-hypothesis.md) —
  LLMs maintain a small, verbalizable, steerable "J-space" atop automatic
  processing; five functional properties (report, modulation, internal
  reasoning, generalization, selectivity) and structural layer evidence,
  from the paper above.

## Experiments

Runs and findings using `jacobian-lens` or other tooling in this project.
See [wiki/experiments/README.md](experiments/README.md) for the page
convention.

*(none yet)*

## Meta

- [log.md](log.md) — chronological record of ingests/queries/lints.
- [../llm_wiki_instructions.md](../llm_wiki_instructions.md) — the pattern
  this wiki follows.
