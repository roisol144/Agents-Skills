# Install: scaffold-tests

**Scope:** User — install once, available across all your projects.

---

## What this skill does

Reads your existing code, learns your project's test framework and style from existing tests, then generates comprehensive tests covering happy paths, edge cases, and error conditions — matching your project's patterns exactly.

**Invoke with:** `/scaffold-tests src/auth/login.ts`

---

## Prerequisites

- Your project should have at least one existing test file for the skill to use as a style reference.

---

## Install for Claude Code

Tell Claude Code:
> "Install the scaffold-tests skill from this directory into my user skills."

---

## Install for Cursor

Tell Cursor:
> "Install the scaffold-tests skill from this directory as a global rule."

---

## No customization needed

This skill detects your test framework and style at invocation time and adapts automatically.
