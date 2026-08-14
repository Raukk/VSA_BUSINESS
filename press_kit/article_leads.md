# Article Leads — First Paragraphs Behind the Headlines

CONCEPTS: MARKETING_COPY|PRESS_RELEASE|PRODUCT_LAUNCH|TECHNICAL_COMMUNICATION

**Status:** Draft. Every launch headline links to an article that opens with the universal lead below — verbatim or lightly adapted — because the headlines carry bare claims and the FIRST sentence must state design status plainly and positively (owner ruling 2026-08-14, `launch_playbook.md` §1). Paragraph two is chosen to match the headline that was clicked. Replace `[REPO_URL]` before publishing.

---

## The universal lead (paragraph one, everywhere)

> The VSA is an open-source AI chip design — a paper chip, released in full today: the specifications, the compiler, and a bit-exact model of exactly what the silicon would do, free for anyone to run, fork, build, or sell, royalty-free, forever. There is no silicon yet, and that's the point. Every performance number below comes from a deterministic public model, so you don't have to trust a benchmark slide — you can re-run the math yourself. [REPO_URL]

(Grounding: license terms [`license_decision_memo.md`]; determinism/bit-exact model [`VSA_ASIC:docs/baseline/core_design.md` §4, `release_plan_2026-09-06.md` §3]; no RTL/silicon stated up front [`PROSPECT_QA.md`].)

---

## Paragraph two, by headline

### "10× faster! Impossible? Check the math."

> Here's the claim, with its edges showing: serving a DeepSeek-class model, this design's ceiling is 4,096 tokens per second *per user* — NVIDIA's best published per-user figure is just over 250 (DGX B200, March 2025). Ours is a 100%-utilization design ceiling at the design's planning clock; theirs is measured on shipping hardware — not the same kind of number, which is why the schedule that produces ours is in the repo for you to check. Even with a generous haircut for reality, the gap doesn't close. [`VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`]

### "Don't trust your eyes: an AI chip design that makes thousands of videos an hour."

> The projection: about one 5-second, 720p video clip per second from a single wafer-scale part, limited by cooling, not compute (the silicon itself could do ~4×). For scale: an H100 takes 12 to 24 *minutes* per clip on the same model, and per dollar of hardware this design projects 100–200× the best independently measured platform we could find. Projected, not measured — the full mapping, the power math, and the comparison table are all public. [`VSA_CASE_STUDIES:t4v-wan21/video_wan14b_hyper_pipeline_throughput_2026-08-04.md` §7.5, `VSA_CASE_STUDIES:t4v-wan21/video_wan14b_external_accelerator_benchmarks_2026-08-04.md` §3–4]

### "It's NVindependence day! A FOSS AI chip that is 1/10th the cost."

> One-tenth is us being careful. The cost model puts DeepSeek-class output on this design at $0.02–0.06 per million tokens with the hardware priced at street rates — against API pricing that runs $0.25 to over $2 per million today. Small models land 2–3 orders of magnitude under current API pricing. These are projected costs (three-year straight-line on the hardware) against current-generation GPUs' public prices, and every input to the projection is in the open. [`VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`, `VSA_CASE_STUDIES:lfm25/lfm25_on_hyper_evaluation.md`, `VSA_CASE_STUDIES:deepseek/agent_fleet_deployment_sketch_2026-07-10.md`]

### "This AI chip design should bring GPU and RAM prices back down. Gamers, rejoice."

> The mechanism is simple: your GPU costs what it costs because AI datacenters are buying the same silicon, memory, and fab capacity you are. This design runs AI inference with no HBM and no graphics-card silicon at all — and it's free for any manufacturer to build, so nobody controls the supply. Every rack of purpose-built inference hardware is demand taken *off* the parts gamers buy. No promises on your next upgrade — but this is the direction prices move when inference stops competing for your memory. [`PROSPECT_QA.md` (no HBM), `license_decision_memo.md` (anyone may build)]

### "Fork the FOSS AI chip design that beats GPUs — no HBM." / "Fork a FOSS AI chip design: build it, sell it, pay nobody"

> This is the RISC-V playbook aimed at AI inference: CERN-OHL-W v2 on the design, Apache-2.0 on the code, a certification mark for compatibility — fork-legal, enclosure-proof, royalty-free, with an owner non-assertion covenant on top. What ships today is the specs, the golden model, the toy compiler, and a CNN compiled and run bit-exact end-to-end; RTL is the next public step. The "beats GPUs" numbers are modeled, and the model is the repo — tear it apart. [`license_decision_memo.md`, `release_plan_2026-09-06.md` §3, `VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`]

### "Tired of waiting on AI? This chip design does 10,000 tokens a second." / "The AI swarms are coming! New chip runs 8,000 prompts at once."

> Ten thousand tokens per second is the *understated* version: for small models the design's per-user ceiling is over 260,000 tokens per second, and a DeepSeek-class deployment holds 8,192 prompts in flight at 4,096 tokens per second each. Those are machine ceilings, not serving promises — real rates depend on filling the machine, which is exactly why the docs talk about agent swarms: at these speeds, keeping the hardware fed is the hard part. The schedules behind every number are public. [`VSA_CASE_STUDIES:lfm25/lfm25_on_hyper_evaluation.md`, `VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`, `VSA_CASE_STUDIES:deepseek/agent_fleet_deployment_sketch_2026-07-10.md`]

### "No vendor lock-in. No license fees. An AI chip design anyone can build."

> Any company, any country, any fab: use it, modify it, manufacture it, sell it — no royalties, no negotiation, and no one who can ever pull the license or enclose the base design. Proprietary products built on top are explicitly welcome; the certification mark keeps implementations compatible with each other, the way RISC-V did it. An independent-style feasibility analysis in the repo prices productization as a standard mid-sized ASIC program, not a moonshot. [`license_decision_memo.md`, `productization_feasibility_2026-07-10.md` §2, §6]

---

## Assembly note

Article = universal lead + matching paragraph two + the relevant deep-dive body (press release body, fact sheet sections, or the Show HN long comment in `social_copy.md` as the technical walkthrough). Citations in brackets are internal grounding — strip from published text, keep the scoping words.
