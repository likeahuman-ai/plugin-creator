# Sprint format

Shape of `.sprint/sprint-v{N}.md` — the sprint's versioned objective, one file per sprint. Written at Plan 1.2 (session-authored, gated at plan-accept), read by Build 2.0 **in full (the contract)** — the Workstreams, Scope, and Proof sections drive the whole build — read by Review 3.5 at the accept gate (the Proof line decides; Review flips `status:`), and read by the next Plan run to enforce the lifecycle. A decision artifact: downstream *trusts it as given* — never re-validates, never recomputes. The body is **frozen once the plan is accepted**; the only sanctioned post-accept mutations are `status:` and the appended `## Release` line.

## Reference, never reproduce

Every fact owned elsewhere appears as a pointer, never a copy:

- `SC-###` — scenario ids; the ledger `.scenarios/scenarios.md` owns the scenario text.
- `D-###` — rules-register ids; `.standard/standard.md#rules` owns the law. An unnumbered new decision is referenced by title — **never invent a number**.
- The Definition of Done — `.standard/standard.md#definition-of-done`, anchored pointer only.
- The north-star and quality goals — `.standard/standard.md`; Success metric references, never restates.

## Frontmatter

```yaml
---
version: 3               # sequential integer, no gaps; derives the filename sprint-v{N}.md and the Sprint: v{N} commit trailer
status: draft            # the ONLY mutable field — see lifecycle
date: 2026-07-02         # creation date, ISO; never updated
author: tim              # bare name of the human who owns the sprint; an agent-run session with no human owner writes Session Agent
previous: sprint-v2.md   # prior sprint's filename; null for v1
---
```

### `status:` lifecycle

| Value | Means | Flipped by |
| --- | --- | --- |
| `deferred` | captured for a future cycle, not active | Plan, when parking scope |
| `draft` | active — covers planning and all building-against | set on creation, Plan 1.2 |
| `built` | every workstream landed and the named proofs held at accept | Review 3.5, riding the closing commit |
| `released` | the plugin version carrying this sprint's work is published | the publishing session, riding the version bump |
| `archived` | superseded — a newer draft exists | the next Plan run's cascade |
| `abandoned` | cancelled before build (user-driven, rare) | Plan |

A status edit is an **annotation, not a change** — it never earns its own commit; it rides the one named in `Flipped by`.

## Body sections — in order, all required unless marked

Consumers key on these exact `##` headings; additional sections are permitted and consumers ignore them.

**Goal** — the pass/fail criterion in full. Default form, one sentence, binary: `This sprint succeeds iff <objective>.` Multi-condition form only when 2–3 conditions genuinely pass or fail together: a lead sentence naming the headline condition (the one the Success metric measures), then binary bullets — past three, split the sprint. No condition that decides pass/fail may live only in Scope or the DoD.

**Non-goals** — short bullets of what the sprint deliberately omits; `None.` when genuinely none (absence reads as intent, not omission).

**Standard impact** — the register deltas this sprint makes: `D-###` by id for existing law (rewrites name the id and say "rewrite in place"), by title for new unnumbered decisions; `None.` when the sprint touches no law.

**Workstreams** — exactly one table, 1–5 rows; a sixth row is scope overflow — split the sprint.

```
| WS | Delivers | Detail |
| --- | --- | --- |
```

`WS` tokens are `WS1…WS5`, sequential from WS1. `Delivers` names the output in a phrase; `Detail` carries the sprint-specific content a builder needs — pointers and constraints, never restated artifact bodies.

**Scope** — exactly one table:

```
| Path | WS |
| --- | --- |
```

`Path` is repo-relative (glob allowed). `WS` is a workstream token — or a short annotation when a path is legitimately in scope without being built this sprint (`delivered ahead — review findings only`). Every `WS` token from the Workstreams table appears in at least one row.

**Proof** — per-tier bullets naming the scenarios that must be green at the accept gate:

```
- **<tier>:** SC-###[, SC-###] — <one line: what green means here>
```

`<tier>` ∈ `turn` | `flow` | `dogfood`. A tier with nothing to prove is simply absent. A proof this sprint cannot run states its deferral target explicitly (`deferred to v{N} — <why>`). **Consumer's resolve-check:** every cited `SC-###` resolves in the target's `.scenarios/scenarios.md` ledger — an unresolvable id fails the accept gate before anything is graded.

**Success metric** — one line tying this sprint's measurable target to the north-star (referenced, never restated).

**Definition of Done** — the anchored pointer `.standard/standard.md#definition-of-done`, plus sprint-specific exit conditions only, if any exist.

**Release** *(optional — appended at publish, never written at plan)* — the shipped-version record, one line per shipped version:

```
v{X.Y.Z} — {YYYY-MM-DD} — marketplace {bumped|pending}
```

Absent = nothing shipped from this sprint yet (the normal state at accept). Appended by the publishing session, riding the same act that flips `status: released`; `pending` means the plugin repo is pushed but the marketplace bump has not landed. This is the sprint file's only body mutation sanctioned after plan-accept besides `status:`.

## Never include

- Step-level implementation detail — the phase SKILL.mds own steps; the step contract has its own home.
- Restated scenario text, DoD text, rule bodies, or quality-goal numbers — pointers only.
- In-sprint progress tracking (done / blocked / in-progress) — commits and the PR own status.
- Ticket, issue, or wave machinery — this loop has no tickets phase.
- Open `[NEEDS CLARIFICATION: …]` markers past plan-accept.
- Implementation code, ever.

## Example

```markdown
---
version: 2
status: draft
date: 2026-07-04
author: tim
previous: sprint-v1.md
---

# Sprint v2 — Trigger reliability

## Goal

This sprint succeeds iff every in-scope phrasing in the trigger scenarios activates the skill, with:
- near-miss phrasings never activating
- the description carrying no workflow summary

## Non-goals

- No new skills — description and frontmatter work only.

## Standard impact

None.

## Workstreams

| WS | Delivers | Detail |
| --- | --- | --- |
| WS1 | Reworked descriptions | Both skills' frontmatter — outcome + quoted utterances, no workflow summary |
| WS2 | Trigger scenarios | SC-014, SC-015 wants with their failing phrasings captured |

## Scope

| Path | WS |
| --- | --- |
| `src/example-plugin/skills/*/SKILL.md` (frontmatter only) | WS1 |
| `src/example-plugin/.scenarios/scenarios.md` | WS2 |

## Proof

- **turn:** SC-014, SC-015 — in-scope phrasings activate in every probe; near-misses never do.
- **dogfood:** SC-002 — graded on this sprint's own review publication.

## Success metric

North-star (clean-loop rate, defined in `.standard/standard.md`): this sprint's metric — zero trigger findings at review.

## Definition of Done

Canonical DoD per `.standard/standard.md#definition-of-done`. Sprint-specific addition: none.
```

The example proves: multi-condition Goal with a headline the metric measures · `None.` absence semantics in Standard impact · WS tokens consistent between the two tables · per-tier Proof lines whose `SC-###` ids the accept gate will resolve against the ledger · anchored DoD with no restatement.

## Field → consumer index

| Field | Read by | Where |
| --- | --- | --- |
| `status:` | the next Plan run (lifecycle enforcement) · Review 3.5 (the flip) | Plan setup · accept gate close |
| `version:` / `previous:` | the next Plan run (sequence, cascade) · Build (derives the `Sprint: v{N}` commit trailer) | Plan setup · every workstream commit |
| Goal | Review (pass/fail against the landed whole) | accept gate |
| Non-goals | Build (scope guard — drift beyond a non-goal stops the workstream) | workstream loop |
| Standard impact | Review (sweep context) · the session (commit trailers `Decision: D-###`) | close-out |
| Workstreams | Build (the unit of the build loop — one commit per WS) | 2.x loop |
| Scope | Build (path guard) · Review (expected diff surface) | workstream exits · review setup |
| Proof | Review (named proofs green; the `SC-###` resolve-check) | accept gate |
| Success metric | the next Plan run (retrospective input) | Plan discovery |
| Definition of Done | Review | accept gate |
| Release | the publish flow (reads `pending` state) · the next Plan run (retrospective) | version bump · Plan discovery |
