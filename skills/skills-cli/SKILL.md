---
name: skills-cli
description: Operate the skills CLI to discover, install, list, update, remove, and initialize skills for Codex and Claude Code. Use when users ask to manage skills from skills.sh, restore from lock files, sync skills from node_modules, or troubleshoot agent/installation scope (project vs global).
---

# Skills CLI

Use `bunx skills` commands to manage skills.

## Workflow

1. Inspect current state with `bunx skills list` for project scope and `bunx skills list -g` for global scope.
2. Use `bunx skills find <query>` or `bunx skills add <source> --list` to discover options.
3. Install with explicit scope and agent targeting:
   - For this repository's published skills, use `https://github.com/sjunepark/custom-skills/tree/main/skills` rather than the repo root or current working tree so installs can update across machines without exposing repo-local `.agents/` and `.claude/` skills.
   - Global personal setup: `-g -a codex -a claude-code`
   - Project setup: omit `-g`
4. Verify install with the matching scope command and test a prompt that should trigger the skill.
5. For updates, run `bunx skills check` then `bunx skills update`.
6. For cleanup, use `bunx skills remove ...` with matching scope/agent flags.

## Key commands

- `bunx skills add <package-or-url>`
- `bunx skills add <source> --skill <name> -a <agent> [-g] -y`
- `bunx skills add <source> --all`
- `bunx skills list [-g] [-a <agent>]`
- `bunx skills find [query]`
- `bunx skills remove [skill...] [-g] [-a <agent>]`
- `bunx skills check`
- `bunx skills update`
- `bunx skills init [name]`
- `bunx skills experimental_install`
- `bunx skills experimental_sync`

## Guardrails

- Prefer explicit `--agent` and `--skill` flags for reproducibility.
- Use `--yes` only for non-interactive runs.
- Default to symlink mode unless the user requests `--copy`.
- `bunx skills list` without `-g` reports project-visible skills for the current directory; do not use it to answer what is globally installed.
- When installing this repository's skills for regular machine use, do not install from `.` or `./skills`, and do not use the repo root URL; use `https://github.com/sjunepark/custom-skills/tree/main/skills`.
- Treat installed skills as executable instructions; avoid untrusted sources.
- If managing dotfiles with chezmoi, avoid `chezmoi add` on live skills directories.

## References

Read [references/cli.md](references/cli.md) for source formats, scopes, and practical command recipes.
