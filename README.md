# Duz Skills

Claude Code skill marketplaces for reusable development workflows.

This repository currently contains two standalone marketplace directories. Each directory has its own `.claude-plugin/marketplace.json`, plugin manifest, skill implementation, README, and license file.

## Skills

| Marketplace | Skill | Purpose |
| --- | --- | --- |
| [`dev-task-workflow`](./dev-task-workflow) | `dev-task-workflow` | Turns a non-trivial development task into a traceable workflow: frame scope, read context first, pick the smallest execution shape, execute with provenance, and validate before handing off. |
| [`knowledge-persistence`](./knowledge-persistence) | `knowledge-persistence` | Captures durable knowledge produced by a task, including task logs, specs, references, and reusable skills, without polluting the source of truth. |

## Install Locally

Install each marketplace from its directory:

```shell
/plugin marketplace add ./dev-task-workflow
/plugin install dev-task-workflow@dev-task-workflow

/plugin marketplace add ./knowledge-persistence
/plugin install knowledge-persistence@knowledge-persistence
```

Validate the marketplaces before publishing or sharing:

```shell
/plugin validate ./dev-task-workflow
/plugin validate ./knowledge-persistence
```

## Publish Layout

The repository root is a collection of marketplaces, not a marketplace itself. Claude Code marketplace metadata lives inside each skill directory:

```text
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

knowledge-persistence/
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    └── knowledge-persistence/
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            └── knowledge-persistence/
                └── SKILL.md
```

If publishing these as remote marketplaces, publish or expose each marketplace directory in the same shape shown above, then update the install commands in the corresponding skill README:

- [`dev-task-workflow/README.md`](./dev-task-workflow/README.md)
- [`knowledge-persistence/README.md`](./knowledge-persistence/README.md)

## Versioning

Each plugin owns its own version:

- `dev-task-workflow/plugins/dev-task-workflow/.claude-plugin/plugin.json`
- `knowledge-persistence/plugins/knowledge-persistence/.claude-plugin/plugin.json`

Bump the relevant plugin version when releasing that skill. The marketplace entries intentionally omit `version` so the plugin manifest remains the single source of truth.

## License

Apache-2.0. See [`LICENSE`](./LICENSE).
