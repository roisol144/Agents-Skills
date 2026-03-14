---
name: release-notes
description: Generates clean, structured release notes from git history between two refs (tags, branches, or commits). Groups changes by type and filters noise. Invoke with: /release-notes <from> <to>
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: user
allowed-tools: Bash
---

# Release Notes Generator

Generate clean, human-readable release notes from git history between two refs.

**Arguments from invocation:**
- `$FROM` — starting ref (previous tag, commit, or branch). Defaults to the last tag if omitted.
- `$TO` — ending ref (new tag, branch, or commit). Defaults to `HEAD` if omitted.

## Step 1 — Resolve the refs

```bash
# If $FROM not provided, find the previous tag
git describe --tags --abbrev=0 HEAD^ 2>/dev/null || git rev-list --max-parents=0 HEAD

# List commits between the two refs
git log $FROM..$TO --oneline --no-merges

# Get full commit messages for context
git log $FROM..$TO --pretty=format:"%H %s%n%b" --no-merges
```

Also get the date range:
```bash
git log $FROM..$TO --pretty=format:"%ad" --date=short --no-merges | sort | head -1
git log $FROM..$TO --pretty=format:"%ad" --date=short --no-merges | sort | tail -1
```

## Step 2 — Check for existing release notes format

```bash
ls CHANGELOG.md CHANGELOG.rst HISTORY.md RELEASES.md 2>/dev/null | head -1
```

If a changelog exists, read the last 2 entries to match the existing format and style exactly.

## Step 3 — Categorize the commits

Group commits into these categories based on their messages and content:

| Category | Conventional commit types | What to include |
|----------|--------------------------|-----------------|
| **New Features** | `feat` | User-facing additions |
| **Bug Fixes** | `fix` | Resolved issues |
| **Performance** | `perf` | Speed or efficiency improvements |
| **Breaking Changes** | `BREAKING CHANGE` footer or `!` | Must always be called out |
| **Dependencies** | `chore(deps)`, `build` | Package updates |
| **Internal** | `refactor`, `test`, `ci`, `chore`, `style`, `docs` | Usually omitted or collapsed |

If the project doesn't use conventional commits, infer category from the commit message content.

**Filter out:**
- Merge commits
- Version bump commits (`bump version to x.y.z`, `release x.y.z`)
- Automated commits (Dependabot, bots)

## Step 4 — Write the release notes

```markdown
## [version] — YYYY-MM-DD

### Breaking Changes
- <description of breaking change and migration path>

### New Features
- <feature description — focus on what the user can now do, not implementation details>

### Bug Fixes
- <fix description — what was wrong and what is correct now>

### Performance
- <improvement description>

### Dependencies
- Updated <package> from x.y to x.z
```

Rules:
- Each bullet is one sentence, written for a developer consuming this release (not the author)
- Breaking changes section always comes first if present
- Omit empty sections entirely
- Omit the "Internal" / "Chore" section unless there's something a consumer genuinely needs to know
- If there are fewer than 5 commits total, skip categorization and use a flat list

## Step 5 — Output and optionally append

Show the release notes. If a `CHANGELOG.md` exists, ask whether to prepend the new entry to it.
