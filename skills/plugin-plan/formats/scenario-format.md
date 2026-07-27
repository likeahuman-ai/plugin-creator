# scenario-format

The shape of a plugin's `.scenarios/scenarios.md` — the scenario ledger. Written at
/plugin-plan 1.2 (a want lands with its pressure scenario) and appended at /plugin-review 3.4
(a testable finding's fix adds a regression entry); read by /plugin-build's Tier-2 probes (an
entry IS the probe brief), by /plugin-review's accept gate (the sprint Proof line names SC ids
that must be green), and by the runs system (the same entry, unchanged, as an eval case). The
ledger is a decision artifact — trust the entries as given; the Given/When/Then half is an
executable claim, proven by running it, never by rereading it.

## Reference, never reproduce

- `SC-###` — cited by id everywhere (sprint Proof lines, finding fixes, results rows); the
  entry text lives here only.
- Quality-goal numbers — an entry never restates a bound; it cites
  `.standard/standard.md#quality-goals` ("within the standard's activation bound", not
  "≥ 2/3").
- Execution and grading mechanics — tier run shapes, must/should aggregation, judge protocol
  live in `.rubric/rubric.md`; an entry names its tier and grade, nothing more.
- Fixture content — `fixtures/sc-###/` by convention; the entry points, never inlines.
- Pass/fail state — run results live in `results.md`; an entry carries no run state.

## Section inventory

File level, in order:

1. Intro paragraph — one, mandatory: what the ledger is, the tier and grade vocabularies.
2. Ledger head — mandatory, exact anchor `Ledger — Next:`:
   `Ledger — Next: SC-{N} · Removed: {ids or none} · Gaps: {text or none}`
   `Removed:` and `Gaps:` always present — `none` states the empty case as intent.
3. `## {Epic}` groups — every entry sits under exactly one. Reuse an existing epic before
   minting a new one; an epic is a title-case capability area ("Planning", "Hygiene").

Entry level, fixed order, all anchors exact-text:

| # | Element | Mandatory | Absence semantics |
|---|---------|-----------|-------------------|
| 1 | `### SC-###: {title}` | yes | — title is the behavioural claim, present tense ("The static gate blocks a broken reference"), never a feature label |
| 2 | Narrative line — `As a {role}, I want {want}, so that {why}.` | yes | — complete and story-readable; the ledger is still the intent pool |
| 3 | `**Tier:**` `turn` \| `flow` \| `dogfood` \| `unproven` | yes | `unproven` MUST carry `(target: {tier})`; a proven tier never carries a target |
| 4 | `**Given:**` — fixture pointer or real-state precondition | yes | — |
| 5 | `**When:**` — the one action driven | yes | — |
| 6 | `**Then:**` — assertion bullets | yes, ≥1 | each bullet ends `— *observe:* {surface}`; a bullet is **must** (holds every run) unless suffixed `(should — within the standard's bound)` |
| 7 | `**Grade:**` `checklist` \| `judge` | yes | `judge` MUST state its criteria in the same line; `checklist` states nothing more |
| 8 | `**Regression of:** {finding ref}` | review-produced entries only | absent on plan-captured wants — presence marks the entry as review-born |

## Template

```markdown
### SC-{###}: {behavioural claim}
As a {role}, I want {want}, so that {why}.
**Tier:** {tier}{ (target: {tier}) when unproven}
**Given:** {fixture pointer or real-state precondition}
**When:** {the action}
**Then:**
- {assertion} — *observe:* {surface}
- {assertion} (should — within the standard's bound) — *observe:* {surface}
**Grade:** {checklist | judge — {criteria}}
{**Regression of:** {finding ref} — review-born entries only}
```

## Worked example

```markdown
### SC-014: Shipped scripts resolve their own location
As a plugin user on a marketplace install, I want shipped scripts to run wherever the plugin
lands, so that an install layout never breaks a skill mid-flow.
**Tier:** unproven (target: turn)
**Given:** fixture `fixtures/sc-014/` — the plugin installed at a non-default cache path
**When:** the skill step that invokes the script runs
**Then:**
- the script executes green from the moved install — *observe:* exit code in transcript
- no absolute or `~/` path appears in the invocation — *observe:* grep of the skill text
**Grade:** checklist
**Regression of:** portability-reviewer finding, sprint v1 review
```

Against the inventory: the title is the claim, not "script portability"; `unproven` carries
its target; the second Then bullet is a must (no should suffix); `Regression of:` marks it
review-born and names its origin without restating the finding.

## Field rules

- **SC id** — three digits, allocated from the ledger head's `Next:`, then `Next:`
  incremented in the same edit. Ids are never renumbered and never reused; a retired entry's
  id moves to `Removed:` and the entry is deleted (git keeps the text).
- **Tier / Grade** — closed domains as above; any other token is a parse error, not a variant.
- **observe surfaces** — name the concrete surface: a file path, `git log`, a transcript, a
  check's output. One surface per bullet; a bullet needing two observations is two bullets.
- **must/should** — the marker is the exact suffix `(should — within the standard's bound)`;
  the bound itself lives in `.standard/standard.md#quality-goals`.

## Field → consumer index

| Field | Read by | Where |
|-------|---------|-------|
| SC id | sprint Proof line · finding fixes · results rows | /plugin-plan 1.2 writes, /plugin-review accept gate resolves |
| Tier (+ target) | accept gate (which proofs are runnable) · runs system (execution mode) | /plugin-review · runs |
| Given / When | Tier-2 probe (precondition + action to drive) · runs fixture selection | /plugin-build workstream exit |
| Then + observe | probe walk against drafted prose · grading · gate evidence | /plugin-build · /plugin-review · runs |
| must/should | probe (a failed must is the RED confirmation) · runs aggregation | /plugin-build · runs |
| Grade | script-vs-judge routing | runs |
| Regression of | finding → scenario traceability | /plugin-review 3.4 |
| Ledger head | id allocation · removed/gaps accounting | both producers |

## Not in this format

When to capture a scenario, when to probe it, and what to do when one fails — the producing
and consuming skills' process. Whether an entry currently passes — run state, in
`results.md`. The RED obligation (a plan-captured scenario must fail against current prose at
capture time) — the producer's duty, stated in /plugin-plan, not a property of the shape.
Block ownership: /plugin-plan writes wants and may edit entries it owns before they are
proven; /plugin-review appends regression entries and re-tiers entries its sprint proved; the
runs system reads entries and writes results elsewhere — it never edits this file.
