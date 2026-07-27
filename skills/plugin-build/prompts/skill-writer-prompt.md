# skill-writer-prompt

Assembled at Build 2.1.2, sent at Build 2.1.3 — one `skill-writer` dispatch inside the
workstream's single parallel batch. **Thin hand-over:** `skill-writer`
(`${CLAUDE_PLUGIN_ROOT}/agents/skill-writer.md`) is a craft-carrying full-contract agent — it
owns its method, self-checks, and report shape; this brief hands over only the per-dispatch
seed the session already holds. Everything inside the blockquote goes to the agent verbatim;
everything outside is session-facing fill guidance.

Fill sources (session-facing):

- `{{workstream_brief}}` ← the sprint file's workstream row — Delivers + Detail — plus any
  plan note naming this skill.
- `{{pillar_row}}` ← the TARGET plugin's `.standard/standard.md`, Pillars section, the Skills
  row only.
- `{{pressure_scenarios}}` ← the full `SC-###` entries the workstream names, quoted whole
  from the target's `.scenarios` ledger.
- `{{topology_slice}}` ← the target's `.steps` slice: phase position, artifact I/O, sibling
  commands.
- `{register}` and `{pointers}` are composed at dispatch — fill rules ride inside the braces.

> Dispatch: `skill-writer` — WS{n} of sprint v{N}, target plugin: {plugin directory name}.
>
> Write the skill package `skills/{skill-name}/` in the target plugin. Your agent file is
> your operating contract; this brief is the seed.
>
> **Workstream brief:**
> {{workstream_brief}}
> If this slot is empty the dispatch was mis-assembled — return NEEDS_CONTEXT; write nothing.
>
> **Skills pillar row — the target's law:**
> {{pillar_row}}
> If empty, the target's pillar table is not yet instantiated — return NEEDS_CONTEXT; write
> nothing.
>
> **Pressure scenarios — your spec, the RED half:**
> {{pressure_scenarios}}
> If empty, the skill has no spec — return NEEDS_CONTEXT; a skill workstream without a
> scenario is a plan gap only the session can route.
>
> **Topology slice:**
> {{topology_slice}}
> If empty, the target ships no sibling commands — the skill stands alone; take its phase
> position from the workstream brief.
>
> **Register:** {team-grade | participant-facing — copied from the plan; mandatory, no
> default}
>
> **Pointers:** {exact names + `${CLAUDE_PLUGIN_ROOT}` paths of the agents this skill
> dispatches and the formats it fills — or "none"}. Cite only what is listed here — a pointer
> you were not handed does not exist yet, and citing it fails the static gate.
>
> **Return** your report per your contract, opening with one status:
>
> - SUCCESS — files written; every scenario walk would-pass; self-check clean.
> - NEEDS_CONTEXT — a seed slot is empty or the brief is ambiguous; name the missing fact;
>   write nothing.
> - BLOCKED — a scenario cannot be drafted to pass, or conflicts with the pillar row; name
>   the conflict; write nothing.
>
> A walk you cannot bring to would-pass is BLOCKED, never SUCCESS-with-caveats — partial
> packages are never written.
>
> **Not this dispatch:** the prompts and formats your skill cites are sibling writers'
> deliverables in this same batch — reference them by name, never write them. The gates are
> the session's, after you. You never commit, never push — the session collects your report
> and authors the commit.

After the return, the session: collects the report at the 2.1.3 barrier → runs the Tier-1
static gate over the package (2.1.4) → dispatches fresh Tier-2 probes against the same
scenarios (2.1.5) → commits the workstream (2.1.6).
