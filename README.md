# Plugin Patisserie

Artisanal Claude Code plugins, slow-proofed to perfection.

## Plugins

| Plugin | Platform | Command | Description |
|--------|----------|---------|-------------|
| [gitlab-review](https://github.com/radzio/gitlab-review) | GitLab | `/triage` | Triage unresolved MR review comments — analyze, plan, and reply |
| [github-review](https://github.com/radzio/github-review) | GitHub | `/triage` | Triage PR review comments — analyze, plan, and reply |
| [jira-levain](https://github.com/radzio/jira-levain) | Jira | `/levain` | Gather Jira ticket context and prepare an implementation plan |
| [deguster](https://github.com/radzio/deguster) | Mobile | `/deguster:test` | Mobile UI testing via Maestro CLI — E2E tests, parity checks, flow generation |

## Install

### Step 1: Add the marketplace

```bash
claude plugin marketplace add https://github.com/radzio/plugin-patisserie
```

### Step 2: Install plugins

```bash
claude plugin install gitlab-review
claude plugin install github-review
claude plugin install jira-levain
claude plugin install deguster
```

## Prerequisites

- [jq](https://jqlang.github.io/jq/) — `brew install jq`
- [glab](https://gitlab.com/gitlab-org/cli) — `brew install glab` (for GitLab plugin)
- [gh](https://cli.github.com/) — `brew install gh` (for GitHub plugin)
- Jira & Confluence MCP — `claude plugin add atlassian` or [mcp-atlassian](https://github.com/sooperset/mcp-atlassian) (for jira-levain)
- [Maestro CLI](https://maestro.mobile.dev/) — `curl -Ls 'https://get.maestro.mobile.dev' | bash` (for deguster)
- [mobilecli](https://www.npmjs.com/package/@mobilenext/mobilecli) — `npm install -g @mobilenext/mobilecli` (for deguster)
