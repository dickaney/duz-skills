# Dev Task Workflow — Claude Code Skill

A Claude Code skill that turns a non-trivial development task into a **traceable workflow** before treating it as done: frame the scope, read the existing context first, pick the smallest execution shape that fits, execute with provenance, and validate before handing off.

It is designed to trigger on substantial work (multi-file changes, unclear requirements, production/data/security risk, multi-step tasks) and to stay out of the way for trivial one-line edits.

This repository is a self-contained **Claude Code plugin marketplace** holding a single plugin with this one skill.

## Install (Claude Code)

```shell
/plugin marketplace add dereck/dev-task-workflow
/plugin install dev-task-workflow@dev-task-workflow
```

> Replace `dereck` with your actual GitHub owner/org once you push this repo.

To test locally before pushing:

```shell
/plugin marketplace add ./dev-task-workflow
/plugin install dev-task-workflow@dev-task-workflow
/plugin validate ./dev-task-workflow
```

## Use

Once installed, the skill is model-invoked: Claude consults it automatically when you give it a substantial task such as "add a login endpoint", "refactor the payments module", or "track down why this test is flaky". You don't need a slash command.

## What it does

- **Four-layer separation** — keeps Request / Context / Execution / Reusable-knowledge apart so a guess never masquerades as a confirmed fact.
- **Smallest execution shape** — matches process weight to task type instead of over-ceremonializing simple changes.
- **Provenance + validation** — records non-obvious decisions and runs the project's real checks (discovered from `package.json` / `Makefile` / CI, not assumed).
- **Repo-safe defaults** — no commits/pushes/PRs unless asked, narrow diffs, no destructive ops without confirmation.

## Repository layout

```
dev-task-workflow/
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    └── dev-task-workflow/
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            └── dev-task-workflow/
                └── SKILL.md
```

## Versioning

The plugin version lives in `plugins/dev-task-workflow/.claude-plugin/plugin.json`. Bump it on every release so installed users receive the update. (The marketplace entry intentionally omits `version` so the manifest is the single source of truth.)

## License

Apache-2.0. See [LICENSE](./LICENSE).
