---
name: new-component
description: Scaffolds a new UI component — component file, styles, stories, and tests — following this project's existing patterns. Invoke with: /new-component <ComponentName> "<description>"
license: MIT
metadata:
  author: agents-skills
  version: "1.0"
  scope: project
allowed-tools: Bash Read Write Glob
---

# New Component Scaffolder

Scaffold a new UI component that is consistent with this project's existing patterns.

**Arguments from invocation:**
- `$NAME` — Component name in PascalCase (e.g., `UserNotificationCard`)
- `$DESCRIPTION` — What this component does and where it's used

## Step 1 — Detect the framework and project structure

```bash
# React / Next.js / Vue / Svelte / Angular
cat package.json 2>/dev/null | grep -E '"react"|"vue"|"svelte"|"@angular"'

# TypeScript?
ls tsconfig.json 2>/dev/null && echo "TypeScript project"

# Styling approach
cat package.json 2>/dev/null | grep -E '"tailwindcss"|"styled-components"|"@emotion|"sass"|"css-modules"'
```

Find where components live:
```bash
find . -type d -name "components" | grep -v node_modules | grep -v ".git" | head -10
```

## Step 2 — Read 1–2 existing components as reference

Pick an existing component that is similar in complexity. Read:
- The component file itself
- Its styles file (if separate)
- Its test file
- Its story file (if Storybook is used)

Understand the project's patterns for:
- File and folder structure (single file? co-located styles? barrel exports?)
- Props definition (TypeScript interface? PropTypes? plain JS?)
- State management patterns (useState? Context? Zustand? Redux?)
- How the component is exported (named? default?)
- Import conventions (absolute paths? aliases like `@/components`?)
- Test patterns (RTL? Enzyme? Vitest? Jest?)
- Story patterns (CSF? MDX? what args look like?)

## Step 3 — Determine the component scope

Ask or infer from `$DESCRIPTION`:
- Is this a **presentational** component (pure UI, no side effects)?
- Is this a **container/smart** component (fetches data, has state)?
- Is this a **layout** component (wraps children)?

This affects where it lives and how it's structured.

## Step 4 — Scaffold the files

Following the patterns from Step 2 exactly:

1. **Component file** (`ComponentName.tsx` / `.vue` / `.svelte`) with:
   - Props interface/type definition
   - Component implementation
   - Default props or prop defaults where appropriate
   - Accessibility attributes (aria-label, role) where relevant

2. **Styles** — only if the project uses separate style files (CSS modules, SCSS, styled-components)

3. **Test file** (`ComponentName.test.tsx`) covering:
   - Renders without crashing
   - Renders correctly with required props
   - Key user interactions (if applicable)
   - Snapshot test (only if the project already uses snapshots)

4. **Story file** (`ComponentName.stories.tsx`) — only if Storybook is configured:
   - Default story
   - Key variants (empty state, loading, error, populated)

5. **Barrel export** — if the project uses `index.ts` barrel files, add the export

## Step 5 — Show a summary

List every file created. Call out every assumption made (especially around prop naming, file location, and test approach) so the developer can correct anything immediately.
