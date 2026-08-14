# TODO — Follow-up Topics for Dedicated Sessions

CONCEPTS: OPEN_SOURCE_LICENSING|GO_TO_MARKET|PRICING_STRATEGY|PATENT_LAW|RED_TEAM

Items queued for their own conversation/analysis. Each should get a dedicated session, not a side-note in another discussion. Add items with a date and enough context to start cold.

## Open

- **(2026-07-10) Price-floor / market-strategy analysis of the agent-fleet economics.**
  Context: `VSA_CASE_STUDIES:deepseek/agent_fleet_deployment_sketch_2026-07-10.md` §6 — read+write billing at ~24K contexts puts nominal payback on the short-context system in days-to-weeks, which really means the operator sets the market's price floor rather than pockets 100× ROI. Owner: tempting enough to justify a real look. Questions for a dedicated session: what does rational pricing look like for an operator with ~10× lower serving cost (undercut-to-grow vs. margin-harvest vs. internal-only); how fast does the token-price equilibrium move once one such fleet exists; does the price-floor position durably advantage the first deployer given the open baseline (anyone can follow — is the moat execution speed, factored-model access, or nothing); and what does that imply for which buyer type should be pitched first.
  *(Migrated from `VSA_ASIC:docs/TODO.md` 2026-08-14.)*

- **(2026-07-10) Licensing wording — baseline patent non-assertion.**
  Context: owner's intent *(designer's note, 2026-07-10)*: adopters may improve/refine/customize and patent those improvements, and may use those patents against copies of *their improvements* — but may not assert any patent to block implementation of the **core baseline design** itself; doing so would violate the grant they received. This is a real, nameable construct (a patent non-assertion / defensive-termination condition scoped to the baseline — Apache-2.0 §3 termination, OIN-style non-assertion pledges, and RISC-V's member IP policy are the reference points), but the scoping line between "baseline" and "improvement" is exactly where the wording gets hard, and it interacts with the conformance-surface definition (what formally *is* the baseline; that definition lives in VSA_ASIC). Deliverable: a license selection + drafted non-assertion clause, alongside `license_decision_memo.md`, reviewed against the CC0-has-no-patent-grant problem (`productization_feasibility_2026-07-10.md` §8). Worth real care; a dedicated session, and ultimately a lawyer before release.
  *(Migrated from `VSA_ASIC:docs/TODO.md` 2026-08-14.)*

- **(2026-07-10) Red-team pass on the whole thesis.**
  Context: all analysis so far (including the feasibility assessment, `productization_feasibility_2026-07-10.md`) has been steel-manning. Task: adversarial review — "assume this failed to get adopted by 2031; write the post-mortem." Candidate attack surfaces: model-architecture drift (INT8-only vs. FP8/FP4 trends, attention variants, model turnover pace vs. the 3–4-static-models deployment premise), DDR bandwidth vs. reconfiguration rate, market timing, merchant price compression. **Process requirement** *(owner + assistant agreed 2026-07-10)*: the red team must run with fresh context — a dedicated session or independent subagents prompted to *refute*, not summarize, and given the docs but NOT this conversation's conclusions; the author of the feasibility doc should not be the judge of which findings stand. Findings then get verified/ruled on separately (owner or another session).
  **Status check (2026-08-14):** partial adversarial passes are already recorded in VSA_ASIC — the press-pack red-team (`VSA_ASIC@8952260` history, commit 8958f88), the LLM hand-wave audit (`VSA_ASIC:docs/llm_handwave_audit_2026-07-10.md`), and per-doc adversarial reviews (power estimates, config-image format). No recorded deliverable exists for the fresh-context whole-thesis post-mortem specified here — owner to confirm whether it is still wanted or covered by the passes above.
  *(Migrated from `VSA_ASIC:docs/TODO.md` 2026-08-14.)*
