# Install: code-style-guide (Team Template)

**Scope:** Team Template — run once per project to generate your custom `code-style` skill.

---

## What this template does

This is a **skill builder**, not a daily-use skill. When you run it, your agent will:

1. Read your existing codebase to infer your naming conventions, structural patterns, and coding style
2. Interview you about anything it couldn't determine from the code alone
3. Generate a custom `code-style` skill that encodes your team's specific conventions
4. Install that generated skill into your project

The generated skill is what your team uses going forward — every developer and AI agent on the team produces consistent, on-convention code.

**Run the template with:** `/code-style-guide`
**Use the generated skill with:** `/code-style`

---

## Prerequisites

- Your project should have at least a few representative source files for the template to analyze.

---

## Install for Claude Code

Tell Claude Code inside your project:
> "Install the code-style-guide team template from this directory and run it to build our team skill."

---

## Install for Cursor

Tell Cursor inside your project:
> "Install the code-style-guide team template from this directory and run it to build our team skill."

---

## After running the template

Commit the generated skill so the whole team gets it:

```bash
git add .claude/skills/code-style/
git commit -m "chore: add code-style team skill"
```

Update it any time by editing the generated `SKILL.md` directly, or re-running this template to rebuild from scratch.
