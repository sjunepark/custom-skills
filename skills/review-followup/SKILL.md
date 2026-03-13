---
name: review-followup
description: Validate code review comments on a diff, PR, or commit and decide the right follow-up. Use whenever the user wants to know whether review feedback is actually valid, whether a comment reflects a real issue or a misunderstanding, why a valid issue was not caught earlier, or whether the fix should stop at the review comment or also include clearer naming, doc comments, tests, or a small refactor. Prefer this skill for review-triage questions even when the user only asks "is this review fair?" or "why did we miss this?"
---

# Review Follow-Up

Start from the review comment, not from a desire to justify more work. The first job is to decide whether the review is supported by the code and requirements. Only after that should you ask why it was missed and whether any broader follow-up is warranted.

This skill is intentionally cautious. Many issues are easier to notice in review than during implementation. Do not over-rotate from "the review is valid" to "the codebase needs refactoring."

## Workflow

1. Anchor the review in the actual change.
- Read the review comment, relevant diff or commit, and the surrounding code.
- Read affected tests or docs when they change the meaning of the review.
- Separate reviewer preference from correctness, clarity, maintainability, or contract issues.

2. Assess validity first.
- Classify each review item as:
  - `Valid`
  - `Partially valid`
  - `Not supported`
- Explain the classification with concrete evidence from the code, behavior, tests, or stated requirements.
- If a comment is not supported, stop the deeper analysis there unless a short clarification would help.

3. For valid comments, ask why the issue was missed.
- Use the smallest explanation that fits the evidence.
- Prefer these buckets:
  - `Reasonable implementation miss`: an oversight, edge case, or interaction that is easier to spot after the code exists
  - `Requirement/context gap`: important context was not explicit during implementation
  - `Test gap`: coverage or scenario setup did not exercise the issue
  - `Clarity gap`: naming, structure, ownership, or data flow made the issue harder to see
  - `Documentation gap`: missing docs, doc comments, or invariants left important behavior implicit
- When the evidence is thin, say the issue was hard to see upfront instead of inventing a root cause.

4. Decide whether follow-up should extend beyond the immediate fix.
- If the issue looks like a normal implementation miss, recommend addressing the valid review and stopping there.
- If the issue traces back to unclear naming, blurred responsibilities, missing invariants, or missing docs, consider targeted follow-up.
- Prefer the smallest follow-up that would likely prevent similar misses:
  - rename a misleading symbol
  - add a focused doc comment or module note
  - tighten a boundary or split one overloaded responsibility
  - add a test covering the missed scenario
- Keep the bar high for refactors. One awkward review round is not enough on its own.

## Review Standard

Treat these as strong signals for additional follow-up:

- The same confusion would likely recur for the next reader because names or boundaries are misleading.
- Important behavior depends on an unstated invariant or contract.
- The valid review points to ownership or control-flow ambiguity that already spreads across multiple call sites.
- The fix naturally wants a small supporting test or comment because otherwise the reasoning stays hidden.

Treat these as weak signals that usually do not justify more action:

- The author simply missed a case that is only obvious in hindsight.
- The reviewer is asking for a stylistic preference without clear maintenance benefit.
- The code is slightly awkward but still locally clear and unlikely to mislead future changes.
- A doc comment would restate code that is already obvious from names and structure.

## Output

Use this structure:

### Review Assessment
- For each review item, state `Valid`, `Partially valid`, or `Not supported`.
- Include the concrete evidence.

### Why It Was Missed
- Only include items that are at least partially valid.
- State whether the miss was reasonable in context or points to a deeper clarity/documentation/design issue.

### Additional Action
- Start with one of:
  - `No additional action beyond addressing the valid review`
  - `Add targeted follow-up to reduce repeat confusion`
- If follow-up is warranted, list only the smallest useful actions and why each one clears the bar.

### Watch Items
- List plausible concerns that do not yet justify action.

### Verdict
- End with one of:
  - `Address the valid review only`
  - `Address the review and add a small supporting follow-up`
  - `Address the review and consider a targeted refactor after confirming constraints`

## Communication Rules

- Treat review comments as hypotheses to evaluate, not facts to defend.
- Lead with validity before discussing process or root cause.
- Be direct, but do not frame a missed issue as negligence without strong evidence.
- Do not recommend docs, comments, renames, or refactors just to show extra initiative.
- When a broader change is justified, explain what it would prevent and why the immediate fix alone is not enough.

## Example Triggers

- "Check whether these PR comments are actually valid before I start changing code."
- "Review these comments on my commit and tell me which ones are fair."
- "For the valid review points, tell me why we missed them and whether this suggests docs or naming problems."
- "I fixed the bug, but should I also clean up the structure that made the reviewer notice it first?"
