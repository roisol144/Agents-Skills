---
name: scaffold-tests
description: Analyzes existing untested code and writes comprehensive unit and integration tests matching the project's test framework, style, and patterns. Invoke with a file path or function name to test.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: user
allowed-tools: Bash Read Glob
---

# Test Scaffolder

Write comprehensive tests for existing code by learning the project's test style first, then generating tests that fit naturally into the codebase.

**Arguments from invocation:**
- `$TARGET` — file path, function name, or module to write tests for

## Step 1 — Understand the test setup

Detect the test framework and runner:
```bash
cat package.json 2>/dev/null | grep -E '"jest"|"vitest"|"mocha"|"jasmine"|"@testing-library"'
cat pyproject.toml requirements.txt 2>/dev/null | grep -iE 'pytest|unittest'
cat Gemfile 2>/dev/null | grep -E 'rspec|minitest'
ls jest.config.* vitest.config.* pytest.ini setup.cfg .rspec 2>/dev/null
```

Find where tests live:
```bash
find . -type d -name "__tests__" -o -name "tests" -o -name "test" -o -name "spec" \
  | grep -v node_modules | grep -v .git | head -10
```

## Step 2 — Read an existing test file as reference

Find a test file close to the target code and read it fully. Note:
- Import style (named vs default, absolute vs relative paths)
- How `describe` / `it` / `test` blocks are structured
- How mocks and stubs are set up (jest.mock? vi.mock? pytest fixtures? factories?)
- Whether tests use `beforeEach` / `afterEach` setup
- Assertion style (`expect().toBe()`, `assert`, `should`, etc.)
- Whether snapshots are used (only replicate if already common in the project)

## Step 3 — Read the target code

Read `$TARGET` fully. Identify:
- Every exported function, class, or component
- All input parameters and their types
- Return values and side effects
- Error cases and edge conditions
- External dependencies that will need mocking (DB, API calls, file system, timers)

## Step 4 — Plan the test cases

Before writing, list the test cases you will cover:

For each function/method:
- **Happy path** — valid input, expected output
- **Edge cases** — empty arrays, zero, null/undefined, boundary values
- **Error cases** — invalid input, thrown exceptions, rejected promises
- **Side effects** — verify calls to mocked dependencies with correct arguments

For React/Vue components:
- Renders without crashing
- Renders correctly with required props
- Key user interactions (click, input, submit)
- Conditional rendering (loading, error, empty, populated states)

## Step 5 — Write the tests

Generate a test file following the exact style from Step 2. Place it where similar test files live (co-located or in `tests/` directory, matching existing convention).

Rules:
- Each test has one clear assertion as its primary purpose
- Test descriptions are plain English: "returns empty array when input is empty" not "test case 1"
- Mocks are set up at the right scope (per-test vs shared) — match existing patterns
- Do not test implementation details; test observable behavior
- Do not add snapshot tests unless the project already uses them heavily

## Step 6 — Show a summary

List the test file path and a brief breakdown of test cases written. Flag any areas where coverage is intentionally incomplete (e.g., requires infrastructure not worth mocking) so the developer can decide how to handle them.
