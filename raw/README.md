# Raw sources

Immutable source material — the wiki reads from this layer but never edits
it. Papers, articles, transcripts, images, exported data. If a source has a
stable canonical URL, a raw copy isn't required (link to it from the
[wiki source page](../wiki/sources/) directly); download a local copy here
when the source might disappear, is large/binary (PDFs, images), or was
clipped from the web (e.g. via Obsidian Web Clipper).

Suggested subdirectories, created as needed rather than upfront:

- `papers/`, `articles/`, `transcripts/` — by source type
- `assets/` — images/attachments referenced by clipped articles
- `experiment-data/` — raw output artifacts from a
  [wiki experiment](../wiki/experiments/) too large/binary for the markdown
  page itself (link to it from the experiment page)

Nothing here is committed to git blindly — check size/sensitivity before
adding large binaries; consider `.gitignore` or Git LFS if this grows large.
