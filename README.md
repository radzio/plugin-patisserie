# Plugin Patisserie

Artisanal Claude Code plugins, freshly baked by Baker.

## Plugins

| Plugin | Platform | Command | Description |
|--------|----------|---------|-------------|
| [gitlab-review](https://github.com/radzio/gitlab-review) | GitLab | `/triage` | Triage unresolved MR review comments — analyze, plan, and reply |
| [github-review](https://github.com/radzio/github-review) | GitHub | `/triage` | Triage PR review comments — analyze, plan, and reply |

## Install

### Step 1: Add the marketplace

```bash
claude plugin marketplace add https://github.com/radzio/plugin-patisserie
```

### Step 2: Install plugins

```bash
claude plugin install gitlab-review
claude plugin install github-review
```

## Prerequisites

- [jq](https://jqlang.github.io/jq/) — `brew install jq`
- [glab](https://gitlab.com/gitlab-org/cli) — `brew install glab` (for GitLab plugin)
- [gh](https://cli.github.com/) — `brew install gh` (for GitHub plugin)
