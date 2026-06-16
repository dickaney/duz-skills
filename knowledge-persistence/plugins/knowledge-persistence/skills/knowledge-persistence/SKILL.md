---
name: knowledge-persistence
description: "Capture the durable knowledge a task produced — without polluting the source of truth. Use when the work should be auditable later (write a task/work log), when confirmed behavior or rules need to land in a spec or doc of record, when a repeated process should be distilled into a reusable Claude Code skill, or when the user says things like 'document this', 'write it down', 'turn this into a skill', 'update the spec', or 'log what we did'. Make sure to reach for this whenever a task generated rules, decisions, or a process that future work (human or Claude) will need to rely on. Do NOT use it for the routine end-of-task summary of a small change — that belongs in the normal handoff, not a persisted artifact."
---

# Knowledge Persistence

## Why this exists

The fastest way to corrupt a project's knowledge is to write the wrong thing in the wrong place: a tentative guess into the spec, raw conversation history into a doc of record, or one project's specifics into a general skill. This skill keeps four kinds of writing in four kinds of place so each stays trustworthy.

## Language of the persisted document

Write the artifact in the language that fits its readers — **auto-detect it, don't ask unless genuinely ambiguous.** Supported: 中文, 日本語, English.

Detection order (first match wins):

1. **Explicit request** — the user says "写成日文" / "用中文记录" / "in English". Always overrides everything below.
2. **Existing docs of record** — if you are adding to or sitting alongside existing logs/specs, match their language and terminology.
3. **The user's working language** — the language the user has been writing in this conversation.
4. **Fallback** — English if still unclear.

Apply the chosen language to **both the prose and the structural headings.** Localize the template headings consistently using the mapping below. In mixed-language codebases, keep code identifiers, file paths, commands, and API names verbatim — only the surrounding prose follows the chosen language.

| English | 中文 | 日本語 |
|---|---|---|
| Work Log | 工作日志 | 作業ログ |
| Date | 日期 | 日付 |
| Objective | 目标 | 目的 |
| Source Context | 来源上下文 | 参照コンテキスト |
| Changes / Findings | 变更 / 发现 | 変更・所見 |
| Confirmed Rules | 已确认规则 | 確定した規則 |
| Validation | 验证 | 検証 |
| Open Items | 未决事项 | 未確定事項 |
| Related Files | 相关文件 | 関連ファイル |

## The four destinations

Decide *which* of these you are writing before you write it. They are not interchangeable.

| Destination | Holds | Trust level |
|---|---|---|
| **Task log** | What happened: files touched, decisions, validation, open items | Historical record — never the source of truth |
| **Current spec / doc of record** | Only *confirmed* behavior, rules, constraints others should rely on | Authoritative |
| **Reference** | Background: old docs, prompts, examples, prior workflow material | Supporting, not binding |
| **Skill** | Concise reusable instructions for future Claude Code use | Operational, repeated across tasks |

Core rule: **a task log is never the source of truth when a spec exists.** Only confirmed results flow from the log into the spec.

---

## Writing a task log

Use when the work should be auditable later (anything non-trivial, risky, or that others will build on). Keep it factual and dated. Render headings/content in the detected language; the canonical structure is:

```md
# {Task Name} Work Log

## Date
YYYY-MM-DD

## Objective
- What the task intended to achieve

## Source Context
- Documents, files, code paths, or sources checked

## Changes / Findings
- What changed or what was found
- Non-obvious decisions and their reasoning

## Confirmed Rules
- Rules or behavior that can now be treated as stable

## Validation
- Commands, render checks, review steps, evidence used — and results

## Open Items
- Unconfirmed assumptions
- Work intentionally deferred

## Related Files
- `path/to/file`
```

---

## Updating a current spec / doc of record

Add to a spec **only** when the content is:

- Implemented or otherwise verified (not hoped-for).
- Useful to a future reader.
- Expressed as behavior / constraint / decision — not as conversation history.
- Clear about its scope and exceptions.

Keep anything unconfirmed visibly flagged rather than silently promoted:

```md
- `{topic}` is unconfirmed. Current implementation assumes `{temporary rule}`
  until `{needed confirmation}` is available.
```

If you find yourself copying narrative ("then we tried X, it failed, so we…") into a spec, stop — that belongs in the task log. The spec gets the *conclusion*, in its own words.

---

## Extracting a reusable skill

Create or update a skill when a workflow is likely to **repeat across tasks**. A one-off does not justify a skill.

Extract only what a future Claude Code instance needs:

- **Trigger conditions** — when to use it (this lives in the `description` frontmatter; it is the primary triggering mechanism, so make it specific and slightly pushy to avoid undertriggering).
- **Required context** to inspect first.
- **Execution sequence** — the essential steps.
- **Validation requirements.**
- **Documentation / handoff rules.**
- **Pitfalls and decision boundaries.**

Design rules that keep skills useful:

- **Scope the trigger.** A skill that "triggers on everything" triggers on nothing useful. Describe the specific situation, not "any task."
- **General skill vs project skill.** Keep a general workflow skill abstract (no project paths). Put concrete paths, commands, schemas, and naming conventions in a *separate project skill or reference file* so the general one stays portable.
- **Use imperative instructions and explain *why*** a step matters, rather than stacking bare "MUST" statements — the reasoning is what makes the model apply it correctly to new cases.
- **Keep SKILL.md focused** (well under ~500 lines). Push long detail into reference files the skill points to, loaded only when needed.

If a richer authoring/testing loop is wanted (test prompts, evals, packaging into a `.skill`), hand off to the `skill-creator` skill rather than reinventing it here.

---

## Final response shape

State, briefly: which destination(s) you wrote to, in which language, where the artifacts live, and what (if anything) is still flagged as unconfirmed. The persisted artifact carries the detail — your reply just points to it.
