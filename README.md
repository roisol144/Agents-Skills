# Agents-Skills

A curated workshop of agent skills for software developers. Works with **Claude Code**, **Cursor**, **GitHub Copilot**, and any AI coding tool that supports the [Agent Skills standard](https://agentskills.io).

> Point your agent at a skill, say "install it" — and your agent learns how to do that task, your way, every time.

---

## How it works

A skill is a reusable instruction set that teaches your AI agent how to perform a specific task — consistently, the way a senior dev would. Skills live in your `.claude/skills/` or `.cursor/rules/` directory and activate when you invoke them.

**Three tiers:**

| Tier | What it is | Who it's for |
|------|-----------|--------------|
| **User** | Install once, works across all your projects | Any developer |
| **Project** | Adapts to your specific codebase on install | Per-project workflows |
| **Team Template** | Interviews you, generates a custom team skill | Teams with shared conventions |

---

## User Skills

Install once globally. Works on any project, no configuration needed.

### Git & PR

| Skill | What it does |
|-------|-------------|
| [pr-description](skills/user/git/pr-description/) | Analyzes your diff and CI status, rewrites your PR description to be accurate and reviewer-friendly |
| [commit-message](skills/user/git/commit-message/) | Writes a conventional commit message from your staged changes |

### Code Review

| Skill | What it does |
|-------|-------------|
| [safe-to-merge](skills/user/code-review/safe-to-merge/) | Pre-merge safety check: debug artifacts, secrets, missing tests, CI status |

---

## Project Skills

These adapt to your codebase on install. Your agent inspects your project, then installs a version tailored to your stack.

### Backend

| Skill | What it does |
|-------|-------------|
| [new-endpoint](skills/project/backend/new-endpoint/) | Scaffolds a new API endpoint: route, handler, validation, and tests — matching your project's conventions |

### Frontend

| Skill | What it does |
|-------|-------------|
| [new-component](skills/project/frontend/new-component/) | Scaffolds a new UI component with stories and tests, matching your component patterns |

---

## Team Templates

These are skill builders. Your agent interviews your team and generates a fully custom skill you own and version in your repo.

| Template | What it builds |
|----------|----------------|
| [api-conventions](skills/team-templates/api-conventions/) | A team skill that enforces your API naming, auth, versioning, and error format conventions |

---

## Installing a skill

### With Claude Code

Tell Claude Code:
```
Go to https://github.com/roisolomon/Agents-Skills, find the [skill-name] skill, and install it.
```

Claude Code will read the skill's `INSTALL.md` and handle everything — including customizing project skills to your stack.

### With Cursor

Tell Cursor:
```
Go to https://github.com/roisolomon/Agents-Skills, find the [skill-name] skill, and install it.
```

---

## Skill structure

Every skill in this repo follows the same layout:

```
skill-name/
  SKILL.md      # The skill itself — this is what gets installed
  INSTALL.md    # Installation guide for your agent
```

---

## Contributing

Good skills solve real, recurring developer problems. Before opening a PR:

- The skill should work across multiple projects, not just your own
- User skills need no project-specific assumptions
- Project skills must include an `INSTALL.md` that guides customization
- Test your skill with at least Claude Code and Cursor

---

## License

MIT
