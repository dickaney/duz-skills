# Dev Task Workflow — Claude Code Skill

A Claude Code skill that turns a non-trivial development task into a **traceable workflow** before treating it as done: frame the scope, read the existing context first, pick the smallest execution shape that fits, execute with provenance, and validate before handing off.

It is designed to trigger on substantial work (multi-file changes, unclear requirements, production/data/security risk, multi-step tasks) and to stay out of the way for trivial one-line edits.

This plugin is part of the **[duz-skills](https://github.com/dickaney/duz-skills) marketplace**.

## Install (Claude Code)

```shell
/plugin marketplace add https://github.com/dickaney/duz-skills.git
/plugin install dev-task-workflow@duz-skills
```

To test locally before pushing:

```shell
/plugin marketplace add ./
/plugin install dev-task-workflow@duz-skills
/plugin validate ./
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
└── plugins/
    └── dev-task-workflow/
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            └── dev-task-workflow/
                └── SKILL.md
```

## Versioning

The plugin version lives in `plugins/dev-task-workflow/.claude-plugin/plugin.json`. Bump it on every release so installed users receive the update. (The marketplace entry in the root `marketplace.json` intentionally omits `version` so the plugin manifest is the single source of truth.)

## License

Apache-2.0. See [LICENSE](./LICENSE).
