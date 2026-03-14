---
name: code-style-guide
description: Team template that reads your existing codebase to infer your coding style, then interviews your team about naming, formatting, patterns, and conventions to fill in what it can't infer — and generates a custom code-style enforcement skill your whole team can use.
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: team-template
allowed-tools: Bash Read Write Glob
---

# Code Style Guide — Team Skill Builder

This is a **template builder**, not a skill itself. When you run it, it reads your codebase, interviews your team, and generates a custom `code-style` skill tailored to your conventions. That generated skill is what gets committed to your repo.

---

## Step 1 — Read the codebase to infer existing style

Before asking any questions, extract as much as possible from the code itself.

### Detect language and tooling
```bash
# JS/TS tooling
ls .eslintrc* .eslintignore eslint.config.* .prettierrc* prettier.config.* \
   .editorconfig tsconfig.json biome.json 2>/dev/null

# Python tooling
ls .flake8 pyproject.toml setup.cfg .pylintrc ruff.toml 2>/dev/null

# Ruby
ls .rubocop.yml .rubocop_todo.yml 2>/dev/null

# Go
ls .golangci.yml .golangci.yaml 2>/dev/null
```

If a linter/formatter config exists, read it — this tells you most of what you need about formatting preferences.

### Read 5–10 representative source files

Pick files from different parts of the codebase (not tests, not generated). Look for consistent patterns in:

**Naming:**
- Variables: camelCase, snake_case, SCREAMING_SNAKE?
- Functions/methods: verb-first? noun-first? abbreviated or full words?
- Files: kebab-case, camelCase, PascalCase, snake_case?
- Booleans: `is`/`has`/`can` prefix convention?
- Constants: ALL_CAPS or not?

**Structure:**
- Imports: grouped (external, internal, relative)? sorted? barrel files?
- Functions: arrow functions or `function` keyword in JS?
- Classes vs. functions: does the codebase prefer OOP or functional style?
- File length: are large files common, or is everything small and split?
- Folder structure: by layer (controllers/services/models) or by feature?

**Patterns:**
- Error handling: try/catch, Result types, callbacks, error-first?
- Async: promises, async/await, or callbacks?
- Comments: rare and meaningful, or frequent? JSDoc everywhere?
- Guard clauses vs. nested conditionals?
- Magic numbers: extracted to named constants?

## Step 2 — Interview the team

Based on what you found, ask only about things that were **ambiguous or inconsistent** in the codebase. Skip anything already obvious. Work through one topic at a time.

**If not already clear from the code:**

- "I see a mix of arrow functions and `function` declarations. Do you have a preference, or is it context-dependent?"
- "Variable naming looks mostly camelCase — is that intentional across the board, or are there exceptions?"
- "I noticed some files use JSDoc and others don't. Is there a rule for when to document?"
- "How do you prefer to handle errors in async code — try/catch everywhere, or only at boundaries?"
- "Are there any rules about maximum function length or file size?"
- "Any naming prefixes or suffixes that mean something specific? (e.g., `*Service`, `use*` hooks, `I*` interfaces)"
- "Are there any patterns explicitly discouraged? (e.g., 'no classes', 'no default exports', 'no any in TypeScript')"

Keep it conversational. Stop when you feel you have enough for a complete, useful style guide.

## Step 3 — Confirm the gathered conventions

Summarize everything — inferred from code + confirmed by team — and present it for review:

```
Here's what I'll encode into your code-style skill:

Naming: <summary>
File structure: <summary>
Functions/async patterns: <summary>
Error handling: <summary>
Comments/docs: <summary>
Discouraged patterns: <summary>
[etc.]

Is this correct? Anything to add or change?
```

Wait for confirmation before generating.

## Step 4 — Generate the custom team skill

Write a `SKILL.md` to `.claude/skills/code-style/SKILL.md` (and `.cursor/rules/code-style.md`) with this structure:

```markdown
---
name: code-style
description: Enforces <project name>'s code style conventions. Apply when writing, reviewing, or refactoring any code in this project.
metadata:
  generated-by: agents-skills/team-templates/code-style-guide
  version: "1.0"
---

# Code Style — <Project Name>

When writing or reviewing code in this project, enforce the following conventions.

## Naming
[Generated from inferences + interview]

## File & Folder Structure
[Generated]

## Functions & Async Patterns
[Generated]

## Error Handling
[Generated]

## Comments & Documentation
[Generated]

## Discouraged Patterns
[Explicit "never do this" list with brief explanations]

## Quick Reference Checklist
Before submitting code, verify:
- [ ] <check 1>
- [ ] <check 2>
[Generated from the above — 5–10 items max]
```

## Step 5 — Wrap up

Tell the team:
1. Where the generated skill was saved
2. How to invoke it: `/code-style`
3. How to commit it: `git add .claude/skills/code-style/ && git commit -m "chore: add code-style team skill"`
4. That it can be updated at any time by editing the generated `SKILL.md` directly
