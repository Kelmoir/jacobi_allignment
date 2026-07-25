# Experiments

One page per experiment run using `jacobian-lens` (or other tooling in this
project) — lens fits, applications, evaluations, ad hoc investigations.
This is the lab notebook layer: unlike [sources](../sources/) and
[concepts](../concepts/), which synthesize *external* material, experiment
pages record *our own* work and its results.

Filename: `YYYY-MM-DD-short-slug.md` (date-prefixed so the directory itself
sorts chronologically).

Frontmatter:

```yaml
title: "..."
type: experiment
date: YYYY-MM-DD
status: planned | running | done | abandoned
model: ...                    # e.g. Qwen/Qwen3-8B
related_concepts: [...]       # concepts/*.md
related_sources: [...]        # sources/*.md
related_entities: [jacobian-lens-tool]
tags: [...]
```

Body should cover: hypothesis/question, setup (model, lens config, prompts,
code path — link to a notebook or script if one exists, e.g. in
`jacobian-lens/walkthrough.ipynb` or a project-local script), results
(including negative/null results — those are worth keeping), and
interpretation. Link findings back into the relevant
[concept pages](../concepts/) so they accumulate into the synthesis rather
than staying siloed here.

Large artifacts (raw output data, images, checkpoints) shouldn't live in
this markdown page — put them under `raw/` (or wherever is appropriate for
their size) and link to them.

On completion: update `status`, update any touched concept/entity pages, add
to [`../index.md`](../index.md), and append an entry to
[`../log.md`](../log.md) (`## [YYYY-MM-DD] experiment | <title>`).
