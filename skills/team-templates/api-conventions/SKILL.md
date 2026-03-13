---
name: api-conventions
description: Team template that interviews your team about your API conventions, reads your existing codebase, and generates a custom team skill that enforces your specific conventions on every new endpoint. Run once per team to build your team's api-conventions skill.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: team-template
allowed-tools: Bash Read Write Glob
---

# API Conventions — Team Skill Builder

This is a **template builder**, not a skill itself. When you run it, it will interview you, read your codebase, and generate a custom `api-conventions` skill tailored to your team. That generated skill is what gets committed to your repo.

---

## Step 1 — Read the existing codebase

Before asking any questions, learn as much as possible from the code:

```bash
# Find existing API routes/endpoints
find . -type f \( -name "*.routes.*" -o -name "*.controller.*" -o -name "*.handler.*" \
  -o -name "router.*" -o -name "routes.*" \) | grep -v node_modules | grep -v ".git" | head -20

# Check for OpenAPI/Swagger spec
ls openapi.yaml openapi.json swagger.yaml swagger.json docs/api* 2>/dev/null

# Check auth middleware
find . -type f | xargs grep -l "middleware\|auth\|authenticate\|authorize" 2>/dev/null | grep -v node_modules | head -10

# Check error handling patterns
find . -type f | xargs grep -l "errorHandler\|AppError\|HttpException\|APIError" 2>/dev/null | grep -v node_modules | head -5
```

Read 3–5 existing endpoint files (routes + handlers) to understand current patterns before asking any questions.

## Step 2 — Interview the team

Based on what you found (and what's unclear or inconsistent), ask the following. Skip any question where the answer was already obvious from the code — don't ask about things you already know.

Work through these as a conversation, one topic at a time:

**URL Structure**
- What is the base URL prefix? (e.g., `/api`, `/api/v1`, `/v1`)
- How do you handle versioning — URL path (`/v1/`), header (`API-Version`), query param (`?version=1`), or no versioning?
- Naming convention for resource segments: plural nouns? (`/users`, `/orders`), singular? kebab-case?

**Authentication & Authorization**
- How is authentication handled — Bearer token? Session cookie? API key? OAuth?
- Where is the token passed — `Authorization` header? Custom header? Cookie?
- How are authorization errors returned — 401 vs 403 distinction?

**Request & Response Format**
- What is the standard success response shape? (e.g., bare object, `{ data: {...} }`, `{ result: {...} }`)
- What is the standard error response shape? (e.g., `{ error: { code, message } }`, `{ message, statusCode }`)
- How are validation errors returned — same error shape? flat list of field errors?
- Pagination format — cursor-based? offset/limit? what field names?

**HTTP Status Codes**
- What status code for a successful POST that creates a resource — 200 or 201?
- What status code for a successful DELETE — 200, 204, or something else?
- Any non-standard status code usage?

**Other**
- Is there a standard request ID / correlation ID header?
- Any required request headers beyond auth?
- Any global rate limiting behavior the endpoint author should know?

## Step 3 — Confirm the gathered conventions

Summarize everything you learned (from code + interview) and present it to the team for confirmation:

```
Here's what I'll encode into your api-conventions skill:

URL Structure: <summary>
Authentication: <summary>
Response format: <summary>
Error format: <summary>
Status codes: <summary>
[etc.]

Is this correct? Any corrections before I generate the skill?
```

Wait for confirmation or corrections before proceeding.

## Step 4 — Generate the custom team skill

Once confirmed, generate a `SKILL.md` file at `.claude/skills/api-conventions/SKILL.md` (and `.cursor/rules/api-conventions.md`) with the following structure:

```markdown
---
name: api-conventions
description: Enforces <team/project name>'s API conventions. Apply when writing, reviewing, or scaffolding any API endpoint.
metadata:
  generated-by: agents-skills/team-templates/api-conventions
  version: "1.0"
---

# API Conventions — <Team/Project Name>

When writing or reviewing any API endpoint in this project, enforce the following conventions.

## URL Structure
[Generated from interview]

## Authentication
[Generated from interview]

## Request Format
[Generated from interview]

## Response Format
[Generated from interview — include example JSON]

## Error Format
[Generated from interview — include example JSON for each error type]

## HTTP Status Codes
[Generated from interview]

## Checklist
Before submitting any new endpoint, verify:
- [ ] URL follows <base> + <versioning> + <resource> pattern
- [ ] Auth middleware is applied
- [ ] Input is validated before use
- [ ] Success response matches the standard shape
- [ ] Errors use the standard error shape
- [ ] Correct HTTP status codes used
[Add any additional checks based on team's answers]
```

## Step 5 — Wrap up

Tell the team:
1. Where the generated skill was saved
2. How to invoke it: `/api-conventions`
3. That they should commit it: `git add .claude/skills/api-conventions/ && git commit -m "chore: add api-conventions team skill"`
4. That they can update it any time by editing the generated `SKILL.md` directly or re-running this template
