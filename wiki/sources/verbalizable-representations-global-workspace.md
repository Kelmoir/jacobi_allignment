---
title: "Verbalizable Representations Form a Global Workspace in Language Models"
type: source
source_type: paper
url: https://transformer-circuits.pub/2026/workspace/index.html
status: read
date_added: 2026-07-25
date_read: 2026-07-25
authors: [Wes Gurnee, Nicholas Sofroniew, Adam Pearce, Mateusz Piotrowski, Isaac Kauvar, Runjin Chen, Anna Soligo, Paul Bogdan, Euan Ong, Rowan Wang, T. Ben Thompson, David Abrahams, Subhash Kantamneni, Emmanuel Ameisen, Joshua Batson, Jack Lindsey]
org: Anthropic
published: 2026-07-06
tags: [interpretability, global-workspace, representation-reading, alignment-auditing]
---

# Verbalizable Representations Form a Global Workspace in Language Models

Anthropic, published 2026-07-06. Correspondence: jacklindsey@anthropic.com.
This is the companion paper for the [[jacobian-lens-tool]] repo vendored
into this project as a submodule.

## Central claim

> "we observe that language models maintain a privileged set of internal
> representations, available for report, modulation, and flexible internal
> reasoning, atop a much larger volume of automatic processing."

LLMs have a functional analog to global workspace theory (GWT) from
consciousness research: a small, selectively-active set of representations
— **verbalizable**, steerable, load-bearing for flexible reasoning —
distinct from routine automatic processing. Full synthesis, evidence, and
caveats: [[global-workspace-hypothesis]].

## Structure

Introduction → Methods (Jacobian lens / J-space) → "The J-space acts as a
Global Workspace" (five subsections, one per functional property) →
"Structure Supports Function" (layer organization, ignition, broadcast) →
Alignment Auditing → The Assistant's Perspective (J-space across training)
→ Counterfactual Reflection Training → Discussion.

## What's filed vs. deferred

Fully synthesized into [[global-workspace-hypothesis]]: the central thesis,
the five functional properties and their experiments, the layer-structure/
ignition findings, and the stated limitations.

Fully synthesized into [[alignment-auditing-via-jspace]]: the production-
model case studies (blackmail scenario, prompt injection, four Opus 4.6
audit examples), the two implanted-misalignment model organisms, the
quantitative eval-awareness classifier (AUC 0.853 vs. [[natural-language-autoencoders]]), the automated
auditing agent benchmark, and the paper's own stated limitations on this
application.

**Deferred** (mentioned in the paper, not yet given a full page — flagged
for a future ingest pass focused specifically on these sections; raw
extracted text for both is already cached locally from this pass, in
`/tmp/.../scratchpad/auditing_p1.txt` and `auditing_p2.txt` — regenerate
via the paper URL if that scratchpad is gone by the time this is picked
up):

- **The Assistant's Perspective** — tracks how J-space content changes
  across training/RLHF (base vs. post-trained model comparisons: user-turn
  reaction concepts, roleplay/character-drift self-monitoring, preference-
  violation signatures, thought-suppression metacognition). →
  [[assistant-perspective-jspace]] (not yet written)
- **Counterfactual Reflection Training** — a training method motivated by
  the workspace findings; implants ethics-reflection concepts into the
  J-space via counterfactual fine-tuning, shown to causally reduce
  dishonesty on two benchmarks. → [[counterfactual-reflection-training]]
  (not yet written; briefly pointed to from
  [[alignment-auditing-via-jspace]]'s "Related" section)

## Open questions from this ingest

- The `jacobian-lens/README.md` claims a §9.3 on lens-fitting sample
  efficiency ("quality saturates quickly... ~100 prompts is usable"). The
  fetched page text did not surface a section 9.3 or that specific claim —
  likely in appendix content (`[??](#fig-app-method-details)`) not captured
  by the fetch. **Unverified**; don't cite the "~100 prompts" figure as
  paper-confirmed until checked directly (e.g. by opening the page in a
  browser or fetching the appendix anchor specifically).
- Philosophical implications of the workspace/consciousness connection are
  explicitly flagged by the authors as "unclear and likely controversial"
  and deferred to a discussion subsection not captured in this ingest pass.

## Method, in brief

`J_ℓ = E[∂h_final,t' / ∂h_ℓ,t]`, averaged over source positions, all
subsequent target positions, and ~1000 pretraining-distribution prompts.
Full formalism and tool-level detail (reading/writing operations, J-space
definition): [[jacobian-lens-tool]].

## References

- Paper: https://transformer-circuits.pub/2026/workspace/index.html
- Tool: [[jacobian-lens-tool]]
- Concept synthesis: [[global-workspace-hypothesis]]
