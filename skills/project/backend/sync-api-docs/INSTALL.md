# Install: sync-api-docs

**Scope:** Project — install per project. Requires an existing OpenAPI/Swagger spec file.

---

## What this skill does

Compares your route/controller code against your OpenAPI spec and flags drift: endpoints missing from the spec, stale entries for removed endpoints, and response schemas that no longer match. Offers to generate missing spec entries from the code.

**Invoke with:** `/sync-api-docs`

---

## Prerequisites

- An existing `openapi.yaml`, `openapi.json`, `swagger.yaml`, or equivalent spec file in your project.
- `git`

---

## Install for Claude Code

Tell Claude Code inside your project:
> "Install the sync-api-docs skill from this directory into this project."

---

## Install for Cursor

Tell Cursor inside your project:
> "Install the sync-api-docs skill from this directory into this project."

---

## Commit the skill to your repo

```bash
git add .claude/skills/sync-api-docs/SKILL.md
git commit -m "chore: add sync-api-docs agent skill"
```
