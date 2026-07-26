---
title: "Sparse Autoencoders Find Highly Interpretable Features in Language Models"
type: source
source_type: paper
url: https://arxiv.org/abs/2309.08600
status: read
date_added: 2026-07-26
date_read: 2026-07-26
authors: [Hoagy Cunningham, Aidan Ewart, Logan Riggs, Robert Huben, Lee Sharkey]
org: EleutherAI, MATS, Bristol AI Safety Centre, Apollo Research
published: 2023-09-15
tags: [interpretability, sparse-autoencoders, dictionary-learning, superposition]
---

# Sparse Autoencoders Find Highly Interpretable Features in Language Models

Posted to arXiv 2023-09-15 (v1), final v3 2023-10-04 — arXiv:2309.08600. The
other of the two near-simultaneous 2023 papers establishing sparse
autoencoders (SAEs) for LM interpretability, posted about three weeks before
its Anthropic counterpart [[towards-monosemanticity]]. Full method/results
synthesis shared with that paper in [[sparse-autoencoders]].

## Motivation

Same starting point as Bricken et al.: neurons are often **polysemantic**,
hypothesized to arise from **superposition** (Elhage et al. 2022b) —
networks representing more features than dimensions via an overcomplete,
non-orthogonal basis, viable because features are sufficiently sparse.
Recovering these directions is framed as sparse dictionary learning
(Olshausen & Field, 1997). Explicitly building on **Sharkey et al. (2023)**
(interim work; Lee Sharkey is a co-author here) and citing similarity to Yun
et al. (2021), who applied dictionary learning across all residual-stream
layers simultaneously.

## Method

A ReLU autoencoder with **tied encoder/decoder weights** (chosen so the
detecting and defining directions for a feature coincide, to halve memory
cost, and to remove encoder/decoder interpretation ambiguity — confirmed not
to hurt performance on residual-stream data, though some degradation was
observed on MLP data), hidden dimension `d_hid = R · d_in` for expansion
factor `R`. Loss: MSE reconstruction + `α‖c‖₁` sparsity penalty on the hidden
code, with the dictionary-feature rows of the weight matrix normalized to
prevent the sparsity term from being gamed by shrinking feature-vector
scale. Applied to residual-stream activations of **Pythia-70M** (`d=512`)
and **Pythia-410M** (`d=1024`).

## Key results

- **Interpretability at scale**: using the automated interpretability
  procedure of Bills et al. (2023) — an LLM (GPT-4) writes an explanation
  from activating examples, then predicts held-out activations from that
  explanation; correlation with true activations is the interpretability
  score — dictionary features scored far higher than the default
  (neuron) basis, random directions, PCA, and ICA on residual-stream layer
  1 (150 features each, 95% CI error bars). The advantage **shrinks moving
  through the model**, becoming comparable to ICA by layer 4 and minimal in
  the final layer — attributed either to SAEs working less well in later
  layers, or to later/more-complex features being harder for the
  autointerpretation protocol itself (which struggles with patterns keyed
  to previous/next tokens rather than the current token).
- **Causal localization (IOI task)**: on the Indirect Object Identification
  task (Wang et al. 2022), activation-patching interventions using SAE
  features reach a given KL-divergence-from-target using **fewer patches**
  and **smaller edit magnitude** than equivalent interventions using PCA
  components — and this advantage disappears for a non-sparse (`α=0`)
  dictionary, isolating sparsity (not just dictionary learning generally)
  as the source of the improvement. Feature subsets for patching were
  selected via the ACDC algorithm (Conmy et al. 2023).
- **Case studies**: an apostrophe-detecting feature whose ablation
  specifically suppresses the following "s" token's logit (matching
  possessive/contraction use); automatic upstream/downstream circuit
  detection via iterative ablation, demonstrated on a closing-parenthesis
  feature in the final layer, tracing back through prior-layer features
  that predict contexts (dates, acronyms, phrases) preceding a closing
  parenthesis.

## Stated limitations & future work

- **Nonzero reconstruction loss**: substituting layer 2's residual-stream
  activations in Pythia-70M with the SAE's reconstruction raises Pile
  perplexity from 25 to 40 — dictionaries do not capture all
  layer-activation information. Proposed remedies: alternative SAE
  architectures, training to minimize output-perplexity change directly
  rather than reconstruction MSE, and incorporating adjacent-layer/weight
  information into training.
- **Residual stream is the sweet spot**: the training pipeline used is not
  yet robust for finding overcomplete bases in MLP intermediate layers
  (some evidence it's applicable, in the appendix) or attention heads.
- The IOI localization result (behavior concentrated in few features) is
  hypothesized, not yet confirmed, to generalize to other tasks/behaviors.
- Longer-term goal stated in the conclusion: tracing causal dependencies
  between features across layers, toward "enumerative safety" — an
  end-to-end, sparse-causal-dependency account of how a model computes its
  outputs.

## Relation to Bricken et al.

Cited by [[towards-monosemanticity]] as the "recent manuscript" culminating
a parallel, independently-motivated investigation track (via the same
Sharkey et al. interim reports) that Bricken et al. found strongly
corroborating. The two papers use different base models (Pythia-70M/410M
vs. a custom one-layer transformer), different validation tasks (IOI
circuit localization vs. pinned-sampling/ablation case studies +
universality/feature-splitting), but the same core architecture and loss
family (SAE, MSE + L1).

## References

- Paper: https://arxiv.org/abs/2309.08600
- Code: https://github.com/HoagyC/sparse_coding
- Concept synthesis: [[sparse-autoencoders]]
- Concurrent/companion paper: [[towards-monosemanticity]]
