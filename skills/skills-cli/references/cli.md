# skills CLI reference

## Sources accepted by `add`

- GitHub shorthand: `owner/repo`
- GitHub URL: `https://github.com/owner/repo`
- Git subpath URL: `https://github.com/owner/repo/tree/main/skills/my-skill`
- Other git remotes (including GitLab)
- Local path: `./my-local-skills`

## Installation scope

- Project (default): installs into project agent skill paths
- Global (`-g`): installs into user-level agent paths

For Codex and Claude Code commonly:

- Codex global path: `~/.agents/skills/`
- Claude Code global path: `~/.claude/skills/`

If the same skill `name` exists in more than one discovered location, Codex can show both entries instead of merging them.

## Practical recipes

### Add one skill to Codex + Claude globally

```bash
bunx skills add vercel-labs/skills --skill find-skills -g -a codex -a claude-code -y
```

### List current installs

```bash
bunx skills list
bunx skills list -g
bunx skills list -g -a codex
bunx skills list -g -a claude-code
```

### Remove a skill globally from both agents

```bash
bunx skills remove find-skills -g -a codex -a claude-code -y
```

### Check and update installed skills

```bash
bunx skills check
bunx skills update
```

### Bootstrap from project lock file

```bash
bunx skills experimental_install
```

### Sync node_modules-provided skills

```bash
bunx skills experimental_sync
```
