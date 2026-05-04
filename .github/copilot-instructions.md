# Copilot Instructions

This repository is a **Copilot CLI plugin marketplace** — a monorepo of installable plugins that extend AI agents with domain-specific skills. It contains no application code, builds, or tests.

## Architecture

- **`.github/plugin/marketplace.json`** — Central manifest listing all plugins. `pluginRoot` is `./plugins`; each entry's `source` is a path relative to that root.
- **`plugins/<name>/`** — Self-contained plugin directories, each with:
  - `.github/plugin/plugin.json` — Plugin manifest (`name`, `description`, `version`, `author`, `keywords`, `category`, `skills` array)
  - `skills/<skill-name>/SKILL.md` — Skill definition with YAML frontmatter and markdown body
  - Optional: `README.md`, reference docs, examples, agents, hooks, MCP server configs

## Key Conventions

### Naming

- **Kebab-case everywhere** — plugin names, skill names, and file names use lowercase letters, numbers, and hyphens only.

### Adding a plugin

1. Create `plugins/<name>/.github/plugin/plugin.json` and `plugins/<name>/skills/<skill>/SKILL.md`.
2. Register the plugin in `.github/plugin/marketplace.json` under the `plugins` array.
3. Add a `README.md` to the plugin directory.

### Writing SKILL.md

- **Frontmatter is required**: `name` (kebab-case, max 64 chars) and `description` (max 1024 chars, third person, describes what it does **and when to use it**).
- **Keep under 500 lines** — split large content into separate reference files linked from SKILL.md.
- **One level of references** — referenced files link directly from SKILL.md, not from each other (progressive disclosure).
- Only `name` and `description` load at startup; the full body loads on demand when matched.

### marketplace.json entries

Each plugin entry needs `name`, `source` (relative to `pluginRoot`), `description`, `version`, `keywords`, and `category`. The `source` field maps to the directory under `plugins/`.

### Compatibility

The SKILL.md format with YAML frontmatter works with both **GitHub Copilot CLI** and **Claude Code**. Plugins should remain agent-agnostic.

## Third-party plugins

Some plugins are sourced from external repositories. When updating one, re-fetch all files from the upstream source and overwrite the local copy. The `plugin.json`, `marketplace.json` entry, and directory structure are maintained locally — only the skill content (SKILL.md, references, examples) comes from upstream.
