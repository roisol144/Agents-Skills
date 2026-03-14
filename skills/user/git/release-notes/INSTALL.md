# Install: release-notes

**Scope:** User — install once, available across all your projects.

---

## What this skill does

Generates clean, structured release notes from git history between two refs (tags, branches, or commits). Groups commits by type (features, fixes, breaking changes), filters out noise, and matches your existing changelog format if one exists.

**Invoke with:** `/release-notes v1.2.0 v1.3.0`

Or with defaults (last tag → HEAD): `/release-notes`

---

## Prerequisites

- `git`

---

## Install for Claude Code

Tell Claude Code:
> "Install the release-notes skill from this directory into my user skills."

---

## Install for Cursor

Tell Cursor:
> "Install the release-notes skill from this directory as a global rule."

---

## No customization needed

This skill detects your existing changelog format at invocation time and matches it automatically.
