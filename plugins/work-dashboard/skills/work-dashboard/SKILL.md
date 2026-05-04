---
name: work-dashboard
description: |
  Aggregates and displays pending work across GitHub, GitLab, and Azure DevOps in a
  unified markdown dashboard. Surfaces PRs (authored, reviewing), issues assigned,
  CI/pipeline failures, and work items. Supports multiple accounts/sources and a
  repo/project blacklist. Use when the user asks about their pending work, open PRs,
  assigned issues, what needs attention, or wants a cross-platform work overview.
---

# Work Dashboard

Produce a **markdown dashboard** of all pending work across configured sources. The dashboard groups items by category (PRs, Issues, CI Failures, Work Items) and annotates each with source, repo, and status.

## Configuration

The skill relies on a YAML config file and a markdown template. Files are resolved in this order:

| Location | Purpose |
|----------|---------|
| `.work-dashboard/config.yaml` (repo root) | Per-repo config — gitignore this directory |
| `.work-dashboard/template.md` (repo root) | Per-repo template override |
| `~/.config/work-dashboard/config.yaml` | Global config (all projects) |
| `~/.config/work-dashboard/template.md` | Global template override |
| Plugin default: [`template.md`](./template.md) | Ships with the skill — used when no override exists |

**Resolution logic (repo config always wins):**
1. If `.work-dashboard/config.yaml` exists at the repo root, **use it exclusively** — do not merge with global.
2. Otherwise, use `~/.config/work-dashboard/config.yaml`.
3. For the template: `.work-dashboard/template.md` → `~/.config/work-dashboard/template.md` → plugin default [`template.md`](./template.md).

When running in a **personal knowledge repo** (secondbrain, notes, memory vault), the repo likely has no local config — the global config is used to show everything.
When running in a **project repo**, the local `.work-dashboard/` directory (gitignored) takes full precedence — it defines exactly which sources and blacklists apply to that project.

### Config file schema

```yaml
sources:
  - type: github           # github | gitlab | azure-devops
    account: jore731       # username, org, or group
    login_method: "use gh auth switch --user jore731"

  - type: github
    account: PulidoJ_basf
    login_method: "use gh auth switch --user PulidoJ_basf"

  - type: azure-devops
    org: basf-global
    project: MyProject     # optional — omit to query all projects in the org
    login_method: "az login with BASF tenant, use AZDO_TOKEN env var"

  - type: gitlab
    host: gitlab.example.com   # omit for gitlab.com
    account: jpulido
    login_method: "use gitlab mcp server authenticated via GITLAB_TOKEN"

blacklist:
  repos:
    - "jore731/archived-thing"
    - "basf-global/legacy-*"   # glob patterns supported
  projects:
    - "OldProject"
```

### First-run behavior

If no config file exists, guide the user through creating one:
1. Ask which platforms they use (GitHub, GitLab, Azure DevOps).
2. For each, ask for account/org names and how to authenticate (free-text `login_method`).
3. Ask if any repos or projects should be blacklisted.
4. Write the config to the appropriate location.

## Data retrieval

For each configured source, retrieve pending work using the best available method. See the provider-specific reference files for detailed instructions:

- [GitHub provider](./provider-github.md)
- [GitLab provider](./provider-gitlab.md)
- [Azure DevOps provider](./provider-azure-devops.md)

**Priority order for each provider:**
1. **MCP server** (if available and connected) — fastest, richest data
2. **CLI tool** (`gh`, `glab`, `az`) — reliable fallback
3. **REST API** via `curl` — last resort

## Dashboard output

The final output is produced by expanding the resolved template with the fetched data. See the default [`template.md`](./template.md) for the built-in format.

If a section's data list is empty, that section is omitted from the output.

## Workflow

1. **Load config** — use `.work-dashboard/config.yaml` if present (takes full priority); otherwise `~/.config/work-dashboard/config.yaml`.
2. **Load template** — resolve from `.work-dashboard/template.md` → `~/.config/work-dashboard/template.md` → plugin default.
3. **Authenticate** — follow `login_method` instructions for each source.
4. **Fetch data** — query each source using provider instructions (see reference files).
5. **Filter** — apply blacklist (repos and projects). Match glob patterns with `fnmatch` style.
6. **Render** — populate the template with fetched data using Jinja2-style substitution.
7. **Present** — output to the user. If in a vault/notes repo, optionally save as a dated note.

## Template syntax

The template uses **Jinja2-style** placeholders (`{{ var }}`, `{% if %}`, `{% for %}`). The agent should interpret the template and produce the final markdown — no rendering engine is needed; the agent expands it inline.

### Available variables

| Variable | Type | Description |
|----------|------|-------------|
| `generated_at` | string | ISO timestamp of generation |
| `pull_requests_authored` | list | PRs created by user, waiting for review |
| `pull_requests_reviewing` | list | PRs where user is requested reviewer |
| `issues` | list | Open issues assigned to user |
| `ci_failures` | list | Latest CI/pipeline failures |
| `work_items` | list | Azure DevOps work items |
| `errors` | list | Warnings/errors from failed sources |

Each list item is an object with fields matching the table columns in the default template.

### Customization

Users can modify the template to:
- Reorder or remove sections
- Add custom columns or formatting
- Change emoji/headers
- Add conditional logic (e.g., hide empty sections)

## Tips

- When the user says "what's on my plate?" or "pending work" or "show my dashboard", trigger this skill.
- If a source is unreachable or auth fails, show a warning in the dashboard but continue with other sources.
- Keep the dashboard concise — limit to 20 items per category by default, mention if more exist.
- For CI failures, only show the latest failure per repo/branch pair.
