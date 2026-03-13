---
name: safe-to-merge
description: Pre-merge safety check. Scans the diff for debug artifacts, potential secrets, unresolved TODOs, missing test coverage, and CI failures. Produces a clear merge readiness report. Invoke before merging any pull request.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: user
allowed-tools: Bash Grep Read
---

# Safe to Merge Checker

Run a structured pre-merge safety check and produce a merge readiness report.

## Step 1 — Establish the diff base

```bash
BASE=$(git remote show origin 2>/dev/null | grep 'HEAD branch' | awk '{print $NF}')
BASE=${BASE:-main}
MERGE_BASE=$(git merge-base HEAD $BASE)
echo "Base branch: $BASE | Merge base: $MERGE_BASE"
```

Get the changed files:
```bash
git diff --name-only $MERGE_BASE...HEAD
git diff --stat $MERGE_BASE...HEAD
```

## Step 2 — Run each check

### Check 1: Debug artifacts

Search for debug statements in added lines only:
```bash
git diff $MERGE_BASE...HEAD | grep '^+' | grep -v '^+++' | \
  grep -iE 'console\.(log|warn|error|debug|info)|debugger;|print\(|pprint\(|dd\(|dump\(|var_dump\(|binding\.pry|byebug|breakpoint\(\)'
```

Flag any matches with the file and line context.

### Check 2: Unresolved TODOs in new code

```bash
git diff $MERGE_BASE...HEAD | grep '^+' | grep -v '^+++' | \
  grep -iE '\b(TODO|FIXME|HACK|XXX)\b'
```

These may indicate work the developer intended to finish before merging.

### Check 3: Potential secrets

```bash
git diff $MERGE_BASE...HEAD | grep '^+' | grep -v '^+++' | \
  grep -iE '(api[_-]?key|secret|password|passwd|token|private[_-]?key|access[_-]?key|auth[_-]?token)\s*[:=]\s*["\x27`][^"\x27`]{8,}'
```

Also check for high-entropy patterns:
```bash
git diff $MERGE_BASE...HEAD | grep '^+' | grep -v '^+++' | \
  grep -E '(sk-[a-zA-Z0-9]{20,}|ghp_[a-zA-Z0-9]{36}|AKIA[A-Z0-9]{16}|xox[baprs]-[0-9a-zA-Z]{10,})'
```

Any match here is **high priority** — flag it prominently.

### Check 4: Test coverage

List changed source files:
```bash
git diff --name-only $MERGE_BASE...HEAD | grep -vE '\.(md|json|yaml|yml|lock|txt|env)$' | grep -vE 'test|spec|__tests__'
```

For each source file changed, check whether a corresponding test file was also modified:
- `src/auth/login.ts` → look for `src/auth/login.test.ts`, `tests/auth/login.test.ts`, `spec/auth/login_spec.rb`, etc.

Flag source files that have no corresponding test changes as a warning (not a blocker).

### Check 5: CI status

```bash
gh pr checks
```

If `gh` is not available or no PR exists, note it. CI failures are **blockers**.

## Step 3 — Produce the report

```
## Merge Readiness Report

Branch: <current-branch>
Base: <base-branch>
Files changed: <N> files, +<additions> -<deletions>

### CI
[✅ All checks passing]
[❌ Failing: <list failing check names>]

### Debug Artifacts
[✅ None found]
[⚠️  Found in: <file>:<line> — <matched text>]

### Unresolved TODOs
[✅ None in new code]
[⚠️  <file>:<line> — <TODO text>]

### Potential Secrets
[✅ None detected]
[🚨 REVIEW REQUIRED — <file>:<line> — <matched pattern>]

### Test Coverage
[✅ All changed source files have corresponding test changes]
[⚠️  No test changes for: <list of files>]

---
Verdict: [✅ Ready to merge | ⚠️ Review recommended | 🚨 Do not merge]
```

**Verdict rules:**
- `🚨 Do not merge` — any CI check failing, or any potential secret detected
- `⚠️ Review recommended` — debug artifacts, unresolved TODOs, or missing test coverage
- `✅ Ready to merge` — all checks pass and no warnings
