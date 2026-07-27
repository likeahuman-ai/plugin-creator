# prompt-writer-prompt

Filled by the session at 2.1.2, sent at 2.1.3 as one call in the workstream's parallel writer
batch — one fill per new prompt file the workstream delivers. Receiver: `prompt-writer`, whose
agent file at `${CLAUDE_PLUGIN_ROOT}/agents/prompt-writer.md` is craft-carrying — so this
brief is a **thin hand-over**: it hands over the new prompt's dispatch context and task
contract, never the method. Two layers, kept straight: this file's reader is the session; the
blockquote's reader is a prompt-writer dispatch; the product is a NEW prompt file in the
target plugin.

Session-facing fill rules:

- `{new-prompt path}` — `skills/<skill>/prompts/<x>-prompt.md` under the target plugin's
  source, named exactly as the citing skill step names it.
- `{dispatching step}` — the target-skill coordinate that sends the new prompt, plus the
  dispatch shape (solo · parallel batch · envelope) as that step states it. Hand the
  coordinate, not the step's text — the agent reads the step at its own discretion.
- `{receiver}` — EITHER the path to the receiving agent's file in the target plugin OR the
  literal statement `general subagent — no agent file`. Mandatory either-or: this value alone
  decides the new brief's entire size (agent file present → sized against it; absent → the
  complete brief). Never leave it inferable.
- `{task contract}` — what the new prompt's dispatch must achieve and return, from the
  workstream brief; one short paragraph.
- `{format pointers}` — the format names the new prompt's output must cite; empty = the
  dispatch fills no formatted artifact — write `none`.

> Write ONE dispatch-brief prompt for the target plugin: `{new-prompt path}`.
>
> Your operating contract is your agent file — read it first and follow it exactly, Process
> and Output included.
>
> Seed:
> - Dispatching step: `{dispatching step}` — read it and its neighbouring steps in the target
>   skill before drafting; your preamble's pipeline placement must match what the step
>   actually does.
> - Receiver: `{receiver}` — this fact alone sets your sizing verdict; resolve it per your
>   contract.
> - Task contract: {task contract}
> - Format pointers: {format pointers} — cite by name in the new prompt; never restate their
>   shapes.
>
> Return per your contract, closing with exactly one status: **SUCCESS** — file written,
> report complete · **NEEDS_CONTEXT** — a seed above is missing or ambiguous; name the gap,
> write nothing · **BLOCKED** — the dispatching step or receiver does not resolve in the
> target's tree; name what you looked for. A seed you would have to guess is NEEDS_CONTEXT,
> not a guess.
>
> You do not edit existing prompts — edits are session work. You do not write the receiving
> agent's file or any format it cites — those belong to sibling writers. You do not widen the
> task contract — a gap is NEEDS_CONTEXT, never improvisation.
>
> After you: the session collects your report, gates the workstream — static checks, then
> probes — and authors the commit. You never commit, never push.
