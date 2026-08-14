# VSA Fact Sheet (Journalist One-Pager)

CONCEPTS: FACT_SHEET|AI_INFERENCE|ASIC|OPEN_SOURCE_LICENSING|MARKET_SIZING|DESIGN_VERIFICATION

**Status:** Draft. Every number below carries its source. Three provenance tiers, labeled: **[spec]** = from the design docs (normative), **[estimate]** = project/industry estimate with stated uncertainty, **[external]** = third-party market/competitor figure, quoted with source and date. `[PLACEHOLDER]` items resolve before release.

---

## What it is

An **open-source reference ASIC design for AI inference**: the Virtual Systolic Array (VSA), a deterministic, weight-stationary MAC fabric [`PROSPECT_QA.md`]. It is a chip *design* — specifications, golden model, compiler — not silicon and not a product. Released **2026-09-06** [`release_plan_2026-09-06.md`].

**In one sentence:** trade peak MACs-per-cycle for massive on-chip weight residency, so whole networks chain layer-to-layer on chip with no HBM and no external-RAM round-trips [`PROSPECT_QA.md`].

## Why it matters

- **Fully deterministic execution** — no data-dependent timing, control, or routing; every cycle's schedule is fixed at configuration time **[spec]** [`VSA_ASIC:docs/baseline/core_design.md` §4]. The golden model is bit-exact against the design spec, so verification becomes equivalence checking and published numbers are re-runnable by the reader [`proof_ladder_2026-07-10.md` §1].
- **No HBM** — whole networks stay resident on chip and chain layer-to-layer, sidestepping the single most supply-constrained component in the AI hardware chain **[spec]** [`PROSPECT_QA.md`, `productization_feasibility_2026-07-10.md` §4].
- **Anyone can productize it.** Feasibility assessment: a top-20 semiconductor company reaches revenue silicon in ~24–36 months for ~$150–400M all-in — a standard mid-sized ASIC program, no missing capability **[estimate: industry-norm, ±2–3×]** [`productization_feasibility_2026-07-10.md` §2, §6, §10].

## Key numbers (with sources)

| Figure | Value | Tier / source |
|---|---|---|
| Public release date | 2026-09-06 | [`release_plan_2026-09-06.md`] |
| First publication of the VSA primitive | Nov 2019 (TensorAsic, github.com/Raukk/TensorAsic) | [`PROSPECT_QA.md`] |
| Planning clock (reference basis for all VSA rate/time/power figures) | 4 GiHz planning clock = 2^32 cycles/s = 4.294967296 GHz (4.3 GHz acceptable rounding) — a planning basis, **not** a silicon claim | **[spec]** [VSA_BUSINESS:CLAUDE.md clock rule, upstream VSA_ASIC ruling 2026-08-04] |
| Productization cost / time (top-20 adopter) | ~$150–400M, ~24–36 months to revenue silicon | **[estimate: industry-norm, ±2–3×]** [`productization_feasibility_2026-07-10.md` §6] |
| Adoption tipping point (project's own stated bar) | benchmarked ≥ ~2× sustained cost-per-inference on the buyer's top models | **[estimate/analysis]** [`productization_feasibility_2026-07-10.md` §9.2] |
| Hardware-cost framing | 3–5× hardware-COGS advantage, scoped **vs. the current HBM-loaded, monopoly-priced market** — not a physical constant | **[estimate]** [`VSA_WIKI:wiki/analyses/economics.md` via `productization_feasibility_2026-07-10.md` §9.2] |
| Per-user decode-rate target | ~1,000 tok/s/user (structural bandwidth argument; simplifications acknowledged; competitor-anchored benchmark queued) | **[estimate: designer's analysis]** [`VSA_CASE_STUDIES:deepseek/llm_decode_speed_estimate_2026-06-11.md` via `productization_feasibility_2026-07-10.md` §9.2] |
| Inference share of AI compute, 2026 | ~two-thirds (vs. one-third in 2023); inference-chip market >$50B within ~$400–450B AI datacenter capex | **[external]** Deloitte TMT Predictions 2026, web-verified 2026-07-10 [`productization_feasibility_2026-07-10.md` §9.1] |
| Worldwide AI spending, 2026 | ~$2.5T (Gartner, +44% YoY, press release 2026-01-15) | **[external]** [`ai_market_size_and_token_throughput_2026-07-23.md` §1] |
| H100-class street price vs. estimated COGS | ~$25–40K street vs. ~$3.3K public cost-bridge COGS | **[external/estimate]** [`VSA_WIKI:wiki/analyses/economics.md`, verified 2026-07-07, via `productization_feasibility_2026-07-10.md` §9.2] |
| FPGA demonstrator cost (post-launch roadmap) | ~$100–500 cloud FPGA rental for validation; one VSA Block <1% of an AMD VU47P | **[estimate: external agent estimate provided by owner, 2026-07-10]** [`proof_ladder_2026-07-10.md` Rung 3] |

*Do not detach the tier labels or scoping clauses from these numbers when quoting.*

## What ships on day one

Finalized specs (EFAU v1.0, WRU consolidated), `docs/baseline/`, FAQ, terminology, wiki, license package, conformance clause, contribution/governance page, **golden model + toy compiler running a 4-layer CNN end-to-end bit-exact** — the launch demo [`release_plan_2026-09-06.md` §2–3]. RTL and FPGA are explicitly post-launch roadmap [`release_plan_2026-09-06.md` §5].

## What it is NOT (pre-empting the obvious objections)

- **Not silicon, not RTL (yet).** The launch proof story is specs + golden model + compiled bit-exact demo; FPGA demonstrator is the announced next milestone [`release_plan_2026-09-06.md` §5].
- **Not a training chip.** Inference only — that constraint enables the determinism [`PROSPECT_QA.md`].
- **Not for 4–8-GPU rigs.** The design targets rack-scale deployments; the sub-$1M SOTA-LLM segment is explicitly conceded to HBM-based hardware [`productization_feasibility_2026-07-10.md` §9.1].
- **Not claiming a silicon clock speed.** All rate figures are on the 4 GiHz planning-clock basis; an adopter picks their own operating point [VSA_BUSINESS:CLAUDE.md clock rule; `productization_feasibility_2026-07-10.md` §5.1].

## License & governance

- **Design/specs:** CERN-OHL-W v2 — use, modify, manufacture, sell freely, incl. proprietary products on top; modifications to the base design itself must be shared; express patent grant [`license_decision_memo.md`].
- **Code:** Apache-2.0 (patent grant, defensive termination) [`license_decision_memo.md`].
- **Compatibility:** reserved "VSA" certification mark — incompatible forks are legal, they just can't wear the badge (RISC-V/OpenPOWER precedent) [`license_decision_memo.md`].
- **Owner pledge:** public non-assertion covenant over any personal IP in the design, against any implementation, compatible or not [`license_decision_memo.md`].
- No royalties ever; no monetization planned [`license_decision_memo.md`, `PROSPECT_QA.md`].
- *(Pending counsel review as of memo date — confirm closed before publication [`license_decision_memo.md`].)*

## Timeline

| When | What |
|---|---|
| Nov 2019 | VSA primitive published as TensorAsic [`PROSPECT_QA.md`] |
| 2026-09-06 | Full open release: specs + golden model + toy compiler + bit-exact CNN demo [`release_plan_2026-09-06.md`] |
| Post-launch | Reproducible benchmarks, open-flow (OpenROAD/ASAP7) PD results, FPGA demonstrator (Block + Epilogue, then a Group), per the public proof ladder [`release_plan_2026-09-06.md` §5, `proof_ladder_2026-07-10.md`] |

## Links

- Public repo: **[PLACEHOLDER — repo URL]**
- Visual wiki: https://raukk.github.io/VSA_WIKI/ [`PROSPECT_QA.md`]
- 2019 prior art: https://github.com/Raukk/TensorAsic [`PROSPECT_QA.md`]
- Press contact: **[PLACEHOLDER — name/email]**
- Hi-res diagrams / images: **[PLACEHOLDER]**
