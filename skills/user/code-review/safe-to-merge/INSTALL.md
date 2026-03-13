# Install: safe-to-merge

**Scope:** User — install once, available across all your projects.

---

## What this skill does

Runs a structured pre-merge safety check and produces a merge readiness report covering CI status, debug artifacts, unresolved TODOs, potential secrets, and test coverage gaps.

**Invoke with:** `/safe-to-merge`

---

## Prerequisites

- `git`
- `gh` (GitHub CLI) — required for CI status checks. [Install guide](https://cli.github.com/)

The skill works without `gh` but will skip the CI check.

---

## Install for Claude Code

Tell Claude Code:
> "Install the safe-to-merge skill from this directory into my user skills."

---

## Install for Cursor

Tell Cursor:
> "Install the safe-to-merge skill from this directory as a global rule."

---

## No customization needed

This skill auto-detects your base branch at invocation time.
