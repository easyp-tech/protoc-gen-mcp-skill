# Skill Authoring Guide

How to create, structure, and publish your own skills for the agent skills ecosystem.

## Quick Start

```bash
npx skills init my-skill
```

This creates `my-skill/SKILL.md` with a template.

## SKILL.md Format

Every skill is a directory containing a `SKILL.md` file with YAML frontmatter:

```yaml
---
name: my-skill
description: What this skill does and when to use it
---

# My Skill

Instructions for the agent to follow when this skill is activated.

## When to Use
Describe the scenarios where this skill should be used.

## Steps
1. First, do this
2. Then, do that
```

## Required Fields

| Field | Rules | Example |
|---|---|---|
| `name` | Unique identifier. Lowercase, hyphens allowed | `web-design-review` |
| `description` | Brief explanation of what the skill does and when | `"Review UI code for accessibility and UX best practices"` |

## Optional Fields

| Field | Description | Example |
|---|---|---|
| `metadata.internal` | Set `true` to hide from normal discovery | `metadata: { internal: true }` |

Internal skills are only visible when `INSTALL_INTERNAL_SKILLS=1` is set:
```yaml
---
name: my-internal-skill
description: An internal skill not shown by default
metadata:
  internal: true
---
```

## Recommended Directory Structure

```
my-skill/
├── SKILL.md           # Required — main instructions
├── scripts/           # Optional — helper scripts for automation
│   ├── check.sh
│   └── deploy.js
├── references/        # Optional — supporting documentation
│   ├── api-guide.md
│   └── patterns.md
└── assets/            # Optional — templates, boilerplate
    └── component.tsx
```

## Progressive Loading Design

Skills are loaded in stages to minimize context usage:

1. **Discovery** (~100 tokens): Agent reads `name` and `description` from frontmatter
2. **Instructions** (<5000 tokens): Agent loads SKILL.md body when task is relevant
3. **Resources**: Additional files load only when referenced in instructions

### Best Practices

- Keep `SKILL.md` under 500 lines
- Move detailed reference material to `references/` directory
- Use relative paths: `[see patterns](./references/patterns.md)`
- Keep file references one level deep from `SKILL.md`

## Writing Effective Descriptions

The `description` field is the discovery surface — agents use it to decide whether to load a skill.

**Good** — includes trigger keywords:
```yaml
description: "Review UI code for compliance with web interface best practices. Audits accessibility, performance, and UX. Use when: reviewing UI, checking accessibility, auditing design."
```

**Bad** — too vague:
```yaml
description: "A helpful skill for frontend work"
```

## Writing Effective Instructions

### Structure Pattern

```markdown
# Skill Name

Brief overview of what this skill does.

## When to Use
- Trigger scenario 1
- Trigger scenario 2

## Procedure
1. Step with clear action
2. Step referencing a resource: [see script](./scripts/check.sh)
3. Decision point: if X → do Y, else → do Z

## Output
Describe what the skill produces (report, file, message, etc.)
```

### Tips

- Be **prescriptive**: "Run this command" not "You might want to run..."
- Include **decision points**: what to do when conditions vary
- Reference **scripts** for automation: agents can execute them
- Define **completion criteria**: how to know the task is done

## Skill Discovery Paths

The CLI searches for `SKILL.md` files in these repository locations:

- Root directory (if it contains `SKILL.md`)
- `skills/`
- `skills/.curated/`
- `skills/.experimental/`
- `skills/.system/`
- All agent directories (`.agents/skills/`, `.claude/skills/`, `.cursor/skills/`, etc.)

If no skills are found in standard locations, a **recursive search** is performed.

## Plugin Manifest Discovery

For Claude Code plugin marketplace compatibility, skills can be declared in `.claude-plugin/marketplace.json`:

```json
{
  "metadata": { "pluginRoot": "./plugins" },
  "plugins": [
    {
      "name": "my-plugin",
      "source": "my-plugin",
      "skills": ["./skills/review", "./skills/test"]
    }
  ]
}
```

Also supported: `.claude-plugin/plugin.json`.

## Publishing and Distribution

### Via GitHub

1. Create a repository with your skill(s) in `skills/` directory
2. Each skill is a subdirectory with `SKILL.md`
3. Users install: `npx skills add your-username/your-repo`

### Repository Structure Example

```
my-skills-repo/
├── skills/
│   ├── skill-one/
│   │   └── SKILL.md
│   ├── skill-two/
│   │   ├── SKILL.md
│   │   └── scripts/
│   │       └── helper.sh
│   └── skill-three/
│       ├── SKILL.md
│       └── references/
│           └── guide.md
└── README.md
```

### Multi-Skill Repositories

A single repo can contain multiple skills. Users can:
- Install all: `npx skills add owner/repo --all`
- Install specific ones: `npx skills add owner/repo --skill skill-one --skill skill-two`
- Preview available: `npx skills add owner/repo --list`

## Common Anti-Patterns

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Vague description | Agent can't discover the skill | Add trigger keywords and "Use when:" phrases |
| Monolithic SKILL.md | Burns context window | Split into SKILL.md + references/ |
| Name mismatch | Folder name ≠ `name` field | Keep them identical |
| Missing procedures | Description without steps | Add numbered step-by-step instructions |
| No completion criteria | Agent doesn't know when done | Add "## Output" or "## Done When" section |
