# Social Copy — Ready to Post

CONCEPTS: SOCIAL_MEDIA|MARKETING_COPY|PRODUCT_LAUNCH|COMMUNITY_MANAGEMENT

**Status:** Draft. Replace `[REPO_URL]` everywhere before posting. All posts go out from real, named project accounts (see playbook §7). Citations in square brackets are internal grounding — strip them from the posted text but keep the scoping words (they are part of the claim).

---

## Hacker News — Show HN

**Where:** news.ycombinator.com, owner's account. Post the repo URL directly.

**Title:**

> Show HN: VSA – an open-source AI inference ASIC design (specs, golden model, bit-exact demo)

*(Alt title if too long: "Show HN: An open-source, deterministic AI inference chip design".)*

**First comment (post immediately after submitting):**

> Author here. This is the open release of a chip architecture I first published in 2019 as TensorAsic and have been developing since: the Virtual Systolic Array, a deterministic, weight-stationary MAC fabric for NN inference.
>
> The core bet: trade peak MACs-per-cycle for massive on-chip weight residency, so whole networks chain layer-to-layer on chip — no HBM, no external-RAM round-trips.
>
> What's in the release today: the architecture specs, a golden reference model, a toy compiler, and an end-to-end demo where a compiled 4-layer CNN runs bit-exact against the spec. No RTL and no silicon yet — that's stated on page one, and the repo includes a "proof ladder" doc laying out exactly what gets proven next and how cheaply (reproducible benchmarks, OpenROAD/ASAP7 open-flow PD results, then an FPGA demonstrator).
>
> The property doing the heavy lifting is determinism: no data-dependent timing, control, or routing — every cycle is scheduled at configuration time. That means the golden model isn't an approximation of the hardware's behavior; it IS the hardware's behavior, exactly. Verification collapses to equivalence checking, and every number we publish is something you can re-run rather than take on faith.
>
> Licensing is the RISC-V playbook applied to an accelerator: CERN-OHL-W v2 for the design, Apache-2.0 for the code, and a certification mark for compatibility — anyone can build and sell it royalty-free, forks are legal, but the base design can't be enclosed. Plus a personal non-assertion covenant on my own IP in it.
>
> Happy to answer anything — architecture, the licensing choices, why inference-only, why no HBM, what I'd expect a real productization to cost (that analysis is in the repo too, with its uncertainty bands).

[Grounding: `PROSPECT_QA.md`; `release_plan_2026-09-06.md` §3, §5; `VSA_ASIC:docs/baseline/core_design.md` §4; `proof_ladder_2026-07-10.md`; `license_decision_memo.md`.]

---

## X/Twitter — Launch thread

**Where:** owner/project account. Link in tweet 1 and last tweet only.

> **1/** Today I'm open-sourcing a complete AI inference chip design: the VSA (Virtual Systolic Array). Specs, golden model, compiler, and a bit-exact end-to-end demo. Free to build, forever. [REPO_URL]
>
> **2/** The bet: instead of chasing peak MACs-per-cycle, keep weights resident on chip and chain whole networks layer-to-layer — no HBM, no external-RAM round-trips. In 2026, "no HBM" is a supply-chain feature, not a bug.
>
> **3/** The superpower is determinism. No data-dependent timing, control, or routing — every cycle scheduled at configuration time. So the golden model doesn't approximate the silicon's behavior. It IS the behavior. Exactly.
>
> **4/** Why that matters: verification is where accelerator programs die. Here, DV becomes equivalence checking and silicon validation becomes replay-and-diff. A feasibility study in the repo puts productization at a standard mid-size ASIC program (~$150–400M, 24–36 months, industry-norm estimate) — not a moonshot.
>
> **5/** Defect tolerance is native: a die with a dead Block gets remapped to a pre-compiled, slightly derated configuration and keeps shipping. (GPUs harvest dies too — an H100 ships 132 of 144 SMs — the difference here is granularity and near-zero unsalvageable area.)
>
> **6/** Licensing = the RISC-V playbook for accelerators: CERN-OHL-W v2 (design) + Apache-2.0 (code) + a "VSA" certification mark for compatibility. Build it, sell it, extend it, royalty-free. Forks are legal; enclosing the base design isn't. And I'm pledging never to assert my own IP against any implementation.
>
> **7/** Being straight with you: there's no silicon and no RTL yet. What ships today is bit-exact and re-runnable; the repo's "proof ladder" doc is the public plan for the rest — reproducible benchmarks, open-flow PD on OpenROAD/ASAP7, then an FPGA demonstrator. Each step is something you can check, not believe.
>
> **8/** This started as a public GitHub design in Nov 2019 (TensorAsic). It's been in the open ever since, and it's yours now. Read it, break it, build it. [REPO_URL]

[Grounding: tweets 2–3 `PROSPECT_QA.md`, `VSA_ASIC:docs/baseline/core_design.md` §4; tweet 4 `productization_feasibility_2026-07-10.md` §2, §4, §6; tweet 5 §4, §7 (H100 figure web-verified 2026-07-10); tweet 6 `license_decision_memo.md`; tweet 7 `release_plan_2026-09-06.md` §5, `proof_ladder_2026-07-10.md`; tweet 8 `PROSPECT_QA.md`.]

---

## Reddit

### r/hardware — architecture angle

**Title:**

> VSA: a complete open-source AI inference ASIC design just released — deterministic execution, no HBM, CERN-OHL-W/Apache-2.0 licensed

**Body:**

> Full disclosure: my project. Released today; it's been in the open since Nov 2019 (first published as TensorAsic).
>
> It's a reference architecture for a weight-stationary inference accelerator where whole networks chain layer-to-layer on chip — no HBM, no external-RAM round-trips. Execution is fully deterministic: no data-dependent timing, control, or routing anywhere; everything is scheduled at configuration time by the compiler.
>
> What that buys: the golden model is bit-exact against the hardware by construction, so verification is equivalence checking, and defective dies get remapped to pre-compiled, slightly derated configurations (finer-grained than SM-level GPU harvesting).
>
> Shipping today: complete specs, golden model, toy compiler, and a 4-layer CNN compiled and run bit-exact end-to-end. Not shipping yet (and stated up front): RTL and silicon — the repo has a "proof ladder" doc with the plan and cost for each next step, including open-flow PD via OpenROAD/ASAP7 and an FPGA demonstrator.
>
> Happy to take architecture questions all day. [REPO_URL]

### r/LocalLLaMA — leveled-with-you angle

**Title:**

> An open-source AI inference chip design released today — honest take on what it means (and doesn't) for local inference

**Body:**

> My project, released today, and I want to be straight about the fit for this sub: this is NOT a design for 4–8-GPU home rigs. It's a rack-scale architecture, and the design docs openly concede the sub-$1M market to HBM hardware — below a certain system size, the weights don't fit and GPUs win by default.
>
> Why post here anyway: (1) it's a fully open chip design — specs, golden model, compiler, all CERN-OHL-W/Apache-2.0, anyone can build it royalty-free, with a RISC-V-style certification mark for compatibility; if open-weights matter to you, open silicon should too. (2) The economics angle is aimed exactly at the thing this sub complains about: inference priced off HBM-loaded, high-margin hardware. (3) Determinism means every performance claim ships as a re-runnable model, not a marketing slide — you can audit the math yourself.
>
> No silicon or RTL yet — the repo's proof-ladder doc lays out what gets proven next and what each step costs. Ask me anything, including the skeptical stuff; the repo's own feasibility doc states the bar an adopter should demand (~2× sustained cost-per-inference on their own models) before anyone spends real money. [REPO_URL]

### r/chipdesign — practitioner angle

**Title:**

> Released: open-source inference accelerator design where the golden model is the behavioral spec — deterministic, weight-stationary, docs-first

**Body:**

> My project. The architectural choice I most want this sub's opinion on: full determinism — no data-dependent timing/control/routing, everything compiler-scheduled at config time. Consequences: golden C model is exact (not approximate) behavior, DV collapses toward equivalence checking, silicon validation is replay-and-diff, and there's no coherence/speculation/dynamic-scheduling surface at all.
>
> One Group is designed and verified once, then arrayed — the hierarchy maps directly onto hierarchical PR and verification flows. Native Block/Group-disable defect tolerance: defect classes map to pre-compiled derated configurations, each validatable bit-for-bit before rollout.
>
> Today's release is specs + golden model + toy compiler with a bit-exact end-to-end CNN demo. RTL is the next phase (stated openly), with open-flow PD (OpenROAD/ASAP7) and an FPGA demonstrator on the public roadmap. License: CERN-OHL-W v2 design / Apache-2.0 code. Tear it apart. [REPO_URL]

[Grounding for all Reddit variants: `PROSPECT_QA.md`; `VSA_ASIC:docs/baseline/core_design.md` §4; `productization_feasibility_2026-07-10.md` §4, §7, §9.1, §9.2; `proof_ladder_2026-07-10.md`; `license_decision_memo.md`; `release_plan_2026-09-06.md` §3, §5.]

---

## LinkedIn — industry/business angle

**Where:** owner's profile.

> Today I released a design I've been building in the open since 2019: the VSA, a reference design for an AI inference accelerator that anyone — any company, any country, any fab — can build royalty-free.
>
> Three things make it different:
>
> 1. **No HBM.** Whole networks stay resident on chip and chain layer-to-layer. In a market where HBM and advanced packaging are the choke points, the design targets mature nodes and standard packaging — an independent-style feasibility analysis in the repo puts productization at a standard mid-sized ASIC program (~$150–400M, 24–36 months; industry-norm estimate), not a frontier bet.
>
> 2. **Deterministic to the bit.** Every cycle is scheduled at compile time. The golden model doesn't estimate the hardware's behavior — it defines it. That collapses the largest cost in any accelerator program (verification) and makes every published performance claim something an evaluation team can re-run, not a slide to trust.
>
> 3. **Licensed so it stays open.** CERN-OHL-W v2 + Apache-2.0 + a certification mark for compatibility — the RISC-V ecosystem playbook applied to accelerators. Proprietary products on top are welcome; enclosing the base design is not. I've also pledged never to assert my own IP against any implementation.
>
> There's no silicon yet, and the repo says so on page one — along with a public "proof ladder" of exactly what gets proven next and what it costs. The release is engineered so that the expensive step for an adopter — the internal evaluation — is as short and cheap as possible.
>
> If you work in silicon, inference infrastructure, or open hardware: read it, challenge it, re-run the numbers. That's what it's for. [REPO_URL]

[Grounding: `productization_feasibility_2026-07-10.md` §2, §4, §6; `VSA_ASIC:docs/baseline/core_design.md` §4; `license_decision_memo.md`; `proof_ladder_2026-07-10.md` §1, §3; `release_plan_2026-09-06.md` §5.]

---

## Posting notes

- Numbers allowed in social copy: only the ones above, with their scoping words intact ("industry-norm estimate", "no silicon yet", "~2× bar"). Anything else → check `fact_sheet.md` first.
- No clock frequencies in social copy at all. If asked: 4 GiHz planning clock (2^32 cycles/s = 4.294967296 GHz), explicitly labeled a planning basis, never a silicon claim [VSA_BUSINESS:CLAUDE.md clock rule].
- No power numbers until the power/thermal note ships [`release_plan_2026-09-06.md` §2].
- No tok/s or $/inference figures in launch social copy — those wait for the Rung 1 reproducible benchmarks so the scripts land with the claim [`proof_ladder_2026-07-10.md` Rung 1].
