# Response Piece: The Agentic Latency Budget Is a Silicon Problem Too

CONCEPTS: INFERENCE_LATENCY|TAIL_LATENCY|EDGE_COMPUTING|AUTOREGRESSIVE_DECODING|KV_CACHE|SERVICE_LEVEL_OBJECTIVE|MARKETING_COPY

**Status:** Draft, owner review required before publication. A call-and-response to Jon Alexander, "Agentic AI has a latency problem that more compute won't solve," The New Stack, 2026-08-18 (https://thenewstack.io/agentic-ai-latency-infrastructure/), and the Akamai material behind it. Press-kit rules apply: natural register, no em-dashes, design status inside the first paragraph, every technical number carrying its source. External survey figures are quoted with source and date and are never re-derived. Replace `[REPO_URL]` before publishing.

**Rate basis (owner ruling 2026-08-19), binding on this piece:** the per-user decode rate depends on the model and the system configuration, so this copy claims the conservative floor, **over 1,000 tokens per second per user**, which is true across the configurations the repo has modeled. The per-configuration ceilings (4,096 tok/s/user DeepSeek-class, 262,144 tok/s/user small-model) appear in the fact block and may be used where a specific configuration is named; the "well over 10,000 tok/s" upper framing stays available for headlines per `launch_playbook.md` §1. Rounding 4,096 down to "over 1,000" is rounding against our own interest, which the presentational-rounding ruling sanctions (`CLAUDE.md`, owner 2026-08-18).

**Publication timing:** this is a launch-week asset, not a same-week reply. The repo is not public until 2026-09-06 (`release_plan_2026-09-06.md`) and a response article whose central link 404s is worse than no response. The thesis is evergreen (the Akamai report is a 2026 report and the 500 ms budget is now a widely quoted number), so it holds. Recommended slot: the Week-1 technical deep-dive in `launch_playbook.md` §5, or the Tuesday 2026-09-08 journalist follow-through window in §3, pitched to The New Stack itself as a contributed counterpoint.

---

## 1. Verdict: does the VSA answer this article?

**Partly, and the part it answers is the one the article's own remedy cannot reach.** Do not publish a flat "this chip solves the latency problem." It does not, and the piece is stronger without the overclaim.

The article decomposes agentic latency into terms. Scored honestly:

| Latency term in the article | Does the VSA address it? |
|---|---|
| Network transport, round trips to distant datacenters | **No.** A chip does not move packets. Akamai's edge argument stands on its own turf. |
| CPU-side tool execution and orchestration (quoted at up to 90.6% of total latency) | **No.** That is host and runtime work, not fabric work. |
| Serial model decode inside each reasoning step | **Yes, directly.** This is the term the design attacks, and the term edge placement leaves untouched. |
| Variance at peak load ("50% of deployments miss their targets") | **Yes, structurally.** Fully deterministic execution has no data-dependent timing (`VSA_ASIC:docs/baseline/core_design.md` §4). |
| Time to first token, prefill, KV cache eviction spikes | **Not claimable.** No sourced VSA prefill or TTFT figure exists. See §5. |

The usable thesis is therefore narrow and true: *the headline is right that more compute will not solve this, because more GPUs buy throughput, not serial turnaround. Different compute changes the serial term.* That is a complement to the edge argument, not a rival to it, and it should be written as one.

## 2. Headline candidates

Owner hook rules apply (`launch_playbook.md` §1): one idea, payload first, target 60 characters, hard cap 80, no em-dashes.

1. **More compute won't fix agent latency. Different compute can.** *(60 chars. Direct mirror of the original headline; the recommended primary.)*
2. **A 500ms agent budget buys 6 tokens at p99. It should buy 500.** *(61 chars.)*
3. **Agentic AI's latency problem is also a silicon problem.** *(55 chars.)*
4. **The edge fixes the network. It can't fix your token rate.** *(57 chars.)*

## 3. Draft article

> **More compute won't fix agent latency. Different compute can.**
>
> The VSA is an open-source AI chip design released in full, royalty-free, for anyone to run, fork, build, or sell. There is no silicon yet, but the full specifications, the basic compiler, and a bit-exact model of exactly what the silicon would do are available for free, forever. Every performance number below comes from that deterministic public model, so you don't have to trust a benchmark slide: you can re-run the math yourself. [REPO_URL]
>
> Jon Alexander's piece in The New Stack last week is right about the thing most hardware people get wrong. Agentic AI has a latency problem, and buying more GPUs does not fix it. The numbers behind it are worth repeating: in Akamai's State of AI Inference 2026 survey of 200 practitioners, 82% of organizations say their most critical use cases need end-to-end responses in 500 milliseconds or less, 64% now target 250 milliseconds or less, and half of deployments miss those targets at peak load. An agent that fans a single request out into fifty sequential tool calls, the article notes, "quickly incurs seconds of transport latency."
>
> The prescribed cure is distribution: keep heavy reasoning in the centralized core, put the orchestration and tool execution on fast CPUs at the edge, and stop paying for round trips. That is a real fix for a real term in the budget, and nothing below argues with it.
>
> But there is a second term, and it is the one that distribution cannot touch. Between the tool calls sits the model, generating tokens one after another, and that generation is strictly serial. You cannot parallelize the fourth token of a reasoning step across two racks; token four waits for token three. Adding accelerators to a cluster raises how many users you serve at once. It does not make any one user's next token arrive sooner. Which is exactly why "more compute won't solve it" is true as written, and also why the conclusion drawn from it is too generous to the silicon.
>
> Do the arithmetic on the budget everyone is quoting. The best per-user decode rate NVIDIA has ever published is just over 250 tokens per second, on a DGX B200 in March 2025. At that rate, 500 milliseconds of pure generation buys 125 tokens. Under MLPerf v5.1 serving conditions on a DGX H200, the published p99 per-user rate is 12.5 tokens per second. At that rate, the same 500 milliseconds buys 6.25 tokens. Six tokens. Not six tool calls, not six reasoning steps. Six tokens, before a single packet has crossed a single network.
>
> That is the shape of the problem. The survey says half of deployments miss their latency targets at peak load, and "at peak load" is the operative phrase, because the gap between the best published per-user rate and the published p99 rate under load is a factor of twenty. Agent SLAs do not live on averages. They live on the bad case, and on a batching, memory-bandwidth-bound accelerator the bad case is whatever the scheduler and the other tenants decide it is.
>
> The VSA is a bet that both halves of that are architectural choices rather than facts of nature. It is a deterministic, weight-stationary MAC fabric that trades peak MACs-per-cycle for enough on-chip weight residency to keep whole networks resident and chain them layer to layer, with no HBM and no external-RAM round trips. What that buys depends on the model and the system you build, and the modeled configurations range from a few thousand tokens per second per user on a DeepSeek-class flagship to well over a hundred thousand on a small model, so take the floor rather than the ceiling: **over 1,000 tokens per second, to a single user, while thousands of other users are being served at the same time.** Both halves of that sentence matter. The per-user rate is not bought by starving the batch.
>
> Put the floor back in the budget. At 1,000 tokens per second, 500 milliseconds buys 500 tokens of generation, and a 100-token reasoning step takes 100 milliseconds. That is four times the best per-user rate NVIDIA has published and eighty times its published p99, and it is the number we are prepared to defend rather than the best one we could quote.
>
> Notice what that does and does not do. Five 100-token reasoning steps land exactly on the 500-millisecond budget, with nothing left for the network or the tools. So this does not replace Alexander's argument; it is what makes his argument worth making. Shave the transport, and the generation term is no longer the thing that ate the entire budget before you started. Leave the generation term where it is, and at the best published GPU rate you get one reasoning step per budget, and at the published p99 you get none, no matter how close to the user you move the CPU. Fix one term and the other still dominates. Fix both and the budget closes. On the specific configurations, the headroom is much larger than the floor suggests: a DeepSeek-class deployment models at 4,096 tokens per second per user with 8,192 users concurrent, and a small routing or tool-selection model, the kind of thing an orchestrator actually runs between the big calls, models at 262,144, which puts fifty consecutive 100-token steps at about 19 milliseconds of total generation.
>
> The second half is the part that matters more for anyone writing an SLA. VSA execution is fully deterministic: no data-dependent timing, no data-dependent control, no data-dependent routing, every cycle's schedule fixed at configuration time by the compiler. There is no scheduler making a choice at runtime, so there is no distribution of outcomes to have a tail. The p99 is the p50. For a workload defined by a hard budget that half the industry is currently missing at peak, a fabric whose worst case equals its typical case is a different kind of object than a faster one.
>
> Now the honest part, because the numbers above are modeled and the project would rather say so than be caught at it. There is no silicon and no RTL. Those figures are design ceilings at 100% utilization on a stated 4 GiHz planning basis, and the NVIDIA figures are measured deliveries on shipping hardware, which is not a like-for-like comparison and is not presented as one. That is part of why the claim above is the floor and not the ceiling: halve it, halve it again, and a 500-millisecond budget still looks completely different on one side of that comparison than the other. The reason we can invite that is determinism: the golden model is not an approximation of the hardware's behavior, it is the hardware's behavior, so the schedule that produces every number above ships in the repo for anyone to run and anyone to break.
>
> Three more things this design does not do, stated plainly. It does not move your packets, so every millisecond Akamai is talking about is still there and still worth eliminating at the edge. It does not run your tools, so the CPU-side work that the report puts at up to 90.6% of agentic latency is untouched by anything here. And we publish no time-to-first-token or prefill figure, because we do not have a sourced one, and an agent step that re-reads a long context is prefill-heavy by nature. That analysis is queued, in public, along with the rest of the open questions.
>
> What is left after all of that subtraction is still the interesting claim. The article's diagnosis is correct: throwing GPUs at an agent does not make it answer faster. The conclusion worth adding is that this is a statement about a particular architecture, not about silicon in general. Distribute the orchestration, absolutely. Then look at what is left in the budget, and notice how much of it is one model, generating one token at a time, at a rate that nobody has treated as a design target.
>
> The design is open, the license is CERN-OHL-W v2 with Apache-2.0 on the code, nobody collects a royalty, and the math is in the repo because tearing it apart is the point. [REPO_URL]

## 4. Fact block: exact figures and sources

Required by the presentational-rounding ruling (`CLAUDE.md`, owner 2026-08-18): every rounded figure in §3 appears here exact, with its path.

**VSA figures (all modeled design ceilings, 4 GiHz planning clock = 2^32 cycles/s = 4.294967296 GHz, 100% utilization; not silicon claims):**

| Figure | Exact value | Source |
|---|---|---|
| Per-user decode rate, claimed floor across modeled configurations | **over 1,000 tok/s/user** (owner ruling 2026-08-19; conservative baseline, true regardless of which modeled configuration is quoted) | derived floor over the two rows below; consistent with the ~1,000 tok/s/user per-user decode-rate target in `VSA_CASE_STUDIES:deepseek/llm_decode_speed_estimate_2026-06-11.md` via `productization_feasibility_2026-07-10.md` §9.2 |
| DeepSeek-V3/R1 configuration, per-user ceiling | 4,096 tok/s/user, 8,192 concurrent users, ~33.5M tok/s aggregate | `VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`, via `launch_playbook.md` §1 (owner-cleared 2026-08-14) |
| LFM2.5-class small model, single wafer, per-user ceiling | 262,144 tok/s/user (system ~16.8M tok/s) | `VSA_CASE_STUDIES:lfm25/lfm25_on_hyper_evaluation.md`, via `launch_playbook.md` §1 |
| Determinism | No data-dependent timing, control, or routing; every cycle's schedule fixed at configuration time | **[spec]** `VSA_ASIC:docs/baseline/core_design.md` §4 |
| Golden model is exact behavior, not an estimate | Verification collapses to equivalence checking; published numbers are re-runnable | `proof_ladder_2026-07-10.md` §1 |
| No HBM, weights resident on chip, layer-to-layer chaining | Architecture premise | `PROSPECT_QA.md`, `productization_feasibility_2026-07-10.md` §4 |
| Short-context agent-fleet deployment point | ~$1–4M rack-scale system | `VSA_CASE_STUDIES:deepseek/agent_fleet_deployment_sketch_2026-07-10.md`, via `productization_feasibility_2026-07-10.md` §9.1 |

**Competitor figures (measured deliveries on shipping hardware; non-basis clocks and conditions are theirs, not ours):**

| Figure | Exact value | Source |
|---|---|---|
| NVIDIA best published per-user rate | >250 tok/s/user, DGX B200, Mar 2025 | `VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`, via `launch_playbook.md` §1 |
| NVIDIA published p99 per-user rate under MLPerf serving conditions | 12.5 tok/s/user, DGX H200, MLPerf v5.1 | same |

**External survey figures [external], quoted never derived:** Akamai, *The State of AI Inference 2026* (survey of 200 AI practitioners): 82% of organizations require end-to-end response ≤500 ms for their most critical use cases; 64% require ≤250 ms; 50% of deployments fail to meet those targets at peak load; CPU-side processing accounts for up to 90.6% of total latency in agentic workloads; a workflow of 50 sequential tool calls "quickly incurs seconds of transport latency." Sourced via Jon Alexander, The New Stack, 2026-08-18 (https://thenewstack.io/agentic-ai-latency-infrastructure/) and Akamai, "Agentic Disconnect: The Latency Crisis Facing Modern AI Architecture" (https://www.akamai.com/blog/ai/agentic-disconnect-latency-crisis-modern-ai-architecture), both web-verified 2026-08-19. Report PDF: https://www.akamai.com/site/en/documents/research-paper/2026/the-state-of-ai-inference.pdf.

**Derived arithmetic used in §3** (decode only; excludes prefill, network transport, and tool execution time, which is stated in the copy):

| Rate | Tokens in 500 ms | Tokens in 250 ms | Time for one 100-token step | 100-token steps inside 500 ms |
|---|---|---|---|---|
| 12.5 tok/s (DGX H200 MLPerf v5.1 p99) | 6.25 | 3.125 | 8.0 s | 0 |
| 250 tok/s (DGX B200 published record) | 125 | 62.5 | 400 ms | 1 |
| **1,000 tok/s (VSA claimed floor)** | **500** | **250** | **100 ms** | **5, exactly filling the budget** |
| 4,096 tok/s (VSA, DeepSeek configuration) | 2,048 | 1,024 | 24.4 ms | 20 (488 ms) |
| 262,144 tok/s (VSA, LFM2.5-class small model) | 131,072 | 65,536 | 0.38 ms | 50 steps = 19.1 ms |

Ratios stated in §3: the "factor of twenty" between the two NVIDIA figures is 250 / 12.5 = 20.0 exactly. "Four times the best per-user rate NVIDIA has published" is 1,000 / 250 = 4.0. "Eighty times its published p99" is 1,000 / 12.5 = 80.0. All three are exact at the claimed floor, and understate the modeled configurations by 4.1× (DeepSeek-class) and 262× (small model).

## 5. Claims deliberately NOT made

- **No per-user rate above the floor as a general claim.** Owner ruling 2026-08-19: the rate depends on model and setup, so general copy claims "over 1,000 tok/s/user" and only names 4,096 or 262,144 alongside the specific configuration that produces it.
- **No time-to-first-token or prefill claim.** No sourced VSA figure exists. Agentic steps re-prefill long contexts, so this is the strongest available counter to the piece and it is conceded in the copy rather than dodged. Queue a prefill/TTFT analysis upstream before any competitor reply forces the question.
- **No KV-cache claim.** The article's TTFT-spike-on-cache-eviction argument is left alone. The repo's only KV-cache-adjacent statement is that diffusion transformers avoid a per-user growing KV cache (`productization_feasibility_2026-07-10.md`, image/video generation note), which is about video generation, not LLM agents, and does not transfer.
- **No power or water claim.** Banned until the power/thermal note ships (`release_plan_2026-09-06.md` §2).
- **No network or transport claim.** Conceded explicitly to Akamai's argument, which costs nothing and buys credibility.
- **No claim that attention is done.** LLM agent serving depends on the attention/MoE variant, which is actively developed against a CNN baseline (`PROSPECT_QA.md`; `productization_feasibility_2026-07-10.md` §9.1). If a reviewer raises it, the honest answer is the roadmap answer.
- **No adversarial framing of Akamai.** They are right, they are cited approvingly, and the piece is additive. A vendor counterpoint that concedes the original's core point is far more likely to be published by the outlet that ran the original.

## 6. Resolved: per-user rate basis

**Closed by owner ruling 2026-08-19.** `fact_sheet.md` carried "~1,000 tok/s/user" (citing `VSA_CASE_STUDIES:deepseek/llm_decode_speed_estimate_2026-06-11.md`) while `launch_playbook.md` §1 carried 4,096 tok/s/user (citing `VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`, owner-cleared 2026-08-14), roughly 4× apart and both quotable from the press kit. The ruling resolves it: the rate is model- and configuration-dependent, so general copy claims the conservative floor of **over 1,000 tok/s/user**, and the per-configuration ceilings are quoted only with their configuration named. Both source documents remain correct under that framing and neither needs a correction. Remaining hygiene task before the T-14 fact-sheet freeze (`launch_playbook.md` §2): restate the `fact_sheet.md` row as "over 1,000 tok/s/user (floor; 4,096 DeepSeek-class, 262,144 small-model)" so a journalist reading the sheet cannot quote the 4× gap as an inconsistency.
