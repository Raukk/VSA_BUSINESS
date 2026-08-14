# CLAUDE.md Authoring Conventions

CONCEPTS: LLM_CONTEXT_PRIMING|PROMPT_ENGINEERING|SIGNAL_TO_NOISE|TOKEN_BUDGET

> **Upstream copy — do not edit here.** The authoritative version is `VSA_ASIC:docs/claude_md_conventions.md`. Changes are made there and re-copied to every sub-repo in the same change; a divergent copy is worse than no copy, because it reads as authoritative.


Companion to `doc_conventions.md`, which governs normal docs. This file governs `CLAUDE.md` files only. It is deliberately not itself a CLAUDE.md — read it when writing or editing one, not on every run.

**Scope, strictly.** Every rule below applies to `CLAUDE.md` and to nothing else. The LLM-only reader, the ban on prose, the inclusion test, the size flags: none of it generalizes to normal docs, and none of it may be cited as a reason to write a doc for a machine audience. A doc that wants these rules must say so in its own status line.

## The reader is never human

A `CLAUDE.md` is loaded by every agent on every run and is read by nothing else. README is where humans go. That single fact drives everything below: optimize signal per token, not readability. Prose that teaches, motivates, or reassures is pure cost — the model already knows the general concepts and does not need to be persuaded.

## The inclusion test

A line earns its place if **either**:

1. **≥90% of runs need it.** Not "is true," not "is interesting" — *needed*, by most sessions, to avoid doing the wrong thing.
2. **It is an inverse signal.** "Ignore `*.changelog.md` by default", "skip `docs/` unless authoring conventions are in question", "`brainstorm/` may be stale" — knowing what *not* to read saves more tokens than it costs, every run.

Everything else goes in a doc the CLAUDE.md points at. When adding a line, ask: would nine sessions in ten be measurably worse without it? If not, it belongs behind a pointer.

## Owner approval

New concepts and new instructions need an owner ruling **before** they land. Specifically:

- A new CONCEPTS keyword or TERMS entry.
- A new standing rule, GATE line, or owner shorthand.
- Any new instruction to the reader — a line telling an agent what to do, as opposed to where to look.
- A new folder pointer that carries a directive rather than a dig-or-skip description.

Not requiring approval: pruning, rewording, fixing a stale path, moving content behind a pointer that already exists, or correcting a factual error.

The file is loaded on every run by every agent, so a rule added without a ruling silently steers all future work. Propose it in session and get the ruling; a directive that arrives without one is removed at the next audit rather than inherited.

## Use trigger tokens

A concept keyword surfaces trained knowledge that an explanation cannot match at any length. `WEIGHT_STATIONARY|SYSTOLIC_ARRAY|INT8_QUANTIZATION` costs a dozen tokens and recalls the entire subject; a paragraph explaining weight-stationary dataflow costs hundreds and recalls less. The software analogue is `DRY|SOLID|ENTERPRISE_APP|USES_AZURE`.

So: **name the concept, do not teach it.** Reserve prose for what the model *cannot* know — project-specific facts, owner rulings, local traps.

Corollary — state facts, omit justification. "Memory binds, not compute" is the load-bearing fact. *Why* it binds belongs in the rationale doc. An agent that needs the reasoning can follow the pointer; the other 90% just need the constraint.

**A trigger token only works if the standard meaning is the one you want.** The mechanism recalls what training holds, so it pays off exactly to the degree the project agrees with training:

- `WEIGHT_STATIONARY` — lands cleanly, the project means what the literature means.
- `SYSTOLIC_ARRAY` — lands at roughly 90%, and is still worth including: you cannot understand the VSA without first holding a standard systolic array in mind, even though the VSA departs from it. Partial recall that you then correct is a good trade.
- `BLOCK` — fails twice. Too generic to recall anything useful, *and* the name of a specific architectural component here.

So a term the project redefines must **never** appear as a trigger token or on a CONCEPTS line, where it silently invokes the wrong subject with no correction anywhere. It belongs in an explicit TERMS line stating the local meaning. Trigger tokens and redefined terms are opposite tools: one borrows training, the other overrides it.

## Style

- One idea per line. Lists over paragraphs.
- Terse but grammatical. Dropping articles and using `->`, `|`, `!=` is fine; degrading into caveman speak is not — it costs comprehension for a few tokens. Compress a section further only where it genuinely reads clean.
- No bold lead-ins, no motivational framing, no "importantly" or "note that".
- Do not restate a number that lives in a doc. Cite the path — a duplicated number drifts.

## Folder pointers

One line per folder: **enough to decide dig-or-skip, and nothing more.** The folder's own content is its own job.

Good: `wafer-mapping/ - HBM detail store, meta-layer floorplan`
Bad: a three-line summary of what the meta-layer section concluded.

Do not pre-write sub-folder `CLAUDE.md` or `README.md` content into the root file. If a subtree needs its own rules, it gets its own gated file; the root just says the subtree exists.

## GATE lines

Bare minimum. A GATE earns its place only for a **misread trap** or an **owner shorthand** — something an agent gets wrong by default and cannot discover in time.

`"mask" in an MoE/attention-projection context may mean gate — ask` is a GATE.
`Read the README before working here` usually is not: if the README is genuinely required, its operative content belongs in the CLAUDE.md, and if it isn't, the line is noise.

## Fill levels

No budget. Size is a gauge that fills as a project earns content, and the thresholds below are **flags, not limits** — a flag means *audit this file line by line against the inclusion test*, not *this file is wrong*. Plenty of files legitimately sit past one.

| Project stage | Flag above | Meaning |
|---|---|---|
| **Empty / scaffold** — structure and rules exist, little or no content | **4 KiB** | Rules are being written for work nobody has done yet. Usually speculative rules, or README prose that migrated in. |
| **Early stage** — content landing, subsystems forming, vocabulary still settling | **8 KiB** | Often the real signal: sessions adding hard-won lessons faster than anyone prunes. |
| **Mature** — full doc tree, settled vocabulary, many standing rules | **16 KiB** | At this size, check that subtrees are carrying their own gates instead of the root carrying them. |

Check with `wc -c`. Current calibration points (snapshot — re-measure rather than trusting these): `VSA_ASIC` (mature) 4.17 KiB, `VSA_GoldenModel` 3.88 KiB, `VSA_WYVERN` 3.47 KiB, `VSA_BENCH` 2.21 KiB (the three sub-repos at scaffold stage). Nothing in the project currently trips a flag.

Read the mature figure as an *achieved* number, not where a mature project naturally rests: `VSA_ASIC/CLAUDE.md` has been pruned extensively and would run roughly double without that ongoing effort. The flags above are calibrated for files that get pruned. One that has never been pruned will hit them early and deserves the audit.

Subtree gates fill far more slowly — they are scoped to one folder, and the project files above run 0.3–0.9 KiB. A subtree gate approaching its project's flag level is itself the flag: the subtree has probably become a project.

### Known reasons a file legitimately runs large

Check these before cutting. If one applies, the size is earned:

- **Project-unique terms, and terms the project redefines.** The highest-value content a `CLAUDE.md` carries, and the one case where the trigger-token mechanism actively works *against* you: the model surfaces confident, well-formed, wrong knowledge automatically and has no signal that it is wrong. A payroll team whose internal tool is named `USPS` is the canonical shape — every unqualified mention pulls in the postal service. In this project: `chip` = one on-wafer die in the wafer-scale docs, not a chiplet on an interposer; `Weave` = a specific split+scramble factorization; `Hyper` = a named chip SKU; `Wyvern` = a model, not a topology. **Give every collision a full line: state the local meaning and name what it is not.** A term that merely lacks trained meaning can be defined by the docs where it appears, but a term that means something *else* in training is silently misread on every run until the `CLAUDE.md` corrects it — and the misreading is confident, which is what makes it expensive. Aliases for one object, and unit conventions that depart from the standard reading, belong here for the same reason.
- **Wide root folder set.** One line per folder is correct, and N folders cost N lines. Never drop a pointer to hit a number — an agent that reads the wrong tree, or misses one entirely, costs far more than the line saved. If pointers dominate the file, **ask the owner whether the folders can be bucketed** (`analysis/ - cost, yield, timing subfolders`) rather than deleting lines unilaterally. Bucketing is a repo-layout decision, not an editing one.
- **Rule-dense or safety-critical repo.** Every rule that prevents a class of irreversible or integrity-destroying mistake earns its place regardless of the count — answer-key isolation, determinism requirements, "never guess an open constant." A repo can simply have a lot of these.
- **Split-project overhead.** Cross-repo citation rules, what-stays-upstream lists, and sync rules are a fixed per-repo cost in a multi-repo project.
- **Owner shorthands and rulings.** Irreducible and unlearnable. There is no shorter form.
- **Conformance repos.** Where nearly every run needs to know which upstream specs exist, the spec index is load-bearing rather than reference material.

### Exempt content

These do not count toward the fill and are not what a flag is pointing at:

- The CONCEPTS line — highest signal density in the file.
- Folder pointer lines, one per real folder.
- Owner shorthands and misread traps.
- Hard rules preventing irreversible mistakes.
- Cross-repo citation rules in a split project.

### What a flag is actually pointing at

When one trips, these are the usual culprits, in the order they are worth hunting:

1. Justification and rationale — move behind the pointer.
2. Folder *summaries* where a dig-or-skip *pointer* would do.
3. Numbers restated from a doc that owns them.
4. Explanations of general concepts the model already knows.
5. Status, history, and progress notes — a changelog sidecar owns those.
6. Sub-folder `CLAUDE.md` content pre-written into the root.

Growth is the normal failure mode: every session wants to add its hard-won lesson and none want to delete. Prune when you edit.
