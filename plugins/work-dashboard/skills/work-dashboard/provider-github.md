# GitHub Provider

Instructions for retrieving pending work from GitHub.

## Authentication

Follow the `login_method` from the config. Common patterns:
- `gh auth switch --user <account>` — switch GitHub CLI identity
- MCP server already authenticated — no action needed

## Method 1: GitHub MCP Server (Preferred)

**Server:** [github/github-mcp-server](https://github.com/github/github-mcp-server)

### PRs authored by me (waiting for review)

Use `search_pull_requests`:
```
query: "author:<account> is:open"
```

### PRs where I'm requested as reviewer

Use `search_pull_requests`:
```
query: "review-requested:<account> is:open"
```

### Issues assigned to me

Use `search_issues`:
```
query: "assignee:<account> is:open"
```

### CI failures on my branches

For each open PR authored by me, use `pull_request_read` with `method: get_check_runs` to get CI status. Filter to failed check runs.

Alternatively, use `actions_list` with `method: list_workflow_runs` filtered by `status: completed` and check for failures on the user's branches.

---

## Method 2: GitHub CLI (`gh`)

### PRs authored

```bash
gh pr list --author <account> --state open --json number,title,repository,createdAt,reviewDecision
```

### PRs to review

```bash
gh pr list --search "review-requested:<account>" --state open --json number,title,repository,author,createdAt
```

### Issues assigned

```bash
gh search issues --assignee <account> --state open --json number,title,repository,labels,createdAt
```

### CI failures

```bash
gh run list --user <account> --status failure --limit 10 --json databaseId,headBranch,name,conclusion,createdAt,url
```

> **Note:** `gh` operates on one repo at a time for `pr list` / `run list`. For cross-repo queries, use `gh search` or iterate over known repos.

---

## Method 3: REST API (Last resort)

### Search PRs

```bash
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/search/issues?q=type:pr+author:<account>+state:open"
```

### Search issues

```bash
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/search/issues?q=type:issue+assignee:<account>+state:open"
```

### Check runs for a ref

```bash
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/<owner>/<repo>/commits/<ref>/check-runs"
```

---

## Data mapping

Map API/CLI output to dashboard columns:

| Dashboard field | GitHub source |
|----------------|---------------|
| # | `number` |
| Title | `title` |
| Repo | `repository.nameWithOwner` or `repository` |
| Created | `createdAt` (convert to relative time) |
| Status (PR) | `reviewDecision` + approval count |
| Priority (Issue) | Infer from labels (e.g., `priority/high`) |
| Labels | `labels[].name` |
| CI Error | `check_runs[].name` + `conclusion` |
