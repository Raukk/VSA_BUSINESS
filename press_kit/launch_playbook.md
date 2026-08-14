# Launch Playbook — Making 2026-09-06 Land

CONCEPTS: GO_TO_MARKET|PRODUCT_LAUNCH|COMMUNITY_MANAGEMENT|PUBLIC_RELATIONS|SOCIAL_MEDIA|DEVELOPER_RELATIONS

**Status:** Draft plan. Everything here is above-board: real accounts, real claims, real sources. No astroturfing, no sockpuppets, no staged controversy, no fabricated endorsements — ever. The design's honesty *is* the marketing: every number in the repo is re-runnable, and that's the story we tell.

---

## 1. The hook

Hook rules (from the 2026-08-14 generate-and-review panel — 36 candidates, 3 writer angles, 3 reviewer lenses):
- ONE idea per hook. Phone crop is real: target ≤60 characters, hard cap 80, payload words first.
- Setup + punch beats description: a price or ownership fact everyone already resents, answered by a true VSA fact.
- Owner rulings 2026-08-14, binding on all copy:
  * The 1% rounding rule applies to TECHNICAL numbers (rates, perf, power), not durations or narrative counts. Avoid year-counting anyway — the project sat idle for most of 2019–2025, so "7 years of work" both overclaims and under-impresses. Use "in the open since 2019".
  * No present-tense "complete/full design" (WRU spec open, no RTL [`PROSPECT_QA.md`]).
  * **Defect-tolerance claims: scrapped from ALL launch copy.** "Same answers" reads as either an NN property (not the chip's) or a same-config claim (wrong — remaps are derated [`productization_feasibility_2026-07-10.md` §7]). Can of worms; the story doesn't need it.
  * Price-vs-free contrasts ("GPUs cost $40K, this is free") are a false equivalency — a design is not a chip. Scrapped.
  * Perf multipliers: once the VSA_ASIC citation lands, headlines may carry the bare multiplier — qualifiers live in the linked article/body, never in the headline. The body's FIRST paragraph must scope it: modeled numbers, re-runnable model, workload and system size.

**The perf angle (owner-cleared 2026-08-14; citations verified against VSA_CASE_STUDIES 2026-08-14):**
- **DeepSeek-V3/R1, per-user speed:** VSA design ceiling **4,096 tok/s per user** (8,192 concurrent users, ~33.5M tok/s aggregate) vs NVIDIA's best *published* per-user records — >250 tok/s/user (DGX B200, Mar 2025) and 12.5 tok/s/user p99 (DGX H200, MLPerf v5.1) [`VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`]. **The 10× headline claim holds** (~16× vs the B200 record). Mandatory body scoping, from that doc's own caveat: VSA numbers are 100%-utilization design ceilings at the 4 GiHz planning clock; NVIDIA numbers are measured deliveries — say so in the first paragraph.
- **LFM2.5-class small model, single wafer:** per-user ceiling **262,144 tok/s** (system ~16.8M tok/s) [`VSA_CASE_STUDIES:lfm25/lfm25_on_hyper_evaluation.md`]. The 10,000 tok/s hook is >25× conservative against its own source. Same ceiling-vs-sustained scoping applies (governor-throttled in practice).
- **WAN 2.1 T2V-14B video: ON HOLD.** The figures exist — ~0.4–1.1 clips/s power-limited per Hyper wafer (~3.06 silicon-limited) [`VSA_CASE_STUDIES:t4v-wan21/video_wan14b_hyper_pipeline_throughput_2026-08-04.md` §7.5], and vs the measured Ironwood TPU figure that's **~35–90×** [`VSA_CASE_STUDIES:t4v-wan21/video_wan14b_external_accelerator_benchmarks_2026-08-04.md` §3 note 3] — so say "up to ~90×", not "~100×". BUT `VSA_CASE_STUDIES:suspected_mistakes_ledger.md` has OPEN entries (SM2 score-buffer arithmetic, K/V operand loading) whose stated blast radius includes exactly these cadence figures. No video hook ships until those entries resolve.
- Modeled numbers, said proudly: the model is public, the math is checkable, "we don't have $100M to build it — you might" is the honest close.

Perf hooks:
> 10× faster! Impossible? Check the math.  *(39 chars — LIVE, DeepSeek cite above; scoping in the linked body's first paragraph)*
> On paper: 10× the tokens/sec of a GPU. Fork it today.  *(53 chars — LIVE, same cite)*
> Fork the FOSS AI chip design that beats GPUs — no HBM.  *(54 chars — LIVE, same cite)*
> Tired of waiting on AI? This chip design does 10,000 tokens a second.  *(69 chars — LIVE, LFM2.5 cite above)*
> Don't trust your eyes: an AI chip design that makes thousands of videos an hour.  *(80 chars — ON HOLD with the WAN figures, pending the mistakes-ledger entries)*

**Spicy pool (owner drafts 2026-08-14 — each blocked until its noted fix):**
- "It's NVindependence day! A FOSS AI chip that is 1/10th the cost." — BLOCKED on a written source. Owner rationale on record: the multiple stacks NVIDIA markup (~10× alone), no HBM cost, no design/licensing cost, open-source competition, defect tolerance, and higher per-chip output — plausibly well past 10× per token. But repo rule: cost figures are quoted, never derived — this needs a written `VSA_CASE_STUDIES:` cost-per-output figure before use (nearest existing anchors: 3–5× COGS [`productization_feasibility_2026-07-10.md` §9.2]; $0.25–0.42/MTok mid-2026 market prices [`VSA_CASE_STUDIES:deepseek/agent_fleet_deployment_sketch_2026-07-10.md`]). The NVindependence pun itself is fair parody — keep it.
- "Less electricity, less water, less datacenters — new AI chip could revolutionize the industry." — PARKED until the power/thermal note ships [`release_plan_2026-09-06.md` §2]; power/water claims are currently banned outright.
- "The AI swarms are coming! New chip runs 8,000 prompts at once." — LIVE with corrected figure: the sourced number is **8,192 concurrent prompts at 4,096 tok/s each** (DeepSeek flagship config) [`VSA_CASE_STUDIES:deepseek/dgx_h200_comparison_2026-07-16.md`]; the swarm story (throughput so high the fleet needs agent swarms/self-review to stay fed, discounting auto-generated prompts to avoid idle) is the agent-fleet deployment sketch [`VSA_CASE_STUDIES:deepseek/agent_fleet_deployment_sketch_2026-07-10.md`] — owner framing 2026-08-14. "Hundreds of prompts per second" specifically has no cite; use the concurrency figure.
- Headline wording (owner ruling 2026-08-14, supersedes the earlier hedging note): headlines carry the bold claim bare — no qualifiers, no character budget spent hedging. The FIRST SENTENCE of the linked article states design status plainly and positively (paper design, re-runnable model, no silicon yet); commenters who skip the link have no leg to stand on.

**Primary (until the perf citation lands; then "10× faster! Impossible? Check the math." takes over):**

> Fork a FOSS AI chip design: build it, sell it, pay nobody

(57 chars. Grounding: [`license_decision_memo.md`] — use/modify/manufacture/sell, royalty-free; forks legal.)

**Per-audience one-liners** (each one idea, sourced):

1. **Tech/software crowd:** the fork hook above — "Fork a FOSS AI chip design" is the strongest phrase in the set for this audience; append "that beats GPUs, no HBM" once the perf citation lands.
2. **Business/industry press:** "No vendor lock-in. No license fees. An AI chip design anyone can build." [`license_decision_memo.md`] — alt with the market stat: "Inference is two-thirds of AI compute. This chip design costs $0" [`productization_feasibility_2026-07-10.md` §9.1 (Deloitte)]
3. **Gamers / general public:** "This AI chip design could bring GPU and RAM prices back down. Gamers, rejoice." (79 chars; owner-approved 2026-08-14 — "should" is an accepted stronger variant. Forward-looking claim; AI demand driving GPU/RAM prices is the setup the audience already believes. Body copy explains the mechanism: free design → more inference supply off gaming silicon.)

**Three deeper angles** (the paragraph-length versions for pitches and posts):

1. **The verification angle (for silicon people):** "Determinism kills the DV monster." No coherence, no speculation, no dynamic scheduling — the golden model *is* the behavioral spec, DV becomes equivalence checking [`productization_feasibility_2026-07-10.md` §4]. This is the angle for Hacker News' hardware crowd and semi-industry newsletters.
2. **The supply-chain angle (for industry/business press):** an accelerator with no HBM and no exotic packaging, on mature nodes, in a year when HBM and CoWoS are the choke points and inference is ~two-thirds of AI compute (Deloitte 2026, via `productization_feasibility_2026-07-10.md` §9.1). Buildable by anyone with fab access — a $150–400M standard program, not a moonshot *(industry-norm estimate)* [`productization_feasibility_2026-07-10.md` §6].
3. **The openness angle (for open-source/RISC-V communities):** CERN-OHL-W + Apache-2.0 + certification mark = the RISC-V playbook applied to AI accelerators: fork-legal, enclosure-proof, badge-gated compatibility, owner non-assertion covenant [`license_decision_memo.md`]. Plus a public paper trail back to TensorAsic, Nov 2019 [`PROSPECT_QA.md`].

**The honest-underdog frame, used deliberately:** we say up front there's no silicon and no RTL yet, and then show the proof ladder [`proof_ladder_2026-07-10.md`]. Pre-empting "vaporware" beats defending against it — the ladder converts the objection into the roadmap. Controversy-adjacent-but-honest framings that are fair game because the docs support them: "GPU margins are the subsidy this design runs on" (H100 ~$25–40K street vs ~$3.3K cost-bridge COGS, `VSA_WIKI:wiki/analyses/economics.md` verified 2026-07-07, via feasibility §9.2 — always with both scoping caveats) and "the sub-$1M market is conceded — this is for people buying racks" [`productization_feasibility_2026-07-10.md` §9.1]. Never a superlative the docs don't carry.

## 2. Pre-launch (T-14 → T-1)

- **T-14:** Freeze the fact sheet; resolve all `[PLACEHOLDER]`s. Confirm license counsel review closed [`license_decision_memo.md`] — do not launch the licensing angle on an unconfirmed license.
- **T-14 → T-7:** Private preview, honestly framed. Email 5–10 people who genuinely cover this space (see §4) with the fact sheet and repo preview access, clearly labeled "releasing Sept 6, happy to answer questions before then." This is standard press pre-briefing, not seeding fake grassroots. Offer the owner for interviews.
- **T-7:** Dry-run the demo: fresh machine, README-only, clone → run the bit-exact 4-layer CNN demo [`release_plan_2026-09-06.md` §2]. The first thing HN does is try the demo; if `make demo` doesn't work in one command on a clean Linux box, fix that before anything else. Time-to-first-bit-exact-result is the single most important onboarding metric.
- **T-3:** Prepare the launch blog post / README top section: one-sentence hook, 3 diagrams, the demo GIF, the proof ladder as a public roadmap. Write the Show HN first comment (see `social_copy.md`) and the FAQ answers (§6) so launch-day replies are fast and consistent.
- **T-1:** Verify the stale bench-keys branch deletion is done (owner task, `release_plan_2026-09-06.md` §2) and the repo is genuinely clean. Nothing torpedoes an open-hardware launch like leftover internal artifacts.

## 3. Launch day sequencing (Sunday 2026-09-06 — see timing note)

**Timing (owner decision 2026-08-14):** launch is **Sunday Sept 6**, deliberately. Tip-line sends and submissions go out **Saturday night (Sept 5)** so the weekend hobbyist crowd — who are home, and home on a holiday weekend — builds momentum into a **Monday viral wave**. Sept 7 is US Labor Day: enthusiast channels peak, press desks lag to Tuesday — treat Tuesday Sept 8 morning as the follow-through window for journalist pitches (§4), riding the weekend numbers as the proof of interest.

Order matters — each step feeds the next:

1. **Repo public + launch post live** (morning, owner's account). Everything links back to one canonical URL.
2. **Show HN** (owner's real account; HN rewards makers answering questions). Title + first comment in `social_copy.md`. Owner or a delegate commits to answering questions for the first 4–6 hours — responsiveness is what keeps a Show HN alive. Never solicit upvotes; never post from multiple accounts. Sharing the link with friends/community and letting them engage genuinely is fine; vote-rings are not (and HN detects them).
3. **X/Twitter thread** (owner account) ~30 min after the HN post, linking the repo (not the HN thread — HN penalizes that traffic pattern; people will find the HN discussion themselves).
4. **Reddit**, staggered over the day, each post written natively for its sub (copy in `social_copy.md`):
   - r/hardware — the architecture/verification angle.
   - r/LocalLLaMA — honest framing required: this is rack-scale, the 4–8-GPU segment is explicitly not the target [`productization_feasibility_2026-07-10.md` §9.1]. Pitch it as "the open design your future inference provider might run," and the open-hardware story itself. This sub hates being marketed at and loves being leveled with.
   - r/chipdesign and/or r/FPGA — the determinism/DV story; these are practitioners, lead with the golden-model methodology.
   - r/opensource or r/hardware crosspost for the licensing angle.
   Respect each sub's self-promotion rules; where the sub requires it, flag it as your own project — which is also just true.
5. **Lobsters** — one submission, `hardware` + `release` tags, from a real member account (Lobsters is invite-only and allergic to marketing; if nobody on the project has an account, ask a genuine acquaintance who's a member to consider it on the merits, or skip — do not manufacture an account for this).
6. **Mailing lists / Discords / forums** (same day or T+1, always per each community's norms, always as ourselves): RISC-V community channels (the licensing model directly cites their precedent [`license_decision_memo.md`]), OpenROAD/OpenLane community (the proof ladder names their flow as Rung 2 [`proof_ladder_2026-07-10.md`]), FOSSi Foundation orbit (the open-silicon home turf), EleutherAI / open-ML Discords (open-infrastructure sympathizers), Hackaday tip line (they love exactly this).
7. **LinkedIn post** (owner) — the business/feasibility angle for the semi-industry crowd.

## 4. Who to pitch (categories, not a private list)

Pitch = fact sheet + 2-paragraph personal email + offer of owner interview. No exclusives promised to more than one outlet.

- **Semiconductor analysis newsletters/sites** — the SemiAnalysis / Chips and Cheese / TechTechPotato-class writers who cover accelerator architecture seriously. Best-fit story: the verification-economics and no-HBM angles; they can actually read the specs, which is our advantage.
- **Open-silicon / EDA community press** — FOSSi-orbit blogs, OpenROAD project channels, academic open-hardware newsletters. Story: a serious, complete, openly licensed accelerator design landing in their ecosystem, with their flow named in the roadmap.
- **Tech press (business/infra desks)** — The Register, Ars Technica, IEEE Spectrum, EE Times, Tom's Hardware. Story: open-source challenger design + supply-chain angle + the RISC-V-style licensing. IEEE Spectrum and EE Times in particular reward the "here's the full spec, check it" posture.
- **AI-infrastructure newsletters** — the inference-economics crowd. Story: cost-per-inference framing with the tipping-point honesty (≥~2× or rational buyers wait [`productization_feasibility_2026-07-10.md` §9.2]) — giving journalists the project's *own* skeptical bar is unusual and quotable.
- **YouTubers covering silicon + open source** — the Asianometry-class chip-industry explainers, EDA/FPGA educators, open-hardware channels. Offer: diagrams, the demo, and owner interview. A 20-minute "how this architecture works" video is the best month-1 artifact someone else can make for us.
- **Podcasts** — semiconductor/EDA podcasts and open-source hardware shows for the owner interview circuit in weeks 2–4.
- **Hackaday** — submit via their tip line (hackaday.com/submit-a-tip), **Saturday night Sept 5** per the §3 timing plan. This is their exact beat (open hardware, real engineering, underdog projects); lead with the fork hook and the bit-exact demo, not the business angle. No exclusivity needed — a tip is not a pitch.

## 5. Week-1 / Month-1 beats (keep the story moving)

A launch is a sequence, not a day. Each beat is already-planned work becoming content [`release_plan_2026-09-06.md` §5, `proof_ladder_2026-07-10.md`]:

- **T+2/T+3 (Tue/Wed):** "What people asked" follow-up — a post answering the top 10 questions from HN/Reddit, in public. Converts launch-day skeptics into subscribers.
- **Week 1:** technical deep-dive post #1: the determinism/golden-model story for practitioners (the strongest unique material).
- **Week 2–3:** deep-dive #2: the licensing/certification-mark design and why it's the RISC-V playbook [`license_decision_memo.md`]. Pitch this one to open-source-policy audiences.
- **Month 1 (as they land, never before):** proof-ladder rung announcements — reproducible benchmarks (Rung 1), open-flow PD results (Rung 2), FPGA demonstrator progress (announced at launch as "next" [`release_plan_2026-09-06.md` §5]). **The FPGA bit-exact demo is the single best future press moment we have** — live hardware matching the golden model bit-for-bit [`proof_ladder_2026-07-10.md` Rung 3]. Save a full push for it. (Defect-disable stays in the demo script but out of the marketing claims per the §1 ruling.)
- **Ongoing:** first-external-contributor and first-conformance-question moments; a public "open problems" list (from `VSA_ASIC:docs/baseline/design_open_items.md` equivalents in the release) — nothing recruits engineers like well-specified unsolved problems.
- **Opportunistic:** any credible third-party writeup or re-run of the numbers gets amplified (with permission), even — especially — if it's critical. "They checked our math" is the brand.

## 6. Objection handling (FAQ for launch-day replies)

Answer fast, honest, and with links. Never bluff a number.

- **"Vaporware — no silicon, no RTL."** Correct, and it's on page one. What exists today is bit-exact and re-runnable: specs + golden model + compiled CNN demo. The proof ladder is the public plan for the rest, with FPGA next [`release_plan_2026-09-06.md` §5, `proof_ladder_2026-07-10.md`]. Because execution is deterministic, the golden model is the silicon's exact behavior, not a simulation estimate [`proof_ladder_2026-07-10.md` §1].
- **"Paper architectures are worthless."** Own it, don't duck it: yes, these are paper numbers — the project doesn't have $100M to fab a chip, and says so. What it has is a deterministic model anyone can re-run, which makes these the *cheapest paper numbers in the industry to falsify* [`proof_ladder_2026-07-10.md` Rung 0–1]. Anyone who can't engage with a modeled number wasn't going to help anyway; the audience is the people who open the model.
- **"What clock does it run at?"** All reference figures are on a stated planning basis (the 4 GiHz planning clock, 2^32 cycles/s = 4.294967296 GHz); an adopter freely re-pipelines and picks their own operating point — frequency is an implementation knob with linear throughput payoff, not an architectural requirement [`productization_feasibility_2026-07-10.md` §5.1]. Never state or imply a silicon frequency claim.
- **"No HBM means no bandwidth — LLMs won't fit."** Partly conceded, honestly: below some system size the weights don't fit and HBM hardware wins by default; the sub-$1M SOTA-LLM segment is explicitly not the target — this is a racks-not-cards design [`productization_feasibility_2026-07-10.md` §9.1]. The counterpoint is the structural per-user token-rate argument [`productization_feasibility_2026-07-10.md` §9.2, designer's analysis, benchmark queued] — offer it labeled as such.
- **"Attention/transformers don't map to a CNN fabric."** CNNs are the baseline; attention/MoE support is an actively developed variant, and the docs are open about which parts are queued vs. done [`PROSPECT_QA.md`; `productization_feasibility_2026-07-10.md` §9.1 on diffusion transformers].
- **"Who'd spend $300M on an unproven design?"** Our own docs say nobody should until the numbers clear ~2× on their workloads — and the entire release is engineered to make *checking* that cheap [`productization_feasibility_2026-07-10.md` §9.2, `proof_ladder_2026-07-10.md` §3]. Quoting our own tipping-point analysis at skeptics is the move; it shows the project holds itself to the skeptic's bar.
- **"CERN-OHL-W reciprocity scares corporate counsel."** Known trade-off, documented, with the fallback (CERN-OHL-P) and the reasoning public [`license_decision_memo.md`]. Products on top stay proprietary; only base-design modifications must be shared.
- **"Why should the license/mark stuff work?"** RISC-V and OpenPOWER precedent, cited in the memo [`license_decision_memo.md`].

## 7. Rules of engagement (non-negotiable)

- One real account per person per platform. No sockpuppets, no vote solicitation, no staged debates, no fabricated or paraphrased-as-quoted endorsements.
- Every number posted anywhere carries its scope ("planning clock", "industry-norm estimate", "vs. current market pricing") — the repo's accuracy rules apply to tweets too.
- Nothing from `VSA_ASIC:staging/` or internal ledgers ever gets posted [`release_plan_2026-09-06.md` §3].
- Power/thermal claims: none, until the power/thermal note ships [`release_plan_2026-09-06.md` §2]; "expected upside, not yet claimed" is the only allowed phrasing [`productization_feasibility_2026-07-10.md` §9.2].
- Dragon product names (`dragon_naming_scheme.md`) are for models/products built *on* VSA later — not part of this launch's naming; don't leak them into launch copy.
