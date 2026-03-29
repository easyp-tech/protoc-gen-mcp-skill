---
name: vercel-skill-cli-expert
description: "Expert guide for the `npx skills` CLI — the open agent skills ecosystem tool by Vercel. Use when: installing skills, removing skills, updating skills, listing installed skills, searching for skills, creating new SKILL.md files, publishing skills, troubleshooting skill installation, managing skills across agents (Claude Code, Cursor, Copilot, Codex, etc.), understanding symlink vs copy, project vs global scope, CI/CD automation with skills CLI, skills-lock.json, experimental_sync from node_modules."
---

# Skills CLI Expert

Complete operational guide for the `npx skills` CLI (npm package: `skills`).

## When to Use

- User wants to **install** a skill from GitHub, GitLab, or a local path
- User wants to **remove**, **update**, or **check updates** for skills
- User wants to **list** installed skills or **search** for new ones
- User wants to **create** a new skill (`SKILL.md`) for distribution
- User needs to install skills for **specific agents** or **globally**
- User wants to **automate** skill installation in CI/CD
- User is **troubleshooting** skill installation or loading issues
- User asks about **symlink vs copy**, **project vs global scope**
- User wants to **publish** or **distribute** their own skill package

## Decision Flow

Follow this tree to determine the right action:

```
User wants to...
│
├─ DISCOVER skills → `skills find [query]`
│  └─ Browse catalog → https://skills.sh/
│
├─ INSTALL skills
│  ├─ From GitHub repo → `skills add owner/repo`
│  ├─ Specific skills only → `skills add owner/repo --skill name1 --skill name2`
│  ├─ To specific agents → `skills add owner/repo -a claude-code -a cursor`
│  ├─ Globally (all projects) → `skills add owner/repo -g`
│  ├─ Non-interactive (CI/CD) → `skills add owner/repo --skill X -a Y -g -y`
│  ├─ Everything at once → `skills add owner/repo --all`
│  └─ From local path → `skills add ./my-skills`
│
├─ MANAGE installed skills
│  ├─ List what's installed → `skills list` or `skills ls -g`
│  ├─ Check for updates → `skills check`
│  ├─ Update all → `skills update`
│  ├─ Remove interactively → `skills remove`
│  ├─ Remove by name → `skills remove skill-name`
│  └─ Remove everything → `skills remove --all`
│
├─ CREATE a new skill
│  ├─ Quick start → `skills init my-skill`
│  ├─ In current dir → `skills init`
│  └─ Full authoring guide → see [skill-authoring.md](./references/skill-authoring.md)
│
└─ TROUBLESHOOT
   ├─ "No skills found" → see [troubleshooting.md](./references/troubleshooting.md)
   ├─ Skill not loading → check agent paths and frontmatter
   └─ Permission errors → check write access
```

## Core Procedures

### Procedure 1: Install Skills from a Repository

1. Determine the source: GitHub shorthand (`owner/repo`), full URL, or local path
2. Decide scope: project (default) or global (`-g`)
3. Choose agents: auto-detect (default), specific (`-a agent-name`), or all (`-a '*'`)
4. Choose skills: interactive (default), specific (`-s skill-name`), or all (`-s '*'`)
5. Choose method: symlink (default, recommended) or copy (`--copy`)
6. Run: `npx skills add <source> [options]`
7. Verify: `npx skills list` to confirm installation

For all source formats and options, see [commands.md](./references/commands.md).

### Procedure 2: Remove Skills

1. Interactive: `npx skills remove` — select from installed skills
2. By name: `npx skills remove skill-name`
3. From specific agent: `npx skills remove skill-name -a cursor`
4. Nuclear option: `npx skills remove --all`

### Procedure 3: Update Skills

1. Check what's outdated: `npx skills check`
2. Update everything: `npx skills update`

### Procedure 4: Create and Publish a Skill

1. Initialize: `npx skills init my-skill`
2. Edit `my-skill/SKILL.md` — add `name`, `description` (required), instructions
3. Optionally add `scripts/`, `references/`, `assets/` directories
4. Push to a GitHub repository
5. Users install via: `npx skills add your-username/your-repo`
6. For detailed authoring guide, see [skill-authoring.md](./references/skill-authoring.md)

### Procedure 5: CI/CD Automation

For non-interactive installation in CI/CD pipelines:
```bash
npx skills add owner/repo --skill skill-name -a agent-name -g -y
```
Key flags: `-y` (skip prompts), `-g` (global), `--copy` (if symlinks unsupported).

### Procedure 6: List and Inspect

- Project skills: `npx skills list`
- Global skills: `npx skills ls -g`
- Filter by agent: `npx skills ls -a claude-code`
- Machine-readable: `npx skills ls --json`

## Key Concepts

| Concept | Description |
|---|---|
| **Project scope** (default) | Skills in `./<agent>/skills/`, committed with project |
| **Global scope** (`-g`) | Skills in `~/<agent>/skills/`, available everywhere |
| **Symlink** (default) | Single canonical copy, agents symlink to it |
| **Copy** (`--copy`) | Independent copies per agent directory |
| **Auto-detection** | CLI detects which agents are installed on the system |
| **SKILL.md** | Markdown file with YAML frontmatter defining a skill |
| **skills-lock.json** | Lock file for reproducible installs (`experimental_install`) |

## Reference Files

- [Full Command Reference](./references/commands.md) — all 9 commands, all options, all examples
- [Supported Agents](./references/agents.md) — 44+ agents with paths and IDs
- [Skill Authoring Guide](./references/skill-authoring.md) — creating, structuring, publishing skills
- [Troubleshooting](./references/troubleshooting.md) — common errors, env vars, diagnostics
