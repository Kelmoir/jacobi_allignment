---
title: "Towards Monosemanticity: Decomposing Language Models With Dictionary Learning"
type: source
source_type: paper
url: https://transformer-circuits.pub/2023/monosemantic-features/index.html
status: read
date_added: 2026-07-26
date_read: 2026-07-26
authors: [Trenton Bricken, Adly Templeton, Joshua Batson, Brian Chen, Adam Jermyn, Tom Conerly, Nicholas L Turner, Cem Anil, Carson Denison, Amanda Askell, Robert Lasenby, Yifan Wu, Shauna Kravec, Nicholas Schiefer, Tim Maxwell, Nicholas Joseph, Alex Tamkin, Karina Nguyen, Brayden McLean, Josiah E Burke, Tristan Hume, Shan Carter, Tom Henighan, Chris Olah]
org: Anthropic
published: 2023-10-04
tags: [interpretability, sparse-autoencoders, dictionary-learning, superposition, monosemanticity]
---

# Towards Monosemanticity: Decomposing Language Models With Dictionary Learning

Anthropic, published 2023-10-04 (Transformer Circuits Thread). One of the two
near-simultaneous 2023 papers establishing sparse autoencoders (SAEs) as a
method for decomposing language model activations into interpretable
features — full synthesis of the method and results shared with its
companion paper in [[sparse-autoencoders]].

## Motivation

Follows on Anthropic's own *Toy Models of Superposition* (Elhage et al.
2022): neurons are often **polysemantic** (a single neuron responding to,
e.g., "academic citations, English dialogue, HTTP requests, and Korean
text"), hypothesized to result from **superposition** — a model
representing more features than it has neurons/dimensions by assigning each
feature its own (non-orthogonal) direction, viable only because features are
sparse. Three strategies were previously identified to recover such
features: architectural sparsity, dictionary learning, hybrid. This paper
commits to dictionary learning via a sparse autoencoder, after the authors
found the architectural route (e.g. the SoLU activation) made some neurons
more interpretable at the cost of others, and standard dictionary-learning
methods overfit.

## Method

A one-layer transformer (512-neuron MLP), with SAEs trained on **8 billion**
MLP-activation datapoints, at expansion factors from 1× (512 features) to
256× (131,072 features); the paper's primary run ("A/1") has 4,096 features.
Architecture: input bias, linear+bias+ReLU encoder, linear+bias decoder,
with input/output biases tied (equivalent to subtracting a fixed bias then
using a single pre-encoder bias). Trained with Adam to reconstruct MLP
activations, MSE reconstruction loss + L1 sparsity penalty. Two practical
findings emphasized: **scale matters** (more training data → subjectively
"sharper" features) and **dead-neuron resampling** (periodically resetting
encoder weights for neurons that stop firing, using datapoints the
autoencoder currently represents poorly) meaningfully improves results. No
single trusted metric for "is the autoencoder working" was found; the
authors triangulate manual inspection, feature density, reconstruction loss,
and toy-model ground truth.

A one-layer model was chosen deliberately: small enough that a dictionary
might cover all its "true features," cheap enough to overtrain heavily, and
simple enough that logit effects are ~linear in feature activations (aiding
validation) — at the cost of uncertain generalization to multilayer models
(mitigated by noting others found interpretable linear features in
multilayer SAEs too).

## Key results

- **Interpretability**: four converging lines of evidence — detailed
  single-feature case studies (Arabic script, DNA, base64, Hebrew script
  features), a blinded human rubric-scoring analysis (median neuron scored
  0 — "could not even form a hypothesis"; median feature scored 12, a
  confident/specific/consistent hypothesis), automated interpretability on
  activations (following Bills et al. 2023's approach: an LLM writes an
  explanation, then predicts held-out activations from it), and automated
  interpretability on logit weights. All show features are far more
  interpretable than neurons.
- **Loss recovered**: 79% of the MLP's log-likelihood loss contribution is
  recovered by the A/1 run's features; an extreme high-expansion run (A/5,
  131,072 features) recovers 94.5%. Explicit caveat: this framing likely
  understates a long tail of features needed to close the remaining gap.
- **Causal validity**: three independent checks agree — logit-weight
  inspection, feature ablation (zeroing a feature, observing loss change per
  token), and **pinned feature sampling** (clamping a feature high and
  sampling text) — e.g. pinning the base64 feature makes the model generate
  base64 text, pinning the Arabic-script feature produces Arabic text.
- **Universality**: SAEs applied to two independently-trained one-layer
  transformers (same architecture/data, different seeds) produce mostly
  similar features — more similar to each other than to their own model's
  neurons.
- **Feature splitting**: as dictionary size grows, features "split" into
  families of more specific sub-features (e.g. one base64 feature splits
  into three more specific ones in a larger dictionary) — different
  dictionary sizes act as different "resolutions" on the same underlying
  structure.
- **"Finite-state automata"-like systems**: some features connect across
  positions to implement compound behaviors, e.g. jointly generating valid
  HTML.
- **Data vs. model**: running SAEs on a random-weight version of the same
  transformer produces far less interpretable non-single-token features,
  arguing the found structure reflects the trained model's computation, not
  just dataset correlations.

## Relation to Cunningham et al.

Explicitly frames Cunningham et al.'s [[sparse-autoencoders-interpretable-features]]
as concurrent, independently-arrived-at work with "very similar themes,"
motivated by the same *Toy Models of Superposition* paper: "This motivated a
parallel investigation by our colleagues Cunningham et al., published as a
series of interim reports... We've been excited to see so many
corroborating findings." Both papers in turn credit **Sharkey et al.
(2023)** — earlier interim/blog-form reports (Lee Sharkey is a co-author of
the Cunningham et al. paper) — with originating the specific sparse
autoencoder approach for LM activations that both formal papers build on.

## Stated limitations / open questions (Discussion, Future Work)

- The clean "isotropic superposition" picture (discrete, evenly-spaced
  one-dimensional feature directions) doesn't fully hold — features cluster
  in correlated groups and may not be strictly one-dimensional ("feature
  manifolds"), which the authors suspect explains continued feature
  splitting at larger dictionary sizes.
- **"Token-in-context" features** (e.g. distinct features for "the" in
  physics vs. mathematics contexts) were unexpected for a one-layer model;
  the authors argue these likely reflect a genuine local code the model
  uses (sharper predictions than a compositional code would allow), not
  purely an artifact of the L1 penalty.
- Named future-work directions: scaling SAEs to frontier models (noting a
  100× expansion factor on a 10k-wide layer implies ~20B autoencoder
  parameters — a real engineering/compute problem); scaling laws for
  dictionary learning; better feature-quality metrics beyond automated
  interpretability; scalability of downstream analysis once superposition
  is "solved"; algorithmic improvements (variational autoencoders,
  alternative sparsity priors); whether attention layers exhibit analogous
  superposition (no clear example found yet); deeper theory of superposition
  covering feature clusters/manifolds.

## References

- Paper: https://transformer-circuits.pub/2023/monosemantic-features/index.html
- Concept synthesis: [[sparse-autoencoders]]
- Concurrent/companion paper: [[sparse-autoencoders-interpretable-features]]
