# Install: new-endpoint

**Scope:** Project — install per project. The skill discovers your stack and conventions automatically at invocation time.

---

## What this skill does

Scaffolds a complete new API endpoint by reading your existing codebase to learn your conventions, then generating a route, handler, validation, tests, and OpenAPI spec entry — all matching your project's patterns.

**Invoke with:** `/new-endpoint POST /users/:id/notifications "Send a notification to a user"`

---

## Prerequisites

- Your project should have at least one existing endpoint for the skill to use as a reference pattern.

---

## Install for Claude Code

Tell Claude Code inside your project:
> "Install the new-endpoint skill from this directory into this project."

---

## Install for Cursor

Tell Cursor inside your project:
> "Install the new-endpoint skill from this directory into this project."

---

## How it adapts to your project

No upfront configuration needed. Every time you invoke it, the skill detects your framework, reads existing endpoints to learn your naming and structure, then scaffolds new files that match exactly what it found.

---

## Commit the skill to your repo

Once installed, commit it so your whole team benefits:
```bash
git add .claude/skills/new-endpoint/SKILL.md
git commit -m "chore: add new-endpoint agent skill"
```
