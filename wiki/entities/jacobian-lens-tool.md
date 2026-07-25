---
title: jacobian-lens (tool)
type: entity
entity_type: tool
tags: [interpretability, representation-reading, tooling, global-workspace]
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
the prompt. Per the paper's Methods section (see
[[verbalizable-representations-global-workspace]]):

1. Take a residual-stream vector `h_ℓ,t` at layer `ℓ`, position `t`.
2. Linearly transport it into the final-layer basis using the **average
   input-output Jacobian** over a corpus of ~1000 pretraining-distribution
   prompts:
   `J_ℓ = E[∂h_final,t' / ∂h_ℓ,t]`, expectation over source positions, all
   subsequent target positions `t'`, and prompts.
3. Decode with the model's own unembedding:
   `lens(h_ℓ) = softmax(W_U · norm(J_ℓ h_ℓ))` — a ranked list of vocabulary
   tokens the activation is disposed to make the model verbalize.

The lens is *fit* once per model (via `jlens.fit`) and then applied cheaply
to new activations. The README states "~100 prompts is usable" for
saturated quality, attributed to the paper's §9.3 — **unverified**, see the
open question on the source page.

### J-space

The set of lens vectors (one per vocabulary concept, per layer) is
overcomplete. The paper defines the **J-space** as the set of points
expressible as a sparse nonnegative combination of J-lens vectors
(sparsity typically k ≤ 25). Empirically only ~10% of activation variance
at a given layer falls inside the J-space — most of the activation lives in
orthogonal, non-J-space directions. This J-space is the paper's proposed
"global workspace" — see [[global-workspace-hypothesis]] for the full claim
and evidence.

### Reading vs. writing

- **Reading**: rank tokens by inner product with the activation; sparse
  decomposition to extract which concepts are active.
- **Writing**: steer (`h ← h + α·v_t` along a lens vector), ablate (project
  out J-space components), or patch/swap J-space coordinates between two
  concepts while preserving the orthogonal (non-J-space) components
  unchanged. Swap/ablation experiments in the paper are the primary causal
  evidence for the workspace claim.

Ships an interactive layer × position "slice" visualization (d3-based) for
exploring what a lens reads out across a whole forward pass.

## Relation to this project

The primary interpretability instrument this project's experiments will be
built on. Fitting is model-specific — new experiments against a new base
model need a freshly fit lens (or a shared one loaded via
`JacobianLens.from_pretrained`).

## Open questions / not yet explored here

- We have not yet fit or applied a lens ourselves in this project — no
  experiments logged yet (see [experiments/](../experiments/README.md)).
- The ~100-prompt sample-efficiency claim (README, attributed to paper
  §9.3) hasn't been directly confirmed against the paper text.

## Alignment application

The paper's primary use of this tool for safety/model-auditing purposes —
case studies, model organisms, a quantitative eval-awareness classifier,
and an automated-auditing-agent benchmark — is synthesized in
[[alignment-auditing-via-jspace]].

## References

- Repo: https://github.com/anthropics/jacobian-lens
- Paper: [[verbalizable-representations-global-workspace]]
