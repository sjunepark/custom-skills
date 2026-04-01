---
name: sync
description: Install every skill from the current repository onto the current machine with the skills CLI, using the repository's remote URL rather than a local path. Use whenever the user wants to sync, bootstrap, or reinstall all skills from this repo and explicitly wants the install to come from GitHub or another git remote, not `./skills` or the working tree.
---

# Sync

Use this skill to install all published skills from the current repository's remote `skills/` subpath onto the current machine.

Always use the skills CLI. Do not install from `.` or `./skills`.

## Workflow

1. Confirm the repository remote URL.
- Run `git remote get-url origin`.
- If `origin` is missing, stop and tell the user you need a remote URL for this repo.
- For this repository, convert the remote into the GitHub `skills/` subpath: `https://github.com/sjunepark/custom-skills/tree/main/skills`.

2. Inspect the remote skill source before installing.
- Run `bunx skills add "<skills-subpath-url>" --list`.
- Confirm the remote resolves and exposes only the published skills from `skills/`.

3. Install every skill from the remote source.
- Target both agents by default with `-a codex -a claude-code`.
- Run `bunx skills add "<skills-subpath-url>" --all -a codex -a claude-code -y`.
- Add `-g` only when the user explicitly asks for a global install.

4. Verify the result.
- Run `bunx skills list` for project scope or `bunx skills list -g` for global scope.
- Report that the installed skills now come from the remote-backed source, not the local working tree.

## Guardrails

- Use the current repo's GitHub `skills/` subpath URL, not a filesystem path and not the repo root URL.
- Prefer the canonical published source `https://github.com/sjunepark/custom-skills/tree/main/skills` for this repo.
- Default to installing for both Codex and Claude Code unless the user narrowed the target agent set.
- If the remote points somewhere unexpected, show it to the user before installing.
- If the install command would overwrite existing skills, report that clearly in the summary.

## Command Pattern

```bash
SKILLS_URL="https://github.com/sjunepark/custom-skills/tree/main/skills"
bunx skills add "$SKILLS_URL" --list
bunx skills add "$SKILLS_URL" --all -a codex -a claude-code -y
bunx skills list
```

## Example Triggers

- "Sync all skills from this repo onto this machine."
- "Install every skill in the current repo from GitHub, not the local path."
- "Reinstall the repo skills using the remote URL."
