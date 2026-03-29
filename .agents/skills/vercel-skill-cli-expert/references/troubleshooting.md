# Troubleshooting

Common issues and solutions for the `npx skills` CLI.

## "No skills found"

**Cause:** The repository doesn't contain valid `SKILL.md` files, or they lack required frontmatter.

**Fix:**
1. Ensure `SKILL.md` files exist in the repository
2. Verify YAML frontmatter has both `name` and `description`:
   ```yaml
   ---
   name: my-skill
   description: What this skill does
   ---
   ```
3. Check that the frontmatter is valid YAML (no tabs, proper quoting)
4. If using `--full-depth`, skills may be in nested directories

## Skill not loading in agent

**Possible causes:**
- Skill was installed to the wrong agent path
- Agent doesn't support skills
- `SKILL.md` frontmatter is invalid YAML

**Diagnosis:**
1. Run `npx skills list` to verify installation
2. Run `npx skills ls -a <agent-name>` to check specific agent
3. Check the file exists at the expected path (see [agents.md](./agents.md) for paths)
4. Validate YAML: no tabs, colons in values must be quoted

## Permission errors

**Cause:** No write access to the target directory.

**Fix:**
- For project scope: ensure you have write access to the project directory
- For global scope (`-g`): ensure write access to your home directory agent paths
- On macOS/Linux: check with `ls -la` on the target path

## Symlink issues

**Cause:** Some environments (Windows, certain containers) don't support symlinks.

**Fix:**
- Use `--copy` flag: `npx skills add owner/repo --copy`
- This creates independent copies instead of symlinks

## YAML frontmatter silent failures

**Cause:** Invalid YAML causes the skill to be silently ignored.

**Common mistakes:**
| Mistake | Example | Fix |
|---|---|---|
| Unescaped colons | `description: Use when: doing X` | `description: "Use when: doing X"` |
| Tabs instead of spaces | `\tname: skill` | Use spaces only |
| Name mismatch | Folder `my-skill/`, name: `myskill` | Make them identical |

## Skills not updating

**Fix:**
1. Run `npx skills check` to see what's outdated
2. Run `npx skills update` to update all
3. If a specific skill fails, remove and re-add it:
   ```bash
   npx skills remove skill-name
   npx skills add owner/repo --skill skill-name
   ```

## CI/CD failures

**Common issues:**
- Interactive prompts block pipeline → use `-y` flag
- Symlinks unsupported → use `--copy`
- Agent auto-detection fails → specify `-a agent-name` explicitly

**Recommended CI/CD pattern:**
```bash
npx skills add owner/repo --skill skill-name -a agent-name -y --copy
```

## Environment Variables

| Variable | Description |
|---|---|
| `INSTALL_INTERNAL_SKILLS=1` | Show and install skills marked as `metadata.internal: true` |
| `DISABLE_TELEMETRY` | Disable anonymous usage telemetry |
| `DO_NOT_TRACK` | Alternative way to disable telemetry |

**Note:** Telemetry is automatically disabled in CI environments.

## Getting Help

- CLI help: `npx skills --help`
- Command-specific: `npx skills add --help`
- GitHub Issues: https://github.com/vercel-labs/skills/issues
- Documentation: https://skills.sh/docs
- Skills catalog: https://skills.sh/
