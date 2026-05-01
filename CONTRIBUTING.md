# Contributing

This guide covers how to contribute plugins to the skills marketplace.

## Overview

This repository is a **plugin marketplace** — a monorepo of plugins that users install via `/plugin marketplace add jore731/skills`. Each plugin is a self-contained directory under `plugins/` with its own manifest, skills, and documentation.

## Plugin Structure

```
plugins/<plugin-name>/
├── .github/plugin/
│   └── plugin.json           # Plugin manifest (required)
├── README.md                 # Plugin documentation (required)
└── skills/                   # One or more skills
    └── <skill-name>/
        ├── SKILL.md           # Skill definition (required)
        ├── reference.md       # Supporting docs (optional)
        └── examples/          # Examples, configs, scripts (optional)
```

Plugins can also include agents (`agents/<name>.agent.md`), hooks (`hooks.json`), or MCP servers (`.mcp.json`), but skills are the most common component.

## Step-by-Step

### 1. Create the plugin directory

```bash
mkdir -p plugins/my-plugin/.github/plugin
mkdir -p plugins/my-plugin/skills/my-skill
```

### 2. Add `plugin.json`

Create `plugins/my-plugin/.github/plugin/plugin.json`:

```json
{
  "name": "my-plugin",
  "description": "What this plugin does and when to use it.",
  "version": "1.0.0",
  "author": { "name": "Your Name", "email": "you@example.com" },
  "keywords": ["relevant", "keywords"],
  "category": "developer-tools",
  "skills": ["./skills/my-skill"]
}
```

### 3. Write `SKILL.md`

Create `plugins/my-plugin/skills/my-skill/SKILL.md` with required YAML frontmatter:

```markdown
---
name: my-skill
description: What the skill does and when to use it. Write in third person.
---

# My Skill

Instructions for the agent go here.
```

#### Frontmatter fields

| Field           | Required | Description                                                                         |
| --------------- | -------- | ----------------------------------------------------------------------------------- |
| `name`          | Yes      | Lowercase letters, numbers, hyphens only. Max 64 characters.                        |
| `description`   | Yes      | What the skill does **and when to use it**. Max 1024 chars. Third person.           |
| `allowed-tools` | No       | Tools the agent can use without confirmation (e.g. `shell`). Use with caution.      |

### 4. Add a `README.md`

Create `plugins/my-plugin/README.md` listing the skills included and what the plugin provides.

### 5. Register in the marketplace

Add an entry to `.github/plugin/marketplace.json` in the `plugins` array:

```json
{
  "name": "my-plugin",
  "source": "my-plugin",
  "description": "What this plugin does.",
  "version": "1.0.0",
  "keywords": ["relevant", "keywords"],
  "category": "developer-tools"
}
```

The `source` path is relative to `pluginRoot` (which is `./plugins`).

### 6. Open a PR

Open a PR with a description of what the plugin provides.

## Writing Effective Skills

### How skills work

Only the `name` and `description` frontmatter is loaded at startup. The agent reads the full `SKILL.md` body only when it determines the skill is relevant. Supporting files are loaded on demand.

Skills trigger in two ways:

- **Automatically** — the agent matches the `description` against the current task.
- **Manually** — the user references the skill by name (e.g. `/my-skill do something`).

### Guidelines

- **Be concise** — the context window is shared. Only include information the agent doesn't already have.
- **Keep SKILL.md under 500 lines** — split into separate files and reference them for progressive disclosure.
- **Write descriptions in third person** — "Configures Devbox projects…" not "I can help you…". Include **what** it does and **when** to use it.
- **Name with kebab-case** — lowercase letters, numbers, hyphens. Be descriptive: `debugging-github-actions`, not `helper`.
- **Name files descriptively** — `form_validation_rules.md`, not `doc2.md`.
- **One level of references** — all referenced files should link directly from `SKILL.md`, not from each other.

### Progressive disclosure example

```markdown
---
name: my-tool
description: Configures and troubleshoots My Tool projects.
---

# My Tool

**Basic usage**: [instructions here]

For advanced features, see [advanced.md](advanced.md).
For full API reference, see [reference.md](reference.md).
```

### Skills vs custom instructions

Use **custom instructions** (`copilot-instructions.md`) for simple, always-relevant guidelines (coding standards, formatting).

Use **skills** for detailed, task-specific knowledge that should only load when relevant (deployment procedures, tool workflows).

## Compatibility

The `SKILL.md` format with YAML frontmatter works with both **GitHub Copilot CLI** and **Claude Code**. Plugins in this marketplace are agent-agnostic.
