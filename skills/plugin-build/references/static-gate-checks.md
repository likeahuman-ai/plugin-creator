# Static gate — the Tier-1 check-set

Consumed at Build 2.1.4 (workstream exit, touched files) and 2.2.1 (phase close, whole
payload). Every check is a mechanically-decidable assertion — decidable with Read/Glob/Grep/
Bash and no model judgement. A check that needs judgement does not belong here — it belongs
to the review roster.

## File checks — every touched shipped file

| ID | Assertion |
|----|-----------|
| F1 | The file's final non-blank line does not end with a closing tool-envelope tag — `</content>` · `</invoke>` · `</parameter>` · `</function_results>` — whether the tag stands alone on the line or is appended to content; the four-tag list is exhaustive (paste debris from tool-mediated authoring; the tags in this row are mentions, and the check reads only the final non-blank line) |

## Skill checks — every touched `skills/<name>/SKILL.md`

| ID | Assertion |
|----|-----------|
| S1 | Frontmatter opens at line 1 with `---` and closes with `---` before the body |
| S2 | `name:` present, kebab-case (`^[a-z0-9]+(-[a-z0-9]+)*$`) |
| S3 | `description:` present, ≤ 1024 characters, containing a recognised WHEN phrase (`Use when` · `Use after` · `Use this skill when` · `Trigger when`) |
| S4 | `argument-hint:` present |
| S5 | Body ≤ 500 lines |
| S6 | Every file under the skill's `references/` is cited by name in the body — no orphans |
| S7 | Every `references/` path the body cites exists on disk |

## Agent checks — every touched `agents/<name>.md`

Two registers exist. **Full-contract** is the default. **Environment-shell** — discriminated
mechanically, both conditions required: the frontmatter declares an `isolation:` or `effort:`
key AND the body carries no `## ` section heading (the file configures an execution
environment; the task contract lives in the dispatch prompt). A body carrying any `## `
heading makes the file full-contract whatever its frontmatter declares. A5 and A7 do not
apply to the environment-shell register.

| ID | Assertion |
|----|-----------|
| A1 | Frontmatter opens and closes with `---` |
| A2 | `name:` 3–50 chars, kebab-case, not a bare generic term (`helper` · `assistant` · `agent` · `tool`) |
| A3 | `description:` present |
| A4 | `model:` present, one of `inherit` · `sonnet` · `opus` |
| A5 | `color:` present — full-contract register only |
| A6 | The body addresses the agent in the second person (`You `) |
| A7 | The last section heading is `## Output` — the file ends at its output contract; full-contract register only |

## Cross-file checks — the touched set together

| ID | Assertion |
|----|-----------|
| X1 | Every `<name>-prompt` / `<name>-format` a skill cites exists at the stated convention path |
| X2 | Every agent name a skill dispatches exists in `agents/` |
| X3 | No absolute paths (`/Users/` · `/home/` · `C:\`) and no `~/` paths in **use position** — a path the text instructs the executor to read, run, write, or cite. Mention passes: an occurrence is exempt when it sits inside backticks or quotes AND a ban/example marker appears on the same line, on the first line of the enclosing bullet, or in the governing section heading. Markers (case-insensitive): `never` · `ban` · `no ` · `example` · `negative` · `fail` · `rot` · `defect`. Negative examples, ban statements, and this check-set's own definition rows are mentions, not uses |
| X4 | Script and reference invocations from shipped files go through `${CLAUDE_PLUGIN_ROOT}` |
| X5 | No shipped file cites `research/` in use position — X3's use-vs-mention rule applies here too (this check-set's own rows, ban statements, and negative examples are mentions). The loop's operating artifacts are exempt: the five homes (`.standard` · `.steps` · `.rubric` · `.scenarios` · `.sprint`) and `todo.md`, which shipped skills legitimately read and write in target repos. The register-ID ban stands: no shipped file defers a definition to `Rule <n>` · `D-###` · `ADR-###` — needed content is stated inline (citing an ID as a pointer beside inline content passes; deferring to it does not). For `ADR-###` the discriminator is form, never record location: the commit-trailer form — an instruction to write an `ADR: ADR-###` trailer in the target repo — passes; every other use-position `ADR-###` deferral fails, whether or not the record can be found |

## Manifest check — always

| ID | Assertion |
|----|-----------|
| M1 | `.claude-plugin/plugin.json` parses (`jq empty` exits 0) and carries `name`, `version`, and `description` |

## Hook checks — when `hooks/` is present in the touched set

| ID | Assertion |
|----|-----------|
| H1 | `hooks/hooks.json` parses — `jq empty hooks/hooks.json` exits 0 |
| H2 | Every hook entry names an event and a command |
| H3 | Every wired script exists and is executable (`test -x`) |
| H4 | In every hook script, each `echo`/`printf` either redirects to stderr (`>&2`) or is the script's single documented stdout response — anything else fails (stdout purity: one stray line wedges the caller) |

## Script checks — every touched executable

| ID | Assertion |
|----|-----------|
| C1 | The header documents invocation (args, exit codes) and names the citing caller |
| C2 | The script exits 0 on the worked example its header documents |

## Running the gate

Evaluate every applicable check; collect ALL failures before reporting — a partial report
invites fix-one-rerun churn. Output shape, one line per failure, then the verdict line:

```
FAIL <ID> <file>:<line> <what failed, with the offending content>
GATE: <n> failures
```

or `GATE: green`.
