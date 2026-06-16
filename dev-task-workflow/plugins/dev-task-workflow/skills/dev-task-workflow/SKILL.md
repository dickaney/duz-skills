---
name: dev-task-workflow
description: "Turn a non-trivial development task into a traceable workflow before treating it as done. Use whenever the work touches multiple files, has unclear or under-specified requirements, carries production/data/security risk, or spans several steps (feature work, refactors, bug investigations, config or schema changes, integration work). Make sure to reach for this even when the user just says 'add X' or 'fix Y' without naming a process — if the change is more than a one-line edit, this applies. Do NOT use it for trivial single-file edits (a typo, a rename, a one-liner) where ceremony would only slow things down."
---

# Dev Task Workflow

## Why this exists

Most AI-assisted development failures are not coding failures. They are bookkeeping failures: acting before reading the existing pattern, mixing a tentative guess into something that looks authoritative, skipping validation, or quietly breaking unrelated work. This skill keeps the work traceable so the result can be trusted and handed off.

## The one mental model: four layers

Keep these four things separate at all times. Collapsing them is the root cause of most mess.

1. **Request** — what the user wants *now*.
2. **Context** — source files, docs, tests, configs, prior decisions, constraints.
3. **Execution** — actions taken, decisions made, validation run.
4. **Reusable knowledge** — confirmed rules worth preserving (handled by the separate `knowledge-persistence` skill, not here).

Never let an assumption masquerade as a confirmed fact, and never let a work note masquerade as a specification.

## Workflow

### 1. Frame the task (briefly)

Before touching anything, get clear on:

- **Objective** — the concrete outcome requested.
- **Scope** — which files/systems/artifacts may change.
- **Done criteria** — what proves it is complete.
- **Risk** — unclear requirements, production impact, data loss, security, external dependencies, missing validation.

Ask the user *only* when a wrong assumption would be costly to reverse. Otherwise state your assumption in one line and proceed. Asking about everything is as much a failure as asking about nothing.

### 2. Read context before acting

Read the real source materials first. Prefer primary artifacts over memory:

- Existing code near where you'll work, plus its tests.
- READMEs, design notes, recent commit messages, issue/PR comments.
- Configs, schemas, migrations, CI files, deployment manifests.

Extract the *stable local pattern* and conform to it. Do not invent a new structure when the codebase already has one. If conventions conflict, follow the nearest/most recent precedent and note the choice.

### 3. Pick the smallest execution shape that fits

| Task type | Process |
|---|---|
| Direct answer | Answer with assumptions + source references; no ceremony |
| Code or config change | Inspect pattern → edit narrowly → run relevant validation → summarize files changed |
| Investigation | Collect evidence → separate fact from inference → list open questions |
| Multi-step / risky change | Share a short plan first, then execute and update it as steps complete |

For substantial work, state a short plan (a few concrete steps) before editing, and keep it current. Skip the plan for small, obvious changes.

### 4. Execute with provenance

While working:

- Follow the repo's existing conventions, helpers, and parsers — don't hand-roll what the project already provides.
- Keep edits scoped to the requested outcome. Preserve unrelated work and any user changes.
- Record non-obvious decisions and *why*, inline in your summary (not as permanent rules).
- Keep tentative ideas tentative — do not promote a guess to a rule because it happened to work once.

Validation is part of the work, not an optional afterthought.

### 5. Validate within the risk level

Run the most relevant checks the project actually supports. **Discover the real commands first** — read `package.json` scripts, `Makefile`, `pyproject.toml`, `justfile`, or CI config rather than assuming `npm test` exists. Then run what fits:

- Typecheck, linter, unit/integration tests, build, or a targeted command.
- Rendered/visual verification for UI, document, or chart output.
- Manual evidence review for investigations.

If you cannot run validation, say so explicitly and state the residual risk. Don't silently skip it.

### Claude Code repo realities

These constraints prevent the most common real-world damage:

- **Don't assume a command exists** — verify it (in package.json/Makefile/CI) before running it.
- **Don't commit, push, or open PRs unless the user asked.** Leave the working tree in a reviewable state and tell them what changed.
- **Don't touch access controls, secrets, or destructive operations** (force-push, history rewrite, dropping data) without explicit confirmation.
- **Prefer narrow diffs.** A reviewer should be able to read the change in one pass.

## Final response shape

End every task with a short handoff (shorter than the work itself):

1. **What was done** — the change, in plain terms.
2. **Where** — files touched.
3. **Validated** — what you ran and the result (or why you couldn't).
4. **Open items** — assumptions made, anything intentionally left for later.

If the task produced knowledge worth keeping across sessions — a confirmed rule, a reusable process, a decision others will depend on — flag that it's worth persisting deliberately (a dated work log, or a focused spec update) rather than leaving it buried in this conversation. Keep that note brief; don't improvise large log files inside this task unless the user asks.
