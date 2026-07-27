# pillar-format

Written by /plugin-plan 1.2 on first contact with a target plugin — or when its shipped
component shape has changed — into the target plugin's `.standard/standard.md`, under the
exact heading `## Pillars`. Read by every writer dispatch (the row for the material being
produced travels in the brief as law) and by every reviewer (the rows are the review
standard — review against intent, not taste). Decision artifact: consumers trust the
instantiated table as written and never re-derive it from the tree mid-flow.

## Reference, never reproduce

- The target's philosophies govern every row but live in its own `.standard` sections — a row
  *applies* a philosophy, it never restates one.
- Writer and reviewer mandates live in agent files — a row names a shipped file kind, never
  the agent that produces or judges it.
- File counts in the pillar-set line describe the tree at instantiation; the tree stays
  ground truth — on drift, the section is re-instantiated whole, never patched number by
  number.

## Sections, in order

1. **Pillar-set line** (mandatory) — one breath of prose: what file kinds the plugin ships,
   which of the generic four it omits and why, what it adds. Absence semantics: a generic
   pillar missing from the table means the plugin ships no files of that kind, and the line
   says so explicitly ("No agents, prompts, or formats pillars — …").
2. **The table** (mandatory, one row minimum) — exact header, verbatim:
   `| Pillar | Function | Passes when | Deviates when |`
3. **Sizing rider** (conditional) — present if and only if the table carries BOTH an Agents
   row and a Prompts row; verbatim: "**Prompt sizing — the inverse rule:** a prompt's size is
   inverse to the receiving agent's own system prompt — agent-backed → thin; no agent file →
   the complete brief." Absent means the plugin ships no dispatch pair — nothing to size.

## Row derivation — determinism over taste

- The row set derives from the target's **shipped tree**, never from the generic template:
  **omit** any generic pillar with no files of that kind; **add** one row per real shipped
  file kind the four don't cover (`references/`, `scripts/`, rule files, checklists — named
  as the actual kind). Never a placeholder row; never an empty cell.
- The generic four, where present, keep this base wording — tightened to the plugin, never
  loosened:

| Pillar | Function | Passes when | Deviates when |
|---|---|---|---|
| Skills | orchestrate one phase (one slash command): drive flow, dispatch, own artifacts | thin control flow that delegates reasoning and trusts upstream | does an agent's work inline, re-checks upstream, or spans two phases |
| Agents | a specialist subagent dispatched for one job | one clear remit; returns evidence, owns no flow | overlaps a sibling unowned, needs unhanded session context, or drives flow |
| Prompts | the brief a skill hands an agent for one dispatch | task + inputs + output contract; references a format, never restates it | duplicates the agent file or inlines a format |
| Formats | the shape of one artifact a skill fills | structure only; reference-not-reproduce; example-led | restates another artifact, adds ceremony, or prescribes process |

- Added rows state a real Function and a pass/deviate pair **testable against actual files**
  — a row a reviewer cannot apply to a concrete file is taste, not law.
- Row names optionally carry the file count in parentheses — `Skills (4)` — tying the row to
  the tree the instantiation read.
- Re-instantiation **replaces the whole `## Pillars` section** — never appends a second
  table, never edits single rows in place.

## Template

```markdown
## Pillars

<pillar-set line: ships <kinds> · omits <generic pillars> because <reason> · adds <kinds>>

| Pillar | Function | Passes when | Deviates when |
|---|---|---|---|
| <Kind (count)> | <what these files do in this plugin> | <testable pass> | <testable deviation> |

<sizing rider — only when both Agents and Prompts rows are present>
```

## Worked example — a minimal skills-only plugin

```markdown
## Pillars

This plugin ships two file kinds: one skill and its bundled references. No agents, prompts,
or formats pillars — it dispatches nothing and fills no structured artifacts.

| Pillar | Function | Passes when | Deviates when |
|---|---|---|---|
| Skills (1) | orchestrate the single command end to end | thin flow; depth delegated to references | inlines reference content, or re-checks what the user already confirmed |
| References (3) | knowledge read at the step that needs it | each cited by exact path from a SKILL.md step | cited but content-free, or never cited (orphan) |
```

What the example proves: the three omitted pillars are declared in the pillar-set line, so
absence reads as intent · References is an added row with a pass/deviate pair a reviewer can
test file by file (the orphan check is even mechanical) · no sizing rider ships because no
dispatch pair exists · counts tie the rows to the tree that was read.

## Fields → consumers

| Field | Read by | Where |
|---|---|---|
| A pillar row, whole | the writer producing that kind | its dispatch brief carries the row as law |
| `Passes when` / `Deviates when` | every reviewer | applied per file kind as the review standard |
| Pillar-set line | /plugin-plan | next contact — shape drift against the tree triggers re-instantiation |
| Sizing rider | prompt-writer · economy-reviewer | the brief-size decision; restatement findings |

## Not in this format

When to instantiate, how the change is gated, and where the delta is discussed are
/plugin-plan's steps. Who may write or dispatch lives in the target's Roles section.
Philosophies are applied by rows, never restated inside them.
