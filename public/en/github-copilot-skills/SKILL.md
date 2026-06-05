---
id: GITHUB_COPILOT_SKILLS
name: GitHub Copilot Skills
description: "Create, install, and manage GitHub Copilot Cloud Agent skills. Use when: building SKILL.md files for Copilot, using gh skill CLI, or setting up project/personal skills. NOT for: regular GitHub operations (use the github skill), or non-Copilot agent systems."
icon: 🧑‍✈️
category: dev
created_at: "2026-06-05"
updated_at: "2026-06-05"
homepage: https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-skills
---

# GitHub Copilot Skills

Create and manage skills for GitHub Copilot Cloud Agent. Skills extend Copilot's behavior with custom instructions, scripts, and resources — loaded automatically when relevant.

## What is a Copilot skill

A skill is a folder containing a `SKILL.md` file (and optional scripts/resources) that Copilot loads when the user's prompt matches the skill's description. Skills work in:
- Copilot Cloud Agent
- Code review
- GitHub CLI
- Agent mode in VS Code

## Skill locations

| Scope | Location |
|---|---|
| Project | `.github/skills/` or `.claude/skills/` or `.agents/skills/` |
| Personal | `~/.copilot/skills/` or `~/.agents/skills/` |

## SKILL.md structure

```markdown
---
name: my-skill-name
description: "What this skill does and when Copilot should use it."
allowed-tools:
  - shell
---

# Skill Title

Instructions for Copilot go here in Markdown.

## When to use

Describe triggers and relevant situations.

## How to use

Step-by-step instructions or examples.
```

### Required frontmatter fields

| Field | Description |
|---|---|
| `name` | Unique identifier (lowercase, hyphens) |
| `description` | Tells Copilot what this skill does and when to activate it |

### Optional fields

| Field | Description |
|---|---|
| `allowed-tools` | Pre-approves tools (e.g. `shell`) without prompting the user each time |
| `license` | License identifier (e.g. `MIT`) |

## Create a skill from scratch

```bash
# Project-level skill
mkdir -p .github/skills/my-skill
cat > .github/skills/my-skill/SKILL.md << 'EOF'
---
name: my-skill
description: "Brief description of what this skill does and when to use it."
---

# My Skill

Instructions here...
EOF
```

## Skill with a script

```bash
mkdir -p .github/skills/run-tests

# The script
cat > .github/skills/run-tests/run.sh << 'EOF'
#!/bin/bash
npm test -- --reporter=json
EOF
chmod +x .github/skills/run-tests/run.sh

# The SKILL.md referencing the script
cat > .github/skills/run-tests/SKILL.md << 'EOF'
---
name: run-tests
description: "Run the test suite and report results. Use when asked to run tests or check test status."
allowed-tools:
  - shell
---

# Run Tests

Execute the test suite using the provided script.

## Usage

```bash
bash .github/skills/run-tests/run.sh
```

Interpret the JSON output and summarize failures.
EOF
```

## gh skill CLI

```bash
# Search skills by topic
gh skill search testing

# Install a skill from a GitHub repo
gh skill install owner/repository skill-name

# List installed skills
gh skill list

# Update all installed skills
gh skill update

# Validate before publishing (dry run)
gh skill publish --dry-run

# Publish a skill
gh skill publish
```

## Tips for good skill descriptions

The `description` field is what Copilot uses to decide when to load the skill. Write it as "What to do and when to use it":

```yaml
# Good — specific, action-oriented
description: "Deploy to Vercel and check deployment status. Use when the user asks to deploy, preview a branch, or check deployment logs."

# Bad — vague
description: "Deployment helper"
```

## Notes

- Copilot loads skills automatically — no explicit invocation needed.
- Keep instructions concise; Copilot loads the full `SKILL.md` into context.
- `allowed-tools: [shell]` removes repeated confirmation prompts for shell commands — use with care.
- Skills in `.github/skills/` are version-controlled and shared with the team; `~/.copilot/skills/` are personal only.
- Full reference: `https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/create-skills`
