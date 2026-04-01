---
name: change-explainer
description: Explain a code or document change set in logical teaching order, with embedded snippets and clear implications. Use whenever the user wants help understanding a `git diff`, unstaged changes, a commit, commit range, PR patch, or two versions of a file, especially when they ask what changed, why it changed, how it works, what design decisions are visible, or what the change implies. Prefer this skill when the user wants snippets instead of file or line references, even if they only ask "walk me through this diff" or "help me understand these changes."
---

# Change Explainer

Explain the change itself, not just the patch mechanics.

The goal is to help the user understand:
- what changed
- why the change likely exists
- how it was implemented
- what design decisions or tradeoffs are visible
- what important implications follow from it

This is primarily an explanatory skill. Do not turn it into a code review by default. Offer opinions only when the change appears to introduce a material issue or an important tradeoff the user should know about.

## First-Class Inputs

Handle these as normal entry points:
- `git diff`
- unstaged or staged local changes
- a commit
- a commit range
- a PR patch or review diff
- two versions of a file or document

If the user provides another concrete comparison format, adapt the same workflow.

## Core Behavior

1. Anchor on the actual change.
- Read the diff first.
- Then read the surrounding code, tests, and nearby docs only where needed to explain intent and implications.
- Do not speculate about the design before checking the code around the changed hunk.

2. Reorder the explanation for comprehension.
- Do not explain hunks in raw diff order unless the diff is already easy to follow.
- Group related edits into a small number of conceptual changes.
- Prefer an order like:
  - user-visible behavior
  - interface or contract changes
  - control-flow or state changes
  - data model or persistence changes
  - tests and docs

3. Teach the implementation, not just the outcome.
- For each conceptual change, explain:
  - what changed
  - why this part likely changed
  - how the implementation now works
  - what this implies for behavior, maintenance, or future changes

4. Use snippets as the primary evidence.
- Embed short, focused code snippets directly in the response.
- Start each snippet with a comment that names the file path it came from.
- Prefer the smallest snippet that makes the point clear.
- Use multiple small snippets instead of one large dump when several ideas matter.
- Show `before` and `after` snippets when the contrast helps.
- Do not send the user to file paths or line numbers unless they explicitly ask for references.

5. Separate explanation from judgment.
- Stay mainly descriptive.
- Add judgment only for issues that look important enough to change how the user should read the patch:
  - correctness risk
  - behavior regression risk
  - broken contract or compatibility risk
  - meaningful security or data-handling concern
  - a design tradeoff that materially affects future work
- If there is no important concern, say so plainly and keep the focus on understanding.

## Workflow

1. Identify the comparison unit.
- Confirm whether you are explaining local changes, a specific commit, a commit range, a PR patch, or a file-to-file comparison.
- If the comparison is ambiguous, infer the most likely source from the task and available context.

2. Read the change before the surroundings.
- Start with the diff or patch.
- Pull in neighboring code only where the diff alone does not explain behavior, ownership, or invariants.

3. Build conceptual buckets.
- Merge scattered hunks that serve one idea.
- Split one large diff into separate conceptual changes when it mixes different concerns.
- Name each bucket by its purpose, not by the file it touched.

4. Explain each bucket in teaching order.
- Start from the highest-level behavior change.
- Then move down into how the implementation supports it.
- If tests clarify the contract, include them near the end of that bucket rather than as an afterthought.

5. Call out implications.
- Mention what the change means for callers, operators, maintainers, or future edits.
- Label inferences as inferences when they are not directly stated in the code or tests.

6. Add warnings only when warranted.
- If a critical or important issue stands out, include it in a compact note.
- Do not pad the answer with minor style opinions or speculative nitpicks.

## Snippet Rules

- Use fenced code blocks for snippets.
- Put the source file path in the first line of the snippet as a comment.
- Match the comment style to the language when practical, for example `// src/auth/session.ts` or `# app/models/user.rb`.
- If the language is unclear, use a neutral comment style or label the snippet in surrounding prose.
- Keep each snippet narrowly scoped to the point being explained.
- Prefer the changed lines plus just enough surrounding context to make them legible.
- If a long function changed, excerpt the relevant branch, condition, signature, or data shape instead of pasting the whole function.
- When comparing versions, make the contrast obvious:

```ts
// src/auth/session.ts
// before
if (!session) return null;

// after
if (!session || session.expiresAt < now) return null;
```

- If the exact old code is unavailable, show the current code and explain the inferred delta without pretending to quote the previous version.

## Output Shape

Use this shape unless the user asks for something else:

### What Changed
- One short paragraph on the overall change and its purpose.

### Walkthrough
- Explain each conceptual change in logical order.
- Use embedded snippets as evidence.
- For each one, cover the implementation and the implication.

### Important Implications
- List only the consequences that materially affect usage, behavior, compatibility, testing, or maintenance.

### Important Concerns
- Include only if there is a critical or important issue.
- If there is no such issue, omit this section or say there are no material concerns.

## Communication Rules

- Optimize for understanding, not completeness.
- Prefer a few well-chosen conceptual sections over file-by-file narration.
- Do not default to review language like "finding" or "severity" unless the user explicitly wants review mode.
- Do not mention file names or line numbers as the primary navigation aid.
- Keep the explanation concrete enough that the user does not need to open an editor just to follow the answer.
- When inferring intent or tradeoffs, say that you are inferring from the change.

## Example Triggers

- "Walk me through my unstaged changes and explain them in an order that makes sense."
- "Explain this diff like I'm joining the review late."
- "I want to understand what changed, why, and how it works now. Show snippets, not file references."
- "Compare these two versions and tell me what the implementation change implies."
- "Teach me this commit, including the design decisions visible in the patch."
