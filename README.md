# skills

A personal plugin marketplace for [GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli) and [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Install curated plugins to extend your AI agent with domain-specific knowledge, commands, and skills.

## Quick Start

### 1. Register the marketplace

```shell
/plugin marketplace add jore731/skills
```

### 2. Browse available plugins

```shell
/plugin marketplace browse skills
```

### 3. Install a plugin

```shell
/plugin install <plugin-name>@skills
```

## Available Plugins

No plugins yet — see [CONTRIBUTING.md](./CONTRIBUTING.md) for how to add one.

## How It Works

This repository follows the [Copilot CLI plugin marketplace](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace) format:

- **`.github/plugin/marketplace.json`** — Marketplace manifest listing all available plugins
- **`plugins/<name>/`** — Each plugin directory contains a `plugin.json` manifest and its skills, agents, or other components

Plugins are compatible with both **GitHub Copilot CLI** and **Claude Code** — both support the same `SKILL.md` format with YAML frontmatter.

## Repository Structure

| Directory | Purpose |
|-----------|---------|
| [`.github/plugin/`](./.github/plugin/) | Marketplace manifest (`marketplace.json`) |
| [`plugins/`](./plugins/) | Plugin directories with manifests and skills |

## Managing Plugins

```shell
# List registered marketplaces
/plugin marketplace list

# List installed plugins
/plugin list

# Update a plugin
/plugin update <plugin-name>

# Uninstall a plugin
/plugin uninstall <plugin-name>

# Remove the marketplace
/plugin marketplace remove skills
```

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for the full guide on creating plugins and writing skills.

Quick steps:

1. Create a new directory under `plugins/` with a kebab-case name.
2. Add a `.github/plugin/plugin.json` manifest with at least `name` and `description`.
3. Add skills under `plugins/<name>/skills/<skill-name>/SKILL.md`.
4. Add a `README.md` to the plugin directory.
5. Register the plugin in `.github/plugin/marketplace.json`.
6. Open a PR with a description of what the plugin provides.

See the [Copilot CLI plugin reference](https://docs.github.com/en/copilot/reference/cli-plugin-reference) for the full `plugin.json` and `marketplace.json` schema.
