# Install: api-conventions (Team Template)

**Scope:** Team Template — run once per team to generate your custom `api-conventions` skill.

---

## What this template does

This is a **skill builder**, not a skill you use repeatedly. When you run it, your agent will:

1. Read your existing codebase to infer your current API patterns
2. Interview you about your conventions (only what it couldn't infer)
3. Generate a custom `api-conventions` skill tailored to your team
4. Install that generated skill into your project

**Run the template with:** `/api-conventions`

---

## Install for Claude Code

Tell Claude Code inside your project:
> "Install the api-conventions team template from this directory and run it to build our team skill."

---

## Install for Cursor

Tell Cursor inside your project:
> "Install the api-conventions team template from this directory and run it to build our team skill."

---

## After running the template

The template replaces itself with your generated custom skill. Commit it so your whole team gets it automatically:

```bash
git add .claude/skills/api-conventions/
git commit -m "chore: add api-conventions team skill"
```

Update it any time by editing the generated `SKILL.md` directly, or re-running this template to rebuild from scratch.
