# format-writer-prompt

Filled by the session at Build 2.1.2, sent at 2.1.3 as one call in the workstream's parallel
writer batch. Receiver: `format-writer` — a craft-carrying agent file
(`${CLAUDE_PLUGIN_ROOT}/agents/format-writer.md` owns the method: anchors, labelled fields,
absence semantics, the worked example). Sizing: **thin hand-over** — this brief carries only
what the session holds; restating the agent's craft is the deviation.

Fill rules (session-facing): every value below comes from the TARGET plugin's own topology —
its `.steps` join and its skills' text — never from the loop's own formats; a target's format
serves the target's skills. Enumerate the consumers before dispatching — if the session
cannot name the readers, the dispatch is premature: a missing consumer list is a plan gap and
routes to `/plugin-plan`, not a blank slot.

> Write one artifact format for the target plugin:
> `{target plugin} — skills/{owning skill}/formats/{name}-format.md`.
>
> **Artifact:** {what this format shapes and where instances live}
> **Producer:** {the target-plugin step that fills it — exact coordinate, who writes}
> **Consumers:** {every reader — step coordinate + what it recovers there} — if this list is
> empty or partial, return NEEDS_CONTEXT with the question: a format without its readers is
> taste, not contract.
> **Trust class:** {authoritative — trust it, never recompute · or verify-on-contact}
> **Registry conventions in force:** {the target's ID schemes, trailers, cite forms this
> shape must honour} — if empty: no conventions bind; mint labels locally and state so in
> the format.
> **Exemplar:** {a house format to model, with its path} — if empty: no exemplar fits; work
> from your contract alone.
>
> Return per your file's Output contract, with one status: SUCCESS — file written, report
> complete · NEEDS_CONTEXT — a mandatory input above is missing or unresolvable; state the
> question · BLOCKED — the target path already exists or sits outside your remit. Edge
> ruling: an existing file at the target path is BLOCKED, never an edit invitation — edits
> are session work.
>
> Not yours: the producing skill's process (dispatch, gating, failure handling), sibling
> formats, the artifact instances themselves. You write the one format file and stop.
>
> After you: the session collects your report, runs the static gate over your file, and
> authors the workstream commit — you never commit, never push.
