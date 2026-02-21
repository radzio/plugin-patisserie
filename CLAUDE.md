# CLAUDE.md

## Project Overview

Plugin Patisserie is a **marketplace/registry for Claude Code plugins**. It acts as a centralized catalog that users can add to their Claude Code CLI, enabling them to discover and install plugins by name. The marketplace itself does not contain plugin code -- it points to external GitHub repositories where the actual plugins live.

The project is authored by `radzio` and currently hosts four plugins across two categories (productivity and testing).

## Project Structure

```
plugin-patisserie/
  .claude-plugin/
    marketplace.json   # The marketplace manifest -- the core of this repo
  README.md            # User-facing docs: plugin list, install steps, prerequisites
  CLAUDE.md            # This file -- project context for contributors and AI agents
```

### Key Files

- **`.claude-plugin/marketplace.json`** -- The marketplace manifest file. This is the single source of truth that Claude Code reads when a user adds this repo as a marketplace. It declares the marketplace name, owner, and an array of available plugins with their metadata and source locations.

- **`README.md`** -- Documents available plugins (as a table), installation instructions (two-step: add marketplace, then install plugins), and prerequisites (jq, glab/gh CLIs).

## How It Works

### Marketplace Mechanism

1. A user registers this repo as a marketplace source:
   ```bash
   claude plugin marketplace add https://github.com/radzio/plugin-patisserie
   ```
2. Claude Code reads `.claude-plugin/marketplace.json` from the repo.
3. The user can then install any plugin listed in the manifest by name:
   ```bash
   claude plugin install gitlab-review
   ```
4. Claude Code resolves the plugin's `source` field to fetch the actual plugin code from the referenced GitHub repository.

### Marketplace Manifest Schema

The manifest at `.claude-plugin/marketplace.json` follows the schema at `https://anthropic.com/claude-code/marketplace.schema.json` and contains:

| Field | Purpose |
|-------|---------|
| `name` | Marketplace identifier (`plugin-patisserie`) |
| `description` | Human-readable marketplace description |
| `owner.name` | Marketplace owner handle |
| `plugins[]` | Array of plugin entries |
| `plugins[].name` | Plugin install name (used in `claude plugin install <name>`) |
| `plugins[].description` | What the plugin does |
| `plugins[].source.source` | Source type (e.g., `"github"`) |
| `plugins[].source.repo` | Source repository in `owner/repo` format |
| `plugins[].version` | Plugin version string (e.g., `"v0.0.1"`) |
| `plugins[].category` | Plugin category (e.g., `"productivity"`, `"testing"`) |

### Currently Registered Plugins

| Name | Repo | Slash Command | Target Platform |
|------|------|---------------|-----------------|
| `gitlab-review` | `radzio/gitlab-review` | `/triage` | GitLab |
| `github-review` | `radzio/github-review` | `/triage` | GitHub |
| `jira-levain` | `radzio/jira-levain` | `/levain` | Jira / Confluence |
| `deguster` | `radzio/deguster` | `/deguster:*` | iOS / Android |

The review plugins (`gitlab-review`, `github-review`) expose a `/triage` command that analyzes unresolved review comments, plans responses, and helps reply. `jira-levain` exposes `/levain` to gather Jira ticket context from hierarchy and Confluence, then prepare an implementation plan. `deguster` exposes multiple slash commands (`/deguster:test`, `/deguster:gen`, etc.) for mobile UI testing via Maestro CLI and mobilecli.

## Development Conventions

- **Commit messages** are short, imperative-mood sentences (e.g., "Fix plugin source format to use GitHub object syntax", "Add README with plugin list and install instructions").
- **Branch strategy**: single `main` branch.
- **Plugin source format**: uses the object syntax `{ "source": "github", "repo": "owner/repo" }` rather than a flat string.
- **Plugin code lives externally** -- this repo is purely a registry/manifest. Do not add plugin implementation code here.
- **Naming**: plugin names are lowercase kebab-case (e.g., `gitlab-review`).

## How to Test / Run Locally

This repo has no build step, test suite, or runtime. It is a static registry. To validate changes:

1. **Verify JSON validity** of `.claude-plugin/marketplace.json`:
   ```bash
   jq . .claude-plugin/marketplace.json
   ```
2. **Test the marketplace end-to-end** by adding it locally:
   ```bash
   claude plugin marketplace add https://github.com/radzio/plugin-patisserie
   claude plugin install <plugin-name>
   ```
3. Ensure any new plugin entry has all required fields: `name`, `description`, `source` (with `source` and `repo`), and `category`.

## Notes for Contributors

- When adding a new plugin, update **both** `.claude-plugin/marketplace.json` (add the plugin entry) **and** `README.md` (add a row to the plugins table and any new prerequisites).
- The actual plugin code must live in its own separate GitHub repository -- only a reference goes here.
- Keep the `$schema` reference in the manifest so tooling can validate the structure.
- Plugin categories should match the Claude Code marketplace taxonomy (currently `"productivity"` and `"testing"` are the categories in use).
- Prerequisites for individual plugins (like `glab` or `gh`) should be documented in the README under the Prerequisites section.
