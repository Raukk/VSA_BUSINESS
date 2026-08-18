# Power Efficiency — Internal Summary

CONCEPTS: ENERGY_EFFICIENCY|MEMORY_HIERARCHY|DATA_LOCALITY|SYSTOLIC_ARRAY|DRAM_BANDWIDTH|POWER_ESTIMATION

**Status:** internal draft, 2026-08-18. Compute figures are simulated (VSA_SIM, 2026-08-16), not silicon. Whole-chip power/thermal remains an open upstream item (`VSA_ASIC:docs/TODO.md` item 1).
**Standing disclaimer (owner, 2026-08-16, `VSA_SIM:docs/rollup_estimates.md`):** node-scaled targets may be up to 30% optimistic on logic density and clock speed. Do not re-litigate per number.

## 1. The argument in one line
Almost all inference energy is spent moving bytes, not multiplying them. The VSA keeps the bytes still.

## 2. Headline number
**Over 5× the FP8 energy efficiency of a current-generation NVIDIA GPU — MAC fabric only, not whole chip.**

| Basis | FP8 TFLOPS/W | Ratio |
|---|---|---|
| VSA MAC cores alone, 7 nm sim | ~35 | 7.8× |
| VSA fabric all-in — store read + wires + clock, gated | ~27 | 6.0× |
| Same, scaled to 2 nm | ~75 | 17× |
| NVIDIA B200, whole card | 4.5 | 1× |

VSA rows derived from `VSA_SIM:docs/rollup_estimates.md`: 57.3 fJ/MAC-cycle (P2b shipping FP8, window-corrected) and ~74 fJ/Unit-cycle all-in, at 2 FLOPs/MAC; 2 nm row from 16,384 MACs at 1.87 W and the 4 GiHz planning clock = 140.7 TFLOPS. B200 row: 4.5 petaFLOPS FP8 dense at 1000 W TGP, [Lenovo ThinkSystem HGX B200 product guide](https://lenovopress.lenovo.com/lp2226-thinksystem-nvidia-b200-180gb-1000w-gpu), retrieved 2026-08-18. INT8 runs better on both our sides — 39.15 fJ cores, ~56 fJ all-in, i.e. ~51 / ~36 TOPS/W at 7 nm — but INT8 against an FP8 GPU number is not a like-for-like comparison, so FP8 is the one to quote.

**The asterisks, which travel with the number wherever it goes.** The VSA figure is a MAC fabric; the B200 figure is a whole card including HBM, memory controllers, SerDes and host interface. Excluded on our side: activation SRAM, global weight distribution, drain, SerDes, DRAM. The gap will narrow once those land. Conservative in our favor: our 7 nm sim is compared against 4NP silicon. Ship ">5×" and nothing larger until the whole-chip pass closes — the 6.0×, 7.8× and 17× rows are internal. One open dependency: the FP8 variant's psum format (44 b Kulisch) still needs upstream spec acknowledgment (`VSA_SIM:docs/rollup_estimates.md` §Open decisions 2).

**Denominator risk — resolve before launch.** B200 is not the current-generation part. Blackwell Ultra B300 is in production (288 GB HBM3e, 1400 W TDP), and Vera Rubin was launched at CES in January 2026, entered full production on 2026-06-01, and begins partner shipments this fall — so it is a named, in-production part by our release date, not a rumor. NVIDIA has published Rubin's **performance** but not its **power**: 17.5 PFLOPS dense FP8 per GPU ([NVIDIA Vera Rubin platform blog](https://developer.nvidia.com/blog/inside-the-nvidia-rubin-platform-six-new-chips-one-ai-supercomputer/), retrieved 2026-08-18), with no per-GPU TDP disclosed in any NVIDIA source found. Only third-party rack estimates exist (VR NVL72 at ~190–230 kW), and those include CPUs, DPUs, switches and cooling, so they are not a GPU TDP.

**Consequence.** Dividing Rubin's published FP8 by rack power gives ~5.5–6.6 TFLOPS/W — better than B200's 4.5 — but rack power includes CPUs, DPUs, switches and cooling, so that denominator is scope-inflated against us and is not sourceable as a GPU figure. Do not quote it. B300's rating, ambiguous when this doc was first drafted, is now resolved from NVIDIA's own GB300 NVL72 table: **5 PFLOPS dense FP8 per GPU at 1400 W = 3.6 TFLOPS/W**, weaker than B200, because Blackwell Ultra spent its added power on FP4 and memory. Full set with sources: `competitor_power_efficiency_table_2026-08-18.md`.

**Posture: use the multiplier; name the part and the basis in the same sentence.** "Over 5× a B200 per watt, VSA MAC fabric, simulated at 7 nm." Against every competitor with a published per-chip power figure — B200 4.5, B300 3.6, MI355X 3.6, Ironwood ~4.6 — ">5×" holds on the 7 nm row. Per the COMPARISON RULE in `CLAUDE.md`, comparing our simulation or projection to shipping silicon is legitimate and needs labeling, not hedging. The one real defect to fix is scope: our fabric against their whole chip. State it, and revisit when the whole-chip pass lands.

**Node-matching makes it larger, not smaller.** Every competitor above ships on 4NP or N3P, one to two nodes ahead of our 7 nm simulated basis, so the rows in §2 understate us by roughly 1.4–2×. Matched node for node, the margin is **8–15× per FLOP** (`competitor_power_efficiency_table_2026-08-18.md` §Node-matched). Those rows rest on extrapolation from the 7 nm sim and carry the 30%-optimism disclaimer; say "node-matched" or "node-mismatched" whenever a ratio is quoted.

## 3. Operating point — read before any number below
Every figure here is the **maximum-performance** point: 4 GiHz planning clock, ≥nominal Vdd. Nothing here is an efficiency-optimized design. Dynamic energy scales with V², so modest throttling buys disproportionate savings, and upstream's own anchor is **~2× J/op** for a wide-slow 0.6 V design against nominal (`VSA_ASIC:docs/baseline/vsa_block_floorplan_weight_buffer_analysis.md` §7). A low-power mode is **not a deliverable of this reference design** beyond throttling — the point is that an implementer targeting efficiency has room to work with, and every approach in that direction improves on these numbers rather than degrading them. The efficiency below is bought with locality, not with a slow clock.

## 4. Mechanism, ranked by savings
1. **No HBM.** Weights and activations live on-chip; external memory is commodity DDR, used sparingly. An off-chip 64b access costs 250–450 pJ (HBM2) or 1300 pJ (DDR I/O only); a 10 µm on-chip hop costs ~1 fJ/bit (`VSA_ASIC:docs/baseline/energy_cost_reference_5nm.md` §1, §2.2). Energy favors SRAM ~10× per byte over HBM across the contested band (`VSA_ASIC:docs/baseline/hbm_sram_crossover_analysis_2026-06-11.md` §6).
2. **Weights sit beside the multiplier.** Permanent design decision (`VSA_ASIC:docs/baseline/core_design.md` §6). Each Unit owns a 64-word weight store read one word per cycle; measured in-context read cost **~5.0–5.3 fJ/Unit-cycle** against a 39.15 fJ INT8 MAC core (`VSA_SIM:primitives/chain_segment/results.md` §P3b). Weight fetch is single-digit percent of compute, not a multiple of it.
3. **Weights stay loaded across many epochs.** One epoch = 64 cycles; inputs stream, weights do not. Quantified on the multicast path: a tile reused 8 epochs amortizes distribution to **~2.7 fJ per mm of bus per MAC-cycle**; reloading every epoch costs **~21 fJ/mm**, rivaling compute (`VSA_SIM:docs/rollup_estimates.md` §Multicast). Residency is load-bearing, not incidental. Scope note: that bound covers the Mega-SRAM/Hyper shared-weight case, where a tile rotates every few epochs — in the standard case weights load once per configuration and the cost amortizes to nothing. Even in the reload case the distance is short: broadcast traces run ~250 µm within a Block, ~500 µm across a Group (`VSA_ASIC:docs/baseline/vsa_block_floorplan_weight_buffer_analysis.md` §4), so the wire term is ~0.7–1.4 fJ/MAC-cycle, and the SRAM read itself amortizes to <0.8 fJ/MAC-cycle even at 100 fJ/word. Distribution is a low-single-digit percentage of a 39.15 fJ MAC, not a threat to the budget — the wire dominates the macro, and both stay small. How long weights actually stay resident is configuration- and variant-specific — minutes to hours of uptime in many serving cases, not a universal property.
4. **Traffic goes to a neighbor, not across the chip.** On-chip layer chaining cuts inference DRAM traffic **4.3×** on a 4-layer CNN case (291 → 67 MiB) and **up to ~97×** with spatial reduction; required DRAM bandwidth falls to **0.19–4 GB/s**, with **262,144× reuse per weight byte** at 1024×1024 single-image (`VSA_ASIC:docs/baseline/core_design.md` §22; `VSA_ASIC:docs/baseline/workload_data_reuse_analysis.md`).
5. **SRAM is distributed, not pooled.** The Standard variant carries ~40 MiB on-chip per chip; the Mega-SRAM variant — the one most LLM cases want — carries **512 MiB per chip** at 256 KiB/Block (`VSA_ASIC:docs/baseline/chip_design_toggles.md`; `VSA_ASIC:docs/baseline/chip_cost_sram_estimate_2026-06-11.md` §Mega-SRAM as-built). Upstream rejected the one-big-macro alternative on power at Standard capacity: a shared 4 KB macro wins area but pays ~1–2 pJ per wide read plus ~5–10 pJ/cycle of bus charging, versus ~3 pJ/cycle for 64 local reads over ~10 µm wires (`VSA_ASIC:docs/baseline/vsa_block_floorplan_weight_buffer_analysis.md` §1.4).

## 5. Simulated numbers (7 nm sim basis, INT8, psum-shift, gated)
| Quantity | Value |
|---|---|
| MAC core | 39.15 fJ/MAC-cycle |
| All-in per Unit-cycle (store read + wires + clock) | ~56 fJ |
| Wire share of chain total | 45.52 fJ = 23.0% |
| Clock share of chain total | 34.33 fJ = 17.3% |
| Clock gating saving | 8.75 fJ/cycle = 4.2% |
| 16,384 MACs all-in @ 4 GiHz | 3.94 W (7 nm) → 2.76 (5) → 1.97 (3) → 1.42 (2 nm) |

Input-shift mode runs ~+16%. FP8 adds ~+18 fJ/Unit-cycle over INT8 (~74 fJ all-in; 5.21 W per 16,384 MACs at 7 nm, 1.87 W at 2 nm). Source: `VSA_SIM:docs/rollup_estimates.md`, `VSA_SIM:primitives/chain_segment/results.md` §P3b.

## 6. What the design still pays for
- **Leakage, entirely uncosted.** No figure here includes it. Upstream's reference puts leakage at 10–20% of total for dense gated MAC arrays and 20–40% for SRAM-heavy designs, ×1.3–1.45 per +10 °C (`VSA_ASIC:docs/baseline/energy_cost_reference_5nm.md` §7) — but that basis is **5 nm-class FinFET, not 2 nm**, and the headline's best row is a 2 nm projection. A GAA-node leakage basis is needed before any 2 nm number carries a leakage term. The 512 MiB Mega variant is where this bites hardest, and it interacts with the baseload framing below: leakage is paid whether or not the array is computing. SRAM is in the next simulation batch.
- **SerDes and off-chip links.** Not in any figure above; large models exceed one chip/interposer/wafer and pay inter-chip transport.
- **Built for baseload, not burst.** The efficiency case assumes sustained high utilization; idle SRAM loses to idle HBM at any rate (`VSA_ASIC:docs/baseline/hbm_sram_crossover_analysis_2026-06-11.md` §6). The right analogy is a baseload plant against a peaker: the VSA is meant to run flat out, continuously. A hybrid deployment follows naturally — VSA carries the sustained floor, GPUs absorb the bursts.

## 7. Framings to correct before this reaches copy
- **"Registers next to the MAC"** — the store is a 64-word latch/SCM array per Unit (variant C, 8,032 T), not flip-flops. Upstream measured a 64-FF ring as the worst of three options, ~4–7× the power (`VSA_ASIC:docs/baseline/vsa_block_floorplan_weight_buffer_analysis.md` §2). Say "held beside the multiplier."
- **HBM** — the cleanest framing: adding HBM does not improve VSA performance. It is an optional fast cache for certain workloads, not a requirement of the design or of any number quoted here. Sanctioned wording: "runs on commodity DDR; the design eliminates HBM entirely" (`VSA_ASIC:staging/press/fact_sheet.md`).
- **Power per token** — not yet quotable. An LLM at scale spans hundreds of chips and the figure is highly architecture-dependent; once the whole-chip power estimate lands a rough per-token number becomes derivable. Owner expectation is that it will be low; that expectation is not itself a claim.
