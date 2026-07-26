---
title: Sparse autoencoders (for LM interpretability)
type: concept
tags: [interpretability, sparse-autoencoders, dictionary-learning, superposition, monosemanticity]
sources: [towards-monosemanticity, sparse-autoencoders-interpretable-features]
updated: 2026-07-26
---

# Sparse autoencoders (for LM interpretability)

**What it is**: a method for decomposing a language model's internal
activations into a larger, overcomplete set of sparsely-active, more
monosemantic "features," by training an autoencoder (ReLU hidden layer,
MSE reconstruction loss + L1 sparsity penalty on the hidden code) to
reconstruct those activations. Motivated by the **superposition
hypothesis** (Elhage et al. 2022, *Toy Models of Superposition*): neurons
are often polysemantic because models represent more features than they
have neurons/dimensions, packing them into an overcomplete, non-orthogonal
basis — viable only because features fire sparsely. An SAE's hidden units
are trained to recover that basis.

## Founding papers

Two papers established this approach for LLM activations within about three
weeks of each other in 2023, both descending from earlier interim reports
by **Sharkey et al. (2023)**:

- [[sparse-autoencoders-interpretable-features]] — Cunningham, Ewart,
  Riggs, Huben, Sharkey (EleutherAI/MATS/Bristol AI Safety
  Centre/Apollo Research), arXiv 2023-09-15. Tied-weight SAE on Pythia-70M/
  410M residual streams; validated via Bills et al.-style automated
  interpretability (beats default basis/random/PCA/ICA, advantage shrinking
  in later layers) and via IOI-task activation patching (fewer, smaller
  edits than PCA to reach the same causal effect).
- [[towards-monosemanticity]] — Bricken, Templeton, Batson, Chen, Jermyn,
  et al. (Anthropic), Transformer Circuits Thread 2023-10-04. Custom
  one-layer transformer, 8B training datapoints, expansion factors 1×–256×.
  Validated via human rubric scoring, automated interpretability (both
  activations and logit weights), and three causal checks (logit
  inspection, ablation, pinned-feature sampling); also demonstrates feature
  universality across model seeds and feature splitting with dictionary
  size.

Bricken et al. explicitly frames Cunningham et al. as a parallel,
independently-arrived-at investigation with "very similar themes" —
"we've been excited to see so many corroborating findings" — rather than
a rival or a refinement. Neither paper claims priority over the other; both
credit the actual originating exploration to Sharkey et al.'s pre-2023-09
interim/blog reports (Lee Sharkey is a co-author of the Cunningham et al.
paper).

## What's convergent between the two papers

- Same core recipe: ReLU-hidden autoencoder, MSE + L1 loss, applied to
  residual-stream (or MLP) activations, at large expansion factors.
- Same headline finding: SAE features are substantially more interpretable
  than the neuron basis (or PCA/ICA), by both human and automated
  (LLM-based) interpretability scoring.
- Same causal validation logic: an interpretable feature should have a
  causal effect on the model's output that matches its interpretation —
  shown via ablation and/or activation patching in both papers, and via
  pinned/clamped sampling in Bricken et al.
- Both explicitly flag that reconstruction is imperfect: Cunningham et al.
  quantify this as a Pile-perplexity increase (25→40) when substituting a
  reconstructed layer; Bricken et al. frame it as loss-recovered percentage
  (79% at their primary run, up to 94.5% at their largest dictionary).

## Where they differ

- **Scale/model**: Cunningham et al. use real pretrained models (Pythia
  family); Bricken et al. use a small custom one-layer transformer chosen
  specifically to make the decomposition problem tractable and causally
  checkable, at the cost of open questions about generalization to
  multilayer models.
- **Validation task**: Cunningham et al. lean on a single well-studied
  behavioral task (IOI) for causal localization; Bricken et al. lean on
  detailed single-feature case studies (Arabic script, DNA, base64, Hebrew)
  plus broader phenomenology (universality across seeds, feature splitting,
  "finite-state-automata"-like multi-feature systems).
- **Open problems each foregrounds**: Cunningham et al. foreground
  extending SAEs to MLP/attention layers and to bigger models toward
  "enumerative safety" (a full sparse-causal account of model computation);
  Bricken et al. foreground the compute cost of scaling (a 100× expansion
  factor on a 10k-wide layer is already ~20B parameters), the absence of
  reliable non-behavioral quality metrics, and open theoretical questions
  about whether features are really one-dimensional or form manifolds
  (raised by the unexpected prevalence of "token-in-context" features).

## Relation to other methods in this wiki

- **[[natural-language-autoencoders]]** (a later, different method, in
  [[natural-language-autoencoders]]'s source page) is explicitly positioned
  against SAEs by its authors: SAEs give unsupervised feature discovery but
  "unpredictable coverage gaps" and require a human/LLM to interpret each
  learned feature after the fact; NLAs aim to fold discovery and
  natural-language readability into one training objective.
- **[[jacobian-lens-tool]] / [[global-workspace-hypothesis]]**: SAEs appear
  in [[alignment-auditing-via-jspace]] as one of the automated-auditing
  benchmark's baselines — "an SAE-equipped [investigator]" underperforms the
  J-lens-equipped one on 4 of 6 benchmark cases there — and the paper's own
  framing treats SAEs as complementary rather than competing: "We regard
  the J-lens as a useful addition to the auditing toolkit, and one that
  composes naturally with other methods (e.g. SAEs)... but not as a
  complete one."

## References

- Sources: [[towards-monosemanticity]], [[sparse-autoencoders-interpretable-features]]
- Contrasting method: [[natural-language-autoencoders]]
- Used as a comparison baseline in: [[alignment-auditing-via-jspace]]
