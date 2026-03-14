# Install: dependency-audit

**Scope:** User — install once, available across all your projects.

---

## What this skill does

Scans your project's dependencies for known vulnerabilities, severely outdated packages, deprecated libraries, and loose version ranges. Produces a prioritized report (critical → high → warnings) with exact commands to fix each issue.

**Invoke with:** `/dependency-audit`

---

## Prerequisites

- The skill uses your ecosystem's native audit tool automatically:
  - Node.js: `npm audit` / `yarn audit` / `pnpm audit` (built-in)
  - Python: `pip-audit` (`pip install pip-audit`) or `safety`
  - Ruby: `bundle-audit` (`gem install bundler-audit`)
  - Go: `govulncheck` (`go install golang.org/x/vuln/cmd/govulncheck@latest`)
  - Rust: `cargo audit` (`cargo install cargo-audit`)

The skill will tell you which tool to install if it's missing.

---

## Install for Claude Code

Tell Claude Code:
> "Install the dependency-audit skill from this directory into my user skills."

---

## Install for Cursor

Tell Cursor:
> "Install the dependency-audit skill from this directory as a global rule."

---

## No customization needed

This skill auto-detects your package ecosystem at invocation time.
