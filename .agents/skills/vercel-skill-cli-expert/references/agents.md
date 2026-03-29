# Supported Agents

The CLI automatically detects which coding agents are installed on the system. If none are detected, you'll be prompted to select which agents to install to.

## Agent Table

| Agent | CLI ID | Project Path | Global Path |
|---|---|---|---|
| Amp | `amp` | `.agents/skills/` | `~/.config/agents/skills/` |
| Antigravity | `antigravity` | `.agents/skills/` | `~/.gemini/antigravity/skills/` |
| Augment | `augment` | `.augment/skills/` | `~/.augment/skills/` |
| Claude Code | `claude-code` | `.claude/skills/` | `~/.claude/skills/` |
| OpenClaw | `openclaw` | `skills/` | `~/.openclaw/skills/` |
| Cline | `cline` | `.agents/skills/` | `~/.agents/skills/` |
| CodeBuddy | `codebuddy` | `.codebuddy/skills/` | `~/.codebuddy/skills/` |
| Codex | `codex` | `.agents/skills/` | `~/.codex/skills/` |
| Command Code | `command-code` | `.commandcode/skills/` | `~/.commandcode/skills/` |
| Continue | `continue` | `.continue/skills/` | `~/.continue/skills/` |
| Cortex Code | `cortex` | `.cortex/skills/` | `~/.snowflake/cortex/skills/` |
| Crush | `crush` | `.crush/skills/` | `~/.config/crush/skills/` |
| Cursor | `cursor` | `.agents/skills/` | `~/.cursor/skills/` |
| Deep Agents | `deepagents` | `.agents/skills/` | `~/.deepagents/agent/skills/` |
| Droid | `droid` | `.factory/skills/` | `~/.factory/skills/` |
| Firebender | `firebender` | `.agents/skills/` | `~/.firebender/skills/` |
| Gemini CLI | `gemini-cli` | `.agents/skills/` | `~/.gemini/skills/` |
| GitHub Copilot | `github-copilot` | `.agents/skills/` | `~/.copilot/skills/` |
| Goose | `goose` | `.goose/skills/` | `~/.config/goose/skills/` |
| Junie | `junie` | `.junie/skills/` | `~/.junie/skills/` |
| iFlow CLI | `iflow-cli` | `.iflow/skills/` | `~/.iflow/skills/` |
| Kilo Code | `kilo` | `.kilocode/skills/` | `~/.kilocode/skills/` |
| Kimi Code CLI | `kimi-cli` | `.agents/skills/` | `~/.config/agents/skills/` |
| Kiro CLI | `kiro-cli` | `.kiro/skills/` | `~/.kiro/skills/` |
| Kode | `kode` | `.kode/skills/` | `~/.kode/skills/` |
| MCPJam | `mcpjam` | `.mcpjam/skills/` | `~/.mcpjam/skills/` |
| Mistral Vibe | `mistral-vibe` | `.vibe/skills/` | `~/.vibe/skills/` |
| Mux | `mux` | `.mux/skills/` | `~/.mux/skills/` |
| OpenCode | `opencode` | `.agents/skills/` | `~/.config/opencode/skills/` |
| OpenHands | `openhands` | `.openhands/skills/` | `~/.openhands/skills/` |
| Pi | `pi` | `.pi/skills/` | `~/.pi/agent/skills/` |
| Pochi | `pochi` | `.pochi/skills/` | `~/.pochi/skills/` |
| Qoder | `qoder` | `.qoder/skills/` | `~/.qoder/skills/` |
| Qwen Code | `qwen-code` | `.qwen/skills/` | `~/.qwen/skills/` |
| Replit | `replit` | `.agents/skills/` | `~/.config/agents/skills/` |
| Roo Code | `roo` | `.roo/skills/` | `~/.roo/skills/` |
| Trae | `trae` | `.trae/skills/` | `~/.trae/skills/` |
| Trae CN | `trae-cn` | `.trae/skills/` | `~/.trae-cn/skills/` |
| Universal | `universal` | `.agents/skills/` | `~/.config/agents/skills/` |
| Warp | `warp` | `.agents/skills/` | `~/.agents/skills/` |
| Windsurf | `windsurf` | `.windsurf/skills/` | `~/.codeium/windsurf/skills/` |
| Zencoder | `zencoder` | `.zencoder/skills/` | `~/.zencoder/skills/` |
| Neovate | `neovate` | `.neovate/skills/` | `~/.neovate/skills/` |
| AdaL | `adal` | `.adal/skills/` | `~/.adal/skills/` |

## Compatibility Matrix

Features vary by agent:

| Feature | Most Agents | Exceptions |
|---|---|---|
| Basic skills | All agents | — |
| `allowed-tools` | Most agents | Not supported: Codex, Universal |
| `context: fork` | Claude Code only | All others: No |
| Hooks | Claude Code, Cline | All others: No |

## Agent-Specific Notes

### Kiro CLI
After installing skills, manually add them to your custom agent's `resources` in `.kiro/agents/<agent>.json`:
```json
{
  "resources": ["skill://.kiro/skills/**/SKILL.md"]
}
```

### Shared Path Agents
Several agents share common paths:
- **`.agents/skills/`** (project): Amp, Cline, Codex, Cursor, Deep Agents, Firebender, Gemini CLI, GitHub Copilot, Kimi Code CLI, OpenCode, Replit, Universal, Warp
- **`~/.config/agents/skills/`** (global): Amp, Kimi Code CLI, Replit, Universal
