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

`bunx skills list` shows the project-visible skills for the current working directory.
Use it to inspect what the current repo exposes locally.
Use `bunx skills list -g` to inspect machine-wide global installs.

For Codex and Claude Code commonly:

- Codex global path: `~/.agents/skills/`
- Claude Code global path: `~/.claude/skills/`

If the same skill `name` exists in more than one discovered location, Codex can show both entries instead of merging them.

For this repository specifically:

- Use `https://github.com/sjunepark/custom-skills` as the normal install source.
- Prefer the remote URL over `.` or `./skills` so installed skills can be updated across multiple machines.
- Use `./skills` only for local validation or unpublished work.
- If the user asks to install for both Codex and Claude Code, pass `-a codex -a claude-code`.
- If the user asks for a global install, add `-g`; that writes to user-level directories, not repo-local `.agents/` or `.claude/`.

## Practical recipes

### Inspect this repo as a remote skill source

```bash
bunx skills add https://github.com/sjunepark/custom-skills --list
```

### Add one skill from this repo to Codex + Claude globally

```bash
bunx skills add https://github.com/sjunepark/custom-skills --skill <skill-name> -g -a codex -a claude-code -y
```

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
