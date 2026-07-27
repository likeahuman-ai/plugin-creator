# agent-writer-prompt

Dispatched at 2.1.3, assembled at 2.1.2 — one Agent call inside the workstream's single
parallel writer batch. The receiver, `agent-writer`
(`${CLAUDE_PLUGIN_ROOT}/agents/agent-writer.md`), is a craft-carrying full-contract agent —
register choice, frontmatter law, body order, boundaries discipline and self-checks all live
in its file — so this brief is a **thin hand-over**: it structures only what the session
holds at dispatch and restates none of the method.

Session-facing fill rules: compose every `{…}` seed field from the sprint's workstream and
the target's homes; drop `{{…}}` injections in verbatim. Send only the blockquote.

> Write one new agent file for the target plugin: {target plugin name, and the exact
> `agents/<name>.md` path the file lands on}.
>
> **Role spec** — your task contract:
> - Mandate: {one paragraph from the workstream deliverable — what the agent does, for whom,
>   on what material}
> - Model tier: {inherit | sonnet | opus} because {the stakes or volume that justify the
>   tier — this reason belongs in the file's opener, per your contract}
> - Posture: {read-only | writes new files | edits one named artifact} — tools follow from
>   this.
> - Place in the flow: {which skill step dispatches it, in what shape — solo, parallel
>   roster, once per sprint — and what downstream does with its output}
>
> **Siblings** — read them before writing anything; boundaries cannot be written blind:
> {paths of every agent file in the target's roster sharing a surface with the new role —
> when in doubt, list all of `agents/`}
> An empty list is legal — the new agent is the target's first; then no `↔` entries are
> possible and the file carries only its own scope guard.
>
> **The new agent's dispatching step:** {coordinate + one line — who will spawn it, with
> what brief}. "None yet — the dispatching skill lands later this sprint" is a legal value:
> note it in your report so the coupling is checked when that skill exists.
>
> **Agents pillar row — the law your file is judged by:**
> {{agents_pillar_row}}
> That is the target's own pillar row, verbatim. If the slot is empty, the target's
> `.standard` carries no Agents row yet — a plan-side instantiation gap: flag it in your
> report and hold the generic pillar function as interim law (one clear remit; works from
> its prompt and self-fetched context; returns evidence, owns no flow).
>
> **Trigger phrasings:** {quoted user utterances — only when the agent is user-facing}
> If empty, the agent is dispatch-only: no proactive example; description keyed to its
> dispatching skill.
>
> Return your report per your contract, carrying one status:
> - **SUCCESS** — file written, self-checks clean; open questions and owed `↔` sibling edits
>   may ride along. Owed edits are SUCCESS — naming them is part of the deliverable, never a
>   failure.
> - **NEEDS_CONTEXT** — the role spec or sibling list left a hole your contract forbids
>   guessing across; no file written; the questions are the report.
> - **BLOCKED** — the brief contradicts the pillar row or your contract and no answer could
>   reconcile them; no file written; quote both sides.
> A self-check you cannot pass is NEEDS_CONTEXT or BLOCKED — never SUCCESS with a caveat.
>
> You do NOT: edit sibling files (owed `↔` edits are named in your report; the session
> applies them) · write the prompt that will brief your new agent (that is prompt-writer's
> dispatch) · widen the mandate beyond the role spec — a gap is a question, not an
> improvisation.
>
> After you return: the session applies your owed `↔` edits, answers NEEDS_CONTEXT from the
> plan and re-dispatches, then gates your file and authors the commit — you never commit,
> never push.

Session post-return acts, one line each: apply owed `↔` sibling edits as session edits
before the 2.1.4 gate · answer NEEDS_CONTEXT from the sprint and re-dispatch — bounded, then
escalate to `/plugin-plan` · treat BLOCKED as a plan gap and route it, never paper over it.
