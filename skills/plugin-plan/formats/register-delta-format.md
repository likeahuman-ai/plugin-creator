# register-delta-format

The shape of a `D-###` register delta — one decision entry in Y-statement
rules-with-rationale form, landing in a target plugin's `.standard/standard.md` Rules
register (under `## Rules`, below the ledger head). Scope: registers carrying the
`Ledger — Next:` head — the `D-###` convention this format defines; a target register under
another convention (plain-numbered rules, tombstoned deletions) keeps its own shape, outside
this format. Composed at /plugin-plan 1.2.2,
presented verbatim at the 1.3.1 accept gate, written into the register at 1.3.2 inside the
plan commit; /plugin-review's fix loop produces one whenever a finding's fix changes law
(the 3.4.1 law tail; its 3.4.3 fix commit carries the `Decision:` trailer).
Read by every later /plugin-plan run at 1.0.2 as the law the sprint plans under; carried as
pointers in /plugin-build 2.1.2 writer briefs; checked by scenario SC-003 on the `.standard`
diff ("the register gains or rewrites a `D-###` entry with a Y-statement"); joined to
history by the `Decision: D-###` commit trailer. The register is law — authoritative: trust
an entry as given, never recompute the decision. What the gate accepts is what the register
receives — a delta lands character-for-character as composed.

## Reference, never reproduce

- `D-###` — cited by ID everywhere (commit trailers, sprint Standard-impact lines, finding
  fixes, writer briefs); the entry text lives in the register only.
- Superseded and retired text — git only (`git show {sha}:.standard/standard.md`); the
  *Since:* parenthetical summarises what the prior text held in one clause, never quotes it.
- The step contract — a decision may bind the flow, but `.steps/steps.md` binds; an entry
  states the rule and cites the contract, never reproduces a step table.
- Measured bounds — a *Revisit when:* trigger cites the standard's own quality-goal bound
  ("exceeds this standard's bound"), never restates the number.

## Section inventory

Register level — the context every delta edits:

1. Ledger head — mandatory, exact anchor `Ledger — Next:`, one line:
   `Ledger — Next: D-{N} · Retired: {ids or none}`
   `Retired:` always present — `none` states the empty case as intent.
2. Entries — ascending ID order; a new entry appends at the end. A retired ID leaves a gap
   in the sequence; the `Retired:` list is the account of every gap.

Delta kinds — every delta is exactly one:

| Kind | Entry effect | Ledger-head effect |
|------|--------------|--------------------|
| NEW | entry appended, ID = the head's current `Next:` | `Next:` increments in the same edit |
| SUPERSESSION | existing entry rewritten in place, same ID; *Since:* gains the rewrite parenthetical | none |
| DELETION | entry removed entirely — no tombstone text remains | ID appended to `Retired:` |

Entry level, fixed order, all anchors exact-text:

| # | Element | Mandatory | Absence semantics |
|---|---------|-----------|-------------------|
| 1 | `### D-### — {title}` | yes | — title states the decided rule ("Plain git, one PR per sprint"), never the topic ("Git strategy") |
| 2 | Y-statement blockquote | yes | — one `>` paragraph; five clauses in fixed order, opened by the verbatim connectives `In the context of` · `facing` · `we decided` · `to achieve` · `accepting` |
| 3 | `- **Since:**` | yes | — ISO date; a supersession appends the rewrite parenthetical (see field rules) |
| 4 | `- **Rejected:**` | no | absent → no alternative earned a record; present → `·`-separated `{alternative} — {refuting reason}` pairs |
| 5 | `- **Note:**` | no | absent → the Y-statement needs no gloss; present → operative semantics a consumer applies (a definition, a scope bound) — never commentary |
| 6 | `- **Revisit when:**` | yes | — a concrete, observable trigger; a decision never expected to reopen writes `never expected; {reason}` — the empty case is stated, never omitted |

## Template

Entry — a NEW delta, or the full rewritten text of a SUPERSESSION:

```markdown
### D-{###} — {decided rule as title}
> In the context of {case}, facing {concern}, we decided {decision}, to achieve {quality}, accepting {cost}.
- **Since:** {YYYY-MM-DD}{ (rewritten {same day | YYYY-MM-DD} — {what the prior text held}; {why the current text won})}
- **Rejected:** {alternative} — {refuting reason} · {alternative} — {refuting reason}
- **Note:** {operative gloss}
- **Revisit when:** {trigger}.
```

A DELETION has no entry text — its whole surface is the entry's removal plus the ledger-head
edit.

## Worked example

A NEW delta as landed (the plugin-creator's own register, founding sprint):

```markdown
### D-001 — Three-phase loop: plan, build, review
> In the context of authoring prose plugins, facing a code workflow whose forge machinery (tickets, waves, worktrees) went unused across four sprints of the team's own prose work, we decided on three phases — `/plugin-plan`, `/plugin-build`, `/plugin-review` (review owns findings end-to-end: publish, fix, close) — to achieve a loop sized to the work, accepting that review carries more responsibility than any single code-workflow phase.
- **Since:** 2026-07-02
- **Rejected:** 5-phase parity — ceremony refuted by revealed preference · a tickets phase with build-order issues — the workstream table already carries the decomposition.
- **Revisit when:** a review session regularly exhausts context, or a fix set genuinely needs parallel dispatch — then split a refine phase out.
```

Against the inventory: the title is the rule, not "Phases"; the five connectives land in
order, with the decision clause quotable as law on its own; *Rejected:* splits on ` · `,
each alternative carrying its refuting reason; no *Note:* — the Y-statement needs no gloss;
the revisit trigger is observable (a session exhausting context), not a sentiment. It landed
with the head's `Next:` moving D-001 → D-002 in the same edit and `Decision: D-001` in the
commit trailer.

A SUPERSESSION's *Since:* line as landed (D-002, rewritten the day it was founded):

```markdown
- **Since:** 2026-07-02 (rewritten same day — the founding text decided one consolidated `.craft/` dotdir; fleet-wide uniformity across every `src/` plugin won over the one-dotdir aesthetic)
```

The parenthetical carries three facts and nothing more: when, what the prior text held, why
the current text won. The prior text itself stays in git.

A DELETION, entirely on the head (illustrative — no live register has retired an ID yet):

```markdown
Ledger — Next: D-012 · Retired: none
Ledger — Next: D-012 · Retired: D-005
```

`Next:` never moves on deletion; D-005 is never reused.

## Field rules

- **D id** — `D-` plus exactly three digits, zero-padded (`D-007`, never `D-7`). Allocated
  from the head's `Next:`, which increments in the same edit. Never renumbered, never
  reused — a retired ID stays in `Retired:` forever.
- **Title** — follows the spaced em-dash in `### D-### — {title}`, exact. A phrase stating
  what was decided, applicable from a register scan without reading the blockquote.
- **Y-statement** — one blockquote paragraph; never a second paragraph, list, or bare line
  inside the quote. The five connectives are verbatim clause openers in fixed order;
  punctuation between clauses may flex (comma, or em-dash when a clause carries internal
  punctuation), the connectives never vary. The `we decided` clause carries the operative
  rule — a reader quoting that clause alone holds the law.
- **Since** — ISO `YYYY-MM-DD`: the date the decision first became law under this ID; it
  never changes. A supersession appends (or replaces) the parenthetical, exact opener
  `(rewritten ` — `same day` when the rewrite shares the *Since:* date, otherwise the
  rewrite's ISO date. One parenthetical only, describing the latest rewrite; git holds the
  chain.
- **Rejected** — split on ` · ` (space-middot-space) — never comma- or newline-separated.
  Each item is `{alternative} — {refuting reason}`; an alternative without its refuting
  reason does not land.
- **Retired list** — comma-space separated, ascending (`Retired: D-004, D-009`) — never
  middot-separated; the middot is the head's field delimiter.
- **Commit trailer** — exact form `Decision: D-###`, one trailer line per delta the commit
  lands. The ID is the join key between register, sprint record, and git history.

## Field → consumer index

| Field | Read by | Where |
|-------|---------|-------|
| D id | trailer join · ID allocation · sprint Standard-impact lines · SC-003 diff check | every consumer |
| Title | register scans — the rule at a glance | /plugin-plan 1.0.2 |
| Y-statement | the law planned under · law cited into writer briefs · SC-003's observable | /plugin-plan 1.0.2 · /plugin-build 2.1.2 · `.standard` diff |
| Since + parenthetical | how settled the law is; what a rewrite displaced, without git | later /plugin-plan runs |
| Rejected | refuted alternatives, not re-litigated | /plugin-plan 1.2.2 of later sprints |
| Note | operative semantics applied where the gloss binds | whichever phase the note governs |
| Revisit when | the trigger, checked against current facts; firing reopens the decision as a new delta | /plugin-plan 1.0.2 |
| Ledger head | ID allocation · retired accounting | /plugin-plan 1.2.2 / 1.3.2 · /plugin-review fix loop |

## Not in this format

When a delta is composed, how it is presented, what acceptance means, and what happens on
rejection — /plugin-plan's process (1.2.2–1.3.2) and /plugin-review's fix loop. The
register's intro prose and the standard's other parts (philosophies, pillars, quality goals,
runtime criteria) — the standard's own; a delta touches exactly one entry plus the ledger
head, nothing else in the file. The reason a deletion happened — never recorded in the
register; the commit carrying its `Decision: D-###` trailer archives it. Which sprint
produced a delta — the sprint file's Standard-impact line and the commit trailer, never the
entry. Block ownership is trivial by design: an entry is written whole, in one edit, by
whichever producer lands it — no block is filled later by another role.
