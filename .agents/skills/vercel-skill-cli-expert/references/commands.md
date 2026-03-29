# Skills CLI — Full Command Reference

Version: 1.4.6 | Package: `skills` (npm) | License: MIT

## Global Options

| Option | Description |
|---|---|
| `--help`, `-h` | Show help message |
| `--version`, `-v` | Show version number |

---

## 1. `skills add <package>` (alias: `a`)

Install skill packages from a repository or local path.

### Source Formats

| Format | Example |
|---|---|
| GitHub shorthand | `skills add vercel-labs/agent-skills` |
| Full GitHub URL | `skills add https://github.com/vercel-labs/agent-skills` |
| Direct path to skill | `skills add https://github.com/owner/repo/tree/main/skills/web-design` |
| GitLab URL | `skills add https://gitlab.com/org/repo` |
| Any git URL | `skills add git@github.com:owner/repo.git` |
| Local path | `skills add ./my-local-skills` |

### Options

| Option | Description |
|---|---|
| `-g`, `--global` | Install globally (`~/`) instead of project-level (`./`) |
| `-a`, `--agent <agents...>` | Target specific agents. Use `'*'` for all agents |
| `-s`, `--skill <skills...>` | Install specific skills by name. Use `'*'` for all |
| `-l`, `--list` | List available skills in the repo without installing |
| `--copy` | Copy files instead of symlinking |
| `-y`, `--yes` | Skip all confirmation prompts |
| `--all` | Shorthand for `--skill '*' --agent '*' -y` |
| `--full-depth` | Search all subdirectories even when a root SKILL.md exists |

### Installation Scope

| Scope | Flag | Path | Use case |
|---|---|---|---|
| Project | (default) | `./<agent>/skills/` | Committed with project, shared with team |
| Global | `-g` | `~/<agent>/skills/` | Available across all projects |

### Installation Methods

| Method | Description |
|---|---|
| Symlink (default) | Creates symlinks from each agent dir to a canonical copy. Single source of truth, easy updates |
| Copy (`--copy`) | Creates independent copies for each agent. Use when symlinks aren't supported |

### Examples

```bash
# List skills in a repository (preview without installing)
npx skills add vercel-labs/agent-skills --list

# Interactive installation (default)
npx skills add vercel-labs/agent-skills

# Install specific skills
npx skills add vercel-labs/agent-skills --skill frontend-design --skill skill-creator

# Skill name with spaces (must quote)
npx skills add owner/repo --skill "Convex Best Practices"

# Install to specific agents
npx skills add vercel-labs/agent-skills -a claude-code -a opencode

# Non-interactive CI/CD installation
npx skills add vercel-labs/agent-skills --skill frontend-design -g -a claude-code -y

# Install ALL skills to ALL agents without prompts
npx skills add vercel-labs/agent-skills --all

# All skills to specific agents
npx skills add vercel-labs/agent-skills --skill '*' -a claude-code

# Specific skills to all agents
npx skills add vercel-labs/agent-skills --agent '*' --skill frontend-design

# From local directory
npx skills add ./my-local-skills
```

---

## 2. `skills remove [skills]` (aliases: `rm`)

Remove installed skills from agent directories.

### Options

| Option | Description |
|---|---|
| `-g`, `--global` | Remove from global scope (`~/`) |
| `-a`, `--agent <agents...>` | Remove from specific agents. Use `'*'` for all |
| `-s`, `--skill <skills...>` | Specify skills to remove. Use `'*'` for all |
| `-y`, `--yes` | Skip confirmation prompts |
| `--all` | Shorthand for `--skill '*' --agent '*' -y` |

### Examples

```bash
# Interactive removal (select from list)
npx skills remove

# Remove by name
npx skills remove web-design-guidelines

# Remove multiple skills
npx skills remove frontend-design web-design-guidelines

# Remove from global scope
npx skills remove --global web-design-guidelines

# Remove from specific agents
npx skills remove --agent claude-code cursor my-skill

# Remove ALL installed skills
npx skills remove --all

# Remove all skills of one agent
npx skills remove --skill '*' -a cursor

# Remove one skill from all agents
npx skills remove my-skill --agent '*'

# Alias: rm
npx skills rm my-skill
```

---

## 3. `skills list` (alias: `ls`)

List installed skills. Similar to `npm ls`.

### Options

| Option | Description |
|---|---|
| `-g`, `--global` | List global skills (default: project) |
| `-a`, `--agent <agents...>` | Filter by specific agents |
| `--json` | Output as JSON (machine-readable, no ANSI codes) |

### Examples

```bash
# List all project skills
npx skills list

# List global skills
npx skills ls -g

# Filter by agents
npx skills ls -a claude-code -a cursor

# JSON output (for scripting)
npx skills ls --json
```

---

## 4. `skills find [query]`

Search for skills interactively (fzf-style) or by keyword.

### Examples

```bash
# Interactive search
npx skills find

# Search by keyword
npx skills find typescript

# Search for React skills
npx skills find react
```

---

## 5. `skills check`

Check if any installed skills have available updates.

```bash
npx skills check
```

---

## 6. `skills update`

Update all installed skills to their latest versions.

```bash
npx skills update
```

---

## 7. `skills init [name]`

Initialize a new skill by creating a `SKILL.md` template.

### Behavior

- Without argument: creates `./SKILL.md` in current directory
- With argument: creates `<name>/SKILL.md` in a subdirectory

### Examples

```bash
# Create SKILL.md in current directory
npx skills init

# Create in subdirectory
npx skills init my-skill
```

---

## 8. `skills experimental_install`

**Experimental.** Restore skills from `skills-lock.json`. Analogous to `npm install` from `package-lock.json`.

```bash
npx skills experimental_install
```

---

## 9. `skills experimental_sync`

**Experimental.** Sync skills from `node_modules` into agent directories.

### Options

| Option | Description |
|---|---|
| `-a`, `--agent <agents...>` | Target specific agents. Use `'*'` for all |
| `-y`, `--yes` | Skip confirmation prompts |

### Examples

```bash
# Interactive sync
npx skills experimental_sync

# Sync without prompts
npx skills experimental_sync -y
```
