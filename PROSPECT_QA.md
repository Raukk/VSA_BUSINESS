# Considering Joining? — Rapid-Fire Q/A

CONCEPTS: ATTENTION|MOE|SOFTMAX|RTL|CONVOLUTIONAL_NEURAL_NETWORK

One-line answers for people deciding whether this project is for them. Details live in the linked docs; don't trust anything longer than a sentence here.

**Q: What is this?**
A: An open reference ASIC design for NN inference, built around the Virtual Systolic Array (VSA) — a deterministic, weight-stationary MAC fabric.

**Q: Is it software?**
A: No — it's a logic/architecture design project. The deliverables are specifications, analyses, and (eventually) RTL, not an app.

**Q: What's the one-sentence pitch?**
A: Trade peak MACs-per-cycle for massive on-chip weight residency, so whole networks chain layer-to-layer on chip with no HBM and no external-RAM round-trips.

**Q: Is there silicon? RTL?**
A: Neither yet. The architecture is defined, core subsystems (Block, E1 interface, Epilogue) have detailed specs; the WRU is next. Early RTL drafts exist but are superseded (`docs/old_drafts/`).

**Q: Who owns it / what's the license?**
A: Intent (not yet final) is permissive commercial or CC0 — anyone builds royalty-free, proprietary improvements stay unrestricted. No monetization planned.

**Q: So I won't get paid?**
A: Correct. This is an open, unpaid proof-of-concept reference design.

**Q: What's the end goal?**
A: A provably correct, feasible baseline that companies can customize into production silicon, with basic interoperability between implementations.

**Q: Training or inference?**
A: Inference only. That constraint is what makes full determinism possible.

**Q: "Deterministic" meaning what?**
A: No data-dependent timing, control, or routing — every cycle's schedule is fixed at configuration time (`VSA_ASIC:docs/baseline/core_design.md` §4).

**Q: CNNs only?**
A: CNNs are the baseline; LLM/transformer support (attention cache, distributed softmax, MoE) is an actively developed variant (`docs/baseline/attention_cache_*.md`).

**Q: Isn't INT8-only obsolete?**
A: INT8 is a PoC simplicity choice, not an architectural constraint — other formats (BF16, MXFP4) swap in as variants (`VSA_ASIC:docs/FAQ.md`).

**Q: What skills are useful?**
A: Digital/ASIC design, computer architecture, NN inference internals, compiler/scheduling thinking, verification planning, technical writing.

**Q: What work is actually open right now?**
A: WRU full specification, verification planning, spec refinement, and the items in `VSA_ASIC:docs/baseline/design_open_items.md`.

**Q: Where do I start reading?**
A: `VSA_ASIC:docs/QUICKSTART.md` (15 min), then `VSA_ASIC:docs/FAQ.md`, then `VSA_ASIC:docs/baseline/core_design.md`. Visual wiki: https://raukk.github.io/VSA_WIKI/

**Q: How is the repo organized?**
A: `docs/baseline/` = authoritative; root `*_v1.0.md` files = locked specs; `docs/brainstorm/` and `docs/old_drafts/` = background/history — the FAQ wins over stale notes.

**Q: How old is this idea?**
A: The VSA primitive was published publicly in Nov 2019 as [TensorAsic](https://github.com/Raukk/TensorAsic); this project is its continuation.

**Q: Biggest thing to internalize before contributing?**
A: Everything is scheduled at compile time by an external compiler — if a proposal needs a runtime decision in the datapath, it's wrong by construction.
