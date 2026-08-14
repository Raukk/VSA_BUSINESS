# Public Release Plan — Target 2026-09-06

CONCEPTS: RELEASE_ENGINEERING|CRITICAL_PATH|GOLDEN_MODEL|RTL|FPGA_PROTOTYPING|OPEN_SOURCE_LICENSE

**Scope:** what ships Sept 6, in what order, and who does what. Coding is sub-agent work (VSA_GoldenModel); owner time is the scarce resource. Weave is decoupled and not launch-gating. FPGA is post-launch (see §5).

## 1. Highest fan-out items — do first

Ranked by dependent count, not size. All four are days-or-less each; everything downstream serializes behind them.

1. **Configuration image format** — spec now exists (`VSA_ASIC:docs/baseline/config_image_format_draft.md`, Draft; most rulings folded). Remaining work: owner ratification of the open D-items (D1 residual — Epilogue aperture base, D3 — Config Spine, D4 — shadow-plane commit, ruled with G-05; plus D7/D11/D13 tracking items) and promotion past Draft. Blocks: toy compiler output, golden-model config load, FPGA config, conformance vectors.
2. **EFAU normalization constants** (N_scale, Xmax, segment indexing, Q3.28 truncation — top golden-model gap, `VSA_ASIC:docs/baseline/design_open_items.md`). Blocks: epilogue golden vectors, EFAU v0.1 → v1.0, any end-to-end bit-exact run.
3. **License ruling** (TODO 9). Blocks: file headers, release-repo assembly, onboarding docs, launch announcement. Ruled: Apache-2.0 (code) + CERN-OHL-W v2 (design) + VSA certification mark — `license_decision_memo.md`; counsel review pending.
4. **Release manifest** — the explicit include list for the fresh public repo. Blocks: cleanup scoping, onboarding docs, wiki alignment. Doubles as the staleness audit: every doc is either in, out, or fixed-then-in.

## 2. Week-by-week

**Now – Aug 17 (rulings batch):** owner rules on §1 items 1–4 in one or two sessions; agents draft each for signoff first. Kick off parallel agent workstreams below.

**Aug 18–24 (build + debt):**
- Golden model: Phase 1 to Block-level golden vectors (CRC conformance values in ImplSpec §8 as seed); requant-harness promotion closed.
- Toy compiler: place-big-squares mapper — any valid config, no optimality; target workload = the 4-layer CNN in `VSA_ASIC:docs/baseline/workload_data_reuse_analysis.md`; emits config images per the new format spec.
- Spec debt: WRU consolidated spec drafted from the existing `wru_*` docs; EFAU updated with ruled constants.
- Cleanup sweep: agents walk every doc against the manifest — current / stale-mark / exclude; owner reviews the deltas, not the docs.
- Power/thermal estimate drafted (TODO 6) — gates which headline numbers may ship.

**Aug 25–31 (verify + assemble):**
- End-to-end: toy compiler → golden model runs the 4-layer CNN bit-exact. This is the launch demo.
- Red-team pass (TODO 10): fresh context, prompted to refute, given docs but not conclusions. Owner adjudicates findings.
- Hand-wave audit folded in (TODO 4), F1–F3 clock re-derivations closed.
- Fresh release repo assembled from manifest; onboarding docs (README, quickstart, "how to read this project") written against it; wiki synced under its workflow gate.

**Sep 1–6 (freeze):** content freeze Sep 1. Owner final read of all launch-facing claims (each headline carries its conservative floor per wiki accuracy protocol). Fix red-team findings or mark open-and-bounded. bench-keys: blind migration to VSA_BENCH_KEY completed 2026-08-14; owner manually deletes the stale branch on this remote (proxy blocks ref deletion) before anything goes public. Launch.

## 3. Ships Sept 6

Fresh public repo: finalized specs (EFAU v1.0, WRU consolidated), `docs/baseline/`, FAQ, terminology, wiki, license, conformance clause (ImplSpec §10.7 seed), contribution/governance page, golden model + toy compiler with the bit-exact CNN demo, power/thermal note alongside the claims. Excluded: ledgers, `brainstorm/`, `old_drafts/`, dated SWAGs, review archives (stay in this repo).

## 4. Owner-time budget (the real constraint)

Agent throughput is not the bottleneck; owner serialization is. The four sinks, largest first:

1. **Cleanup/manifest review** — agents propose, but every include/exclude/stale call is a judgment only the owner can make at launch-credibility stakes. Mitigation: review diffs and deltas, never whole docs; batch by directory.
2. **Red-team adjudication** — each finding needs an owner verdict (fix / bound / reject). Mitigation: agents pre-sort by severity with a proposed disposition.
3. **VSA_BENCH checking + stale bench-keys branch deletion** (migration to VSA_BENCH_KEY completed 2026-08-14; only manual deletion of the stale branch remains — proxy blocks ref deletion) — owner-only by standing rule; schedule early, not launch week.
4. **Rulings backlog + wiki approval loop** — each item small, all serialized. Mitigation: standing ruling sessions (2–3/week) with agent-prepared decision memos: one page, options, recommendation.

## 5. FPGA — post-launch, deliberately

RTL does not exist; three weeks covers writing it by agents but not verifying it to public-claim standard, and rental logistics add schedule risk with no launch payoff. First post-launch milestone: one Block + Epilogue on rented FPGA, bit-exact against golden vectors; then a Group. The launch proof story is specs + golden model + compiled bit-exact demo (proof ladder Rungs 1–3); FPGA is Rung 4, announced as "next."

## 6. Not gating

Weave (own process, own timeline); Hyper/wafer economics re-derivations beyond what red-team flags; SM2–SM4 (mark open-and-bounded if unresolved); Mega node re-cost beyond the dual-node basis already landed; Octa interconnect spec (mark as roadmap).
