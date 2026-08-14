# The Proof Ladder — Cheapest Artifacts That Unlock Adoption

CONCEPTS: PLACE_AND_ROUTE|MULTI_PROJECT_WAFER|PREDICTIVE_PDK|GOLDEN_REFERENCE_MODEL|DESIGN_VERIFICATION|RTL

**Status:** Analysis (2026-07-10). Companion to `productization_feasibility_2026-07-10.md`, which concluded that the real adoption barrier is not the ~$150–400M program cost but the **first-mover proof burden**: someone must be first to commit silicon to an architecture unproven in silicon. This document maps how cheaply that burden can be shrunk, rung by rung, and what the open release should therefore ship with.
*(Migrated from `VSA_ASIC:docs/proof_ladder_2026-07-10.md` 2026-08-14.)*
**Figure provenance:** *(industry-norm estimate)* = order-of-magnitude from general industry knowledge, ±2–3×; *(designer's note)* = owner-stated intent.

---

## 1. Framing: two different "yes" decisions

The ladder targets two distinct decisions inside an adopter, with very different bars:

- **Decision A — fund an internal evaluation.** A few architects and PD engineers for a few months, ~$1–5M of internal cost *(industry-norm estimate)*. This is cheap for a top-20 company; the bar is "credible enough that spending engineer-months isn't embarrassing."
- **Decision B — fund the program.** The ~$150–400M commitment. No external artifact fully substitutes for the adopter's own evaluation here; the ladder's job is to make Decision A easy and to make the evaluation that follows it *fast and cheap*, because that evaluation is what produces Decision B.

Corollary: the project should optimize its artifacts for **shortening the adopter's own evaluation**, not for being maximally impressive. An evaluation team's week-1 activities are predictable — synthesize a Group with their libraries, run their models through the cost model, check the verification story. Every one of those the repo pre-answers converts evaluation weeks into days.

**The determinism dividend, stated once:** because execution is bit-exact and data-independent, the gap between rungs is smaller than for any conventional accelerator. A cycle-accurate model is not an approximation of the silicon — it *is* the silicon's behavior, exactly. Each rung below therefore only needs to prove what the previous rung structurally cannot: the cost model can't prove area/timing/power; synthesis can't prove the system works end-to-end; FPGA can't prove frequency; only silicon proves silicon. Nothing needs to re-prove *behavior* twice.

## 2. The rungs

### Rung 0 — Specs + docs (the repo today, completed)

- **Cost:** already planned. **Who:** the project.
- **Buys:** architectural credibility; the determinism/verifiability story; enough for an architect to form an opinion.
- **Doesn't buy:** any independently checkable number. Every silicon team has seen paper architectures with great stories; Rung 0 alone moves no one with a budget.

### Rung 1 — Golden model + cycle-accurate cost model, with published reproducible benchmarks

- **Cost:** engineering time only (~free in dollars; the largest time item on the ladder after the compiler). **Who:** the project.
- **What:** the golden C model (behavioral truth) plus a cycle/energy cost model, run on *named open models* — a ResNet/YOLO-class CNN, a Llama-class LLM, an SDXL/FLUX-class image model, the existing DeepSeek-V3 system analysis — publishing $/inference and tokens/sec against published GPU/TPU numbers, with the scripts in the repo so the numbers are **reproducible by the reader**. (This is the competitor-anchored benchmark queued in `VSA_ASIC:docs/TODO.md`, promoted to a release deliverable.)
- **Buys:** converts every headline claim ("3–5×", "~1,000 tok/s/user") from marketing into checkable arithmetic. Because of determinism, an adopter re-running these is auditing facts, not sampling a simulator. This is the single highest-leverage artifact per unit effort on the ladder.
- **Doesn't buy:** area, timing, or power — the three numbers silicon people distrust most in paper designs. A cost model's cycle counts are exact, but its mm² and watts are still estimates.

### Rung 2 — Independent physical-design results on an open flow

- **Cost:** engineering time + compute (~$10K-scale). **Who:** the project (or a university partner).
- **What:** synthesis + place-and-route of one Group (and ideally the mesh tile) through an open flow — OpenROAD with the ASAP7 predictive 7nm PDK is the standard choice — publishing area, achieved clock, and estimated power, with the flow scripts in the repo. Not production numbers, but *independent* numbers from a flow anyone can re-run, on a node class near the target.
- **Buys:** kills the "the area/clock/SRAM estimates are made up" objection to first order, which is the reflexive first objection to Rung 1's $/inference numbers (they all divide by area-dependent cost). Also directly feeds the queued power/thermal TODO item.
- **Doesn't buy:** commercial-node truth (open PDKs are predictive; real N4 libraries are under NDA — which is precisely what the adopter's week-1 evaluation does with their own licenses, now with a reference point to diff against).

### Rung 3 — FPGA demonstrator (a 4–8 Group slice, rented)

- **Cost:** ~$100–500 of cloud FPGA rental for the one-time validation *(external agent estimate provided by the owner, 2026-07-10)* — the dominant real cost is writing the synthesizable RTL, not hardware. Sizing per that estimate: one VSA Block is <1% of an AMD VU47P (would fit a $150 hobby board); one full Group ≈ 12% of its DSPs; the practical ceiling is a **4–8 Group slice**, which is the demo worth doing. AWS EC2 F2 (f2.6xlarge, one VU47P-class part with HBM) rents at ~$1.98/hr on-demand / ~$0.66/hr spot with the required Vivado Enterprise license bundled into the FPGA Developer AMI; synthesis runs on a ~$0.75/hr CPU instance. **Who:** the project — at this price, unconditionally.
- **What:** 4–8 Groups with real inter-Group mesh, on-chip layer chaining through the epilogues, GALS clock crossings, dual-mode Blocks, and config loading — running a small CNN end-to-end **bit-exact against the golden model**, with the defect-disable demo (disable a cell, remap, re-run, identical answers). The recorded deterministic, repeatable outputs are the confirmation artifact. FPGA clocks (~200–400 MHz) prove *function*, not frequency — determinism makes correctness clock-independent, so report cycle-accurate results and say so wherever the demo is shown.
- **Buys:** kills "vaporware" completely; it computes, on hardware, today. Crucially, this is the artifact that convinces **non-silicon decision-makers** — hyperscaler infrastructure leadership, procurement, boards — who cannot read an OpenROAD report but understand a live demo with a defect-tolerance party trick. It also becomes the compiler team's development target, so it partially pays for itself.
- **Doesn't buy:** frequency, power, density — anything physical.

### Rung 4 — MPW shuttle test chip (a few Groups, mature node)

- **Cost:** ~$1–5M including packaging and test *(industry-norm estimate; multi-project-wafer shuttles on N16/N12-class are the affordable end; N4-class shuttles exist at higher cost)*. **Who:** **not the solo project** — this rung is where a first sponsor enters: a university program, a government efficiency-procurement program (already a named deployment class in the project's targets), or an "angel adopter" running it as the cheap front end of their own evaluation.
- **What:** a few Groups + mesh + minimal I/O (CSR/SPI path; DDR optional), characterized for achieved clock, power, and — with intentionally relaxed screening — defect-tolerance behavior on real defects.
- **Buys:** silicon-proven timing/power/yield at *some* node — the artifact risk-averse boards fund programs on. It converts "evaluation says it works" into "silicon says it works."
- **Honest caveat:** an AMD-class adopter would rather run their own test chip on their own node with their own libraries — for them, a project-funded Rung 4 mostly proves diligence, not physics. Rung 4's real audience is **smaller fabless players and hyperscaler teams** deciding whether to start, and its real function is insurance if Rungs 1–3 fail to attract anyone.

### Rung 5 — Full program (~$150–400M)

The adopter's rung, per the feasibility assessment. Not the project's problem, by design.

## 3. Which rung unlocks Decision A?

**Rungs 1 + 2 together are the plausible trigger** for a top-20 company's funded evaluation. Reasoning: the evaluation team's job is exactly "check the cost model on our workloads, check PD on our libraries" — Rungs 1+2 hand them a running start on both, collapsing a speculative multi-quarter study into a bounded few-week diff-against-reference exercise. The cheaper the evaluation, the lower the bar to approving it; that is the entire mechanism of the ladder.

Rung 3 is not strictly required for Decision A at a silicon-first company — but it is disproportionately valuable for **hyperscalers**, where the decision-makers who control custom-silicon agendas are infrastructure/economics people, and for defusing the vaporware reflex in every audience. Given its modest cost and its double life as the compiler testbed, it earns its place in the release.

Rung 4 is **not** required for Decision A anywhere, and for the best-fit adopters it is partially redundant with their own process. It should be treated as opportunistic (do it if a sponsor appears) rather than gating.

## 4. What "completion" should therefore ship

The feasibility assessment assumed a completion artifact of specs + RTL + verification suite + basic compiler at between-toy-and-production maturity. The ladder adds three specific, cheap, high-leverage items to that definition:

1. **Published reproducible benchmarks** (Rung 1) — the cost model isn't complete until it has *named models, named competitor parts, and scripts in the repo*. This is the difference between "has a cost model" and "makes checkable claims."
2. **Open-flow PD results for one Group** (Rung 2) — flow scripts included, ASAP7 or equivalent, with the area/clock/power table in the docs.
3. **The FPGA demonstrator** (Rung 3) — bit-exact against the golden model, with the defect-disable demo scripted.

With those, the open release doesn't ask an adopter to believe anything — it asks them to *re-run* things. That is the cheapest possible form of the first-mover proof, and everything beyond it (Rung 4+) rightly belongs to whoever captures the value.

## 5. Summary table

| Rung | Artifact | Cost | Who | Kills which objection | Decision unlocked |
|---|---|---|---|---|---|
| 0 | Specs + docs | done/planned | project | "no architecture" | none alone |
| 1 | Exact cost model + reproducible benchmarks | eng. time | project | "the numbers are marketing" | A (with 2) |
| 2 | Open-flow PD of a Group (ASAP7/OpenROAD) | ~$10K + time | project | "area/clock/power are made up" | A (with 1) |
| 3 | FPGA 4–8-Group demo (rented, AWS F2), bit-exact + defect-disable | ~$100–500 rental + RTL effort | project | "vaporware"; convinces non-silicon audiences | strengthens A, esp. hyperscalers |
| 4 | MPW test chip, few Groups, mature node | ~$1–5M | sponsor, not project | "never proven in silicon" | B for risk-averse/smaller adopters |
| 5 | Full program | ~$150–400M | adopter | — | the product |

*(All costs: industry-norm estimates, ±2–3×.)*

## Sources

`productization_feasibility_2026-07-10.md` (first-mover barrier, tipping point, completion-artifact assumption); `VSA_ASIC:docs/TODO.md` (benchmark, power/thermal items this ladder promotes/feeds); the VSA_ASIC CLAUDE.md licensing-direction note (now `license_decision_memo.md`); owner rulings from the 2026-07-10 session as marked *(designer's note)*. Rung costs and evaluation-team dynamics are industry-norm estimates as labeled, not verified quotes.
