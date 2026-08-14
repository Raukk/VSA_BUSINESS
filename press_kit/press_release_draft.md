# Press Release — Draft

CONCEPTS: PRESS_RELEASE|OPEN_SOURCE_LICENSING|ASIC|AI_INFERENCE|PRODUCT_LAUNCH

**Status:** Draft for owner review. Embargo/date line assumes launch 2026-09-06 (`release_plan_2026-09-06.md`). All bracketed `[VSA_ASIC:...]` and `[repo-doc]` tags are source citations to carry through editing; strip only at final layout, never before owner sign-off. `[NEEDS SOURCE]` and `[PLACEHOLDER]` items must be resolved before release.

---

## Headline candidates (pick one)

1. **Anyone Can Now Build and Sell This AI Chip — Free, Forever**
2. **The RISC-V Playbook Comes for AI Chips**
3. **Seven Years of Chip Design, Given Away: VSA Open-Sources a Complete AI Inference Accelerator**
4. **An AI Chip Design With No HBM — and No Owner**
5. **This Chip's Simulator Is the Chip: VSA Open-Sources a Fully Deterministic AI Inference Design**

*(Headline test: does it open a question the reader has to click to close? A headline that merely describes the release — "VSA Releases Open-Source Design" — is a changelog entry, not a headline.)*

## Subhead

A complete reference architecture for a deterministic, no-HBM neural-network inference accelerator — specifications, a bit-exact golden model, and a working compiler demo — released under open licenses modeled on the RISC-V compatible-ecosystem playbook: anyone may build it, sell it, and extend it, royalty-free, and nobody can enclose the base design.

---

## Body

**[CITY, DATE — 2026-09-06]** — The VSA project today released the complete design of an open-source AI inference accelerator: the Virtual Systolic Array, a deterministic, weight-stationary MAC fabric architecture for neural-network inference [`PROSPECT_QA.md`]. The release includes finalized architecture specifications, a golden reference model, a toy compiler, and an end-to-end demonstration in which a compiled convolutional neural network runs bit-exact against the specification — the same answer, every cycle, every time [`release_plan_2026-09-06.md` §3].

The design takes a deliberately different bet from GPU-class accelerators: trade peak MACs-per-cycle for massive on-chip weight residency, so whole networks chain layer-to-layer on chip with no HBM and no external-RAM round-trips [`PROSPECT_QA.md`]. Execution is fully deterministic — no data-dependent timing, control, or routing; every cycle's schedule is fixed at configuration time [`VSA_ASIC:docs/baseline/core_design.md` §4]. Inference-only by design: that constraint is what makes full determinism possible [`PROSPECT_QA.md`].

Determinism is more than a correctness story — it is an economic one. Because a cycle-accurate model of the design *is* the design's exact behavior rather than an approximation, verification becomes equivalence checking and silicon validation becomes replay-and-diff, removing the design categories (coherence, speculation, dynamic scheduling, cache hierarchies) that consume most verification effort in conventional accelerator programs [`productization_feasibility_2026-07-10.md` §4]. An independent feasibility assessment concluded that a top-20 semiconductor company could take the design to production silicon as a standard mid-sized ASIC program — roughly 24–36 months and $150–400M all-in *(industry-norm estimate, ±2–3×)* [`productization_feasibility_2026-07-10.md` §2, §6].

The architecture also avoids the most constrained parts of today's AI supply chain: no HBM, no exotic 2.5D packaging required for the baseline, and a design targeted at mature process nodes [`productization_feasibility_2026-07-10.md` §4]. Its uniform tiling — one Group designed and verified once, then arrayed — and native defect tolerance (dead Blocks are disabled and the workload remapped, with identical numerical results) further push it toward the inexpensive, forgiving end of accelerator productization [`productization_feasibility_2026-07-10.md` §4, §7].

The timing is deliberate. Inference now accounts for roughly two-thirds of all AI compute in 2026, with the inference-optimized chip market alone estimated at over $50B within roughly $400–450B of AI datacenter capex (Deloitte, TMT Predictions 2026) [`productization_feasibility_2026-07-10.md` §9.1].

**Licensing: open like RISC-V, protected like a standard.** The design ships under CERN-OHL-W v2 (design and specifications), Apache-2.0 (golden model, compiler, and tools), and a reserved "VSA" certification mark for implementations that pass the public conformance surface [`license_decision_memo.md`]. Anyone may use, modify, manufacture, and sell — including proprietary products built on top — royalty-free, forever; the weak-reciprocity term prevents anyone from enclosing the base design itself, and the certification mark is what makes cross-vendor compatibility real, following the RISC-V and OpenPOWER precedent [`license_decision_memo.md`]. The project's owner additionally pledges a public non-assertion covenant: any personal IP rights in the design will never be asserted against any implementation, compatible or not [`license_decision_memo.md`]. *(License package pending final counsel review [`license_decision_memo.md`]; confirm complete before release.)*

**A design with a public paper trail.** The VSA primitive was published openly in November 2019 as TensorAsic (github.com/Raukk/TensorAsic); this release is that idea carried through to a complete architecture [`PROSPECT_QA.md`]. The dated public record doubles as prior art protecting future implementers [`productization_feasibility_2026-07-10.md` §8].

**What ships today, and what's next.** Today's release contains the finalized specifications, documentation and FAQ, the golden model and toy compiler with the bit-exact CNN demonstration, the conformance clause, and a contribution/governance page [`release_plan_2026-09-06.md` §3]. The project's published "proof ladder" lays out the next steps in public: reproducible cost-model benchmarks on named open models, independent physical-design results through an open OpenROAD/ASAP7 flow, and an FPGA demonstrator running bit-exact against the golden model — each artifact chosen to convert claims into things a reader can re-run rather than believe [`proof_ladder_2026-07-10.md`].

> **[PLACEHOLDER — owner quote.** Suggested territory, to be written/approved by the owner in their own words: why open, why now, and the "audit it, don't trust it" framing. No quote may be fabricated.]

The full release is available at **[PLACEHOLDER — public repo URL]**. A visual wiki is at https://raukk.github.io/VSA_WIKI/ [`PROSPECT_QA.md`].

---

## Boilerplate

**About the VSA project.** The VSA (Virtual Systolic Array) project is an open reference ASIC design for neural-network inference: a deterministic, weight-stationary MAC fabric in which whole networks chain layer-to-layer on chip with no HBM [`PROSPECT_QA.md`]. First published as TensorAsic in November 2019 [`PROSPECT_QA.md`], the design is released under CERN-OHL-W v2 and Apache-2.0 with a VSA certification mark for conformant implementations [`license_decision_memo.md`]. The project's goal is a provably correct, feasible baseline that companies can customize into production silicon, with basic interoperability between implementations [`PROSPECT_QA.md`]. No royalties, no monetization planned [`license_decision_memo.md`, `PROSPECT_QA.md`].

## Contact

**[PLACEHOLDER — press contact name / email]**
Repo: **[PLACEHOLDER — public repo URL]** · Wiki: https://raukk.github.io/VSA_WIKI/

---

## Editor's notes (not for publication)

- Do NOT quote clock-rate, tokens/sec, or cost-per-inference figures in the release body beyond what is above. The reference planning clock is the 4 GiHz planning clock (2^32 cycles/s = 4.294967296 GHz; "4.3 GHz" acceptable rounding) — if a clock number is requested by a journalist, give that, labeled as a planning basis, never as a silicon claim.
- Performance-advantage figures (e.g. the 3–5× hardware-COGS framing, ~1,000 tok/s/user target) are project estimates scoped against current market pricing — see `fact_sheet.md` for the properly labeled versions with citations; do not promote them into the release body unlabeled.
- Power figures: none may ship unless the power/thermal note lands per `release_plan_2026-09-06.md` §2; treat power as upside, not a claim [`productization_feasibility_2026-07-10.md` §9.2].
