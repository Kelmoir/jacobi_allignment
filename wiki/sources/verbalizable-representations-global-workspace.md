---
title: "Verbalizable Representations Form a Global Workspace in Language Models"
type: source
source_type: paper
url: https://transformer-circuits.pub/2026/workspace/index.html
status: stub
date_added: 2026-07-25
tags: [interpretability, global-workspace, representation-reading]
---

# Verbalizable Representations Form a Global Workspace in Language Models

**Status: stub.** Not yet read directly — everything below is inferred from
[`jacobian-lens/README.md`](../../jacobian-lens/README.md), the companion
code's own description. Needs a real ingest pass once the paper is read.

## What we know so far

- Introduces (or uses) the **Jacobian lens**: reads what a residual-stream
  activation is disposed to make a model verbalize, via the average
  input-output Jacobian transported into the final-layer basis (see
  [[jacobian-lens-tool]]).
- Title implies a central claim: representations that are "verbalizable"
  (i.e., legible to this kind of readout) form something like a **global
  workspace** — language possibly functioning as a shared/broadcast layer
  across the model's computation. This is a guess from the title, not yet
  verified against the actual text.
- §9.3 of the paper is referenced in the README re: lens-fitting sample
  efficiency ("quality saturates quickly... ~100 prompts is usable").
- Ships synthetic replication and lens-eval prompt sets, Apache-2.0
  licensed, in [`jacobian-lens/data/`](../../jacobian-lens/data/)
  (`data/experiments/` and `data/evaluations/`).

## To do on next real ingest

- Read the actual paper at the URL above.
- Extract the actual global-workspace claim/evidence, not just the guess
  above.
- Check `data/experiments/` and `data/evaluations/` READMEs for what each
  prompt set covers — likely maps directly to the paper's experiment
  sections.
- Update [[jacobian-lens-tool]] and this page's frontmatter (`status:
  read`) once done.
