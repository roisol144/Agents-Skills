---
name: new-endpoint
description: Scaffolds a complete new API endpoint — route, handler, validation, and tests — following this project's existing conventions. Invoke with: /new-endpoint <METHOD> <path> "<description>"
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: project
allowed-tools: Bash Read Write Glob
---

# New Endpoint Scaffolder

Scaffold a new API endpoint that is consistent with this project's existing patterns.

**Arguments from invocation:**
- `$METHOD` — HTTP method (GET, POST, PUT, PATCH, DELETE)
- `$PATH` — Route path (e.g., `/users/:id/notifications`)
- `$DESCRIPTION` — What this endpoint does

## Step 1 — Detect the framework and project structure

Run the following to identify the stack:

```bash
# Node.js / TypeScript
cat package.json 2>/dev/null | grep -E '"express"|"fastify"|"koa"|"hapi"|"nestjs"'

# Python
cat requirements.txt pyproject.toml 2>/dev/null | grep -iE 'fastapi|django|flask'

# Ruby
cat Gemfile 2>/dev/null | grep -E 'rails|sinatra'

# Go
ls go.mod 2>/dev/null && cat go.mod | grep -E 'gin|echo|fiber|chi'
```

Then find where routes and handlers live:
```bash
find . -type f -name "*.routes.*" -o -name "*.controller.*" -o -name "*.handler.*" \
  -o -name "router.*" -o -name "routes.*" | grep -v node_modules | grep -v ".git" | head -20
```

## Step 2 — Read 1–2 existing endpoints as reference

Find the closest existing endpoint to use as a template:
- Pick one that is similar in method or resource type
- Read the route definition file
- Read the handler/controller file
- Read the corresponding test file

Understand the project's patterns for:
- How routes are registered
- How handlers are structured (async/await? error handling middleware? response helpers?)
- How input is validated (Zod, Joi, express-validator, Pydantic, etc.)
- How errors are returned (what shape, what status codes)
- Where types/interfaces are defined

## Step 3 — Check for an API spec

```bash
ls openapi.yaml openapi.json swagger.yaml swagger.json docs/api* 2>/dev/null
```

If found, read the existing spec to understand response schemas and conventions.

## Step 4 — Scaffold the files

Generate the following, strictly following the patterns from Step 2:

1. **Route definition** — Add the new route in the correct router file
2. **Handler/Controller** — New handler file with:
   - Input parsing from `req.params`, `req.query`, `req.body`
   - Input validation
   - Business logic placeholder (with a clear comment)
   - Proper response and error handling
3. **Types/Interfaces** — If the project uses TypeScript or typed Python, add types for request/response
4. **Test file** — Create a test covering:
   - Happy path (valid input, expected response)
   - Validation errors (missing/invalid fields)
   - At least one error case (not found, unauthorized, etc.)
5. **OpenAPI spec** — If a spec exists, add the endpoint definition

## Step 5 — Show a summary

List every file created or modified. For each decision you made (naming convention, file location, validation approach, error format), call it out explicitly so the developer can correct anything that doesn't match their intent.

If you made assumptions due to insufficient existing examples, flag them clearly.
