# Install: db-migration-checker

**Scope:** Project — install per project. Works with any SQL database and ORM.

---

## What this skill does

Validates migration files changed in your branch against a checklist of production safety issues: destructive operations, missing rollbacks, unsafe index creation without CONCURRENTLY, missing transaction wrapping, and NOT NULL columns without defaults.

**Invoke with:** `/db-migration-checker`

---

## Prerequisites

- `git`
- Your project should have at least one existing migration file.

---

## Install for Claude Code

Tell Claude Code inside your project:
> "Install the db-migration-checker skill from this directory into this project."

---

## Install for Cursor

Tell Cursor inside your project:
> "Install the db-migration-checker skill from this directory into this project."

---

## Commit the skill to your repo

```bash
git add .claude/skills/db-migration-checker/SKILL.md
git commit -m "chore: add db-migration-checker agent skill"
```
