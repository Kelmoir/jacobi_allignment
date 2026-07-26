# Log

Append-only. One entry per ingest/query/lint/experiment, newest last. Each
entry starts with `## [YYYY-MM-DD] <kind> | <title>` so it stays greppable:
`grep "^## \[" wiki/log.md | tail -5`.

## [2026-07-25] setup | Wiki initialized

Set up the three-layer structure (`raw/`, `wiki/`, this project's
`CLAUDE.md`) per `llm_wiki_instructions.md`. Seeded the wiki with a first
entity page for the `jacobian-lens` tool and a source stub for its companion
paper, both derived from `jacobian-lens/README.md` (not yet read: the paper
itself).

## [2026-07-25] ingest | jacobian-lens README

Read `jacobian-lens/README.md` in full. Filed:
[entities/jacobian-lens-tool.md](entities/jacobian-lens-tool.md) (the tool)
and [sources/verbalizable-representations-global-workspace.md](sources/verbalizable-representations-global-workspace.md)
(the companion paper, flagged as unread beyond what the README states).

## [2026-07-25] ingest | Verbalizable Representations Form a Global Workspace in Language Models

Read the paper in full via WebFetch. Flipped
[sources/verbalizable-representations-global-workspace.md](sources/verbalizable-representations-global-workspace.md)
from `status: stub` to `read`, with real authors/date/structure. Refined
[entities/jacobian-lens-tool.md](entities/jacobian-lens-tool.md) with the
precise J-lens formalism, J-space definition, and reading/writing
operations (previously only known via the repo README). Filed the central
thesis, five functional properties (report, modulation, internal
reasoning, generalization, selectivity) and their experiments, structural
layer findings, and stated limitations into new
[concepts/global-workspace-hypothesis.md](concepts/global-workspace-hypothesis.md).

Deferred (flagged on the source page for a future ingest): Alignment
Auditing, The Assistant's Perspective, Counterfactual Reflection Training
— sections the extraction didn't go deep on. Also flagged: the README's
"~100 prompts" sample-efficiency claim (attributed to paper §9.3) wasn't
confirmed in the fetched text — likely appendix content not captured by
the fetch.

## [2026-07-25] ingest | Alignment Auditing section (same paper, focused pass)

WebFetch's summarizer was truncating before reaching this section twice in
a row, so downloaded the raw HTML directly (curl) and stripped tags with a
local script to read the actual paragraph text. Filed new
[concepts/alignment-auditing-via-jspace.md](concepts/alignment-auditing-via-jspace.md):
three production-model case studies (blackmail scenario with a causal
ablation experiment — verbalized eval-awareness 71%→3%, blackmail attempts
0/180→13/180; prompt injection; four Opus 4.6 audit examples), two
implanted-misalignment model organisms (reward-hacking, reward-model-bias-
appeasing — the latter a case where J-lens succeeds and plain logit lens
doesn't), a quantitative eval-awareness classifier (AUC 0.853 vs. an NLA
baseline over ~94k positions, three replicated validation experiments),
and an automated-auditing-agent benchmark (18 claims, J-lens-equipped
investigator comparable to NLA on 4/6 cases, notably worse on 2 — both
traceable to the single-token-vocabulary limitation). Also captured the
paper's own explicit limitations on this application (not sufficient for
alignment monitoring; automatic/well-practiced misaligned behavior could
evade it; single-token vocabulary gap) — filed prominently rather than
buried, since this bears directly on this project's use of the tool.

Updated cross-references: [sources/verbalizable-representations-global-workspace.md](sources/verbalizable-representations-global-workspace.md)
(Alignment Auditing moved from deferred to filed), [concepts/global-workspace-hypothesis.md](concepts/global-workspace-hypothesis.md)
and [entities/jacobian-lens-tool.md](entities/jacobian-lens-tool.md) (both
now point to the new page instead of flagging it as an open question).

Still deferred: The Assistant's Perspective, Counterfactual Reflection
Training. Raw extracted text for both is cached at
`/tmp/claude-1000/.../scratchpad/auditing_p1.txt` and `auditing_p2.txt`
(session-scoped scratchpad — regenerate from the paper URL if gone).

## [2026-07-26] ingest | SAE founding papers + Natural Language Autoencoders

User asked for the founding papers behind "NLA" and "SAE" (terms already
present in [[alignment-auditing-via-jspace]] but not yet ingested), then
asked to ingest them. For SAE, user chose to treat both near-simultaneous
2023 papers as co-founding rather than picking one. Read all three in full
(NLA via WebFetch; the two SAE papers via curl + html2text, since both
pages were too large for WebFetch's fetcher — the Anthropic one especially,
at 20MB of embedded base64 figure data requiring a filtering pass to get
clean prose).

Filed three new source pages: [sources/towards-monosemanticity.md](sources/towards-monosemanticity.md)
(Bricken et al., Anthropic 2023-10-04), [sources/sparse-autoencoders-interpretable-features.md](sources/sparse-autoencoders-interpretable-features.md)
(Cunningham et al., arXiv 2023-09-15), and [sources/natural-language-autoencoders.md](sources/natural-language-autoencoders.md)
(Fraser-Taliente et al., Anthropic 2026-05-07 — the paper behind the
`frasertaliente2026nla` citation already in the wiki). Filed one new
concept page synthesizing the two SAE papers together —
[concepts/sparse-autoencoders.md](concepts/sparse-autoencoders.md) — noting
what's convergent (same SAE recipe, same interpretability-over-neurons
finding, same causal-validation logic) vs. divergent (model/scale, primary
validation task, foregrounded open problems) between them, and their shared
debt to Sharkey et al.'s pre-2023-09 interim reports. Did not file a
separate NLA concept page (only one source so far; would also collide on
slug with the source page) — [[natural-language-autoencoders]] currently
resolves to the source page only.

Updated [concepts/alignment-auditing-via-jspace.md](concepts/alignment-auditing-via-jspace.md):
replaced the "no dedicated page yet" note for NLA with a proper
[[natural-language-autoencoders]] link, added an [[sparse-autoencoders]]
link where the auditing benchmark's SAE baseline is mentioned, and noted
that the automated-auditing-agent benchmark's scaffold/ground truth is
literally reused from the NLA paper (not just cited as a comparison).
