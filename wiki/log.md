# Log

Append-only. One entry per ingest/query/lint/experiment, newest last. Each
entry starts with `## [YYYY-MM-DD] <kind> | <title>` so it stays greppable:
`grep "^## \[" wiki/log.md | tail -5`.

## [2026-07-25] setup | Wiki initialized

Set up the three-layer structure (`raw/`, `wiki/`, this project's
`CLAUDE.md`) per `llm_wiki_instructions.md`. Seeded the wiki with a first
entity page for the `jacobian-lens` tool and a source stub for its companion
paper, both derived from `jacobian-lens/README.md` (not yet read: the paper
itself).

## [2026-07-25] ingest | jacobian-lens README

Read `jacobian-lens/README.md` in full. Filed:
[entities/jacobian-lens-tool.md](entities/jacobian-lens-tool.md) (the tool)
and [sources/verbalizable-representations-global-workspace.md](sources/verbalizable-representations-global-workspace.md)
(the companion paper, flagged as unread beyond what the README states).

## [2026-07-25] ingest | Verbalizable Representations Form a Global Workspace in Language Models

Read the paper in full via WebFetch. Flipped
[sources/verbalizable-representations-global-workspace.md](sources/verbalizable-representations-global-workspace.md)
from `status: stub` to `read`, with real authors/date/structure. Refined
[entities/jacobian-lens-tool.md](entities/jacobian-lens-tool.md) with the
precise J-lens formalism, J-space definition, and reading/writing
operations (previously only known via the repo README). Filed the central
thesis, five functional properties (report, modulation, internal
reasoning, generalization, selectivity) and their experiments, structural
layer findings, and stated limitations into new
[concepts/global-workspace-hypothesis.md](concepts/global-workspace-hypothesis.md).

Deferred (flagged on the source page for a future ingest): Alignment
Auditing, The Assistant's Perspective, Counterfactual Reflection Training
— sections the extraction didn't go deep on. Also flagged: the README's
"~100 prompts" sample-efficiency claim (attributed to paper §9.3) wasn't
confirmed in the fetched text — likely appendix content not captured by
the fetch.
