# Work Dashboard

A Copilot CLI / Claude Code skill that aggregates pending work across **GitHub**, **GitLab**, and **Azure DevOps** into a unified markdown dashboard.

## What it surfaces

- **Pull Requests / Merge Requests** — authored (waiting for review) and assigned as reviewer
- **Issues** — assigned to you, open
- **CI/Pipeline failures** — latest failures on your branches
- **Work Items** (Azure DevOps) — assigned, active

## Configuration

Create config and template files in one of these locations:

| Location | Scope |
|----------|-------|
| `.work-dashboard/config.yaml` + `template.md` (repo root, gitignored) | Per-repo |
| `~/.config/work-dashboard/config.yaml` + `template.md` | Global |

The plugin ships a default template; override it by placing your own `template.md` in the config directory.

```yaml
sources:
  - type: github
    account: myuser
    login_method: "use gh auth switch --user myuser"

  - type: gitlab
    host: gitlab.company.com
    account: myuser
    login_method: "use gitlab mcp server via GITLAB_TOKEN"

  - type: azure-devops
    org: my-org
    project: MyProject
    login_method: "az login, use AZDO_TOKEN env var"

blacklist:
  repos:
    - "myuser/archived-*"
    - "my-org/legacy-repo"
  projects:
    - "DeprecatedProject"
```

## Usage

Ask your AI agent:
- "Show my pending work"
- "What's on my plate?"
- "Work dashboard"
- "Any PRs waiting for me?"

## Provider priority

For each source, the skill tries (in order):
1. **MCP server** — fastest and richest
2. **CLI tool** (`gh`, `glab`, `az`) — reliable fallback
3. **REST API** — last resort
