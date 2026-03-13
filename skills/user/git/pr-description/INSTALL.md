# Install: pr-description

**Scope:** User — install once, available across all your projects.

---

## What this skill does

Reviews your PR diff and CI status, then rewrites the PR description to be accurate, complete, and easy for reviewers to follow. Invoke it before opening or updating any pull request.

**Invoke with:** `/pr-description`

---

## Prerequisites

- `git`
- `gh` (GitHub CLI) — [install guide](https://cli.github.com/) then run `gh auth login`

---

## Install for Claude Code

Tell Claude Code:
> "Install the pr-description skill from this directory into my user skills."

---

## Install for Cursor

Tell Cursor:
> "Install the pr-description skill from this directory as a global rule."

For project-level, add it to `.cursor/rules/pr-description.md` instead.

---

## No customization needed

This skill discovers your branch and base automatically at invocation time and works on any project.
