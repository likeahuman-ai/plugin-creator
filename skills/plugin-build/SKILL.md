---
name: plugin-build
description: >
  Builds a target plugin's approved draft sprint into committed plugin source and one draft
  PR, every workstream gated before it lands. Use when the user says "build the sprint",
  "start building", "implement the plan", or after /plugin-plan has an accepted draft sprint.
  Never re-plans — a blocker routes back to /plugin-plan; never for code projects (that is
  the development plugins' work).
argument-hint: "target plugin name (optional — defaults to the src/ plugin whose .sprint holds the latest draft)"
---

# /plugin-build — Build (Phase 2): workstreams → writers → gates → one PR

Phase 2 of the loop: turn the accepted draft sprint into the target plugin's built source.
Drive the sub-phases in order — 2.0 Setup → 2.1 Workstream loop → 2.2 Close. **No human
gates in this phase** — the human decision was Plan's plan-accept; Build runs autonomously
from the accepted plan to the draft PR. The gates here are agent gates — the Tier-1 static
gate and the Tier-2 probe — and they block commits, not people.

Boundaries that hold for the whole phase:

- **Trust the plan.** The sprint is the contract — executed as written, never re-litigated
  mid-build; a genuine blocker routes back to `/plugin-plan`, a silent re-scope is a
  deviation.
- **Dispatch creation, own the edits.** New files come from writers; edits to existing prose
  are the session's own hands — writers never edit (prose concerns overlap in files; one
  editor keeps them coherent).
- **Nothing lands ungated.** No commit while a static check fails or a probe returns
  would-fail — fix and re-gate, bounded then escalate.
- **Sole committer, plain git.** Writers return content and reports; the session makes every
  commit; no worktrees — writer outputs are disjoint new paths by construction.

Prompts referenced as `<name>-prompt` live at
`${CLAUDE_PLUGIN_ROOT}/skills/plugin-build/prompts/<name>-prompt.md`; references live under
`${CLAUDE_PLUGIN_ROOT}/skills/plugin-build/references/`.

**Initial request:** $ARGUMENTS

## 2.0 Phase setup

**Goal:** locate the contract, ready the branch.

### 2.0.1 Resolve the target plugin

$ARGUMENTS names the plugin; if empty → scan `src/*/.sprint/` for sprint files with
`status: draft`; if exactly one plugin qualifies → build that one; if none → stop: nothing to
build — run `/plugin-plan` first; if several → ask which (one question, recommended answer:
the most recently dated draft).

### 2.0.2 Read the contract

Read the draft sprint **in full** (the contract) — goal, workstreams in order, scope, Proof
line, non-goals. Read the target's `.standard/standard.md` **in full** (the law — its pillar
rows travel in every writer brief) and the `.steps/steps.md` slice for the touched surface.
*Trust the artifact* — the plan is approved, execute as written; if the tree contradicts a
plan assumption (a named path is gone, a deliverable already exists) → stop and route to
`/plugin-plan` — never silently re-scope.

### 2.0.3 Cut or resume the branch

```bash
git fetch origin development
git checkout -b feat/<plugin>-sprint-v<N> origin/development
```

if the branch already exists → resume on it — completed workstreams are the commits carrying
the `Sprint: v{N}` trailer; continue at the first workstream without one.

## 2.1 Workstream loop

**Goal:** build each workstream to green — loop 2.1.1–2.1.6 per workstream, in the plan's
order, until all are built.

### 2.1.1 Split the work

Per the workstream's deliverables: **new files** → writer dispatches (2.1.2); **edits to
existing prose** → done directly by the session, before any dispatch. if a workstream is all
edits → do them, then skip to 2.1.4.

### 2.1.2 Assemble the briefs

One brief per **deliverable** — a writer's contract is one deliverable per dispatch, so a
workstream with three new prompts fills `prompt-writer-prompt` three times. Use the
deliverable's dispatch prompt — `skill-writer-prompt` · `prompt-writer-prompt` ·
`format-writer-prompt` · `agent-writer-prompt` · `hooks-writer-prompt` ·
`script-writer-prompt`. Fill the prompt's slots from what the session already holds — a step
coordinate counts as held; the writer reads the step at its own discretion (*direction from
the session, discretion to the agent*): the workstream brief · the target's pillar row for
that material · the pressure scenarios (skill-writer only — behaviour lives in skills) · the
topology slice · pointers to siblings, agents, formats · the register. The prompt owns the
brief shape — fill it, never restate the writer's craft.

### 2.1.3 Dispatch the writers

ONE parallel batch per workstream — a single message, one Agent call per **deliverable**
(the same writer appears once per deliverable it owns); outputs are disjoint new paths by
construction. Barrier — wait for the whole batch, collect every report. Reports use the
closed status vocabulary — **SUCCESS | NEEDS_CONTEXT | BLOCKED**, plus **REFUSED** from
`hooks-writer` only (a verdict that the mechanism is unwarranted, routing to `/plugin-plan`);
the six prompts cite this vocabulary, they never restate it. on NEEDS_CONTEXT → answer it
from the plan and re-dispatch, bounded (a round or two), then escalate; if the plan cannot
answer it → a plan gap: stop and route to `/plugin-plan`. on BLOCKED → the named blocker is
the session's to clear or escalate, never the writer's to work around.

### 2.1.4 Gate — Tier-1 static

Run the check-set in `references/static-gate-checks.md` over the workstream's touched files —
deterministic assertions only, no model calls. Collect ALL failures before reporting; name
each failing check with its file and the failing content in the transcript. on any failure →
fix (session edit, or re-dispatch the owning writer with the named defect) → re-gate;
bounded, then escalate. **The gate blocks the commit — nothing is committed while a check
fails.** The tempting shortcut — "the failure is cosmetic, commit now and fix later" — is the
deviation this gate exists to prevent: later never re-gates.

### 2.1.5 Gate — Tier-2 probe

For each pressure scenario the workstream's deliverables were written against: dispatch one
probe subagent per `references/probe-protocol.md` — it receives the drafted prose and the
scenario, cold, and returns **would-pass** or **would-fail** with the failing step quoted.
Persist each returned walk per the protocol — to the target's `.sprint/probes.md`, before
acting on the verdict. on would-fail → fix → re-probe with a fresh probe (a probe never sees
its previous verdict); same bound as 2.1.4. if the workstream touches no scenario → skip and say so in the
transcript — legitimate for pure prompt, format, or hooks workstreams (a hook's sample-run
obligation already exercises it); a *skill* workstream with no scenario is a plan gap → stop
and route to `/plugin-plan`.

### 2.1.6 Commit the workstream

One commit — subject `feat(<plugin>): WS<n> — <deliverable>`, trailer `Sprint: v{N}`. Then →
next workstream (2.1.1); all built → 2.2.

## 2.2 Phase close

**Goal:** publish the sprint branch, hand off to review.

### 2.2.1 Whole-payload gate

Re-run the static gate over the target plugin's entire shipped payload — not just this
sprint's files (cross-file references rot at a distance: a renamed format breaks a citing
skill the sprint never touched). on failure → fix → one commit
`fix(<plugin>): gate — <what>` + `Sprint: v{N}` trailer → re-gate.

### 2.2.2 Publish

Push the branch; open ONE draft PR onto `development` — title `<plugin> — sprint v{N}`;
body: the sprint goal, the workstream list with one line each, the Proof line and its current
status. Draft blocks the merge buttons until review lifts it.

### 2.2.3 Handoff

Summarise per workstream — what was built, gate and probe results. Recommend
`/plugin-review` in a **fresh session** — the PR carries the handoff state (*phases don't
share a session — artifacts are the bridge*). This phase ends here — never start reviewing.

## Key principles

- **The plan is the contract** — approved scope is executed as written; a blocker routes back
  to `/plugin-plan`, never a silent re-scope.
- **Writers create, the session edits** — new files by dispatch, edits by the session's own
  hand, one committer either way.
- **Two gates before every commit** — deterministic checks first, then a behavioural probe; a
  failure blocks the commit, bounded then escalate.
- **One workstream, one commit** — `Sprint: v{N}` trailer on every one; the PR reads as the
  plan.
- **Zero human gates** — the human decided at plan-accept; the next human decision is
  review's accept-vs-DoD.
