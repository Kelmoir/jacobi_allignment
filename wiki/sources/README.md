# Sources

One page per ingested source (paper, article, transcript, repo README,
etc.). Filename: kebab-case, short slug of the title (no date prefix —
`date_added` in frontmatter covers ordering).

Frontmatter:

```yaml
title: "..."
type: source
source_type: paper | article | transcript | repo | other
url: ...            # or `path:` if it's a raw/ file with no canonical URL
status: stub | read | superseded
date_added: YYYY-MM-DD
tags: [...]
```

`status: stub` means we know *about* the source (e.g. via a citation or a
companion repo's README) but haven't actually read it — the page should say
so explicitly and list what a real ingest still needs to cover. Flip to
`read` once it's been properly processed.

On ingest: read the source, write/update this page, link it from any
[concept](../concepts/) or [entity](../entities/) pages it touches, add it
to [`../index.md`](../index.md), and append an entry to
[`../log.md`](../log.md).
