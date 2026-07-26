---
updated: 2026-07-26
---

# Wiki Index

Catalog of everything in the wiki. Read this first when answering a query,
then drill into the linked pages. Updated on every ingest.

## Entities

Tools, models, papers-as-artifacts, people, orgs.

- [jacobian-lens (tool)](entities/jacobian-lens-tool.md) — the `jlens`
  Python package/repo (submodule at [`../jacobian-lens/`](../jacobian-lens/)):
  fits and applies a Jacobian lens to decoder transformers, reading out and
  manipulating the "J-space" described in
  [global-workspace-hypothesis](concepts/global-workspace-hypothesis.md).

## Sources

One page per ingested paper, article, transcript, etc.

- [Verbalizable Representations Form a Global Workspace in Language Models](sources/verbalizable-representations-global-workspace.md) —
  Anthropic, 2026-07-06. The paper `jacobian-lens` is companion code for.
  **Read.** Central thesis + alignment-auditing application both
  synthesized (see concepts below); Assistant's Perspective and
  Counterfactual Reflection Training sections still deferred (see the
  source page).
- [Towards Monosemanticity: Decomposing Language Models With Dictionary Learning](sources/towards-monosemanticity.md) —
  Bricken et al., Anthropic, 2023-10-04. One of the two founding papers
  applying sparse autoencoders to LM activations. **Read.**
- [Sparse Autoencoders Find Highly Interpretable Features in Language Models](sources/sparse-autoencoders-interpretable-features.md) —
  Cunningham et al., 2023-09-15 (arXiv:2309.08600). The other founding SAE
  paper, posted ~3 weeks before Bricken et al., independently arrived at.
  **Read.**
- [Natural Language Autoencoders Produce Unsupervised Explanations of LLM Activations](sources/natural-language-autoencoders.md) —
  Fraser-Taliente, Kantamneni, Ong, et al., Anthropic, 2026-05-07. The
  `frasertaliente2026nla` paper, cited but not previously ingested, used as
  the comparison baseline in [[alignment-auditing-via-jspace]]. **Read.**

## Concepts

Synthesis pages for ideas/topics that span multiple sources.

- [Global workspace hypothesis (in language models)](concepts/global-workspace-hypothesis.md) —
  LLMs maintain a small, verbalizable, steerable "J-space" atop automatic
  processing; five functional properties (report, modulation, internal
  reasoning, generalization, selectivity) and structural layer evidence,
  from the paper above.
- [Alignment auditing via J-space](concepts/alignment-auditing-via-jspace.md) —
  reading J-lens at decision points surfaces silent strategic/deceptive
  cognition: blackmail-scenario and prompt-injection case studies, two
  implanted-misalignment model organisms, a quantitative eval-awareness
  classifier (AUC 0.853 vs. NLA), an automated-auditing-agent benchmark,
  and the paper's own stated limits on how far this can be trusted.
- [Sparse autoencoders (for LM interpretability)](concepts/sparse-autoencoders.md) —
  the SAE method (ReLU autoencoder, MSE + L1 loss) for decomposing LM
  activations into monosemantic features; synthesizes the two founding
  2023 papers (Bricken et al., Cunningham et al.), what's convergent vs.
  divergent between them, and how SAEs relate to
  [[natural-language-autoencoders]] and the J-lens auditing benchmark.

## Experiments

Runs and findings using `jacobian-lens` or other tooling in this project.
See [wiki/experiments/README.md](experiments/README.md) for the page
convention.

*(none yet)*

## Meta

- [log.md](log.md) — chronological record of ingests/queries/lints.
- [../llm_wiki_instructions.md](../llm_wiki_instructions.md) — the pattern
  this wiki follows.
