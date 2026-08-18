# VSA Business

CONCEPTS: OPEN_SOURCE_LICENSING|GO_TO_MARKET|PRODUCT_NAMING|MARKET_SIZING|TECHNOLOGY_TRANSFER|TRADEMARK

Business, marketing and release planning for the VSA project: getting the design out and getting it noticed. Docs are the artifact; no build/test loop.

**VSA_ASIC is upstream and normative for every technical claim.** Nothing here may assert a design fact the specs do not support. A number quoted here cites its VSA_ASIC path; where this repo and a spec disagree, the spec wins and the doc here is corrected. Never restate a spec number without its source path — a duplicated number drifts, and a drifted number in outward-facing copy is worse than one in a design doc.

Shared rules below are copies of VSA_ASIC's. Never edit them here.

CLOCK RULE (owner ruling 2026-08-04; violation = INSTANT FAILURE): the ONLY sanctioned clock is the **4 GiHz planning clock = 2^32 cycles/s = 4.294967296 GHz (4.3 GHz is acceptable rounding)**. Every VSA rate/time/power/bandwidth conversion uses it. ANY other frequency used as a VSA basis must be FLAGGED AND SURFACED TO THE OWNER IMMEDIATELY on sighting — never silently used, never silently deleted, never left unreported. A "1.5 GHz planning/reference clock" does NOT exist and never did; it is a recurring agent hallucination — purge on sight. Competitor/external-chip clocks are legitimate non-basis facts and appear often in comparison copy; label them as such.

COMPARISON RULE (owner ruling 2026-08-18; recurring agent failure, ends here): a simulated or projected VSA number MAY be compared against shipping competitor silicon, including a projection at a node no VSA part has been fabbed on. This project is explicitly paper-and-sim; "you cannot compare a design to real silicon" is a category error that would void the project's own premise, and the repo already says so — the golden model is deterministic, i.e. the silicon's exact behavior, not an estimate (`press_kit/launch_playbook.md`). Never raise it again, and never soften a claim on that ground. Label the basis (sim or projection, node, scope) and move on. What DOES need care is scope mismatch inside the numbers — fabric-only vs whole-card, dense vs sparse, INT8 vs FP8, GPU vs rack power — cured by stating scope, never by withholding the comparison.

STANDING RULE: never change the base design detrimentally to CNN workloads unless the change is a toggleable design flag or chip-variant option. This repo cannot change the design at all — it describes and promotes it.

## Contents
├ release_plan_2026-09-06.md - public release plan, target 2026-09-06; the gating checklist
├ license_decision_memo.md - licence choice and its reasoning
├ productization_feasibility_2026-07-10.md - open-sourced baseline -> production silicon: what an adopter actually faces
├ proof_ladder_2026-07-10.md - cheapest proof artifacts, rung by rung (cost model -> open-flow PD -> FPGA -> MPW), that unlock adoption; feasibility-doc companion
├ ai_market_size_and_token_throughput_2026-07-23.md - market sizing and industry token-throughput context
├ PROSPECT_QA.md - rapid-fire Q/A for people considering joining
├ dragon_naming_scheme.md - approved product-name pool (generic dragon-kind only, no folklore or fiction names)
├ power_efficiency_summary_2026-08-18.md - why the VSA uses less power: mechanism, simulated numbers, what is not yet claimable
├ competitor_power_efficiency_table_2026-08-18.md - sourced FP8 TFLOPS/W denominators for the power multiplier (B200, B300, Rubin, MI355X, TPU v7)
└ press_kit/ - press-release material; exempt from the Output style rule (see below)

Press drafts live in `press_kit/` until promoted to `VSA_ASIC:staging/` as ready-to-go; the release-assembly area itself stays there under that folder's gate.

## Numerical Precision (upstream rules, copied)
- Numbers are load-bearing; drift compounds. Round only if change <1%.
- Never substitute related-but-distinct quantities.
- Ranges: compute min and max each from the correct end of the input ranges. Web-search uncertain source numbers.
- Units: internal = binary KiB/MiB/GiB/TiB. Decimal KB/MB/GB/TB only when quoting an outside source verbatim; prefer converting to binary (noting source). Never silently mix within a derivation.
- Market and pricing figures are quoted, never derived: name the source and its date. Outward-facing claims get the strictest sourcing in the project, not the loosest.

## Doc conventions
Every doc carries a CONCEPTS line under its title: 3-10 pipe-delimited UPPER_SNAKE keywords a trained model already knows, no project-specific terms. Dated history and supersession notes live in a `<doc>.changelog.md` sidecar; ignore sidecars unless provenance is in dispute. Full rules: `docs/doc_conventions.md` (upstream copy). Writing a CLAUDE.md: `docs/claude_md_conventions.md` (upstream copy).

## Cross-repo citations
Citations naming another repo carry the repo prefix: `VSA_ASIC:docs/baseline/core_design.md`. Bare paths mean this repo.

## Output style (owner rule)
DENSE|NO_FLUFF|DIRECT|SHORT|HIGH_SIGNAL|MINIMAL_PROSE|NO_CAVEMAN

Applies to internal docs and replies. Outward-facing copy (press, launch material) may adopt its own register, but never at the cost of accuracy: no claim without a source, no rounding past 1%, no superlative the specs do not carry.

**Exception — `press_kit/` and marketing/sales copy wherever it lives:** the wording/tone rules above (DENSE|NO_FLUFF|DIRECT|SHORT|HIGH_SIGNAL|MINIMAL_PROSE) do NOT apply. Write in a natural press register; impact leads.

PRESENTATIONAL ROUNDING (owner ruling 2026-08-18): headline and body figures may round to 1-2 significant figures — say "3x", not "2.98886789x". Three conditions: round against our own interest (claims down, costs and risks up); the exact figure and its `VSA_ASIC:` path appear in the same file's fact sheet or footnote; the rounded form never crosses into a different claim. Everything else holds in full — no invented number, no figure the specs do not carry, no exaggeration, no unsupported superlative, market/pricing figures quoted with source and date. This relaxes presentation only: the <1% rule in Numerical Precision still governs every derivation, including those feeding press copy.
