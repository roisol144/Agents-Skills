---
name: pr-description
description: Reviews and improves your PR description by analyzing the actual diff and CI status. Rewrites it to be accurate, complete, and easy for reviewers to follow. Invoke before opening or updating a pull request.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: user
allowed-tools: Bash Read
---

# PR Description Reviewer

You are helping a developer write or improve a pull request description. The description must reflect the actual diff — not what the developer thinks they changed, but what they provably changed.

## Step 1 — Find the base branch

```bash
git remote show origin | grep 'HEAD branch' | awk '{print $NF}'
```

Store this as `<base>`. If the command fails, try `main` then `master`.

## Step 2 — Get the diff

```bash
git diff $(git merge-base HEAD <base>)...HEAD
git diff --name-only $(git merge-base HEAD <base>)...HEAD
git diff --stat $(git merge-base HEAD <base>)...HEAD
```

Read and internalize the changes. Pay attention to:
- What files changed and in which areas of the codebase
- What was added, removed, or refactored
- Any obvious behavioral changes (new routes, changed interfaces, config changes)

## Step 3 — Get the current PR description

If a PR exists for this branch:
```bash
gh pr view --json title,body,url,state
```

If no PR exists yet, note that one needs to be created.

## Step 4 — Check CI status

```bash
gh pr checks
```

If any checks are **failing**, stop and tell the developer which checks are failing before continuing. Do not write a description until the developer acknowledges the CI failures.

## Step 5 — Evaluate the current description

Compare the existing description against the diff. Flag any of these issues:
- Description is empty or only contains a template
- Description mentions changes not present in the diff
- Breaking changes are not called out
- No mention of how to test
- Written in past tense or vague ("made some changes", "fixed stuff")

## Step 6 — Write an improved description

Using the diff as the source of truth, write an improved description in this format:

```
## What changed
[1–2 paragraphs. What you did and why — written for someone with project context but who hasn't seen the code. Focus on intent and impact, not implementation details. Every sentence must be grounded in the diff.]

## How to test
[Concrete, numbered steps a reviewer can follow to verify the change works correctly. If automated tests cover this entirely, say so and point to the test files.]

## Notes
[Breaking changes, side effects, follow-up tickets, related PRs. Omit this section entirely if there's nothing to add.]
```

Rules:
- Keep "What changed" under 150 words
- If the change is trivial (typo fix, version bump, config tweak), a single sentence is fine — no need for all three sections
- Do not restate the PR title
- Do not describe code mechanics that are obvious from the diff

## Step 7 — Apply

Show the improved description and ask for approval. Once approved:

```bash
# If PR exists:
gh pr edit --title "<title>" --body "<body>"

# If no PR yet:
gh pr create --title "<title>" --body "<body>"
```
