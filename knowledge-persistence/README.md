# Knowledge Persistence — Claude Code Skill

A Claude Code skill for capturing the **durable knowledge** a task produced — work logs, specs/docs of record, and reusable skills — **without polluting the source of truth**. It keeps four kinds of writing in four kinds of place so each stays trustworthy.

It also **auto-detects the document language** and writes the persisted artifact in **Chinese (中文), Japanese (日本語), or English**, based on the user's explicit request, the existing docs, or the working language of the conversation.

This plugin is part of the **[duz-skills](https://github.com/dickaney/duz-skills) marketplace**.

## Install (Claude Code)

```shell
/plugin marketplace add https://github.com/dickaney/duz-skills.git
/plugin install knowledge-persistence@duz-skills
```

To test locally before pushing:

```shell
/plugin marketplace add ./
/plugin install knowledge-persistence@duz-skills
/plugin validate ./
```

## Use

Once installed, the skill is model-invoked: Claude consults it automatically when you ask it to "document this", "write it down", "log what we did", "update the spec", or "turn this into a skill" — or whenever a task generated rules, decisions, or a process future work will rely on.

## What it does

- **Four destinations** — Task log / Current spec / Reference / Skill, with the rule that a task log is *never* the source of truth when a spec exists.
- **Language auto-detection** — writes the artifact (prose **and** headings) in 中文 / 日本語 / English, with a localized heading map for the task-log template; code, paths, and commands stay verbatim.
- **Spec hygiene** — only confirmed, scoped, behavior-level content lands in a spec; unconfirmed items stay visibly flagged.
- **Skill extraction rules** — distills repeated workflows into well-scoped, portable skills, and hands off to `skill-creator` for the heavier authoring/eval/packaging loop.

## Repository layout

```
knowledge-persistence/
└── plugins/
    └── knowledge-persistence/
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            └── knowledge-persistence/
                └── SKILL.md
```

## Versioning

The plugin version lives in `plugins/knowledge-persistence/.claude-plugin/plugin.json`. Bump it on every release so installed users receive the update. (The marketplace entry in the root `marketplace.json` intentionally omits `version` so the plugin manifest is the single source of truth.)

## License

Apache-2.0. See [LICENSE](./LICENSE).
