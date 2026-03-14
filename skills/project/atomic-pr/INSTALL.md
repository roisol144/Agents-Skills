# Install: atomic-pr

**Scope:** Project — install per project.

---

## What this skill does

Analyzes a large diff, identifies distinct concerns (refactor vs feature, schema vs code, infra vs logic), and decomposes the work into a sequence of small focused PRs with correct dependency ordering — each independently reviewable and testable.

**Invoke with:** `/atomic-pr`

---

## Prerequisites

- `git`
- `gh` (GitHub CLI) — for opening PRs automatically. [Install guide](https://cli.github.com/)

---

## Install for Claude Code

Tell Claude Code inside your project:
> "Install the atomic-pr skill from this directory into this project."

---

## Install for Cursor

Tell Cursor inside your project:
> "Install the atomic-pr skill from this directory into this project."

---

## How it works

The skill analyzes your current branch diff against the base branch, proposes a decomposition plan, waits for your approval, then creates the branches, commits, and PRs automatically.

---

## Commit the skill to your repo

```bash
git add .claude/skills/atomic-pr/SKILL.md
git commit -m "chore: add atomic-pr agent skill"
```
