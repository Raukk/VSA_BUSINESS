# VSA Business

CONCEPTS: OPEN_SOURCE_LICENSING|GO_TO_MARKET|MARKET_SIZING|PRODUCT_NAMING|TECHNOLOGY_TRANSFER

Business, marketing and release planning for the Virtual Systolic Array (VSA) — a deterministic, weight-stationary ASIC reference design for neural-network inference. The design itself lives in [VSA_ASIC](https://github.com/Raukk/VSA_ASIC), which is upstream and normative for every technical claim made here.

## What belongs here

Plans for getting the design out and getting it noticed: release planning, licensing, productization guidance for adopters, market context, recruiting material, and naming/branding.

What does not: any assertion about how the hardware works. Those live in the specs and are cited from here by path. This repo describes and promotes the design; it never defines it.

## Contents

| Doc | What it covers |
|---|---|
| `release_plan_2026-09-06.md` | Public release plan targeting 2026-09-06, and the checklist gating it |
| `license_decision_memo.md` | The licence choice and the reasoning behind it |
| `productization_feasibility_2026-07-10.md` | What an adopter actually faces taking the open-sourced baseline to production silicon |
| `ai_market_size_and_token_throughput_2026-07-23.md` | Market sizing and industry token-throughput context |
| `PROSPECT_QA.md` | Rapid-fire Q/A for people considering joining the project |
| `dragon_naming_scheme.md` | Approved product-name pool — generic dragon-kind terms only, no named dragons from folklore or fiction |

The release-assembly area and press drafts remain in `VSA_ASIC:staging/` under that folder's gate until the release is ready, and are deliberately not duplicated here.

## Sourcing rule

Outward-facing claims get the strictest sourcing in the project. Every technical number carries its `VSA_ASIC:` path; every market or pricing figure names its source and date. Where this repo and a spec disagree, the spec wins and the document here is corrected — never the other way round.

## Conventions

Authoring rules are upstream copies, not editable here: `docs/doc_conventions.md`, `docs/claude_md_conventions.md`. Agent-facing rules (clock basis, precision, units, citation form) are in `CLAUDE.md`. Migration provenance is in `MIGRATION.md`.
