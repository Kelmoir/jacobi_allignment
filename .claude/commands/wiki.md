---
description: Wiki agent interface — ingest a source, answer a query, or lint the wiki
argument-hint: [ingest|query|lint] [details...]
---

# Wiki agent

Arguments: `$ARGUMENTS`

The operation is the first word of the arguments above, case-insensitive:
`ingest`, `query`, or `lint`. Everything after it is that operation's input
(a source to ingest, a question to answer, or lint scope/focus — may be
empty).

If the first word isn't exactly one of those three (including if no
arguments were given), stop and ask the user which of the three they want
via AskUserQuestion, using the one-line descriptions below as the option
descriptions. Do not guess which operation was meant.

Before doing anything else, if you haven't already this session: read
`llm_wiki_instructions.md` (the pattern behind all of this) and the
`## The wiki` section of `CLAUDE.md` (this project's concrete schema —
directory layout, frontmatter conventions per page type). Then read
`wiki/index.md` regardless of which operation this is — it's the map.

## ingest

*Process a new source into the wiki: read it, discuss it, file it.*

1. Locate the source. If given a path or URL, read it in full — don't
   summarize from a title or abstract alone. If it's an external file
   worth persisting locally (large/binary, or might disappear), save it
   under `raw/` per `raw/README.md`; if it has a stable canonical URL, a
   link from the source page is enough.
2. Discuss key takeaways with the user before writing pages — don't file
   blind on a first pass unless they've said to batch-ingest without
   supervision.
3. Write or update its page in `wiki/sources/`, following
   `wiki/sources/README.md`'s frontmatter and filename convention. If this
   ingest completes an existing `status: stub` page (e.g. one filed from a
   secondhand mention), flip it to `status: read` rather than leaving both.
4. Update every `wiki/concepts/` and `wiki/entities/` page the source
   touches — create new ones where warranted. If it contradicts or refines
   an earlier claim on an existing page, say so explicitly in place; don't
   silently overwrite.
5. Update `wiki/index.md`: add or update the entry under the right
   category.
6. Append to `wiki/log.md`: `## [YYYY-MM-DD] ingest | <title>`, noting what
   was filed and touched.
7. Report back concisely: what got filed, what got updated, what's still
   open (e.g. a concept that deserves its own page but wasn't written yet).

## query

*Answer a question against the wiki, then decide whether the answer is worth keeping.*

1. Read `wiki/index.md` first to find candidate pages — don't grep the
   whole repo or start from raw sources blind.
2. Read the relevant `sources/`, `concepts/`, `entities/`, and
   `experiments/` pages it points to.
3. Synthesize an answer with citations back to specific wiki pages (and,
   through them, to the underlying sources/experiments).
4. If the answer is substantive — a comparison, an analysis, a synthesis
   that took real work — offer to file it back into the wiki as a
   new/updated page (per `llm_wiki_instructions.md`: good answers shouldn't
   disappear into chat history). Ask first; not every query is worth a
   permanent page. If filed, update `wiki/index.md` and append to
   `wiki/log.md` (`## [YYYY-MM-DD] query | <title>`).

## lint

*Health-check the wiki.*

Scan `wiki/` (all of `sources/`, `concepts/`, `entities/`, `experiments/`)
for:

- contradictions between pages
- claims a newer source has superseded but an older page still states
  unqualified
- orphan pages — nothing else links to them via `[[slug]]`
- concepts or entities mentioned repeatedly across pages but lacking their
  own page
- missing cross-references between pages that clearly relate
- `experiments/` pages stuck at `status: planned` or `running` that read as
  actually finished
- `sources/` pages still at `status: stub` that have since been
  superseded by a real ingest, or vice versa

Report findings as a punch list (page → issue), most important first.
Ask before making bulk edits — lint surfaces problems, it doesn't silently
rewrite the wiki. Small, obviously-correct fixes (a broken `[[link]]`
typo) can just be fixed and mentioned. If filed as a report, append to
`wiki/log.md` (`## [YYYY-MM-DD] lint | <scope>`).
