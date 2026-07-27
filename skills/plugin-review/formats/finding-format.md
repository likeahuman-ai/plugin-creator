# finding-format

The reviewer finding block and the published `### Plugin Review` comment — finder blocks
written by each of the seven reviewers on return from the roster dispatch (phase 3), the
comment assembled and posted by the session at consolidation (/plugin-review 3.3); read by
the session's consolidation, the fix loop, and the note ledger as a **claim** — every field
is checkable, nothing in it is trusted unverified.

## Reference, never reproduce

- **Confidence anchors** — live in each reviewer's `## Confidence` section; this format
  carries the number, never the anchor table.
- **Empty-result sentences** — live in each reviewer's `## Output` section; a clean dimension
  reports its sentence, it does not emit an empty block.
- **Bucket rules, probe protocol, dedup procedure** — live in the producing skill
  (`plugin-review`); this format shapes what they read and write, not how they decide.
- **Regression scenario shape** — owned by the plan phase's scenario ledger; `Testable: yes`
  points there, it does not restate the entry shape.

## The finding block

Filled in two stages by different roles — block ownership is fixed:

- **Finder stage** (a reviewer, on return): the first six fields, all mandatory — a block
  missing any of them is not a finding.
- **Session stage** (consolidation, on publish): appends `Reviewers`, `Testable`, and — for
  probe-evidenced findings only — `Probe`. The session never edits the finder's six fields;
  a wrong finder field is grounds to drop, not to rewrite.

```
**Where:** [path]:[line]
**What:** [the defect, one sentence]
**Evidence:** [quoted text — or: needs-probe — [the behavioural claim]]
**Impact:** [what the executing model does wrong if unfixed]
**Suggestion:** [concrete replacement]
**Confidence:** [0-100]
**Reviewers:** [agent-name · agent-name]
**Testable:** [yes | no]
**Probe:** reproduced — [one-line observation] (probe: .sprint/probes.md#[anchor])
```

| Field | Domain and semantics |
|---|---|
| `**Where:**` | `path:line` — path as it appears in the diff (repo-relative), line in the post-change file; must resolve against the reviewed head. **The dedup key and the block's parse anchor**: a block opens at `**Where:**` and runs to the next `**Where:**` or the section's end; blocks are separated by one blank line. |
| `**What:**` | One sentence, the defect itself — never the category name alone. |
| `**Evidence:**` | Quoted text from the reviewed file. A behavioural claim the prose alone cannot prove opens with the exact-text anchor `needs-probe — ` followed by the claim; that anchor is the finder's only legal way to assert behaviour. |
| `**Impact:**` | What the executing model does wrong if unfixed — observable consequence, not restated defect. |
| `**Suggestion:**` | Concrete replacement text or change — specific enough to drive the edit. |
| `**Confidence:**` | Integer 0–100, the finder's honest self-assessment — a signal for weighing, never a threshold and never mutated downstream. |
| `**Reviewers:**` | Session-added. The reporting dimensions as full agent names, joined with ` · ` in dispatch-roster order; a merged double-report keeps every reporter. |
| `**Testable:**` | Session-added. `yes` or `no` — `yes` means a regression scenario is owed with the fix. |
| `**Probe:**` | Session-added, present **iff** the finder's Evidence opened with `needs-probe — `. Value: `reproduced — ` + one line of what the probe observed + ` (probe: .sprint/probes.md#<anchor>)` — the anchor of the walk the session persisted per the probe protocol; the persisted walk is the citable evidence, the one-liner is its summary. Absent = static finding, fully evidenced by quoted text. |

## The published comment

Section inventory, in order — mandatory unless stated:

1. `### Plugin Review` — the exact-text marker, alone on its line; downstream phases key off
   it and read the **latest** comment carrying it.
2. `**Raw → published:** N → M` — exact shape; `N` = finder blocks received across the whole
   roster before dedup, `M` = published blocks below. The cull made visible.
3. The published finding blocks — nine-field form. **Absent when M = 0**: the count line
   stands alone and reads as a clean review, not an omission.
4. `Notes: K further findings recorded in .sprint/findings.md` — exact shape; present **iff**
   the note bucket is non-empty. Note-bucket entries reuse the published block unchanged; the
   note ledger's own file structure belongs to the producing skill.

## Worked example

```
### Plugin Review

**Raw → published:** 11 → 2

**Where:** skills/pitch/SKILL.md:47
**What:** "clean up the output" is readable as delete-artifacts or as reformat-prose.
**Evidence:** "When the run ends, clean up the output." — reading A: remove generated
files; reading B: tidy the pitch text.
**Impact:** The executor deletes deliverables on some runs and edits prose on others.
**Suggestion:** "When the run ends, delete `out/tmp/`; leave `out/pitch/` untouched."
**Confidence:** 85
**Reviewers:** ambiguity-reviewer
**Testable:** yes

**Where:** skills/pitch/SKILL.md:12
**What:** The description summarises the full workflow, inviting the executor to skip the body.
**Evidence:** needs-probe — the description lists steps 1–6; prose alone cannot show the
executor will shortcut.
**Impact:** The skill triggers, then the executor improvises the pipeline from the summary.
**Suggestion:** Describe outcome and triggers only; the step list stays in the body.
**Confidence:** 70
**Reviewers:** trigger-reviewer · economy-reviewer
**Testable:** yes
**Probe:** reproduced — the probe run followed the description's summary and skipped steps 4–6. (probe: .sprint/probes.md#sc-014-fail-shortcut)

Notes: 3 further findings recorded in .sprint/findings.md
```

What the example proves: the first block is a **static finding** — quoted prose is complete
evidence, so no `Probe` field exists; the second is **probe-evidenced** — its Evidence opens
with the `needs-probe — ` anchor exactly as the finder wrote it, the session appended a
`reproduced` Probe line carrying the persisted walk's anchor, and its `Reviewers` line shows
a merged double-report keeping both dimensions. `11 → 2` shows nine findings deduped, filtered, or bucketed to notes — three of
which the Notes line accounts for.

## Field → consumer index

| Field | Read by |
|---|---|
| Where | consolidation (dedup key) · fix loop (edit target) |
| What / Evidence | consolidation (grounding check, semantic dedup) · fix loop (defect context) |
| Impact | consolidation (bucket weighing) |
| Suggestion | fix loop (the edit's seed) |
| Confidence | consolidation (weighed against the reviewers' anchors) |
| Reviewers | fix loop (which dimension re-checks the fix) |
| Testable | fix loop (regression scenario owed) |
| Probe | consolidation (grounding for behavioural claims) · fix loop (regression scenario seed) |

## Not in this format

No severity labels, ever — importance is carried by Confidence and the session's bucket, not
by adjectives. No bucket verdicts inside blocks — publish/note/drop is the comment's
structure, not a field. No raw roster reports — finder blocks that don't publish or note are
not persisted anywhere. No process: when to probe, how to dedup, when to relabel, and what
the fix loop does next all live in the producing skill.
