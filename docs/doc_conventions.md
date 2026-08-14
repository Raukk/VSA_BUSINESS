# Doc Authoring Conventions

CONCEPTS: LLM_CONTEXT_PRIMING|BPE_TOKENIZATION|CHANGELOG

> **Upstream copy — do not edit here.** The authoritative version is `VSA_ASIC:docs/doc_conventions.md`. Changes are made there and re-copied to every sub-repo in the same change; a divergent copy is worse than no copy, because it reads as authoritative.


Two conventions for docs in this repo.

Scope: these apply to normal docs. `CLAUDE.md` files are governed separately by `claude_md_conventions.md`, and **none of its rules apply here** — they are specific to `CLAUDE.md` and do not generalize.

## 1. CONCEPTS header line

Purpose: one dense line that primes an LLM's trained knowledge before it reads the doc. Keywords are triggers, not definitions.

Format:
- Exactly one line, placed directly below the doc's first heading (blank line between heading and CONCEPTS line, blank line after).
- `CONCEPTS: KEYWORD_ONE|KEYWORD_TWO|KEYWORD_THREE` - UPPER_SNAKE_CASE, pipe-delimited, no spaces around pipes.
- 3-10 keywords. Every keyword must be load-bearing for understanding THIS doc.

Include ONLY general concepts a trained LLM already knows, e.g.: KV_CACHE, MOE, ATTENTION, HBM, SRAM_MACRO, EUV_LITHOGRAPHY, CHIPLET, INTERPOSER, ROOFLINE_MODEL, DEPTHWISE_CONVOLUTION, YIELD_MODEL, CLOCK_DOMAIN_CROSSING.

Exclude:
- Project-specific terms (VSA, WRU, EFAU, PSUM_SHIFT, OCTA, Q8_TRUNK, ...). They trigger no trained knowledge; they need definitions, which the doc body and VSA_Core_Terminology.md own.
- Concepts already on the root CLAUDE.md CONCEPTS line (always loaded) - unless the doc is specifically ABOUT that concept.
- Anything the doc merely mentions in passing.

Vocabulary discipline: before inventing a keyword, reuse the spelling another doc already uses (KV_CACHE, not KEY_VALUE_CACHE; MOE, not MIXTURE_OF_EXPERTS). Grep `CONCEPTS:` across the repo when unsure.

## 2. Changelog sidecar (CHANGELOG_SIDECAR)

Dated decision history, supersession notes, and justifications live in a sidecar file named `<docname>.changelog.md` beside `<docname>.md` - never inline in the main doc. The main doc states only the current rule/spec, undated. Create a sidecar only when a doc actually needs history/justification; most don't.

IGNORE RULE: agents must ignore `*.changelog.md` files by default - do not load, skim, or sync them during normal work. Read one only when there is a strong specific reason: provenance of a decision is questioned, the reasoning behind a rule is in dispute, or history must be reconstructed.

Version counters: a doc's revision number (v0.3, v1.2, ...) lives ONLY in its sidecar entries - or nowhere at all. Main docs never carry a revision counter in their status line or body; a Status line describes maturity in words ("brainstorm", "early design", "authoritative register", "finalized"). Rationale: nothing in a main doc should require sync-only edits that don't change content. (Release labels naming a finalized spec artifact - e.g. a v1.0 in a spec's title or filename - are the artifact's name, not a revision counter, and are unaffected.)
