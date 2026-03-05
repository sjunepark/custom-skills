---
name: agents-md-writer
description: Create, review, and improve AGENTS.md files for Codex repositories with concise, actionable instructions and correct precedence behavior. Use when drafting a new AGENTS.md, migrating legacy agent-instruction files, defining nested overrides in monorepos, or debugging why Codex loads the wrong guidance.
---

# AGENTS.md Writer

Create AGENTS.md files that are short, specific, and executable. Prefer repository facts over generic advice.

## Workflow

1. Audit current instruction sources.
- Check `~/.codex/AGENTS.md`, `~/.codex/AGENTS.override.md`, repo-root `AGENTS.md`, and nested `AGENTS.md` or `AGENTS.override.md`.
- Read [references/openai-codex-agents-md.md](references/openai-codex-agents-md.md) when discovery behavior matters.

2. Collect project facts before writing.
- Read package manager scripts, CI workflows, linters/formatters, and test commands.
- Extract only commands and constraints that are true in the current repository.

3. Draft the smallest useful AGENTS.md.
- Include only instructions an agent cannot infer quickly from code.
- Use imperative bullets with concrete commands and paths.
- Cover setup/build/test, code conventions, testing expectations, safety constraints, and change-management rules.

4. Place overrides intentionally.
- Put shared defaults at repo root.
- Put subproject-specific guidance near the subtree it governs.
- Use `AGENTS.override.md` only when replacing same-directory `AGENTS.md` is intentional.

5. Verify and tighten.
- Remove duplicates from README or docs unless needed for execution.
- Remove vague guidance.
- Ensure each rule is observable, testable, or clearly enforceable.

## Authoring Rules

- Prefer exact commands over abstract advice.
- Encode hard constraints: security boundaries, data policies, migration safeguards, approval gates.
- Keep architecture prose brief; link docs by path instead.
- Keep instructions concise to avoid truncation by byte limits.
- Resolve conflicts explicitly so nearest-file precedence is unambiguous.

## Recommended Skeleton

```md
# AGENTS.md

## Setup commands
- Install deps: `...`
- Start dev env: `...`

## Build and test
- Full checks: `...`
- Fast checks for touched package: `...`

## Code conventions
- Formatter/linter commands: `...`
- Framework or language constraints: `...`

## Change expectations
- Add or update tests for changed behavior.
- Update docs in `...` when public APIs change.

## Safety and approvals
- Do not ...
- Ask before ...
```

## Debug Discovery

- Run `codex --ask-for-approval never "Summarize the current instructions."` from repo root.
- Run `codex --cd <subdir> --ask-for-approval never "Show which instruction files are active."` in nested paths.
- Inspect `~/.codex/log/codex-tui.log` or recent `session-*.jsonl` when source attribution is unclear.
- If instructions are missing, verify filename precedence, non-empty files, and byte-limit settings.

## Resources

- [references/openai-codex-agents-md.md](references/openai-codex-agents-md.md): Official discovery order, override behavior, fallback filename config, limits, and verification commands.
