---
name: sync-api-docs
description: Detects drift between your codebase and OpenAPI/Swagger spec. Flags endpoints missing from the spec, stale entries for removed endpoints, and response schemas that no longer match the code. Invoke before merging any PR that touches API routes or response shapes.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: project
allowed-tools: Bash Read Glob
---

# API Docs Sync Checker

Detect and fix drift between your code and your OpenAPI/Swagger specification.

## Step 1 — Find the spec file

```bash
ls openapi.yaml openapi.json swagger.yaml swagger.json \
   docs/openapi.yaml docs/api.yaml docs/swagger.json \
   api/openapi.yaml src/openapi.yaml 2>/dev/null
```

If no spec file is found, tell the developer and stop — this skill requires an existing spec.

Read the spec file to understand its current state.

## Step 2 — Find changed routes in this branch

```bash
BASE=$(git remote show origin 2>/dev/null | grep 'HEAD branch' | awk '{print $NF}')
BASE=${BASE:-main}
git diff --name-only $(git merge-base HEAD $BASE)...HEAD
```

Among the changed files, identify:
- Route/controller files (likely have new, modified, or deleted endpoints)
- The spec file itself (if it was also updated)

Read the full route/controller files for any changed file that defines API endpoints.

## Step 3 — Extract routes from code

For each changed route file, identify all HTTP endpoints:
- Method (GET, POST, PUT, PATCH, DELETE)
- Path (including path parameters)
- Request body shape (if applicable)
- Response shape (success and error cases)
- Auth requirements

Use the framework's routing pattern:
- **Express**: `router.get('/path', handler)`, `app.post('/path', ...)`
- **FastAPI**: `@app.get('/path')`, `@router.post('/path')`
- **Rails**: `routes.rb` resource definitions
- **NestJS**: `@Get()`, `@Post()`, `@Controller()` decorators

## Step 4 — Compare code vs spec

**Check 1: Missing from spec**
For each endpoint found in code, check if it exists in the spec. Flag any that are absent.

**Check 2: Stale entries**
For each endpoint in the spec, check if it still exists in code. Flag any that appear to have been removed.

**Check 3: Path/method mismatch**
Check that the method and path in the spec match exactly what's in the code (watch for `/users/{id}` vs `/users/:id` normalization).

**Check 4: Response schema drift**
For endpoints that changed, compare the response shape in the spec against what the handler actually returns. Flag obvious mismatches (added/removed fields, changed types).

**Check 5: Missing request body schema**
For POST/PUT/PATCH endpoints, check that the spec includes a request body schema if the handler expects one.

## Step 5 — Produce the report and offer fixes

```
## API Docs Sync Report

Spec file: <path>
Endpoints in spec: <N>
Endpoints checked (changed in branch): <N>

### Missing from spec — Add these
[METHOD /path — <one-line description>]

### Stale entries — Remove or verify
[METHOD /path — appears removed from code]

### Schema drift — Review these
[METHOD /path — <what differs between code and spec>]

### In sync
[✅ N endpoints checked and matching]

---
Verdict: [✅ In sync | ⚠️ Drift detected — see above]
```

For any missing endpoints, offer to generate the OpenAPI entry based on the handler code. Show the proposed YAML/JSON and ask for confirmation before writing it to the spec file.
