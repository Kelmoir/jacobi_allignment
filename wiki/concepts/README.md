# Concepts

Synthesis pages for ideas/topics that span multiple sources — e.g. "global
workspace hypothesis," "activation steering," "representation reading,"
"linear probing vs. lens-based readout." These are where cross-referencing
and contradiction-flagging matter most: a concept page should reflect
*everything currently known* about it across all ingested sources, not just
the last one read.

Filename: kebab-case slug of the concept name.

Frontmatter:

```yaml
title: "..."
type: concept
tags: [...]
sources: [slug-of-source-1, slug-of-source-2]   # sources/*.md this draws on
updated: YYYY-MM-DD
```

When a new source touches an existing concept, update the concept page in
place — don't just add a new one. Note explicitly if the new source
contradicts or refines an earlier claim on the page, rather than silently
overwriting it.
