# Duz Skills

Claude Code skill marketplace for reusable development workflows.

This repository is a single marketplace. The root `.claude-plugin/marketplace.json` registers all plugins. Each plugin lives in its own subdirectory with its plugin manifest, skill implementation, README, and license file.

## Skills

| Plugin | Purpose |
| --- | --- |
| [`dev-task-workflow`](./dev-task-workflow) | Turns a non-trivial development task into a traceable workflow: frame scope, read context first, pick the smallest execution shape, execute with provenance, and validate before handing off. |
| [`knowledge-persistence`](./knowledge-persistence) | Captures durable knowledge produced by a task, including task logs, specs, references, and reusable skills, without polluting the source of truth. |

## Install

**Remote (from GitHub):**

```shell
/plugin marketplace add https://github.com/dickaney/duz-skills.git
/plugin install dev-task-workflow@duz-skills
/plugin install knowledge-persistence@duz-skills
```

**Local:**

```shell
/plugin marketplace add ./
/plugin install dev-task-workflow@duz-skills
/plugin install knowledge-persistence@duz-skills
```

Validate before publishing or sharing:

```shell
/plugin validate ./
```

## Repository Layout

```text
.claude-plugin/
└── marketplace.json          ← single marketplace entry point

dev-task-workflow/
└── plugins/
    └── dev-task-workflow/
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            └── dev-task-workflow/
                └── SKILL.md

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

Each plugin owns its own version:

- `dev-task-workflow/plugins/dev-task-workflow/.claude-plugin/plugin.json`
- `knowledge-persistence/plugins/knowledge-persistence/.claude-plugin/plugin.json`

Bump the relevant plugin version when releasing that skill. The marketplace entries intentionally omit `version` so the plugin manifest remains the single source of truth.

## License

Apache-2.0. See [`LICENSE`](./LICENSE).
