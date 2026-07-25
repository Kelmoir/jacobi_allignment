# Entities

Pages for concrete named things: tools, models, people, orgs, papers-as-
artifacts (as opposed to [sources](../sources/), which are about *ingesting*
a document — an entity page for a paper is about the paper as a reference
point other pages link to, not a processing record).

Filename: kebab-case slug, suffixed with what it is if the bare name is
ambiguous (e.g. `jacobian-lens-tool.md` vs. a hypothetical
`jacobian-lens-concept.md`).

Frontmatter:

```yaml
title: "..."
type: entity
entity_type: tool | model | person | org | paper
tags: [...]
sources: [...]     # sources/*.md that discuss this entity
updated: YYYY-MM-DD
```
