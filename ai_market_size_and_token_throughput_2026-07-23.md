# AI Market Size & Industry Token Throughput (context notes)

CONCEPTS: TOTAL_ADDRESSABLE_MARKET|CAGR|CAPEX|GENERATIVE_AI|HYPERSCALER|PREFILL_DECODE

**Compiled:** 2026-07-23
**Purpose:** Rough sizing context for the VSA ASIC effort — how big the AI market is
today, where it's forecast to go, and how many tokens the industry is processing per
hour. These are external-market reference numbers, not VSA design parameters.

---

## 1. Current AI market / spending (as of 2026)

Estimates vary a lot depending on scope (vendor revenue vs. total organizational
spending vs. infrastructure). Rough current-year figures:

| Metric | Figure (2026) | Source |
|---|---|---|
| **Worldwide AI spending** (broadest — all org spending) | **~$2.5 trillion** (Gartner: $2.52T, +44% YoY) | [Gartner](https://www.gartner.com/en/newsroom/press-releases/2026-1-15-gartner-says-worldwide-ai-spending-will-total-2-point-5-trillion-dollars-in-2026) |
| AI infrastructure segment | ~$401B (~54% of total spend) | [Gartner](https://www.gartner.com/en/newsroom/press-releases/2026-1-15-gartner-says-worldwide-ai-spending-will-total-2-point-5-trillion-dollars-in-2026) |
| **AI market size** (vendor revenue: hardware+software+services) | **~$540–640 billion** | [Grand View Research](https://www.grandviewresearch.com/industry-analysis/artificial-intelligence-ai-market) ($539.5B), [MarketsandMarkets](https://www.marketsandmarkets.com/Market-Reports/artificial-intelligence-market-74851580.html) ($601.93B) |
| **Generative AI** sub-market | ~$140–161 billion | [New Market Pitch](https://newmarketpitch.com/blogs/news/generative-ai-market-size) (~$140B), companieshistory/others (~$161B) |
| Enterprise AI spending | ~$407B | [Value Add VC](https://valueaddvc.com/blog/enterprise-ai-spending-by-industry-whos-deploying-the-most-in-2026) |

**Rough takeaway:** "The AI market" is on the order of **a few hundred billion dollars**
in vendor revenue, or **~$2.5 trillion** if you count all AI-related spending
economy-wide. Which number is "the market" depends entirely on how you draw the box.

### Frontier-lab revenue anchors (mid-2026)
- **Anthropic:** ~$47B annualized revenue (May 2026); overtook OpenAI in annualized
  revenue around April 2026. [source](https://www.getpanto.ai/blog/anthropic-ai-statistics)
- **OpenAI:** ~$2B/month → ~$20–24B annualized run rate. [source](https://aibusinessweekly.net/p/openai-statistics)
- Enterprise LLM-API spend share: Anthropic ~40%, OpenAI ~27% (down from ~50% in 2023).

---

## 2. Future market projections (the "many-trillion" outlook)

Widely-cited forecasts — spread reflects different methodologies and scope:

| Horizon | Projection | Source |
|---|---|---|
| **2030** | **~$1.8T** (37.3% CAGR) up to **~$6.4T** | Statista/others (~$1.8T); [Peak State Consulting](https://www.businesswire.com/news/home/20260624054244/en/Peak-State-Consulting-Forecasts-AI-Market-Will-Reach-$6.40-Trillion-by-2030) (~$6.4T) |
| **2033** | **~$3.5T** (Grand View, 31.5% CAGR) / ~$3.64T (MarketsandMarkets, 29.3% CAGR) / **~$4.8T** (UNCTAD) | [Grand View](https://www.prnewswire.com/news-releases/ai-market-poised-to-hit-3-5-trillion-by-2033--powered-by-31-5-annual-growth--grand-view-research-302621678.html), [MarketsandMarkets](https://www.marketsandmarkets.com/PressReleases/artificial-intelligence.asp), [UNCTAD](https://unctad.org/news/ai-market-projected-hit-48-trillion-2033-emerging-dominant-frontier-technology) |

UNCTAD frames it as a **25× increase in a decade** ($189B in 2023 → $4.8T by 2033).

**Rough takeaway:** Multiple independent forecasts put the AI market in the
**$3.5–6.4 trillion** range by 2030–2033.

---

## 2b. AI datacenter spending, next ~5 years (through 2030)

This is the infrastructure build-out specifically (land, buildings, power, servers,
GPUs/accelerators), distinct from the total-AI-spending figure in §1. Estimates diverge
mainly on scope — "data center capex" vs. "all AI-related capex" — and on how bullish
the source is.

| Estimate | Figure (through ~2030) | Scope | Source |
|---|---|---|---|
| **Dell'Oro Group** | **~$1.7T by 2030** | Worldwide data center capex | [Dell'Oro](https://www.delloro.com/news/ai-boom-drives-data-center-capex-to-1-7-trillion-by-2030/) |
| **NVIDIA (mgmt view)** | **~$3–4T by 2030** | Global data center capex | via search results |
| **JPMorgan** | **~$5.5T through 2030** | Global AI-related capex | via search results |
| Hyperscalers | ~$5.3T by 2030 | AI + data centers | via search results |
| **McKinsey** | **~$6.7T by 2030** (of which ~$5.2T AI-specific, ~$1.5T traditional IT) | Global data center build-out (~70% AI-driven) | [McKinsey](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-7-trillion-dollar-data-center-build-out-how-industrials-can-capture-their-share) |

**Near-term hyperscaler capex trajectory** (Amazon, Google, Meta, Microsoft + peers):
- **2026:** ~$725B  ·  **2027:** ~$880B  ·  **2028:** ~$1.06T
- Top-4 US hyperscalers entered 2026 with combined data center capex approaching ~$600B.
- Morgan Stanley: ~$2.9T cumulative data center build cost 2025–2028 globally (~$1.7T in
  the US alone); ~$3T of AI infra investment flowing through the economy by 2028 with
  >80% still ahead. [Morgan Stanley](https://www.morganstanley.com/insights/articles/ai-market-trends-institute-2026)

**Rough takeaway:** Narrow "data center capex" lands around **$1.7T by 2030** (Dell'Oro);
broader "all AI infrastructure capex" runs **~$3–7T through 2030** depending on scope,
with McKinsey's ~$6.7T (~$5.2T AI-specific) the high anchor. Near-term hyperscaler spend
alone is roughly **$0.7T→$1T/year across 2026–2028**.

---

## 3. Industry-wide LLM token throughput (mid-2026)

> The section below is a separate analysis (provided, not web-sourced here) estimating
> how many tokens the LLM industry processes per hour. Kept alongside the market figures
> because throughput is the physical demand the VSA is ultimately sized against.

### Bottom line
**Best estimate: roughly 8 trillion tokens per hour** processed across mid-range and
frontier LLMs industry-wide, as of mid-2026. That's about **2.2 billion tokens per
second**.

**Honest range:** 5–15 trillion/hour (≈1.4–4.2 billion/sec). If you only count
generated (output) tokens rather than everything the models read, divide by ~4–5: on
the order of **1.5–2.5 trillion/hour**, or **~500 million tokens/second**.

### How the three independent methods landed
Three separate approaches were run so they'd act as a cross-check on each other rather
than one guess dressed up three ways:

| Method | Low | Central | High |
|---|---|---|---|
| Revenue ÷ price (bottom-up) | ~3 T/hr | ~6–9 T/hr | ~15 T/hr |
| GPU fleet × throughput (top-down) | ~10 T/hr | ~45 T/hr | ~150 T/hr |
| Disclosed figures (anchors) | — | ~5–9 T/hr | — |

The revenue method and the public-disclosure anchors agree tightly at **~6–9
trillion/hour**. The compute-capacity method runs higher, but its own author flagged
that its mid and high scenarios almost certainly overestimate — they assume
DeepSeek-grade batching and utilization across the entire large-model fleet, whereas
real deployments hold latency headroom and idle capacity. Its low scenario (~10 T/hr)
is the credible part, and it overlaps the top of the other two methods. So the estimate
is weighted toward where two of three converge, treating the compute ceiling as an
upper bound, not the center.

### The disclosed anchors (the most solid ground)
Companies' own stated numbers, converted to per-hour:
- **Google** — 3.2 quadrillion tokens/month across all surfaces (I/O, May 2026) =
  **~4.4 trillion/hour** all by itself
- **OpenAI API** — 15 billion tokens/minute (March 2026) = **~0.9 trillion/hour** (API
  only; ChatGPT consumer traffic is on top)
- **OpenRouter** — ~100 trillion tokens/month = **~0.14 trillion/hour** (the whole
  open-weight aggregator long tail)

Google alone is roughly half the central estimate for the entire rest of the industry
combined — a sign of how concentrated this is.

### The one big judgment call: does Google Search count?
The single largest swing factor is whether you count Google's AI Overviews and
Workspace tokens. Those run on Gemini (a frontier model, so they fit the "mid-range and
higher" criterion), but they're invisible background generation, not someone
deliberately prompting a chatbot. Google's own disclosure is even described by analysts
as partly "window dressing" because of this.
- Include them → lean toward the **~9–15 T/hr** upper half.
- Count only deliberate chat + API usage (closer to the spirit of
  "Sonnet/Opus/ChatGPT/Qwen/DeepSeek") → **~5–6 T/hr**.
- The headline **~8 T/hr** sits deliberately between the two.

### Caveats worth knowing
- **Geography:** worldwide figure. A USA-only number would be materially smaller —
  Chinese-origin models (DeepSeek, Qwen, MiniMax, Xiaomi) crossed ~60% of developer
  traffic on OpenRouter by mid-2026, so a lot of the volume is Asia-based.
- **Biggest uncertainties, in order:** (1) real per-GPU utilization, which spans a 5×
  range; (2) the Google-Search scope question above; (3) double-counting between
  Anthropic's revenue and the same Claude calls resold through AWS Bedrock and Google
  Vertex (resolved by not adding Bedrock separately); (4) a stale DeepSeek revenue
  figure with no 2026 update.
- **"Processed" vs "generated":** input tokens dominate (~75–80% of the total) and are
  cheap to process in parallel; output tokens are the smaller, expensive slice. The
  headline counts both.

---

## Sources
- [Gartner — Worldwide AI Spending Will Total $2.5 Trillion in 2026](https://www.gartner.com/en/newsroom/press-releases/2026-1-15-gartner-says-worldwide-ai-spending-will-total-2-point-5-trillion-dollars-in-2026)
- [Grand View Research — AI Market Size & Share Report, 2026-2033](https://www.grandviewresearch.com/industry-analysis/artificial-intelligence-ai-market)
- [MarketsandMarkets — AI Market Report 2026-2033](https://www.marketsandmarkets.com/Market-Reports/artificial-intelligence-market-74851580.html)
- [MarketsandMarkets — AI Market worth $3,638.08 billion by 2033](https://www.marketsandmarkets.com/PressReleases/artificial-intelligence.asp)
- [UNCTAD — AI market projected to hit $4.8 trillion by 2033](https://unctad.org/news/ai-market-projected-hit-48-trillion-2033-emerging-dominant-frontier-technology)
- [Grand View / PR Newswire — AI Market Poised to Hit $3.5 Trillion by 2033](https://www.prnewswire.com/news-releases/ai-market-poised-to-hit-3-5-trillion-by-2033--powered-by-31-5-annual-growth--grand-view-research-302621678.html)
- [Peak State Consulting — AI Market Will Reach $6.40 Trillion by 2030](https://www.businesswire.com/news/home/20260624054244/en/Peak-State-Consulting-Forecasts-AI-Market-Will-Reach-$6.40-Trillion-by-2030)
- [Value Add VC — Enterprise AI Spending by Industry 2026](https://valueaddvc.com/blog/enterprise-ai-spending-by-industry-whos-deploying-the-most-in-2026)
- [New Market Pitch — Generative AI Market Size 2026](https://newmarketpitch.com/blogs/news/generative-ai-market-size)
- [Anthropic AI Statistics 2026 (getpanto)](https://www.getpanto.ai/blog/anthropic-ai-statistics)
- [OpenAI Statistics 2026 (aibusinessweekly)](https://aibusinessweekly.net/p/openai-statistics)
- [Dell'Oro Group — Data Center Capex to $1.7 Trillion by 2030](https://www.delloro.com/news/ai-boom-drives-data-center-capex-to-1-7-trillion-by-2030/)
- [McKinsey — The $7 trillion data center build-out](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-7-trillion-dollar-data-center-build-out-how-industrials-can-capture-their-share)
- [Morgan Stanley — AI Market Trends 2026](https://www.morganstanley.com/insights/articles/ai-market-trends-institute-2026)

*Market figures are third-party analyst estimates and vary widely by methodology; treat
as order-of-magnitude. Token-throughput section is a provided cross-method estimate,
not independently web-verified here.*
