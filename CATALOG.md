# Skill Catalog

This file is the single source of truth for all available skills. Read this file first to find the right skill, then navigate to its directory for the full `SKILL.md` and `INSTALL.md`.

---

## User Skills

Install once globally (`~/.claude/skills/` or global Cursor rule). Works across all your projects with no configuration.

| Name | Invoke | Description | Path | Prerequisites |
|------|--------|-------------|------|---------------|
| pr-description | `/pr-description` | Reviews your PR diff and CI status, rewrites the description to be accurate and reviewer-friendly | [skills/user/git/pr-description](skills/user/git/pr-description/) | git, gh |
| commit-message | `/commit-message` | Writes a conventional commit message from your staged changes, matching your project's commit style | [skills/user/git/commit-message](skills/user/git/commit-message/) | git |
| safe-to-merge | `/safe-to-merge` | Pre-merge safety check: CI status, debug artifacts, potential secrets, unresolved TODOs, test coverage | [skills/user/code-review/safe-to-merge](skills/user/code-review/safe-to-merge/) | git, gh (optional) |

---

## Project Skills

Install per project (`.claude/skills/` or `.cursor/rules/`). The skill reads your codebase at invocation time and adapts to your stack — no upfront configuration needed.

| Name | Invoke | Description | Path | Prerequisites |
|------|--------|-------------|------|---------------|
| new-endpoint | `/new-endpoint <METHOD> <path> "<description>"` | Scaffolds a complete API endpoint — route, handler, validation, and tests — matching your project's conventions | [skills/project/backend/new-endpoint](skills/project/backend/new-endpoint/) | At least one existing endpoint in the project |
| new-component | `/new-component <ComponentName> "<description>"` | Scaffolds a UI component with styles, tests, and Storybook stories, matching your component patterns | [skills/project/frontend/new-component](skills/project/frontend/new-component/) | At least one existing component in the project |

---

## Team Templates

Run once per team. The agent interviews your team, reads your codebase, and generates a custom skill you own and commit to your repo.

| Name | Invoke | Description | Path | Output |
|------|--------|-------------|------|--------|
| api-conventions | `/api-conventions` | Interviews your team about API conventions (URLs, auth, response shapes, errors) and generates a custom enforcement skill | [skills/team-templates/api-conventions](skills/team-templates/api-conventions/) | Custom `api-conventions` SKILL.md in your project |

---

## How to install any skill

Tell your agent:
> "Look at the CATALOG.md in this repo, find the [skill-name] skill, and install it."

Your agent will read the skill's `INSTALL.md` and handle the rest.
