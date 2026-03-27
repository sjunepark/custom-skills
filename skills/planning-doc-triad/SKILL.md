---
name: planning-doc-triad
description: "Maintain project planning with three distinct documents: a big-picture roadmap, a near-term TODO list, and one active detailed plan. Use this skill whenever a software or coding project needs planning hygiene, execution structure, backlog cleanup, or a reset of stale planning docs, even if the user only mentions roadmap, todo, priorities, next steps, milestones, implementation plan, or 'what should we work on next.'"
---

# Planning Doc Triad

Use three planning documents with non-overlapping roles:

- `docs/roadmap.md`: strategic direction and phased sequencing
- `TODO.md`: near-term actionable queue
- `PLAN.md`: one active detailed plan for the current job

This structure separates strategy, near-term execution, and active working context.

## When To Apply It

Apply this skill when:

- roadmap items are mixed with implementation tasks
- there is no clear "what next" list
- multiple plan files keep going stale
- detailed planning is happening only in chat history
- priorities are hard to infer from the current docs

Use it both for new repos and for cleaning up an existing planning system.

## Core Rules

### `docs/roadmap.md`

Use the roadmap for:

- product or technical direction
- phases or milestones
- intentional deferrals
- sequencing across multiple chunks of work

Do not use the roadmap for:

- session-level checklists
- fine-grained task breakdowns
- ephemeral notes

### `TODO.md`

Use the todo file for:

- active or next-up work
- small actionable items
- ordered execution priority

Keep it:

- short
- concrete
- easy to scan

Do not turn it into a strategy memo or a storage place for distant ideas.

### `PLAN.md`

Use the plan file for:

- one active initiative
- one detailed job breakdown
- current scope, non-goals, steps, and open questions

Keep exactly one active `PLAN.md`.
Rewrite it freely as the current job changes.
Do not preserve stale plan history in multiple plan files.

## Setup Workflow

When introducing this methodology to a repo:

1. Check whether the repo already has a roadmap, todo list, or plan docs.
2. Reuse existing files when they already match the intended role.
3. Create missing files in these locations:
   - `docs/roadmap.md`
   - `TODO.md`
   - `PLAN.md`
4. Remove overlap so each file has one clear job.
5. Move durable decisions into canonical docs such as architecture or product docs.

## Maintenance Workflow

When updating planning docs during normal work:

1. Update `PLAN.md` first if the current job needs a detailed plan.
2. Reflect near-term actionable items in `TODO.md`.
3. Update `docs/roadmap.md` only if priorities, phasing, or major direction changed.
4. After execution, remove completed todo items and rewrite the active plan for the next job.
5. If the plan produced durable decisions, persist them outside `PLAN.md`.

## Writing Standards

- Keep all three files concise.
- Lead with what matters now.
- Use direct, concrete wording.
- Prefer ordering over categorization unless categories materially help.
- Avoid repeating the same item across files.

## Recommended File Shapes

### `docs/roadmap.md`

Prefer this structure:

- purpose or current recommendation
- phases or milestones
- likely deliverables per phase

### `TODO.md`

Prefer this structure:

- one short rule line
- `## Next Up`
- a flat ordered task list

### `PLAN.md`

Prefer this structure:

- current job
- goal
- in scope
- out of scope
- work plan
- open questions
- exit criteria

## Anti-Patterns

Avoid these failure modes:

- turning the roadmap into a backlog
- turning the todo file into a strategy memo
- keeping several plan files for parallel jobs without a clear active one
- leaving `PLAN.md` as stale history after the job changes
- storing important product decisions only in `PLAN.md`

## Output

When applying this methodology, produce:

- the updated files
- a short explanation of what each file now owns
- the current next actions
