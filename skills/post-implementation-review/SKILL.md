---
name: post-implementation-review
description: Review code after implementation work to identify design flaws, abstraction issues, or maintenance risks that only became clear once real code was written. Use whenever the user asks whether a recent change exposed architectural problems, whether an abstraction is fighting the implementation, or whether a refactor is justified. Prefer embedded snippets with file-path comments over editor-oriented file and line references. Be conservative and avoid suggesting refactors without concrete evidence of recurring cost or complexity.
---

# Post-Implementation Review

Review the code after the implementation exists. Focus on issues that were hard to see upfront and only became obvious once the change touched real interfaces, control flow, state, or tests.

This skill is for cautious design review, not aggressive redesign. A strange-looking design is not enough on its own. Prefer keeping the current structure when the evidence is thin or when hidden constraints could plausibly explain the shape.

## Workflow

1. Anchor the review in the actual change.
- Read the changed files, nearby interfaces, and affected tests or docs.
- Distinguish between:
  - pre-existing design debt
  - issues introduced by the change
  - issues the change merely made easier to see

2. Look for post-implementation signals.
- Repeated branching, mapping, or glue code that suggests the abstraction boundary is off.
- A module now knows details it should not need to know.
- The change forced edits across too many files for one concept.
- Naming became awkward because responsibilities are blurred.
- Data is being reshaped repeatedly between layers.
- Tests became hard to set up because dependencies or ownership are misplaced.
- The implementation needed workarounds that will probably repeat.

3. Keep the refactor bar high.
- Do not recommend a refactor just because another design looks cleaner in theory.
- Do not treat unfamiliar code as broken code.
- Assume there may be constraints you cannot yet see unless the code gives clear contrary evidence.
- Suggest refactoring only when the current shape creates recurring cost, blocks straightforward extension, or makes correctness meaningfully harder to preserve.

4. Separate evidence from recommendation.
- First state what the implementation revealed.
- Then state whether that evidence is strong enough to justify action.
- If the evidence is weak, say so directly and recommend leaving the design in place for now.

## Review Standard

Treat these as strong signals for a refactor recommendation:

- The same workaround or translation layer now appears in multiple places.
- A single change required cross-cutting edits that are likely to recur.
- One abstraction claims to hide complexity but instead leaks it to callers.
- Ownership of data, side effects, or orchestration is ambiguous enough to create likely bugs.
- The new code had to special-case around an existing interface in a way that will spread.

Treat these as weak signals that usually do not justify a refactor by themselves:

- The code feels inelegant but is still locally understandable.
- A helper or abstraction is slightly misnamed but still usable.
- There is some duplication, but it is small and isolated.
- The current design is awkward only for one edge case with no sign of repetition.

## Output

Use this structure when reporting:

### Findings
- List only concrete design or abstraction issues supported by the code.
- For each issue, explain why it became visible after implementation rather than being obvious upfront.
- Support each issue with a short embedded snippet when existing code is central to the point.

### Keep As-Is
- Call out odd-looking choices that should probably remain unchanged for now.
- Explain what evidence is missing for a refactor recommendation.

### Refactor Candidates
- Include only changes that clear the bar above.
- For each one, state:
  - the smallest reasonable refactor scope
  - what it would improve
  - what it would cost or risk
  - why now is or is not the right time

### Open Questions
- List hidden constraints, ownership questions, or missing context that could change the recommendation.

### Verdict
- End with one of:
  - `No refactor recommended`
  - `Small targeted refactor is justified`
  - `Broader redesign may be warranted, but only after clarifying constraints`

## Snippet Rules

- When citing existing code, prefer embedded snippets over bare file paths and line numbers.
- Put the source file path on the first line of each snippet as a comment.
- Match the comment style to the language when practical, for example `// src/service.ts` or `# backend/jobs/sync.py`.
- Keep snippets tight and evidence-focused: signatures, branches, translations, ownership boundaries, and repeated glue code are usually enough.
- Use multiple small snippets instead of one large excerpt when separate points need separate evidence.
- Use file and line references only when a snippet would add noise or the exact location itself is the point.

## Communication Rules

- Be direct and specific.
- Prefer file-level or seam-level observations over vague architectural commentary.
- Do not force the user back into the editor just to follow the review.
- If there are no meaningful design flaws, say so explicitly.
- If a concern is plausible but not yet well supported, label it as a watch item rather than a recommendation.
- Do not manufacture findings to make the review feel useful.

## Example Triggers

- "Now that this feature is implemented, do you see any design flaws we missed?"
- "Did this change reveal any abstraction problems?"
- "Should we refactor this now, or leave it alone?"
- "Review this implementation and tell me if the current design will age poorly."
