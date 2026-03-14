---
name: dependency-audit
description: Scans your project's dependencies for outdated packages, known vulnerabilities, deprecated libraries, and license issues. Produces a prioritized report with recommended actions. Invoke any time to check dependency health.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: user
allowed-tools: Bash Read
---

# Dependency Auditor

Scan project dependencies for health issues and produce a prioritized action plan.

## Step 1 — Detect the package ecosystem

```bash
ls package.json requirements.txt Pipfile pyproject.toml Gemfile go.mod Cargo.toml pom.xml build.gradle 2>/dev/null
```

Handle each ecosystem found.

## Step 2 — Run ecosystem-specific audits

### Node.js / npm / yarn / pnpm
```bash
# Check for known vulnerabilities
npm audit --json 2>/dev/null || yarn audit --json 2>/dev/null || pnpm audit --json 2>/dev/null

# Check for outdated packages
npm outdated 2>/dev/null || yarn outdated 2>/dev/null

# Check for unused dependencies (if depcheck is available)
npx depcheck 2>/dev/null | head -30
```

### Python / pip
```bash
# Check for vulnerabilities
pip-audit 2>/dev/null || safety check 2>/dev/null

# Check for outdated packages
pip list --outdated 2>/dev/null
```

### Ruby / Bundler
```bash
bundle audit check --update 2>/dev/null
bundle outdated 2>/dev/null
```

### Go
```bash
go list -m -u all 2>/dev/null
govulncheck ./... 2>/dev/null
```

### Rust
```bash
cargo audit 2>/dev/null
cargo outdated 2>/dev/null
```

If the audit tool isn't installed, note which command to run to install it — don't fail silently.

## Step 3 — Read the package file for additional analysis

Read the primary package file (`package.json`, `requirements.txt`, etc.) and flag:
- Packages pinned to very old major versions (e.g., React 16 when React 19 is current)
- Packages with known deprecation notices (look for `deprecated` in npm output)
- Packages that are unmaintained (0 recent releases, archived repos — use judgment)
- Overly broad version ranges (`*`, `>=1.0.0`) that could pull in breaking changes

## Step 4 — Produce the report

```
## Dependency Audit Report

Ecosystem: <npm / pip / etc.>
Total dependencies: <N direct + N transitive>
Scan date: <today>

---

### Critical — Fix Before Next Deploy
[Packages with high/critical CVEs]
- **package-name** vX.Y.Z → Upgrade to vA.B.C
  CVE-XXXX-XXXX: <one-line description of vulnerability>
  Command: npm install package-name@A.B.C

### High — Fix Soon
[Medium severity CVEs, severely outdated major versions]

### Warnings — Address in Next Cleanup
[Low severity CVEs, deprecated packages, loose version ranges]
- **package-name**: deprecated — recommended replacement: <alternative>

### Informational
[Minor outdated packages, cosmetic issues]

---

**Summary:** X critical, X high, X warnings
**Recommended next step:** <specific command to run>
```

## Step 5 — Offer to fix

If there are straightforward upgrades (patch or minor version bumps with no breaking changes), ask if the developer wants to apply them:

```bash
# npm example:
npm update
npm audit fix
```

Always warn before applying major version upgrades — these may have breaking changes.
