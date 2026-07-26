---
title: "Natural Language Autoencoders Produce Unsupervised Explanations of LLM Activations"
type: source
source_type: paper
url: https://transformer-circuits.pub/2026/nla/index.html
status: read
date_added: 2026-07-26
date_read: 2026-07-26
authors: [Kit Fraser-Taliente, Subhash Kantamneni, Euan Ong, Dan Mossing, Christina Lu, Paul C. Bogdan, Emmanuel Ameisen, James Chen, Dzmitry Kishylau, Adam Pearce, Julius Tarng, Alex Wu, Jeff Wu, Yang Zhang, Daniel M. Ziegler, Evan Hubinger, Joshua Batson, Jack Lindsey, Samuel Zimmerman, Samuel Marks]
org: Anthropic
published: 2026-05-07
tags: [interpretability, natural-language-autoencoders, activation-verbalization, alignment-auditing]
---

# Natural Language Autoencoders Produce Unsupervised Explanations of LLM Activations

Anthropic, published 2026-05-07 (Transformer Circuits Thread). This is the
`frasertaliente2026nla` paper already cited (but not previously ingested) in
[[alignment-auditing-via-jspace]] as the comparison baseline for the J-lens
eval-awareness classifier (AUC 0.853) and as the source of ground truth for
the automated-auditing-agent benchmark there.

## Method

A **Natural Language Autoencoder (NLA)** pairs two LLM modules, jointly
trained by RL to reconstruct a target activation:

- **Activation Verbalizer (AV)**: an LLM identical to the target model,
  given a fixed prompt plus a special token embedding the (scaled) target
  activation; autoregressively samples a natural-language explanation at
  temperature 1.
- **Activation Reconstructor (AR)**: the target model architecture
  truncated to the first `l` layers, wraps the AV's explanation text in a
  fixed prompt, and applies a learned affine map to the resulting layer-`l`
  activation to predict the original.

Training objective: `L = E[‖h_l − AR_θ(z)‖²]` where `z` is the AV's
generated text — equivalent to an RL problem maximizing reward
`r = −‖h_l − AR_θ(z)‖²` (log-transformed reward, GRPO, with a KL penalty
toward the initialization to bound drift). Models are warm-started via
supervised fine-tuning on a text-summarization proxy task using Claude Opus
4.5-generated summaries (~0.3–0.4 initial FVE). Applied to Claude Haiku 3.5,
Haiku 4.5, and Opus 4.6, at middle-to-late layers.

## Key results

- **Fraction of Variance Explained (FVE)** — 0 for predicting the mean, 1
  for perfect reconstruction — "grows roughly linearly in log(training
  steps)," reaching **0.6–0.8** for the NLAs evaluated.
- Five prediction tasks (testing whether NLA text captures inferrable
  traits like user gender/occupation) show consistent improvement with
  training across all three model sizes.
- **Automated auditing benchmark**: NLA-equipped auditing agents more
  effectively discover the root cause of a model's misalignment than
  baselines, and — unlike prior approaches — can do so **without access to
  training data**.
- Positioned explicitly relative to [[sparse-autoencoders]]: SAEs give
  unsupervised discovery but "unpredictable coverage gaps"; supervised
  activation oracles give readability but need labels; NLAs aim to offer
  both — unsupervised discovery from the reconstruction objective, plus
  natural-language readability from the verbalization bottleneck.

## Alignment-auditing case studies

Many drawn from Anthropic's pre-deployment alignment assessment of **Claude
Opus 4.6**. Headline finding: NLAs surfaced **unverbalized evaluation
awareness** — cases where the model internally registered (per the AV's
explanation) that it was being evaluated without ever saying so in its
visible output — validated by constructing inputs designed to vary how
strongly the target model should believe it's being evaluated and checking
that NLA-based measurements track this as expected.

## Stated limitations

- **Confabulation**: explanations can contain verifiably false claims about
  input context, though "typically thematically faithful to the context"
  even when specific details are wrong.
- **No mechanistic grounding**: the AV is a black box by construction —
  which parts of an activation drove which part of an explanation can't be
  determined.
- **Excessive expressivity**: since the AV is a full LLM, it can make
  inferences beyond what's actually stored in the activation.
- **Cost**: joint RL training of two full LLMs; inference requires
  generating several hundred tokens per activation explained.
- **Degenerate training objective risk**: in principle the AV could achieve
  good reconstruction by parroting input context verbatim, or by producing
  uninterpretable-but-reconstruction-optimal text; the KL penalty toward
  initialization is only a partial mitigation, and the authors are explicit
  that long-run benignity of the objective is unclear. Related failure
  modes flagged: steganography, writing-quality degradation, layer
  sensitivity.

## Concurrent work

"Concurrently with this work, Chalnev independently arrived at a closely
related approach (Cycle-Consistent Activation Oracles): a
verbalizer-reconstructor pair with a supervised warm-start, trained using RL
for activation reconstruction under a KL penalty." Not yet a wiki entry.

## References

- Paper: https://transformer-circuits.pub/2026/nla/index.html
- Open-source implementation: https://github.com/kitft/natural_language_autoencoders
- Cited as comparison baseline / ground-truth source in:
  [[alignment-auditing-via-jspace]]
- Related method (unsupervised, non-verbal alternative): [[sparse-autoencoders]]
