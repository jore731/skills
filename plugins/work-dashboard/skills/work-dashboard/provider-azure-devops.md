# Azure DevOps Provider

Instructions for retrieving pending work from Azure DevOps.

## Authentication

Follow the `login_method` from the config. Common patterns:
- Azure DevOps MCP server authenticated via PAT
- `az login` + `az devops configure --defaults organization=https://dev.azure.com/<org>`
- Direct PAT for REST calls

## Method 1: Azure DevOps MCP Server (Preferred)

**Server:** [microsoft/azure-devops-mcp](https://github.com/microsoft/azure-devops-mcp)

### Pull Requests authored by me

Use the MCP tool to query PRs created by the current user with status `active`.

### Pull Requests where I'm a reviewer

Use the MCP tool to query PRs where the current user is a required reviewer.

### Work Items assigned to me

Use the MCP tool to run a WIQL query:
```wiql
SELECT [System.Id], [System.Title], [System.State], [System.IterationPath]
FROM WorkItems
WHERE [System.AssignedTo] = @Me
  AND [System.State] <> 'Closed'
  AND [System.State] <> 'Done'
  AND [System.State] <> 'Removed'
ORDER BY [System.ChangedDate] DESC
```

### Pipeline failures

Use the MCP tool to list recent pipeline runs with result `failed`.

---

## Method 2: Azure CLI (`az`)

### PRs authored

```bash
az repos pr list --creator <account> --status active --org https://dev.azure.com/<org> --project <project> --output json
```

### PRs to review

```bash
az repos pr list --reviewer <account> --status active --org https://dev.azure.com/<org> --project <project> --output json
```

### Work Items assigned (via WIQL)

```bash
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [System.IterationPath], [System.WorkItemType] FROM WorkItems WHERE [System.AssignedTo] = @Me AND [System.State] <> 'Closed' AND [System.State] <> 'Done' AND [System.State] <> 'Removed' ORDER BY [System.ChangedDate] DESC" --org https://dev.azure.com/<org> --project <project> --output json
```

### Pipeline failures

```bash
az pipelines runs list --pipeline-ids <id> --result failed --top 10 --org https://dev.azure.com/<org> --project <project> --output json
```

> **Note:** `az devops` commands require `--org` and `--project` flags unless defaults are configured.

---

## Method 3: REST API (Last resort)

Base URL: `https://dev.azure.com/<org>/<project>/_apis`

### PRs authored

```bash
curl -u ":<PAT>" \
  "https://dev.azure.com/<org>/<project>/_apis/git/pullrequests?searchCriteria.creatorId=<userId>&searchCriteria.status=active&api-version=7.1"
```

### Work Items (WIQL)

```bash
curl -X POST -u ":<PAT>" \
  -H "Content-Type: application/json" \
  -d '{"query": "SELECT [System.Id] FROM WorkItems WHERE [System.AssignedTo] = @Me AND [System.State] <> '\''Closed'\''"}' \
  "https://dev.azure.com/<org>/<project>/_apis/wit/wiql?api-version=7.1"
```

Then fetch details:
```bash
curl -u ":<PAT>" \
  "https://dev.azure.com/<org>/_apis/wit/workitems?ids=<id1>,<id2>&fields=System.Id,System.Title,System.State,System.IterationPath,System.WorkItemType&api-version=7.1"
```

### Pipeline runs

```bash
curl -u ":<PAT>" \
  "https://dev.azure.com/<org>/<project>/_apis/pipelines/runs?result=failed&$top=10&api-version=7.1"
```

---

## Data mapping

| Dashboard field | Azure DevOps source |
|----------------|---------------------|
| ID | `pullRequestId` / `System.Id` |
| Title | `title` / `System.Title` |
| Project | `project.name` |
| Created | `creationDate` / `System.CreatedDate` |
| State (Work Item) | `System.State` |
| Iteration | `System.IterationPath` (last segment) |
| Type | `System.WorkItemType` (Bug, Task, User Story) |
| Pipeline error | `run.name` + `result` + `finishedDate` |

## Azure DevOps-specific notes

- Azure DevOps has "Projects" within an "Organization". The config should specify both `org` and optionally `project`.
- If `project` is omitted, iterate over all projects in the org (use `az devops project list`).
- Work items use WIQL (Work Item Query Language) — a SQL-like syntax specific to Azure DevOps.
- PRs in Azure DevOps can have "required reviewers" and "optional reviewers" — surface required ones in the dashboard.
