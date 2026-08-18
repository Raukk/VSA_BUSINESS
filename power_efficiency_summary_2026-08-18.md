# Power Efficiency — Internal Summary

CONCEPTS: ENERGY_EFFICIENCY|MEMORY_HIERARCHY|DATA_LOCALITY|SYSTOLIC_ARRAY|DRAM_BANDWIDTH|POWER_ESTIMATION

**Status:** internal draft, 2026-08-18. Compute figures are simulated (VSA_SIM, 2026-08-16), not silicon. Whole-chip power/thermal remains an open upstream item (`VSA_ASIC:docs/TODO.md` item 1) — no W/chip or perf/W-vs-GPU claim is available yet.
**Standing disclaimer (owner, 2026-08-16, `VSA_SIM:docs/rollup_estimates.md`):** node-scaled targets may be up to 30% optimistic on logic density and clock speed. Do not re-litigate per number.

## 1. The argument in one line
Almost all inference energy is spent moving bytes, not multiplying them. The VSA keeps the bytes still.

## 2. Mechanism, ranked by savings
1. **No HBM.** Weights and activations live on-chip; external memory is commodity DDR, used sparingly. An off-chip 64b access costs 250–450 pJ (HBM2) or 1300 pJ (DDR I/O only); a 10 µm on-chip hop costs ~1 fJ/bit (`VSA_ASIC:docs/baseline/energy_cost_reference_5nm.md` §1, §2.2). Energy favors SRAM ~10× per byte over HBM across the contested band (`VSA_ASIC:docs/baseline/hbm_sram_crossover_analysis_2026-06-11.md` §6).
2. **Weights sit in the MAC's own store.** Permanent design decision (`VSA_ASIC:docs/baseline/core_design.md` §6). Each Unit owns a 64-word weight store read one word per cycle; measured in-context read cost **~5.0–5.3 fJ/Unit-cycle** against a 39.15 fJ INT8 MAC core (`VSA_SIM:primitives/chain_segment/results.md` §P3b; `VSA_SIM:docs/rollup_estimates.md`). Weight fetch is single-digit percent of compute, not a multiple of it.
3. **Weights stay loaded for many epochs.** One epoch = 64 cycles; inputs stream, weights do not. Quantified on the multicast path: a tile reused 8 epochs amortizes distribution to **~2.7 fJ per mm of bus per MAC-cycle**; reloading every epoch costs **~21 fJ/mm**, rivaling compute (`VSA_SIM:docs/rollup_estimates.md` §Multicast). Residency is load-bearing, not incidental. In service, a resident model holds weights in place for minutes to hours.
4. **Traffic goes to a neighbor, not across the chip.** On-chip layer chaining cuts inference DRAM traffic **4.3×** on a 4-layer CNN case (291 → 67 MiB) and **up to ~97×** with spatial reduction; required DRAM bandwidth falls to **0.19–4 GB/s**, with **262,144× reuse per weight byte** at 1024×1024 single-image (`VSA_ASIC:docs/baseline/core_design.md` §22; `VSA_ASIC:docs/baseline/workload_data_reuse_analysis.md`).
5. **SRAM is distributed, not pooled.** Reference Standard variant carries ~40 MiB on-chip per chip (32 MiB WRU main buffers + ~8 MiB weight + 1 MiB edge); a 16-chip Hexa aggregates ~640 MiB, at which point many models run with zero DRAM round-trips (`VSA_ASIC:docs/baseline/core_design.md` §26). Upstream rejected the one-big-macro alternative on power: a shared 4 KB macro wins area but pays ~1–2 pJ per wide read plus ~5–10 pJ/cycle of bus charging, versus ~3 pJ/cycle for 64 local reads over ~10 µm wires (`VSA_ASIC:docs/baseline/vsa_block_floorplan_weight_buffer_analysis.md` §1.4).

## 3. Simulated numbers (7 nm sim basis, INT8, psum-shift, gated)
| Quantity | Value | Source |
|---|---|---|
| MAC core | 39.15 fJ/MAC-cycle | `VSA_SIM:docs/rollup_estimates.md` |
| All-in per Unit-cycle (store read + wires + clock) | ~56 fJ | same, §Power+area |
| Wire share of chain total | 45.52 fJ = 23.0% | `VSA_SIM:primitives/chain_segment/results.md` §P3b |
| Clock share of chain total | 34.33 fJ = 17.3% | same |
| Clock gating saving | 8.75 fJ/cycle = 4.2% | same |
| 16,384 MACs all-in @ 4 GiHz | 3.94 W (7 nm) → 2.76 (5) → 1.97 (3) → 1.42 (2 nm) | `VSA_SIM:docs/rollup_estimates.md` |

Excluded from that all-in figure: activation SRAM, global weight distribution, drain. Input-shift mode runs ~+16%. FP8 adds ~+18 fJ/Unit-cycle over INT8.

## 4. What the design still pays for
- **SerDes and off-chip links.** Not in any figure above; large models exceed one chip/interposer/wafer and pay inter-chip transport.
- **Clock rate vs voltage.** The 4 GiHz planning clock forces ≥nominal Vdd, ~2× J/op against a wide-slow 0.6 V design (`VSA_ASIC:docs/baseline/vsa_block_floorplan_weight_buffer_analysis.md` §7). Efficiency here is bought with locality, not with a slow clock.
- **Timing is close, not settled.** P3b chain as built missed 4 GiHz by ~5%; a later INT8 stress run closes it with ~10% margin (`VSA_SIM:docs/rollup_estimates.md` §Queued CI).
- **Idle loses.** The efficiency case assumes sustained multi-user utilization; idle SRAM loses to idle HBM at any rate (`VSA_ASIC:docs/baseline/hbm_sram_crossover_analysis_2026-06-11.md` §6).

## 5. Framings to correct before this reaches copy
- **"Registers next to the MAC"** — the store is a 64-word latch/SCM array per Unit (variant C, 8,032 T), not flip-flops. Upstream measured a 64-FF ring as the worst of three options, ~4–7× the power of the chosen scheme (`VSA_ASIC:docs/baseline/vsa_block_floorplan_weight_buffer_analysis.md` §2). Say "held beside the multiplier," not "in registers."
- **"No HBM at all"** — the sanctioned wording is "runs on commodity DDR; the design eliminates HBM entirely" (`VSA_ASIC:staging/press/fact_sheet.md`). Mega-SRAM and attention-cache variants still use external memory, at a small fraction of GPU-class provisioning. Keep the qualifier.
- **No power-per-token or perf/W comparison exists.** Any such claim is unsupported until the upstream power/thermal pass lands.
