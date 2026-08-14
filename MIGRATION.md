# Migration Record — VSA_ASIC → VSA_BUSINESS

CONCEPTS: MONOREPO_SPLIT|REPOSITORY_MIGRATION|SOURCE_OF_TRUTH|PROVENANCE

**Source:** `Raukk/VSA_ASIC` @ `a523961` (tree state after the 2026-08-14 three-repo split)
**Date:** 2026-08-14
**Authority:** owner ruling 2026-08-14 — a repo "for business and marketing and other plans on getting this out and popularized".

6 documents migrated, all md5-verified byte-identical to source **before** any citation edit. No number, claim, or conclusion changed — citation paths only.

## Path map

| New path | Old VSA_ASIC path |
|---|---|
| `release_plan_2026-09-06.md` | `docs/release_plan_2026-09-06.md` |
| `license_decision_memo.md` | `docs/license_decision_memo.md` |
| `productization_feasibility_2026-07-10.md` | `docs/productization_feasibility_2026-07-10.md` |
| `ai_market_size_and_token_throughput_2026-07-23.md` | `docs/ai_market_size_and_token_throughput_2026-07-23.md` |
| `PROSPECT_QA.md` | `docs/PROSPECT_QA.md` |
| `dragon_naming_scheme.md` | `docs/dragon_naming_scheme.md` |

Flat at the repo root: six documents do not need a folder hierarchy, and every one of them is a top-level concern of this repo rather than a subsection of something larger.

## Deliberately not moved

- **`VSA_ASIC:staging/`** (the public-release assembly area, its manifest and the press drafts — fact sheet, lab brief, launch copy). Owner ruling: staging stays local until the release is ready. It has its own strict gate and its staged copies are explicitly non-authoritative, so splitting it now would fragment the release process mid-flight. `release_plan_2026-09-06.md` and `license_decision_memo.md` are cited from there across the repo boundary.
- **`VSA_ASIC:docs/model_turnover_analysis_2026-07-10.md`**. Reads as business at a glance (deployment cadence, release turnover) but is an engineering analysis: per-chip reload mechanics, quantization calibration, golden-reference comparison, rolling and canary deployment. It stayed upstream.

## Citation rewrites

21 occurrences across 14 distinct path tokens. One resolved locally (`release_plan` → `license_decision_memo.md`, both migrated); the other 13 became `VSA_ASIC:` cross-repo refs. Bare filenames were resolved against each doc's original directory, so `design_open_items.md` correctly became `VSA_ASIC:docs/baseline/design_open_items.md` rather than a guess.

Refs already qualified by the earlier split waves (`VSA_WIKI:`, `VSA_CASE_STUDIES:`) were left untouched — `productization_feasibility` and `PROSPECT_QA` both cite the wiki, and `release_plan` cites the DeepSeek case-study material.

One token was left as-is: the fragment `_v1.0.md` in `PROSPECT_QA.md`, which is part of a filename pattern in prose, not a link.

## Clock audit

Every migrated file grepped for `GHz`/`MHz`/`GiHz`. **No violation found.** Hits are the 4 GiHz planning clock or figures explicitly derived from it, plus labelled competitor-hardware clocks in comparison passages — legitimate non-basis facts under the CLOCK RULE, and expected in this repo given how much of it is comparison copy.
