# Consolidation — the session's grading guidance

Read at 3.2. This file owns the filter list, the anchors, and the bucket semantics; the order
of operations lives in the skill. You are weighing seven reviewers' evidence — the burden of
proof sits on the finding, and your judgement runs on the whole set at once, which is exactly
what no single reviewer could see.

## The grounding rule (3.2.1) — mechanical, no judgement

A finding survives only with BOTH: a `file:line` that resolves in the PR's tree, and quoted
text as evidence. Everything else is dropped and counted. Behavioural claims additionally
need probe evidence to publish (3.2.5) — a reviewer's `needs-probe` mark is a request, not a
pass.

## The false-positive filter (3.2.2)

Drop, and count, findings that are:

- **Pre-existing** — the defect was not introduced or touched by this sprint's diff; record
  genuinely important ones as a note instead, never as published.
- **Mechanically catchable** — anything the static gate checks (frontmatter shape, dangling
  references, orphans, name collisions); if the gate missed it, the finding is against the
  gate, not the prose — note it as such.
- **Taste** — style preferences with no consequence for the executing model; "I'd phrase it
  differently" without an Impact field that names a real misexecution.
- **Standard-contradicting** — findings that ask for what the target's own pillar rows or
  quality goals forbid; the standard wins, and a reviewer arguing with it is a note for the
  next plan (the criterion may be wrong — that route exists, but it is a plan decision).

## Dedup and the double-report rule (3.2.3)

Same `file:line` merges first; semantic duplicates by judgement. Boundary-designed overlaps
(the roster's `↔` pairs) arrive twice ON PURPOSE — merge into one finding that keeps both
lenses' Evidence and Impact; dropping one lens loses exactly the perspective the boundary was
designed to add.

## Calibration anchors (3.2.4) — honesty references, never thresholds

- **0–25** — likely taste; publishes only with corroboration from a second dimension.
- **50** — plausible; needs corroboration (a second reviewer, or probe evidence) to publish.
- **75** — a clear violation of the target's standard; publishes on its own evidence.
- **90+** — explicit law broken, or a probe-evidenced behavioural break; publishes, always.

Reviewer confidence is signal, never binding — an honest 60 from a reviewer whose evidence is
exact outweighs a padded 90 whose evidence is a paraphrase. Weigh the number against the
quoted evidence, not instead of it. Anchors never auto-bucket: a 75 with stale evidence still
drops, a corroborated 40 still publishes.

## Bucket semantics

- **publish** — must fix this sprint; enters the `### Plugin Review` comment and the 3.4 fix
  loop; only leaves the sprint via a fix or an explicit gate waiver.
- **note** — real but not this sprint's obligation; appended to the target's
  `.sprint/findings.md`, drained by the next `/plugin-plan`. Demoted behavioural claims
  (probe did not reproduce) land here with the probe result attached.
- **drop** — discarded; exists only as a count in the published totals. The count is the
  audit trail — never publish a drop, never hide one.
