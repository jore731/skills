# GitLab Provider

Instructions for retrieving pending work from GitLab.

## Authentication

Follow the `login_method` from the config. Common patterns:
- GitLab MCP server authenticated via `GITLAB_TOKEN` env var
- `glab auth login` for the CLI
- Direct API token for REST calls

## Method 1: GitLab MCP Server (Preferred)

**Server:** [zereight/gitlab-mcp](https://github.com/zereight/gitlab-mcp)

### Merge Requests authored by me

Use the MCP tool to search for MRs where the current user is the author and state is `opened`.

### Merge Requests where I'm a reviewer

Use the MCP tool to search for MRs where the current user is listed as a reviewer.

### Issues assigned to me

Use the MCP tool to list issues assigned to the current user with state `opened`.

### Pipeline failures

Use the MCP tool to list pipelines with status `failed` for projects the user contributes to.

---

## Method 2: GitLab CLI (`glab`)

### MRs authored

```bash
glab mr list --author <account> --state opened
```

### MRs to review

```bash
glab mr list --reviewer <account> --state opened
```

### Issues assigned

```bash
glab issue list --assignee <account> --state opened
```

### Pipeline failures

```bash
glab ci list --status failed
```

> **Note:** `glab` operates on the current repo by default. Use `--repo` flag or iterate over repos for cross-project queries.

---

## Method 3: REST API (Last resort)

Base URL: `https://<host>/api/v4` (use `gitlab.com` if no custom host).

### MRs authored

```bash
curl -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  "https://<host>/api/v4/merge_requests?author_username=<account>&state=opened&scope=all"
```

### MRs to review

```bash
curl -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  "https://<host>/api/v4/merge_requests?reviewer_username=<account>&state=opened&scope=all"
```

### Issues assigned

```bash
curl -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  "https://<host>/api/v4/issues?assignee_username=<account>&state=opened&scope=all"
```

### Pipeline failures

```bash
curl -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  "https://<host>/api/v4/projects/<project_id>/pipelines?status=failed&per_page=5&order_by=updated_at"
```

---

## Data mapping

| Dashboard field | GitLab source |
|----------------|---------------|
| # | `iid` |
| Title | `title` |
| Repo | `project_id` → resolve via `/projects/:id` or `web_url` |
| Created | `created_at` (convert to relative time) |
| Status (MR) | `merge_status` + approvals count |
| Priority (Issue) | `labels` (look for priority labels) |
| Labels | `labels[]` |
| CI Error | `pipelines[].status` + stage/job name |

## GitLab-specific notes

- GitLab uses "Merge Requests" instead of "Pull Requests" — map both to the same dashboard section.
- The `scope=all` parameter is needed to search across all projects the user has access to.
- For self-hosted instances, replace `gitlab.com` with the configured `host` value.
