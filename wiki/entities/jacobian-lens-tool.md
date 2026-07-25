---
title: jacobian-lens (tool)
type: entity
entity_type: tool
tags: [interpretability, representation-reading, tooling]
sources: [verbalizable-representations-global-workspace]
updated: 2026-07-25
---

# jacobian-lens (tool)

Reference implementation repo (`jlens` Python package), vendored into this
project as the [`jacobian-lens/`](../../jacobian-lens/) git submodule from
`git@github.com:anthropics/jacobian-lens.git`. Not maintained upstream, not
accepting contributions — treat as read-only vendored code (see
[`CLAUDE.md`](../../CLAUDE.md)).

## What it does

Reads out what an internal residual-stream activation is "disposed to make
the model say," without needing that content to appear verbatim anywhere in
the prompt. Method:

1. Take a residual-stream vector `h` at some layer `l` and position.
2. Linearly transport it into the final-layer basis using the **average
   input-output Jacobian** over a text corpus:
   `lens_l(h) = unembed(J_l @ h)`, where `J_l = E[∂h_final / ∂h_l]`.
3. Decode with the model's own unembedding into a ranked list of vocabulary
   tokens.

The expectation is over prompts, source positions, and all
current-and-future target positions in a generic web-text corpus. The lens
is *fit* once per model (via `jlens.fit`, ~100-1000 prompts) and then applied
cheaply to new activations.

Ships an interactive layer × position "slice" visualization (d3-based) for
exploring what a lens reads out across a whole forward pass.

## Relation to this project

This is the primary interpretability instrument [[jacobi-alignment-project]]
experiments will be built on. Fitting is model-specific — new experiments
against a new base model need a freshly fit lens (or a shared one loaded via
`JacobianLens.from_pretrained`).

## Open questions / not yet explored here

- We have not yet fit or applied a lens ourselves in this project — no
  experiments logged yet (see [experiments/](../experiments/README.md)).
- The underlying claim (verbalizable representations forming a "global
  workspace") is only known secondhand via this repo's README — the paper
  itself is unread. See
  [sources/verbalizable-representations-global-workspace.md](../sources/verbalizable-representations-global-workspace.md).

## References

- Repo: https://github.com/anthropics/jacobian-lens
- Paper: [[verbalizable-representations-global-workspace]]
