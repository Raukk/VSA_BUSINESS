# Competitor Power-Efficiency Denominators

CONCEPTS: BENCHMARKING|ENERGY_EFFICIENCY|ACCELERATOR_HARDWARE|COMPETITIVE_ANALYSIS|TECHNICAL_MARKETING

**Status:** internal reference, compiled 2026-08-18. Purpose: give the power multiplier a sourced set rather than one cherry-picked part, so the claim survives whichever competitor ships next. All figures retrieved 2026-08-18. Vendor-published specifications, not measurements.

## The table

| Part | FP8 dense per chip | Power basis | FP8 TFLOPS/W | Source quality |
|---|---|---|---|---|
| NVIDIA B200 SXM | 4.5 PFLOPS | 1000 W TGP | **4.5** | Strong — dense/sparse split published |
| NVIDIA B300 (GB300 NVL72) | 5 PFLOPS | 1400 W TDP | **3.6** | Strong — derived from NVIDIA's own rack table |
| NVIDIA Rubin | 17.5 PFLOPS | **not published** | 5.5–6.6 (rack-derived) | Weak — see note |
| AMD MI355X | 5.0 PFLOPS | 1400 W TDP | **3.6** | Medium — confirm against AMD's own brief |
| Google TPU v7 Ironwood | 4.614 PFLOPS | ~1 kW/chip | **~4.6** | Medium — power is pod-derived |
| **VSA, fabric all-in, 7 nm sim** | — | — | **27** | `VSA_SIM:docs/rollup_estimates.md` |
| **VSA, fabric all-in, 2 nm projection** | — | — | **75** | same |

## Notes per row
- **B200** — "4.5 / 9 petaFLOPS" without/with sparsity, 1000 W Total Graphics Power ([Lenovo ThinkSystem HGX B200 product guide](https://lenovopress.lenovo.com/lp2226-thinksystem-nvidia-b200-180gb-1000w-gpu)). The cleanest denominator available: both halves published and unambiguous.
- **B300** — NVIDIA's GB300 NVL72 table gives FP8/FP6 720 PFLOPS across 72 GPUs, with "all Tensor Core specifications are with sparsity unless otherwise noted" ([nvidia.com/data-center/gb300-nvl72](https://www.nvidia.com/en-us/data-center/gb300-nvl72/)) ⇒ 10 PFLOPS sparse, **5 PFLOPS dense** per GPU at 1400 W. This resolves an ambiguity worth recording: third-party sources circulate "7 PFLOPS dense FP8" for B300, which NVIDIA's own figures do not support. Note also that Blackwell Ultra spent its added power budget on FP4 and memory, so it is **less** FP8-efficient per watt than B200 — the newer part is the weaker denominator.
- **Rubin** — 17.5 PFLOPS dense FP8 per GPU is published ([NVIDIA Rubin platform blog](https://developer.nvidia.com/blog/inside-the-nvidia-rubin-platform-six-new-chips-one-ai-supercomputer/)); **no per-GPU TDP appears in any NVIDIA source found.** The 5.5–6.6 range divides published FP8 by third-party VR NVL72 rack estimates (~190–230 kW ÷ 72), which include CPUs, DPUs, switches and cooling. That denominator is scope-inflated against us and is not sourceable as a GPU figure. Do not quote it; track for when NVIDIA publishes a TDP.
- **MI355X** — 5.0 PFLOPS dense FP8/INT8 at 1400 W TDP, currently from specification aggregators. AMD's own MI355X platform brief should be read before this row ships anywhere.
- **Ironwood** — 4,614 TFLOPS FP8 per chip (Google). Per-chip power is inferred from Google's own "9,216 chips, nearly 10 MW" statement ⇒ ~1 kW/chip, which includes pod-level cooling and network overhead. Directionally sound, not a chip TDP.

## What this means for the claim
- Against **every part with a published per-chip power figure** — B200 4.5, B300 3.6, MI355X 3.6, Ironwood ~4.6 — the VSA fabric at 7 nm (27 TFLOPS/W) clears **5×**, and the 2 nm projection clears **16×**.
- Against **Rubin**, only the 2 nm row clears 5×, and only on a denominator we should not be quoting anyway.
- The strongest defensible framing: quote against **B200**, name it, and state the basis. It is the most efficient FP8-per-watt part in the set with fully published numbers, so choosing it rounds against our own interest — the newer Blackwell Ultra and MI355X would both flatter us more.
- Scope caveat that travels with every row: ours is a MAC fabric, theirs are whole chips including HBM and interconnect. Per the COMPARISON RULE in `CLAUDE.md`, state the scope; do not withhold the comparison.

## Refresh triggers
Re-check when NVIDIA publishes a Rubin per-GPU TDP; when MI400 or TPU v8 specifications appear; before any outward-facing use after 2026-10-01.
