# Productization Feasibility Assessment — Open-Sourced Baseline to Production Silicon

CONCEPTS: ASIC_TAPEOUT|DESIGN_VERIFICATION|DIE_HARVESTING|YIELD_MODEL|TCO|PATENT_PROSECUTION|DIFFUSION_TRANSFORMER|FREEDOM_TO_OPERATE

**Status:** Brainstorm/analysis (2026-07-10). Not a design document. Assesses the *path to market*, not the correctness of the design itself.
**Provenance of figures:** three tiers, labeled throughout — *(web-verified 2026-07-10)* for externally checked facts, *(industry-norm estimate)* for order-of-magnitude program costs from general industry knowledge (unverified against quotes; treat as ±2–3×), and *(designer's note)* for owner-stated intent or opinion.

---

## 1. Question and scope

If this project reaches completion and is released under a permissive commercial license or CC0 (see the licensing-direction note in VSA_ASIC:CLAUDE.md/README — not yet final), could a large experienced chip company take it to a production-grade product in reasonable time and at reasonable cost?

**Scope as agreed with the owner (2026-07-10 session):**

- **Completion artifact assumed:** docs + architecture specs + synthesizable RTL + verification suite + basic compiler, all at a maturity *between* toy-example and production — minimum functionality working in all areas at the docs/software level; no silicon. The adopter does the final hardening, optimization, and productization.
- **Target product:** datacenter inference accelerator; server-grade (water cooling, industrial power); process node 5 nm-class or below (adopter's choice); possible interposer for multi-chip assemblies (electrically simple die-edge-to-die-edge wiring).
- **Target end customer:** hyperscaler/frontier-lab-class buyers purchasing ~$100M+ of compute at once to run three or four specific models. Hand-tuned final configurations by a few customer engineers are an accepted deployment model; the base compiler must handle standard (Keras/TF-class) layers at reasonable efficiency.
- **Adopter archetype:** top-20 semiconductor company with full physical-design capability (AMD as best case). Differences for smaller players noted where material.
- **Software stack in scope** (compiler/runtime is historically where accelerators die, not silicon).
- **Explicitly out of scope:** whether the architecture is correct or performant; documentation gaps; company politics / competitive-response scenarios. "Would they" is limited to a theoretical tipping-point estimate.

## 2. Verdict

**Feasible, comfortably.** For a top-20 player this is a mid-sized ASIC program: roughly **24–36 months from adoption decision to revenue silicon** and roughly **$150–400M all-in** for a first production generation *(industry-norm estimate)*. The architecture's own properties (determinism, uniform tiling, no HBM, mature node, defect tolerance) push it toward the cheap/fast end of AI-accelerator productization. The wide-band risks are fleet-scale software automation and one packaging decision (the large multi-chip interposer), not the silicon.

## 3. What the gap consists of

From "mid-maturity reference RTL + basic compiler" to production, the adopter's work is the standard ASIC productization stack:

- RTL hardening and **full design verification** — the dominant engineering cost of any ASIC program (~half of typical program effort)
- Physical design at N5/N4-class, DFT/BIST, and productizing the repair/binning flow (fuses, defect maps, bin definitions)
- Third-party IP procurement and integration: DDR PHY + controller, PLLs, a small CPU core for the scheduling core, test/monitor IP (the reference design already draws this boundary cleanly — see `VSA_ASIC:docs/baseline/chip_io_ip_integration.md`)
- Packaging, silicon bring-up/validation, reliability qualification (JEDEC-class), production test development
- Board/system/cooling design for the server-grade target
- Compiler, runtime, firmware, and fleet-management software productization

None of this requires capabilities a top-20 company lacks; it is what their methodology teams do for every chip. The question is only how much the design fights them or helps them.

## 4. Structural advantages (why this sits at the easy end)

1. **Uniformity collapses PD and DV cost.** One Group is designed, verified, and hardened once, then arrayed ~128×. GPU-class programs carry dozens of heterogeneous blocks each needing separate attention. The Unit → Block → Group → chip hierarchy maps directly onto the hierarchical verification and place-and-route flows every large company already runs.
2. **Determinism is a verification gift.** Bit-exact, data-independent execution means a golden C model *is* the behavioral spec; DV becomes equivalence checking, silicon validation becomes replay-and-diff. No coherence, speculation, dynamic scheduling, or cache hierarchy — the categories that consume most DV effort and cause most respins don't exist here. This materially lowers the probability of a third tapeout.
3. **No HBM, no CoWoS, mature node.** Removes the largest cost/supply/schedule risk in contemporary AI accelerators. A 100–200 mm² die on N4/N5 with DDR and standard FCBGA is boring, and boring is what makes "reasonable time and cost" true. The mature-node IP ecosystem is fully de-risked and cheaper to license.
4. **Native defect tolerance de-risks the ramp.** Block/Group-disable means early production with immature test coverage still ships sellable parts; that flexibility usually has to be retrofitted. (See §7 for the binning software obligation this creates, and the GPU-harvesting comparison.)

## 5. Friction points

### 5.1 Clock frequency — a knob, not a gate

> **⚠ CLOCK RULE — out of compliance, not yet fixed.** The paragraph below names 3–4 GHz as the planning/spec basis. The sanctioned basis is the **4 GiHz planning clock** (2³² cycles/s); a reduced operating point must be expressed as a fraction of it (half-clock 2.147 GHz). Left in place rather than reworded because this doc's economics may have been computed on the stated basis and need checking first — tracked in `VSA_ASIC:docs/TODO.md` (2026-08-05 clock sweep). The re-pipelining argument itself is unaffected.

The reference design's 3–4 GHz figures are **planning numbers, not requirements** *(designer's note)*: the spec numbers use ~4 GHz because the design already carries deep inherent pipelining (adding stages is cheap for a throughput-target architecture) and compiled SRAM at these nodes demonstrably reaches 3–4+ GHz (`VSA_ASIC:docs/baseline/sram_density_speed_reference.md`). Because execution is deterministic, an adopter may **freely re-pipeline the implementation** — halve or double pipeline fill as their flow prefers — and the change is absorbed as compiler/schedule constants, not an architectural modification.

Assessment: this claim holds, with one precision. Re-pipelining freedom removes *logic-depth* timing as a constraint, which is the usual accelerator limiter. The remaining frequency limiters are the ones re-pipelining cannot fix — SRAM macro access time (addressed: the macros demonstrably run at target), clock distribution quality, and power/thermal density (flagged TBD in the timing doc's own risk list). So: no specific clock is required for feasibility; frequency is an implementation quality knob with linear throughput payoff. An adopter shipping a first generation at 2–2.5 GHz with conservative flows has a working product and a known upgrade path — *that is a statement about an adopter's silicon, not a VSA planning basis; all reference-design figures stay on the 4 GiHz clock and scale linearly from it.*

### 5.2 The large multi-chip interposer — the biggest hardware wildcard, coupled to the biggest market

"Basically just wires" is true electrically, but the LLM deployment sketches use 5×5 (25-die) assemblies — far beyond reticle-limited standard 2.5D. At that scale it becomes a genuine packaging program (panel-level fan-out or very large organic substrates, warpage, assembly yield, module test strategy). Budget impact if treated as a v1 requirement: plausibly +1 year and a nine-figure add *(industry-norm estimate)*.

The strategic coupling *(flagged by owner, confirmed)*: the workloads that most need the huge assemblies are the **LLM variants — which are also the dominant market**. So the interposer isn't an optional nice-to-have commercially; it is the ticket to the largest demand pool, and it feeds directly into the tipping-point math in §9 rather than into "can they build it" (they can — it's money and schedule, not capability).

Mitigation path: single chips and small (2×2 Quad) organic-substrate assemblies are standard packaging and serve the CNN/vision/smaller-model markets in v1, while the large-panel option matures as a v2 program.

## 6. Cost and timeline

*(All figures in this section: industry-norm estimates — order-of-magnitude from general knowledge of typical program costs, not verified quotes; ±2–3×.)*

| Bucket | Estimate | Notes |
|---|---|---|
| Engineering | $100–250M | ~150–300 peak headcount, 2–3 yrs: RTL/DV ~80–100, PD/DFT ~40–60, validation/test ~30, system/board ~20, software ~40–80 |
| Masks + tapeouts | $30–45M | Budget two full N5/N4 mask sets (~$15–20M each); determinism argues one respin suffices |
| Third-party IP | $10–30M | DDR PHY/controller, PLLs, CPU core, test IP |
| EDA, emulation, prototyping | $15–40M | |
| Packaging/test NRE, qual, board/system | $15–40M | Excludes the large-interposer program (§5.2) |
| **Total to production** | **~$150–400M** | |

**Timeline:** ~18–24 months decision → first tapeout (mid-maturity reference RTL saves perhaps 6–12 months vs. from-scratch; DV-to-signoff is the long pole regardless), + ~9–12 months bring-up/qual/ramp → **~30–36 months to revenue shipments**; ~24 months as an AMD-best-case with platform reuse. Calibration: 3–5× cheaper and faster than a leading-edge GPU flagship program; comparable to hyperscaler custom-silicon (Annapurna-class) programs.

**Actor sensitivity:** mid-size fabless — same program, stretched to ~3.5–4.5 years, IP/EDA line items proportionally heavier. Hyperscaler in-house silicon team — near-ideal fit; this is exactly the program shape they already run, and they own the deployment (no first-customer problem).

## 7. Software band — wide, but bounded by the deployment model

The accepted deployment contract (base compiler handles standard layers decently; the buyer runs 3–4 models on homogeneous $100M fleets with two or three dedicated engineers hand-tuning and formally checking final configurations) is what makes the software tractable — it is how early TPU and AWS Inferentia deployments actually operated. Determinism makes the "formally check" step unusually strong: a configuration can be exhaustively validated bit-for-bit before fleet rollout.

**The one place hand-tuning cannot stretch: per-chip defect binning at fleet scale.** Ten thousand chips with individual defect maps cannot each get engineer attention. Resolution *(owner-endorsed)*: normalize defects into a small number of equivalence classes (uniformity means "any one dead Block in a Group" is one class, not 16 positions), pre-compile one slightly-derated configuration per class, accept the utilization loss on derated bins. Commercially this is strictly better than scrap — a part sold at 50–75% price beats 0% *(designer's note)*. The deliverable is an automated remap layer between defect map and configuration image; engineering, not research — but it touches test, fuses, compiler, and fleet ops simultaneously, so it must be explicitly scoped or it falls between teams.

**Correction to a common assumption** *(web-verified 2026-07-10)*: large-GPU vendors do **not** trash defective dies wholesale — die harvesting is standard practice. The H100 physically carries 144 SMs but ships with 132 enabled, absorbing defects across up to 12 SMs while still selling as the flagship; lower bins recover more. The VSA's yield advantage over GPUs is therefore *not* "they scrap, we don't" — it is (a) finer disable granularity (Block ≪ 6 mm² SM), (b) near-zero unsalvageable area (a GPU die has non-redundant regions — memory controllers, command front-end — where a defect kills the die; the VSA minimizes these), and (c) the derating cost being a few percent of capacity rather than a bin drop. The yield estimate doc's framing already accounts for this (its GPU comparison notes harvested-die recovery); marketing claims should follow that framing.

Beyond the compiler: firmware, telemetry, RAS, config rollout — unglamorous, mandatory for hyperscaler procurement, ~a quarter of software headcount.

## 8. Licensing and IP-risk feasibility

Licensing direction *(designer's note, not final)*: permissive commercial license or CC0; royalty-free to build; shared baseline for basic cross-manufacturer interoperability; proprietary extensions unrestricted; no monetization planned.

- **Instrument choice matters:** CC0 expressly excludes patent rights — it is a copyright waiver only. For adopters betting nine figures, **Apache-2.0 or a hardware-specific equivalent (Solderpad, CERN-OHL-P)** with an express patent grant from contributors is the safer fit for the stated goal. The owner is willing to grant all his IP freely, and to pursue formal patent filings if an adopter needs and funds them *(designer's note)* — a defensively-filed patent with a royalty-free grant to all implementers (OIN/RISC-V-style) is the strong version of this.
- **Third-party infringement risk** (someone else claiming the design infringes *their* patents): the open release cannot eliminate this — freedom-to-operate analysis is the adopter's routine job, and top-20 companies carry patent war chests and cross-licenses. What the project *does* provide: the 2019 TensorAsic publication plus this repo are dated public prior art that (a) invalidates later-filed third-party claims on the disclosed techniques and (b) makes an aggressor's narrative awkward *(designer's note on the practical litigation dynamics)*. Prior art does not help against patents filed *before* 2019 — that residual risk is ordinary for any chip program.
- **Interoperability needs a conformance surface, not just a license.** A permissive license permits divergence; it doesn't prevent it. The baseline should designate which interfaces are conformance surfaces (configuration image format, inter-chip link, numeric conventions, compiler-visible contracts) versus free-to-modify internals, ideally with a minimal conformance test suite — the mechanism that makes RISC-V compatibility real. This can be a spec chapter plus a test directory; no foundation required.
- The third-party IP boundary (DDR PHY etc. licensed by the adopter, outside the open release) is already structured correctly.

## 9. Market fit and the tipping point ("would they," kept theoretical)

### 9.1 Workload landscape *(web-verified 2026-07-10, with assessment)*

- **Inference now dominates:** roughly two-thirds of all AI compute in 2026 (vs. one-third in 2023); the inference-optimized chip market alone is estimated >$50B in 2026 within ~$400–450B of AI datacenter capex ([Deloitte](https://www.deloitte.com/us/en/insights/industry/technology/technology-media-and-telecom-predictions/2026/compute-power-ai.html)). The product category is the right one at the right time.
- **LLMs are the dominant demand pool**, and serving them at scale is exactly the segment that wants the large multi-chip assemblies (§5.2). The biggest market and the biggest packaging risk are the same bet.
- **Classic computer vision / machine vision** *(owner's opinion: works great on VSA with far less headache — confirmed)*: genuinely CNN-shaped, maps naturally, and the edge-AI hardware market is real (~$25–30B in 2026, ~20%+ CAGR per [Grand View](https://www.grandviewresearch.com/industry-analysis/edge-ai-market-report)/[Fortune BI](https://www.fortunebusinessinsights.com/ai-inference-market-113705)) — but it is fragmented, largely edge/embedded rather than datacenter, and no single buyer writes $100M checks for it. It is a fine *second* market and a good v1-silicon proof market, not the program's economic engine.
- **Image/video generation — one important correction to the CNN framing** *(owner's opinion, partially revised by evidence)*: modern video/image generators (Sora-class, Veo, SD3/FLUX) are **diffusion transformers, not CNNs** — attention reportedly consumes 85%+ of video-generation inference time with quadratic scaling ([Introl](https://introl.com/blog/video-generation-ai-infrastructure-sora-scale-models-guide-2025)). So video gen does not escape the attention problem. **However**, it may fit the VSA *better than LLM serving does*, for a different reason: diffusion attention is **fixed-shape and bidirectional over a known token grid, repeated for a fixed number of denoising steps** — no autoregressive decode, no per-user growing KV cache, batch-friendly, throughput-bound, statically schedulable. That is far closer to the VSA's deterministic sweet spot than LLM decode is, *provided* the attention-capable variants exist. And the economics are hungry: video generation runs $0.50–$2.00 of GPU compute per 10-second clip — orders of magnitude above text — with analyses putting mass-market viability 100–300× below current cost ([Leonis](https://www.leoniscap.com/research/sora-and-the-future-of-ai-video-generation), [cost breakdown](https://aedelon777.substack.com/p/i-did-the-math-on-sora-ai-video-is)); a segment desperate for a big cost-per-inference reduction is a natural early adopter for an architecture claiming one. **Open question that decides the fit** *(owner, 2026-07-10)*: whether video-gen's dominant cost is HBM *bandwidth* (→ strong VSA win: on-chip SRAM + chaining attack data movement directly) or HBM *capacity* (→ poor fit: SRAM per bit costs far more than HBM). Queued for a dedicated analysis — see `VSA_ASIC:docs/TODO.md`.

**Deployment-scale floor** *(designer's note, 2026-07-10; extends the sub-$1M market-positioning assumption already recorded on the wiki decode-speed page)*: the VSA does not compete everywhere on the size axis, by design. For SOTA LLMs there is a hard floor — below some chip count the weights simply don't fit, and HBM-based hardware wins by default; the sub-$1M SOTA-LLM market is conceded. Each model has its own cutover point where shrinking budget flips the answer to GPU (or even CPU); smaller LLMs run on proportionally smaller VSA systems, and for CNNs the floor is far lower — plausibly a single server, or cutover against a single AI GPU. The 4–8-GPU-rig segment is not a target; the buyers who need this are the thousand-GPU-scale deployments. Productization implication: an adopter should not burden the program with small-system SKUs — the product is racks, not cards. *(Addendum 2026-07-10: three deployment points now sketched span the price axis — max-context LLM flagship ~$20–22M, short-context agent fleet ~$1–4M (`VSA_CASE_STUDIES:deepseek/agent_fleet_deployment_sketch_2026-07-10.md`), video pod ~$100–300K — all rack-scale products built from the same chip variants and interposers.)*

### 9.2 The tipping point

An adopter risks ~$200–400M plus 2–3 years of opportunity cost, and someone must be *first* to burn silicon on an architecture unproven in silicon — that first-mover proof burden, not program cost, is the real barrier.

The math that overcomes it: a hyperscaler-class buyer spending $1B+/yr on inference needs a **credible, benchmarked ≥ ~2× sustained cost-per-inference advantage on their actual top models** — at that level savings repay the whole program within roughly a year, with supply-chain independence riding along free. Below ~1.5×, program risk, software friction, and binning derates plausibly eat the margin and rational actors wait.

Supporting dynamics *(owner-stated, assessed as sound)*:

- **Against merchant GPUs at market price**, the markup alone plausibly funds a ~3× perf/$ story before any architectural factor. Nobody outside the vendor knows the true markup *(owner's caveat)*, but the project's already web-verified anchors (`VSA_WIKI:wiki/analyses/economics.md`, verified 2026-07-07) bound it well enough: H100-class street price ~$25–40K against a public cost-bridge COGS of ~$3.3K (~8–12× street/COGS), consistent with NVIDIA's reported FY2026 gross margin of 71–75%. Even if the true COGS were double the estimate, the street/COGS ratio stays ≥4× — above the ~2× tipping point on pricing structure alone. Two scoping notes: (a) this is an *against-NVIDIA-at-current-prices* claim, not a physical constant — a merchant price war compresses it; (b) against non-NVIDIA hardware sold nearer conventional fabless margins (2.5–3× COGS), the gap shrinks materially, though the owner's assessment is that the architectural factors (yield, no HBM) keep it above the cutover *(designer's note — untested against a specific competitor part)*. The economics page's 3–5× hardware-COGS figure is scoped the same way (vs. the current HBM-loaded, monopoly-priced market). The project's stated optimization target throughout is **perf/$, not peak perf** *(designer's note)* — the same metric the tipping point is denominated in.
- **Power** should favor the design (no HBM draw), but is not yet nailed down — treat as upside, not a load-bearing input, until the thermal/power work lands.
- **Per-user token rate is a structural, not pricing, advantage** *(designer's summary of the June DeepSeek-V3 analysis — `VSA_CASE_STUDIES:deepseek/llm_decode_speed_estimate_2026-06-11.md`, simplifications acknowledged)*: HBM-based systems can only raise tokens/sec/user by adding bandwidth, and past ~50 tok/s the only way to add bandwidth is to overprovision HBM capacity (twice the chips at half-full stacks = double the bandwidth). Matching the VSA's ~1,000 tok/s target that way is price-crippling. Unlike the markup argument, a merchant price war does not erase this — it is capacity bought for bandwidth. A dedicated competitor-anchored benchmark is queued in `VSA_ASIC:docs/TODO.md` to firm these numbers up per workload class.
- **Weave layers** (formerly "factored layers") could multiply the advantage substantially but require models trained/fine-tuned for the scheme; correctly treated as unproven-at-scale upside, not baseline *(designer's note — concurred; a first adopter should underwrite the program on the hardware factor alone)*.
- **Model secrecy friction is real but routine:** frontier buyers keep top-model architectures closed, but every custom-silicon evaluation handles this the same way — the buyer runs the vendor's cost model on their own workloads internally, under NDA, or on published proxy models. Determinism actually helps here: a cycle-accurate cost model is exact, not a simulation estimate, so a paper evaluation is unusually trustworthy.
- **First-mover patent capture** *(designer's note, 2026-07-10)*: because the open baseline is deliberately bare-bones, the optimization and special-function space above it is unclaimed — the first serious productizer gets first pick of patents on all the low-hanging fruit (datatype variants, dedicated units, physical-design tricks), which is permitted and expected under the licensing intent so long as those patents never block the baseline itself (see §8 and the non-assertion TODO). This is an ARM-shaped moat available to the *adopter* rather than the architecture's author, and evaluation teams are unlikely to notice it unaided — worth stating explicitly in any adopter-facing material.
- **Geopolitical forcing function** *(scenario consideration)*: an open, mature-node, no-HBM baseline is buildable by manufacturers cut off from leading-edge GPUs — if Western incumbents pass, embargo-constrained ecosystems have both the motive and the fabs to adopt it first, which converts "we can wait" into "waiting cedes the architecture." Whether an evaluation team weighs this is unknowable; it is the kind of argument that moves strategy offices rather than business units.
- The best de-risking levers the open release can offer are whatever shrinks the first proof: the golden-model determinism story, FPGA-scale demonstrators, and the conformance suite — each shortens the distance from "interesting repo" to "board approval."

## 10. Summary table

| Question | Answer |
|---|---|
| Can a top-20 company productize it? | Yes — standard-practice program, no missing capability |
| Time to revenue silicon | ~24–36 months *(industry-norm estimate)* |
| Program cost | ~$150–400M *(industry-norm estimate)* — mid-tier ASIC program |
| Widest bands | Fleet-scale binning/compiler automation; the 25-die interposer (coupled to the LLM market); power/thermal validation |
| Structural advantages | Determinism → cheap DV/validation; uniform tiling → amortized PD; no HBM/CoWoS → boring supply chain; defect tolerance → forgiving ramp + derated-bin revenue |
| Best-fit first adopters | Hyperscaler in-house silicon (owns deployment, no first-customer problem); video-generation-scale inference buyers (cost-desperate, throughput-bound, statically schedulable workloads) |
| Tipping point | Benchmarked ≥ ~2× sustained cost-per-inference on the buyer's actual top models; <1.5× → rational actors wait |

## Sources

Web-verified 2026-07-10: [Deloitte TMT Predictions 2026 (compute/inference share)](https://www.deloitte.com/us/en/insights/industry/technology/technology-media-and-telecom-predictions/2026/compute-power-ai.html) · [Grand View — Edge AI market](https://www.grandviewresearch.com/industry-analysis/edge-ai-market-report) · [Fortune Business Insights — AI inference market](https://www.fortunebusinessinsights.com/ai-inference-market-113705) · [Introl — video-generation infrastructure (attention share)](https://introl.com/blog/video-generation-ai-infrastructure-sora-scale-models-guide-2025) · [Leonis Capital — Sora cost/compute](https://www.leoniscap.com/research/sora-and-the-future-of-ai-video-generation) · [Sora cost analysis (per-clip GPU economics)](https://aedelon777.substack.com/p/i-did-the-math-on-sora-ai-video-is) · [Tom's Hardware forum + Cerebras on GPU die harvesting/SM disable](https://www.cerebras.ai/blog/100x-defect-tolerance-how-cerebras-solved-the-yield-problem) (H100: 144 SMs physical, 132 enabled).
Repo: `VSA_ASIC:docs/baseline/chip_io_ip_integration.md`, `VSA_ASIC:docs/baseline/manufacturing_yield_estimate_2026-06-10.md`, `VSA_ASIC:docs/baseline/sram_density_speed_reference.md`, `VSA_ASIC:docs/baseline/timing_bottleneck_analysis.md`, `VSA_WIKI:wiki/analyses/economics.md`, VSA_ASIC:CLAUDE.md licensing-direction note.
Conversation: owner rulings and opinions from the 2026-07-10 session, labeled *(designer's note)* where used.
