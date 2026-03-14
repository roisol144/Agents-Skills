---
name: review-pr
description: Performs a structured code review of a pull request. Checks correctness, security, test coverage, style consistency, and potential side effects. Produces clear, actionable reviewer comments. Invoke on any open PR.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: user
allowed-tools: Bash Read
---

# PR Reviewer

Perform a thorough, structured code review and produce actionable comments a developer can act on immediately.

## Step 1 — Get the PR details

```bash
gh pr view --json number,title,body,author,baseRefName,headRefName,additions,deletions,changedFiles
gh pr diff
gh pr checks
```

If no PR number is provided, use the current branch's open PR.

## Step 2 — Understand the intent

Read the PR title and description. Understand:
- What problem is this solving?
- What approach did the author take?
- Are there any areas the author flagged as uncertain?

If the description is missing or vague, note it — you'll flag this in the review.

## Step 3 — Read the diff carefully

Work through the diff file by file. For each changed file, read the full context (not just the changed lines) to understand how it fits into the surrounding code.

## Step 4 — Apply each review lens

### Correctness
- Does the code do what the PR description says it does?
- Are there logic errors, off-by-one errors, or incorrect conditionals?
- Are async operations handled correctly (missing await, unhandled rejections)?
- Are race conditions possible?
- Are there any null/undefined access issues?

### Security
- Is user input validated before use?
- Are there SQL injection, XSS, or command injection vectors?
- Are secrets or credentials hardcoded?
- Are authorization checks in place where needed?
- Are error messages leaking internal implementation details?

### Test coverage
- Are there tests for the changed behavior?
- Do the tests cover happy path, edge cases, and error cases?
- Are there obvious scenarios the tests miss?

### Performance
- Are there N+1 query patterns?
- Are expensive operations inside loops that could be hoisted?
- Are there missing indexes for new query patterns?
- Are large payloads being loaded into memory unnecessarily?

### Maintainability
- Is the code readable without comments?
- Are functions doing too many things?
- Is there duplicated logic that should be extracted?
- Are variable and function names clear?

### Consistency
- Does the code follow the patterns used elsewhere in the codebase?
- Are error handling patterns consistent with the rest of the project?
- Does naming follow project conventions?

## Step 5 — Produce the review

Structure your output as a review a human would be proud to leave. Use these severity levels:

- **[blocker]** — Must be fixed before merging. Correctness or security issues.
- **[suggestion]** — Should be fixed. Quality or maintainability issues worth addressing.
- **[nit]** — Minor style or naming preferences. Optional.
- **[question]** — Something unclear that needs clarification before you can assess it.
- **[praise]** — Call out something done particularly well.

Format each comment as:
```
**[severity] filename:line-range**
<Observation — what you see>
<Why it matters>
<Suggestion — what to do instead, with a code snippet if helpful>
```

End with an overall summary:
```
## Review Summary

**Verdict:** [Approve | Request Changes | Comment]

**Overview:** [2-3 sentences on the overall quality and main concerns]

**Blockers:** [count]
**Suggestions:** [count]
**Nits:** [count]
```

Keep the tone constructive. Assume good intent. Focus on the code, not the author.
