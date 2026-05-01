# Copilot Instructions

This repository is a **Copilot CLI plugin marketplace** — a monorepo of installable plugins that extend AI agents with domain-specific skills. It contains no application code, builds, or tests.

## Architecture

- **`.github/plugin/marketplace.json`** — Central manifest listing all plugins. The `pluginRoot` is `./plugins`, and each entry's `source` is the directory name under `plugins/`.
- **`plugins/<name>/`** — Self-contained plugin directories, each with:
  - `.github/plugin/plugin.json` — Plugin manifest (`name`, `description`, `version`, `author`, `keywords`, `category`, `skills` array)
  - `skills/<skill-name>/SKILL.md` — Skill definition with YAML frontmatter (`name`, `description`) and markdown body
  - Optional: `README.md`, reference docs, examples, agents, hooks, MCP server configs

## Key conventions

### Adding a plugin

1. Create `plugins/<name>/.github/plugin/plugin.json` and `plugins/<name>/skills/<skill>/SKILL.md`.
2. Register the plugin in `.github/plugin/marketplace.json` under the `plugins` array.
3. Use **kebab-case** for all plugin and skill names (lowercase, hyphens, no spaces).

### Writing SKILL.md

- **Frontmatter is required**: `name` (kebab-case, max 64 chars) and `description` (max 1024 chars, third person, describes what it does **and when to use it**).
- **Keep under 500 lines** — split large content into separate reference files linked from SKILL.md.
- **One level of references** — referenced files link directly from SKILL.md, not from each other (progressive disclosure).
- Only `name` and `description` are loaded at startup; the full body loads on demand when matched.

### plugin.json schema

```json
{
  "name": "my-plugin",
  "description": "What this plugin does and when to use it.",
  "version": "1.0.0",
  "author": { "name": "Name", "email": "email@example.com" },
  "keywords": ["relevant", "keywords"],
  "category": "developer-tools",
  "skills": ["./skills/my-skill"]
}
```

### marketplace.json entries

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

The `source` field is relative to `pluginRoot` (`./plugins`).

### Compatibility

The SKILL.md format with YAML frontmatter works with both **GitHub Copilot CLI** and **Claude Code**. Plugins should remain agent-agnostic.

## Third-party plugins

Some plugins may be sourced from external repositories. When asked to update one, re-fetch all files from the upstream source and overwrite the local copy. The plugin.json, marketplace.json entry, and directory structure are maintained locally — only the skill content (SKILL.md, references, examples, LICENSE) comes from upstream.
