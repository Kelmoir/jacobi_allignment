---
title: Global workspace hypothesis (in language models)
type: concept
tags: [interpretability, global-workspace, alignment]
sources: [verbalizable-representations-global-workspace]
updated: 2026-07-25
---

# Global workspace hypothesis (in language models)

**Claim** (Gurnee, Sofroniew, et al., Anthropic, 2026 — see
[[verbalizable-representations-global-workspace]]): language models
maintain a small, selectively-active set of internal representations —
**verbalizable**, steerable, load-bearing for flexible reasoning — sitting
atop a much larger volume of automatic processing. This is proposed as a
functional analog to global workspace theory (GWT) from human consciousness
research. The instrument used to locate and manipulate this set is the
[[jacobian-lens-tool]]; the set itself is called the **J-space**.

## What "verbalizable" means

Not "currently being verbalized in this context." The paper's distinction:

> "Verbalizable [representations are] poised to be spoken about, should the
> occasion arise—from those that merely happen to be verbalized in one
> particular context."

The J-lens's averaging over many contexts is specifically what isolates
this *disposition* rather than one-off verbalization. See
[[jacobian-lens-tool]] for the formalism.

## The five functional properties (the evidence)

The workspace claim isn't a single proof — it's convergent evidence across
five properties, each tested with targeted swap/ablation/steering
experiments on the J-space:

1. **Verbal report** — asking the model what it's thinking names J-space
   concepts; swapping J-space vectors changes its self-report. Swapping the
   J-space component of a concept vector alone drives the target into
   top-5 output on 59% of trials vs. 5% for the non-J-space component at
   matched magnitude — the causal effect routes *through* J-space.
2. **Directed modulation** — explicit instructions ("concentrate on X")
   pull a concept into J-space at a rate that increases with model size;
   negative/suppression instructions work less reliably than positive
   focus instructions. Whether a computed property surfaces in J-space
   depends on whether the question asks for it *explicitly* — computed-but-
   unasked properties don't appear.
3. **Internal reasoning** — intermediate inference steps are represented in
   J-space and are causally load-bearing even when never stated. Swap
   examples: spider→ant flips a leg-count answer (8→6); swapping a planned
   rhyme word shifts earlier word choices in a couplet; swapping an
   English-language intermediate concept changes a Chinese-language output
   correspondingly, suggesting a shared multilingual intermediate routed
   through J-space. On two-hop prompts, swapping the J-space component of
   an intermediate-answer probe flips the final answer on 61% of trials vs.
   28% for the non-J-space component.
4. **Flexible generalization** — the same J-space representation (e.g. the
   entity "France") serves as a valid argument across many different
   downstream question templates (capital-of, language-of, etc.); swap
   success correlates with the entity's baseline "workspace loading."
5. **Selectivity** — J-space is necessary for flexible/report tasks but not
   for automatic/routine ones, even when the same content is computable
   either way. E.g. swapping a language-identity vector flips report/
   inference tasks (~90%) but leaves text continuation and anomaly
   detection essentially untouched (~0%), despite the language concept
   appearing in the lens at comparable rates across all four conditions.
   Character counts appear in the lens only when a task requires reporting
   them, not during automatic line-wrapping at the same count.

Broad-strokes ablation evidence (projecting out top J-space directions):
automatic tasks (classification, extraction, acceptability judgments) stay
robust; flexible tasks (analogy, free-form generation, chained reasoning,
translation, sonnet-writing) are severely impaired. Chain-of-thought GSM8K
is more ablation-robust than direct-answer GSM8K — externalizing reasoning
as text lets the model route around the ablated J-space.

## Structural correlates

- **Three-layer-block architecture**: early layers are noisy/non-workspace;
  a middle band (paper's models: roughly L38–L92) shows abstract,
  persistent, workspace-like content by multiple independent metrics (CKA
  layer-alignment blocks, kurtosis of the lens readout distribution,
  position-autocorrelation of the top-1 token, effective linear
  dimensionality); late layers align with imminent output.
- **Ignition**: blending two concepts' input embeddings at ratio α produces
  smooth interpolation in early layers, but a sharp binary commitment to
  one endpoint or the other at the workspace-onset layer — evidence the
  onset layer is functionally significant, not an artifact of the lens.
- **Experiential-report content**: ablating the top J-space directions
  sharply reduces experiential language (words like "thinking,"
  "feeling," "conscious") in open-ended self-report answers, producing a
  more mechanical/detached register. These tokens appear in J-space at
  much higher relative rates (58%/23%/17%/7% for
  thinking/thoughts/feeling/conscious) than their frequency in the actual
  output distribution would suggest.

## Limitations (as stated by the authors)

- The J-lens only identifies concepts corresponding to **single vocabulary
  tokens**; many important concepts are multi-token. Extensions exist but
  are incomplete.
- Explicitly called "an imperfect tool, which we believe only
  approximately and incompletely captures the model's underlying workspace
  structure." Content outside J-space might still be part of a "true"
  workspace not captured by this method — early layers show no J-space
  content, but the authors don't rule out an uncaptured earlier workspace.
- The paper does **not** claim architectural correspondence to biological
  GWT: no obvious specialized/encapsulated input processors, broadcast
  happens within a single feedforward pass rather than via recurrent
  loops, and the degree of genuine competitive "ignition" is unclear.
- Philosophical implications for consciousness are explicitly flagged by
  the authors as "unclear and likely controversial" — not resolved by this
  paper.
- What surfaces in J-space is strongly modulated by task framing (implicit
  vs. explicit questions) — noted as underexplored.

## Not yet ingested from this paper

**Alignment auditing** — now fully synthesized: [[alignment-auditing-via-jspace]].

Two sections still not yet fully synthesized here — natural next
`/wiki ingest` targets:

- **The Assistant's Perspective** — J-space content tracked across training/RLHF — [[assistant-perspective-jspace]]
- **Counterfactual Reflection Training** — a training method motivated by these findings — [[counterfactual-reflection-training]]

## References

- Source: [[verbalizable-representations-global-workspace]]
- Tool/method: [[jacobian-lens-tool]]
