# thirdparty-skills

Manage third-party plugins in a skills marketplace — import from external repos, review for quality, and update from upstream with provenance tracking.

## What it does

This plugin provides a custom agent (`thirdparty-skills`) that handles three workflows:

### Import

Onboard a plugin from an external GitHub repository. The agent fetches upstream content at a specific commit, creates the plugin directory structure, registers it in `marketplace.json`, and commits with provenance trailers (`Upstream-Source`, `Upstream-Commit`).

```
@thirdparty-skills import write-a-skill from mattpocock/skills
```

### Review

Validate an imported plugin against quality guidelines using the `write-a-skill` skill. Improvements are committed separately from the import to preserve pristine upstream content in git history.

```
@thirdparty-skills review write-a-skill
```

### Update

Check upstream for changes and apply them. If local modifications exist, the agent performs a manual rebase — reading diffs, applying the upstream update, then re-applying local changes on top. Never uses `git rebase`.

```
@thirdparty-skills update write-a-skill
```

## Provenance tracking

Every third-party plugin's `plugin.json` includes an `upstream` object:

```json
{
  "upstream": {
    "repository": "owner/repo",
    "path": "path/to/plugin",
    "commit": "abc123..."
  }
}
```

## Commit discipline

- **Import/update commits** contain only upstream content + registration metadata
- **Local modifications** are always in separate commits
- Upstream commits use `Upstream-Source`, `Upstream-Commit` trailers
- Local modification commits use `Local-Modification: true` trailer
