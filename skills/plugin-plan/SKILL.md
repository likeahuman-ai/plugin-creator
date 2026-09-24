---
name: plugin-plan
description: >
  Turn intent and a plugin's accumulated todo items into an approved, committed sprint plan —
  decisions landing in the target's Rules register, wants landing as pressure scenarios, proof
  obligations named per tier. Phase 1 of the plugin-authoring loop. Use when the user wants to
  plan plugin work: "plan the next sprint for orientation", "let's update the development
  plugin", "add a skill to font-hunt", "triage plugin-creator's todo" — or when plugin changes
  are being discussed and no draft sprint exists.
argument-hint: "target plugin (optional — inferred from the conversation, asked when ambiguous)"
---

# /plugin-plan — Plan (Phase 1): drain → discover → author → accept

Phase 1 of the loop `/plugin-plan → /plugin-build → /plugin-review`: turn intent and the
target plugin's todo drop-box into an approved, committed `.sprint` plan, with decisions
captured as living register deltas and wants captured as pressure scenarios. Drive the
sub-phases in order — 1.0 Setup → 1.1 Discovery → 1.2 Author → 1.3 Close. Two human gates,
both on **content acceptance**.

Boundaries that hold for the whole phase:

- **Decision artifacts are session-authored.** No writers are dispatched here — the plan, the
  register deltas, and the scenarios are the gated conversation's output; the only dispatch is
  the optional read-only explorer batch (1.1.3).
- **Nothing touches disk before acceptance.** Both gates precede every file write — 1.2
  composes and presents; 1.3.2 writes, only after the accept gate. The tempting shortcut —
  "write the sprint file now, amend it after the gate" — fails the design: a gate that follows
  a write is theatre.
- **The register is living.** A superseding decision rewrites its `D-###` entry in place with
  the supersession noted in *Since:*; IDs are never renumbered; dead law is deleted — git is
  the archive.
- **Self-application is unspecial.** The target may be this plugin itself — same steps, same
  gates, no special-casing.
- **One commit, no push.** Plain git: the single `docs(plan)` commit (1.3.3) follows
  acceptance autonomously — never gate it; publication belongs to Build's close.

The formats this phase fills — `sprint-format`, `pillar-format`, `scenario-format`,
`register-delta-format` — live at
`${CLAUDE_PLUGIN_ROOT}/skills/plugin-plan/formats/<name>-format.md`.

**Initial request:** $ARGUMENTS

## 1.0 Setup

**Goal:** resolve the target, read the law, triage the drop-boxes.

### 1.0.1 Resolve the target plugin

From the initial request, else the conversation; if ambiguous → ask — one question, with a
recommended answer. The target may be plugin-creator itself.

### 1.0.2 Read the homes

`.standard` **in full** (the law this sprint plans under) · `.steps` — the slice: this phase's
contract rows plus the coupling rows for surfaces the intent touches · `.scenarios` — the
ledger head plus the entries the intent neighbours (selective — it can be large) · the latest
`.sprint` file's status. Each skip-if-absent — a missing home marks **first contact**: the
missing pieces are composed at 1.2.4 and written at 1.3.2, per their formats.

### 1.0.3 Enforce the sprint lifecycle

From `.sprint/sprint-v{N}.md` frontmatter statuses: hold the **one-draft rule** — a live draft
means this run amends that draft (new scope grows it; a second draft needs the user's explicit
override) · set the next `v{N}` · mark everything older for cascade → `archived`. Status flips
are annotations — they ride the 1.3.3 commit, never their own.

### 1.0.4 Triage todo.md

Every item gets a verdict: **enters the sprint** · **becomes a scenario** · **struck with a
reason**. Nothing is dropped silently. Verdicts are composed now, applied at 1.3.2.

### 1.0.5 Drain the findings backlog

`.sprint/findings.md` — the note-bucket findings `/plugin-review` deposited (skip-if-absent).
Same drain semantics as 1.0.4: every note gets a verdict — **enters the sprint** · **becomes a
scenario** · **struck with a reason** — composed now, applied at 1.3.2. The notes lane ends
here by design: what review could not publish, planning must answer.

## 1.1 Discovery

**Goal:** map the touched prose surface and confirm intent.

### 1.1.1 Discuss the intent

Intent-driven — one question at a time, each with a recommended answer; read the target's
source instead of asking what it can answer.

### 1.1.2 Map text coordinates

Locate every file:line the change touches across the target's skills, agents, prompts, and
formats. The `.steps` coupling map scopes the sweep; the prose carries the detail — *verify on
contact*: a coupling row that no longer matches the text is drift, recorded as sprint input,
never silently fixed.

### 1.1.3 Dispatch explorers (conditional)

if the surface exceeds what the session can hold → dispatch read-only explorers in ONE
parallel batch — general subagents; inject: the intent, the coupling-map slice, the file list
to divide (*direction from the session, discretion to the agent*). Barrier — collect the
coordinate maps; the session consolidates. if the surface is small → skip.

### 1.1.4 Gate — confirm discovery

Present the touched-surface map and the todo and findings verdicts; the user confirms this
captures the intent. Content only — nothing has been written, and nothing will be until 1.3.2.

## 1.2 Author

**Goal:** decide the approach, compose every artifact — in conversation, nothing on disk.

### 1.2.1 Discuss the approach

Settle the approach before slicing into workstreams (*design before decomposition*). A
decision that is hard to reverse, surprising without context, or a real trade-off → 1.2.2.

### 1.2.2 Compose register deltas

One `D-###` delta per captured decision, shaped per `register-delta-format` (the Y-statement
and its fields live there): new law takes the next ID; superseding law **rewrites its entry in
place**, supersession noted in *Since:*; dead law is deleted. Present each delta verbatim —
law lands in the target's `.standard` Rules register, never a side log, never an append-only
file.

### 1.2.3 Compose scenarios, RED-first

Every new want lands WITH its failing pressure scenario, per `scenario-format`: narrative line
+ Given/When/Then with a named *observe:* per bullet + grade + tier. A want whose check cannot
be stated yet is still captured — tier `unproven`. The scenario is written to fail against
today's prose — if it would already pass, it proves nothing.

### 1.2.4 Compose the pillar instantiation (first contact only)

Per `pillar-format`: instantiate the target's pillar table for its `.standard` — omit pillars
the plugin lacks, add the file kinds it really ships. if the target already carries a pillar
table → skip.

### 1.2.5 Compose the sprint plan

Per `sprint-format`: single pass/fail Goal · Non-goals · Standard impact (the 1.2.2 deltas, by
ID) · 1–5 workstreams (typically 3–5; a sixth is scope overflow — split the sprint) · Scope
table · the **Proof:** line naming `SC-###` per tier — every
named ID must resolve in `.scenarios` once 1.2.3's additions land · success metric · DoD
pointer.

## 1.3 Close

**Goal:** accept, persist, hand off.

### 1.3.1 Gate — accept the plan

Present the composed set as one package: sprint plan · register deltas · scenarios · pillar
table (first contact) · todo verdicts · lifecycle cascades. Acceptance is on content;
everything below runs autonomously — no "shall I write?", no "shall I commit?".

### 1.3.2 Write the artifacts

Apply the accepted set: `.sprint/sprint-v{N}.md` · the `.standard` register deltas (and pillar
table on first contact) · the `.scenarios` additions · the todo and findings strikes · the
lifecycle cascades. These are the run's first writes, by design.

### 1.3.3 Commit

One commit, all of 1.3.2 bundled — one logical change: *plan the sprint*.

```bash
git add <the written files>   # scope to the target plugin's paths — never -A across the repo
git commit -m "docs(plan): sprint-v{N} <target-plugin>" \
  -m "Decision: D-###" -m "Scenario: SC-###"   # one trailer per touched ID — pointers, never copies
```

No push — publication is Build's close (Build 2.x).

### 1.3.4 Handoff

Summarise what was captured. Recommend `/plugin-build` in a **fresh session** — the `.sprint`
plan carries the state (*phases don't share a session — artifacts are the bridge*). This phase
ends here — never start building.

## Key principles

- **Both gates precede every write** — compose in conversation, persist only after acceptance;
  a gate after a write is theatre.
- **The plan names its own proof** — a sprint without a resolving Proof line is not a plan.
- **Law lives in the register** — decisions rewrite `D-###` entries in place; no side logs.
- **RED before GREEN** — a scenario that would already pass proves nothing.
- **Trust what you approved** — downstream phases execute the plan as written; re-litigating
  it there is the deviation.
