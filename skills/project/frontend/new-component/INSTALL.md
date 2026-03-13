# Install: new-component

**Scope:** Project — install per project. The skill discovers your framework and component patterns automatically at invocation time.

---

## What this skill does

Scaffolds a new UI component by reading your existing components to learn your conventions, then generating the component, styles, tests, and Storybook stories — all matching your project's patterns.

**Invoke with:** `/new-component UserNotificationCard "Card that displays a single user notification with read/unread state"`

---

## Prerequisites

- Your project should have at least one existing component for the skill to use as a reference pattern.

---

## Install for Claude Code

Tell Claude Code inside your project:
> "Install the new-component skill from this directory into this project."

---

## Install for Cursor

Tell Cursor inside your project:
> "Install the new-component skill from this directory into this project."

---

## Commit the skill to your repo

```bash
git add .claude/skills/new-component/SKILL.md
git commit -m "chore: add new-component agent skill"
```
