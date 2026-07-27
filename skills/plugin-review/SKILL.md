---
name: plugin-review
description: "Reviews a plugin sprint's PR with the prose reviewer roster and closes the sprint — grounded findings published and fixed, the scenario suite grown, the step contract swept, the PR landed. Use when a sprint PR is open after /plugin-build, or the user says 'review the plugin', 'review the sprint', 'plugin review', or 'is this ready'."
argument-hint: "PR number or URL (optional — defaults to the open PR on the current sprint branch)"
---

# /plugin-review — Review (Phase 3): roster → consolidate → publish → fix → close

Phase 3 of the loop (Plan → Build → Review). Review the sprint's PR with the seven-reviewer
roster, consolidate their reports in-session, publish the grounded set, fix it, grow the
scenario suite, sweep the target's step contract, and land the sprint. **Exactly one human
gate** — `Gate — accept vs DoD` (3.5.2); everything before it runs autonomously.

## You are the orchestrator and the judge, never a finder

Dispatch the roster, weigh what it returns, apply fixes — but never generate a finding
yourself. If you catch yourself reading the diff to *produce* findings, STOP — that work
belongs to the seven reviewers; your judgement begins only when their reports are in.
Consolidation (3.2) and fixes (3.4) are yours — *edits are session work*.

| Tool | Allowed | Purpose |
|------|---------|---------|
| Agent | YES | The roster batch (3.1.1), probes (3.2.5), `steps-writer` (3.5.1) |
| Bash (`git`, `gh`) | YES | PR discovery, diff, comment, commits, merge — via `gh`, never an MCP server |
| Read / Grep / Glob | YES | Review standard, reports, scenario ledger |
| Edit / Write | 3.3.2, 3.4.1, 3.4.2 only | Notes file, fixes, regression scenarios — nothing else |

Reviewers are read-only and run in parallel — no isolation needed, nothing mutates. Probes
and `steps-writer` follow their own contracts. *Trust the artifact* governs the PR's envelope
— never re-litigate the sprint's scope or re-run Build's gates (the static gate was Build's
exit condition); adversarial review governs the prose inside it.

Formats referenced as `<name>-format` live at `${CLAUDE_PLUGIN_ROOT}/skills/plugin-review/formats/`.

**Initial request:** $ARGUMENTS

---

## 3.0 Setup

**Goal:** establish the review surface — the PR, its diff, and the review standard.

### 3.0.1 Find the sprint's PR

- If `$ARGUMENTS` names a PR number or URL → use exactly that.
- Else → the open PR on the current sprint branch (`gh pr list --head <branch>`).
- if none found → stop and tell the user: no open sprint PR — run `/plugin-build` first, or
  pass a PR number.

### 3.0.2 Check eligibility

Read the PR state (`gh pr view`). if closed or merged → stop: this sprint's review is done —
next work starts at `/plugin-plan`. if the diff is empty → stop and surface it: an empty
sprint PR is a Build signal, not a review target.

### 3.0.3 Read the diff and the review standard

- The PR diff (`gh pr diff`) — the **target**; the changed files' full text is the context.
- The target plugin's `.standard/standard.md` — **in full** (the review standard: pillar
  rows + quality goals; the roster judges against intent, never taste).
- The target's `.rubric/rubric.md` — skip-if-absent (target-specific measurement law only;
  the consolidation procedure itself ships with this skill, 3.2).
- The target's `.scenarios/scenarios.md` — selective (the ledger head + entries the diff
  touches; it can be large).
- The target's `.steps/steps.md` — the slice for touched surfaces.

## 3.1 Review

**Goal:** gather the roster's reports. Fan-out declared: seven reviewers, one batch.

### 3.1.1 Dispatch the roster

Dispatch all seven in ONE parallel batch — a single message with one Agent call per reviewer,
by name: `ambiguity-reviewer` · `conflict-reviewer` · `economy-reviewer` · `flow-reviewer` ·
`trigger-reviewer` · `coupling-reviewer` · `portability-reviewer`. Inject into each: the
diff (the target) · the review-standard pointers (3.0.3) · the **context mandate** — judge
the system, not just the diff: read the whole containing file and the files that consume a
changed instruction before flagging; target the change, never unrelated pre-existing prose
(*direction from the session, discretion to the agent*).

Do NOT narrow the batch because the diff "looks small" — a small diff still gets all seven;
skipping a dimension is how its failure mode ships.

### 3.1.2 Barrier — collect the reports

Wait for the full batch; collect all seven reports. Each arrives as a flat findings list in
the `finding-format` field set, or its dimension's honest empty-result sentence. A reviewer
that returns neither → re-dispatch once with the gap named; a second failure → surface it and
proceed with six (the gap is itself reported at 3.3.1).

## 3.2 Consolidation — a session step

**Goal:** turn seven raw reports into one graded set. Read
`${CLAUDE_PLUGIN_ROOT}/skills/plugin-review/references/consolidation.md` now — it owns the
filter list, the calibration anchors, and the bucket semantics; this section owns only the
order of operations.

### 3.2.1 Grounding discard — mechanical

Drop every finding without a resolvable `file:line` AND quoted evidence. No judgement: a
finding that "seems obviously right" without a coordinate is still dropped — obviousness is
not evidence. Count what you drop.

### 3.2.2 False-positive filter

Apply the filter list from `consolidation.md`. Count what you drop.

### 3.2.3 Dedup and merge

Same `file:line` first, judgement for semantic duplicates. Boundary-designed overlaps arrive
from two reviewers by design — merge into one finding carrying both lenses' evidence, never
pick a winner. A static prediction and a probe-evidenced observation of the same defect merge
into one finding carrying both evidence kinds.

### 3.2.4 Bucket — publish / note / drop

Bucket per `consolidation.md`'s anchors and semantics. Reviewer confidence is signal, never
binding — weigh it against the anchors on your global view of the set.

### 3.2.5 Probe the behavioural claims

For each **publish** finding resting on a behavioural claim — its Evidence opens with the
exact-text anchor `needs-probe — `; treat a behavioural assertion lacking the anchor the same
way and correct the block in the merge (the anchor is the finder's only legal behaviour
claim): dispatch a probe — a general subagent given the exact prose and a scenario
reproducing the claim. Persist the probe's walk verbatim to the target's `.sprint/probes.md`
under its anchor heading (the walk on disk is the citable evidence — the probe's message is
transport), then append the result as the finding's `**Probe:** reproduced — <one line>
(probe: .sprint/probes.md#<anchor>)` line per `finding-format`. if the probe does not
reproduce the claim → demote the finding to **note**, with the probe result persisted and
recorded all the same. No probe evidence, no published behavioural claim — prediction alone
never publishes.

## 3.3 Publish

**Goal:** the findings leave the session — the publication is the fix loop's work order.

### 3.3.1 Post the review comment

Post ONE comment on the PR in `finding-format`'s published-comment shape, verbatim anchors:
the `### Plugin Review` marker · the `**Raw → published:** N → M` count line (the cull made
visible) · the **publish** bucket's blocks · the Notes line iff 3.3.2 writes notes. Per-stage
detail (grounded/filtered counts, any dimension that did not report — 3.1.2) rides as prose
after the count line. Zero published findings still posts — the counts and the empty result
are the record.

### 3.3.2 Record the notes

Append the **note** bucket to the target's `.sprint/findings.md` under a dated heading — the
next `/plugin-plan` drains it. if the bucket is empty → skip, write nothing.

## 3.4 Fix loop

**Goal:** the published set gets fixed — by you. Loop 3.4.1–3.4.3 per published finding until
the set is exhausted; deferrals are gate business (3.5.2), never silent.

### 3.4.1 Compose the fix

Fix the finding in the target's prose — *edits are session work*; no writer dispatch. No
commit act in this step — the commit is 3.4.3, after the suite has grown.

### 3.4.2 Grow the suite

**The finding's `Testable:` field is the ruling — never re-judge encodability at fix time.**
if `Testable: yes` → compose the `SC-###` entry or update in the target's
`.scenarios/scenarios.md` — the failure it caught, with its observable. No exceptions for
"trivial" fixes: trivial-but-testable is exactly what regresses silently. The
state-it-in-the-commit-body escape exists only for findings the session marked
`Testable: no` at consolidation.

### 3.4.3 Commit

ONE commit per finding, carrying the fix AND its scenario together — `fix(<plugin>):
<finding>` with a `Sprint: v{N}` trailer. A testable fix whose commit shows an empty
scenarios diff is the deviation 3.4.2 exists to prevent.

### 3.4.4 Re-run the static gate

The fixes are new prose — run the target's static gate over the whole payload. if red → fix
forward (3.4.1), bounded — two rounds, then surface the failure at the gate rather than
looping.

## 3.5 Close

**Goal:** true the step contract, gate the sprint, land.

### 3.5.1 Dispatch steps-writer

Dispatch `steps-writer` — once per sprint, over the landed whole. Inject: the sprint's full
diff (post-fixes) · the target's current `.steps/steps.md` · the sprint file. Barrier — wait
for its report; its resolve-check must be green and its `drift:` lines go into the gate
summary. if the resolve-check fails → re-dispatch once with the failures named; twice →
surface at the gate.

### 3.5.2 Gate — accept vs DoD

Present the sprint against the target's Definition of Done: the sprint's **Proof** line —
each named scenario green at its tier · static gate green payload-wide (3.4.4) · every
published finding fixed, or **explicitly waived here with a recorded reason** (the waiver
goes into the closing commit body) · the `.steps` sweep done (3.5.1) · `drift:` lines
reviewed. The gate approves **content** — the merge and flip that follow are autonomous;
never ask "shall I merge?".

### 3.5.3 Land

Merge the PR per the repo's policy (`gh pr merge`). Flip the sprint file `draft → built` as
an annotation riding the closing commit — a status edit is never its own commit.

### 3.5.4 Handoff

Summarise: raw → published counts, fixes landed, scenarios added, drift recorded, waivers.
This phase — and the sprint — end here; never start planning. Next: `/plugin-plan` in a fresh
session (*phases don't share a session — artifacts are the bridge*: the landed PR, the grown
ledger, and the `built` sprint file carry the state).

---

## Key principles

- **Grounded or gone** — no coordinate + evidence, no finding (3.2.1); no probe evidence, no
  published behavioural claim (3.2.5). Mechanical, before any judgement.
- **The session judges, the roster finds** — seven finders, zero arbiters; consolidation is
  your judgement applied to their evidence, and you never add findings of your own.
- **The cull is visible** — raw → grounded → published counts ship in the comment; a silent
  cull reads as a clean sprint when it wasn't.
- **The suite grows from failures** — every testable published finding lands its regression
  scenario in the fix commit; the same failure cannot ship twice.
- **One sweep, at close** — `steps-writer` runs once over the landed whole, never per fix;
  the join describes the system as it IS.
- **One gate, on content** — accept-vs-DoD is the sprint's only human decision; every
  commit, merge, and flip around it runs autonomously.
- **Small diffs get the full roster** — dimension coverage is never a size call.
