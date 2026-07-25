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
