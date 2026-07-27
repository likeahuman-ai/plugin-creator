# Tier-2 probe — the protocol

Consumed at Build 2.1.5. A probe is a behavioural check of drafted prose against one pressure
scenario, run in-session by a dispatched subagent — no harness, no fixture install, no
statistics. It answers exactly one question: **would the scenario pass if the executing model
followed this text?**

## Dispatch

One probe per scenario, one scenario per probe. The probe receives exactly two things — the
drafted file(s), verbatim · the scenario (its Given/When/Then and observables) — and
deliberately nothing else: no plan context, no author intent, no prior verdicts. The probe
reads cold, because the executing model will. A re-probe after a fix gets a **fresh** probe —
a probe that has seen the previous draft reads with memory the executor won't have.

## The walk

Adopt the executor's stance: follow the text literally, top to bottom, granting no benefit of
the doubt and repairing no gaps from common sense. Where a step is ambiguous, take the
reading the scenario would fail under — the adversarial reading is the point; a charitable
probe proves nothing. At each Then-bullet, find the exact step or passage that satisfies it,
or declare the failure.

## Return contract

Per scenario, exactly one verdict:

- **would-pass** — each Then-bullet paired with the quoted step or passage that satisfies it.
- **would-fail** — the first failing Then-bullet · the quoted step, or the named absence,
  where the text lets it fail · the adversarial reading taken.

No fix suggestions — finding the failure is the whole job; a suggested fix anchors the fixer
and hides alternative readings. No confidence scores — a probe verdict is a walk result, not
an opinion.

## What the session does with the walk

Persist the probe's returned walk **verbatim** to the target's `.sprint/probes.md`, under the
anchor heading `## <SC-id> — <pass|fail> — <one-word subject>`, BEFORE acting on the verdict.
The walk on disk is the citable evidence — anything downstream that leans on the verdict cites
the anchor; the probe's message is transport, not a record. Git is the archive.

Anchor derivation is mechanical: lowercase the heading text, drop the `##` and the em-dash
separators, join the remaining tokens with single hyphens — `## SC-014 — fail — shortcut`
cites as `#sc-014-fail-shortcut`.
