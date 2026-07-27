# hooks-writer-prompt

Assembled at 2.1.2, dispatched at 2.1.3 inside the workstream's single parallel writer batch —
one dispatch per hook-set deliverable. The receiver is `hooks-writer`
(`${CLAUDE_PLUGIN_ROOT}/agents/hooks-writer.md`), a craft-carrying agent file — it owns the
event model, the header contract, the write-to-pass checklist, and the sample-run obligation —
so this brief is a **thin hand-over**: it carries only the dispatch facts the session holds
and the agent cannot know.

Fill notes (session-facing): `{event contract}` is fillable only when the plan carries the
observed failure mode per hook — what failed, where it was seen (transcript, finding id,
field report). A wish with no failure evidence is not a fillable brief; take it back to the
plan before dispatching. `{{existing_hooks_json}}` is the verbatim current file, or empty.
`{{environment_assumptions}}` is the Assumptions section of the target's
`.standard/standard.md`, verbatim.

> **Dispatch: hooks-writer — one hook set for {target plugin name, and its source root —
> `src/<plugin>`}.**
>
> Your agent file is your operating contract; this brief hands over the dispatch facts only.
> Sibling writers run in the same batch on disjoint paths — yours are the target's `hooks/`
> paths alone.
>
> **Event contract:** {per hook: the event(s) · the desired behaviour · block or warn · the
> observed failure mode it prevents, with provenance}
>
> **Existing hook config:**
> {{existing_hooks_json}}
> The block above is the target's current `hooks/hooks.json`, verbatim. Empty slot → no
> hooks.json exists; you create it complete. Present → that file is the session's to edit;
> your contract's entry-delivery rule applies.
>
> **Environment assumptions:**
> {{environment_assumptions}}
> The block above is the Assumptions slice of the target's standard. Empty slot → the target
> declares none; design for a stock Claude Code runtime.
>
> **Return exactly one status:**
> - `SUCCESS` — hook set written, every sample-run green, report complete per your contract
>   (including the verbatim hooks.json entry when the config pre-existed).
> - `NEEDS_CONTEXT` — a named fact is missing (an event's payload shape, the failure mode's
>   evidence, a coexisting script's behaviour): state the question and stop. A red sample-run
>   you cannot fix is `NEEDS_CONTEXT`, not `SUCCESS`.
> - `REFUSED` — the failure mode is stated but no mechanism is warranted (an instruction
>   suffices, or the check could never fire): say why and stop. Refusal is a verdict on the
>   plan, not a gap in the brief.
>
> **You do not:** draft the in-skill cleanup your header pairs with — name the pairing in
> your report and the citing skill's workstream carries it; touch the skill or steps text
> that will cite the hook; anything your contract's Never list already bans. A question whose
> answer is "yes, as written" is noise — proceed.
>
> **After you return:** the session gates your scripts (Tier-1, 2.1.4) · applies your
> verbatim hooks.json entry when the config pre-existed · commits the workstream (2.1.6) ·
> routes `REFUSED` back to the plan. You never commit, never push.
