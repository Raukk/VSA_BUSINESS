# Article Leads: First Paragraphs Behind the Headlines

CONCEPTS: MARKETING_COPY|PRESS_RELEASE|PRODUCT_LAUNCH|TECHNICAL_COMMUNICATION

**Status:** Draft. Every launch headline links to an article that opens with the universal lead below (verbatim or lightly adapted), then the paragraph matched to the headline that was clicked. Owner rulings 2026-08-14: (1) design status must land within the first paragraph / ~30 seconds of reading, woven into flowing prose, not a first-sentence disclosure; (2) NO em-dashes in any marketing or public copy (an AI-slop tell that draws backlash); use commas, colons, periods, or parentheses. Replace `[REPO_URL]` before publishing.

---

## The universal lead (paragraph one, everywhere; owner-written 2026-08-14)

> The VSA is an open-source AI chip design released in full today, royalty-free, for anyone to run, fork, build, or sell. There is no silicon yet, but the full specifications, the basic compiler, and a bit-exact model of exactly what the silicon would do are available for free, forever. Every performance number below comes from a deterministic public model, so you don't have to trust a benchmark slide: you can re-run the math yourself. [REPO_URL]

(Grounding: license terms [`license_decision_memo.md`]; determinism/bit-exact model [`VSA_ASIC:docs/baseline/core_design.md` §4, `release_plan_2026-09-06.md` §3]; no RTL/silicon stated up front [`PROSPECT_QA.md`].)

---

## Paragraph two, by headline

### "10× faster! Impossible? Check the math."

> Serving a DeepSeek-class model, the VSA's schedule delivers 4,096 tokens per second to a single user. The best per-user number NVIDIA has published is just over 250, on a DGX B200. Yes, one of those is a design ceiling at full utilization and the other is measured on shipping hardware, so take whatever haircut feels honest: halve it, halve it again, and the gap still doesn't close. The schedule that produces the number ships in the repo, so nobody has to take our word for anything. [`VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`]

### "Don't trust your eyes: an AI chip design that makes thousands of videos an hour."

> A single wafer-scale VSA part turns out about one five-second, 720p video clip every second, and that's the cooling-limited figure; the silicon underneath could go roughly four times faster. An H100 works on the same model for 12 to 24 *minutes* per clip. Per dollar of hardware, the projection runs 100 to 200 times the best independently measured platform anyone has published. The whole mapping (pipeline, power math, comparison table) sits in the open repo, waiting for someone to find the mistake. [`VSA_CASE_STUDIES:t4v-wan21/video_wan14b_hyper_pipeline_throughput_2026-08-04.md` §7.5, `VSA_CASE_STUDIES:t4v-wan21/video_wan14b_external_accelerator_benchmarks_2026-08-04.md` §3–4]

### "It's NVindependence day! A FOSS AI chip that is 1/10th the cost."

> Run the cost model and DeepSeek-class output lands at two to six cents per million tokens, with the hardware priced at street rates. Today's APIs sell that same output for a quarter to over two dollars. Small models fall further still, two to three orders of magnitude under current API pricing. "One-tenth the cost" is what survives after rounding against ourselves: three-year hardware amortization, current-generation GPU prices, and every input to the projection sitting in the open. [`VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`, `VSA_CASE_STUDIES:lfm25/lfm25_on_hyper_evaluation.md`, `VSA_CASE_STUDIES:deepseek/agent_fleet_deployment_sketch_2026-07-10.md`]

### "This AI chip design should bring GPU and RAM prices back down. Gamers, rejoice."

> Your GPU costs what it costs because AI datacenters buy the same silicon, memory, and fab capacity you do. The VSA runs inference with no HBM and no graphics silicon at all, and it's free for any manufacturer to build, so every rack of it that gets made is demand walking away from the parts in your shopping cart. Nobody can promise your next upgrade gets cheaper. But this is what it looks like when AI stops bidding against gamers for memory. [`PROSPECT_QA.md` (no HBM), `license_decision_memo.md` (anyone may build)]

### "Fork the FOSS AI chip design that beats GPUs, no HBM needed." / "Fork a FOSS AI chip design: build it, sell it, pay nobody"

> CERN-OHL-W v2 on the design, Apache-2.0 on the code, a certification mark to keep implementations compatible: the RISC-V playbook, aimed at AI inference. Fork it and the license holds; build it and sell it and nobody collects a royalty; try to enclose the base design and the license snaps shut. What shipped today is the specs, the golden model, a toy compiler, and a CNN running bit-exact end to end. RTL is the next rung on a public roadmap, and the beats-GPUs math is in the repo because tearing it apart is the point. [`license_decision_memo.md`, `release_plan_2026-09-06.md` §3, §5, `VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`]

### "Tired of waiting on AI? This chip design does 10,000 tokens a second." / "The AI swarms are coming! New chip runs 8,000 prompts at once."

> Ten thousand tokens a second is the modest version. The small-model ceiling is over 260,000 tokens per second to a single user, and a DeepSeek-class deployment holds 8,192 prompts in flight at 4,096 tokens a second each. A machine this fast inverts the usual problem: the design docs spend less time on going faster than on keeping the thing fed, which is where the agent swarms come in. At these speeds, idle silicon is the only real waste. Every schedule behind every number is public. [`VSA_CASE_STUDIES:lfm25/lfm25_on_hyper_evaluation.md`, `VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`, `VSA_CASE_STUDIES:deepseek/agent_fleet_deployment_sketch_2026-07-10.md`]

### "No vendor lock-in. No license fees. An AI chip design anyone can build."

> Any company, any country, any fab can use it, modify it, manufacture it, and sell it: no royalties, no negotiation, no license that can ever be pulled, and no base design that can ever be enclosed. Proprietary products on top are welcome; the certification mark keeps them compatible with each other, the way RISC-V did it. And the repo prices what adoption actually costs (a standard mid-sized ASIC program, not a moonshot) because the fastest way to be taken seriously is to hand the evaluation team their homework already done. [`license_decision_memo.md`, `productization_feasibility_2026-07-10.md` §2, §6]

---

## Assembly note

Article = universal lead + matching paragraph two + the relevant deep-dive body (press release body, fact sheet sections, or the Show HN long comment in `social_copy.md` as the technical walkthrough). Citations in brackets are internal grounding: strip from published text, keep the scoping words.
