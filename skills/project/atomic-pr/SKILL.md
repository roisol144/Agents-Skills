---
name: atomic-pr
description: Analyzes a large diff and decomposes it into a sequence of small, independently-reviewable pull requests with correct dependency ordering. Each resulting PR is focused, testable on its own, and easier to review. Invoke when your diff is too large or touches too many concerns at once.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: project
allowed-tools: Bash Read
---

# Atomic PR Decomposer

Break a large, multi-concern diff into a sequence of small, focused, independently-reviewable PRs.

## Step 1 — Assess the current diff

```bash
BASE=$(git remote show origin 2>/dev/null | grep 'HEAD branch' | awk '{print $NF}')
BASE=${BASE:-main}

# Summary stats
git diff --stat $(git merge-base HEAD $BASE)...HEAD

# Full file list
git diff --name-only $(git merge-base HEAD $BASE)...HEAD

# Line count
git diff $(git merge-base HEAD $BASE)...HEAD | wc -l
```

If the diff is under 300 lines and touches fewer than 5 files, tell the developer this PR is already a reasonable size and doesn't need decomposition.

## Step 2 — Understand what changed

Read the diff in full and identify distinct concerns. A "concern" is a self-contained change that:
- Has a clear, singular purpose
- Can be merged without the other concerns
- Passes tests on its own (or can be made to)
- Makes sense to a reviewer without context from the other changes

Common concern patterns:
- **Refactor then feature** — renaming/restructuring existing code, then adding new behavior on top
- **Dependency upgrade** — updating a library separately from the code changes that use new APIs
- **Schema + code** — DB migration separately from the application code that uses it
- **Infrastructure + feature** — config/setup changes separately from the feature using them
- **Bug fix + cleanup** — a targeted fix, then unrelated cleanup in the same area
- **Shared utility + consumers** — new shared function/component, then the callers

## Step 3 — Propose the decomposition

Present a decomposition plan listing each proposed PR in merge order:

```
## Proposed PR Decomposition

Current diff: +X -Y lines across N files

---

PR 1: <short title>
Purpose: <one sentence — what this PR does and why it stands alone>
Files: <list>
Depends on: base branch
Can be tested independently: yes / yes with stub / no (explain)

PR 2: <short title>
Purpose: <one sentence>
Files: <list>
Depends on: PR 1
Can be tested independently: yes / yes with stub / no (explain)

...

Estimated review complexity: <simpler than original / similar / note any trade-offs>
```

Ask for approval or adjustments before proceeding.

## Step 4 — Create the branches and commits

Once the developer approves the plan, for each PR in order:

1. Create a branch from the correct base:
```bash
# PR 1 branches from main
git checkout $BASE
git checkout -b <branch-name-1>

# PR 2 branches from PR 1
git checkout <branch-name-1>
git checkout -b <branch-name-2>
```

2. Cherry-pick or selectively stage the relevant files/hunks:
```bash
# Stage specific files
git checkout <source-branch> -- <file1> <file2>
git add -p  # for partial file staging
```

3. Commit with a clear message:
```bash
git commit -m "<type>(<scope>): <subject>"
```

4. Push the branch:
```bash
git push origin <branch-name>
```

5. Open a PR targeting the correct base (PR 2 targets PR 1's branch, not main):
```bash
gh pr create --base <base-branch> --head <branch-name> --title "<title>" --body "<description>"
```

## Step 5 — Summary

After all PRs are created, show:
- Links to all opened PRs in merge order
- Reminder that PRs 2+ should be rebased and retargeted to `main` after each predecessor merges
- Note: most CI systems run against the target branch, so PRs targeting intermediate branches may show additional diff — this is expected
