# script-writer-prompt

Assembled at 2.1.2, sent at 2.1.3 as part of the workstream's single parallel batch — one
dispatch per executable the workstream delivers. Receiver: `script-writer`
(`${CLAUDE_PLUGIN_ROOT}/agents/script-writer.md`) — craft-carrying, so this is a **thin
hand-over**: the agent owns minimal-and-deterministic, language choice, interface discipline,
and the run-it-green proof; this brief hands over only what the session holds. Fill every
`{…}` seed before sending. The **invocation contract is mandatory** — a script nobody calls
is an orphan: if the plan names no citing step for this executable, do not dispatch — that is
a plan gap, route it to `/plugin-plan`.

> You are dispatched as script-writer for the `{target plugin}` build — workstream
> `{WS-n — the deliverable line from the sprint}`.
>
> **Write:** `{destination path — skills/<name>/scripts/<file> or scripts/<file>}`
>
> **Invocation contract** — the citing step is the other end of this contract; both ends
> must agree:
> - Caller: `{citing skill step — coordinate + file}`
> - Arguments: `{argument list — arity and meaning of each}`
> - Outputs: `{what stdout must carry per result — the exact machine-readable shape a
>   consumer parses}`
> - Exit codes: `{the codes the caller distinguishes and what each means to it}`
>
> **I/O shapes:** `{inputs — stdin or file paths and what they look like, including the
> edge the caller cares about (empty file, missing field, malformed line)}`
>
> **Language constraint:** {{language_constraint}}
> If the slot above is empty, the caller imposes none — choose per your own craft.
>
> **Pillar row (your law):** {{pillar_row}}
> If the slot above is empty, the target's `.standard` carries no row for executables yet —
> flag that in your report and proceed on your file's own craft.
>
> Return per your operating contract's Output section, closing with exactly one status:
> - `SUCCESS` — file written, all three proof runs green, both contract ends agree.
> - `NEEDS_CONTEXT` — the brief and the citing step disagree, or a named fact is missing;
>   state the question. A contract disagreement is NEEDS_CONTEXT, never a silent pick.
> - `BLOCKED` — the script cannot run green across the three proof cases after honest
>   attempts; a red proof run you cannot resolve is BLOCKED, not SUCCESS.
>
> You do NOT: write hook scripts (hooks-writer's domain) · edit any existing file, including
> the citing SKILL.md (session work) · commit, push, or touch git · ask a question whose
> answer is "yes, as written".
>
> After you return, the session:
> - verifies both ends of the invocation contract against the citing step,
> - marks the executable bit,
> - re-runs your worked proof case inside the Tier-1 gate,
> - authors the workstream commit — your job ends at the report.
