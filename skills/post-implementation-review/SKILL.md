---
name: post-implementation-review
description: Review implemented code for design flaws, abstraction issues, structural problems, or refactors that only became clear in real code. Use when a recent change may have exposed architectural problems, over- or under-structure, or whether a refactor is worth doing now. Prefer embedded snippets with file-path comments over editor-oriented file and line references. Default toward credible design or readability improvements, call out real tradeoffs, and separate straightforward recommendations from items worth discussion.
---

# Post-Implementation Review

Review the code after the implementation exists. Focus on issues that were hard to see upfront and only became obvious once the change touched real interfaces, control flow, state, tests, or module boundaries.

This skill is for improvement-oriented design review after the real code exists. Default toward recommending a cleanup when the implementation reveals a credible design or readability improvement, even if the gain is modest or the refactor touches a fair amount of code. Do not let change size alone veto a recommendation. If tradeoffs, uncertainty, or hidden constraints matter, surface them clearly.

It is still acceptable for the review to conclude that the implementation is fine. Do not invent a flaw just to produce feedback. If the code does not reveal a meaningful improvement, say plainly that there is nothing worth changing right now.

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
- The implementation introduced speculative structure that current behavior barely uses: extra fields, hooks, wrappers, options, or generic layers.
- The feature now feels too flat or mixed: readers must scan unrelated files or responsibilities to follow one concern.
- The code technically works, but a targeted refactor would make local reasoning, naming, ownership, or seam boundaries meaningfully clearer.

3. Prefer credible improvements, not only obvious failures.
- Do not recommend a refactor just because another design looks cleaner in theory.
- Do not treat unfamiliar code as broken code.
- Assume there may be constraints you cannot yet see unless the code gives clear contrary evidence.
- Recommend a refactor when the implementation shows a solid improvement path for readability, boundaries, extension, or correctness, even if the current code still works.
- Do not reject a worthwhile recommendation just because it touches many files. Discuss that cost when it matters, but size alone is not a veto.

4. Separate evidence, recommendation, and decision level.
- First state what the implementation revealed.
- Then state whether there is a likely recommended path.
- Classify every finding or recommendation into one of two buckets:
  - `Bucket I — Straightforward / recommended`: likely has a clear answer or strongly recommended fix path; present briefly for awareness.
  - `Bucket II — Worth discussing`: real tradeoffs, multiple plausible directions, unclear constraints, or technical decisions.
- Use Bucket II for the items that deserve the user's attention.

## Review Standard

Treat these as strong signals for a refactor recommendation:

- The same workaround or translation layer now appears in multiple places.
- A single change required cross-cutting edits that are likely to recur.
- One abstraction claims to hide complexity but instead leaks it to callers.
- Ownership of data, side effects, or orchestration is ambiguous enough to create likely bugs.
- The new code had to special-case around an existing interface in a way that will spread.
- Naming or control flow is now consistently awkward enough that a targeted cleanup would materially improve readability.
- The current shape is workable, but a concrete refactor would noticeably reduce mental overhead or repeated explanation.

Treat these as weak signals that often do not justify action on their own:

- The code feels a bit inelegant but is still locally understandable.
- A helper or abstraction is slightly misnamed but still usable.
- There is some duplication, but it is small and isolated.
- The current design is awkward only for one edge case with no sign of repetition.

If the evidence is weak, say so. A clean review is still valid.

## Output

Use this structure when reporting:

### Bucket I — Straightforward / Recommended
- Put findings or recommendations here when there is a likely answer or a strongly recommended path that probably does not need much user review.
- Keep these concise.
- For each item, include:
  - what the implementation revealed
  - why it became visible after implementation
  - the recommended fix in short form
  - the main cost or tradeoff only if it matters
- Support with a short embedded snippet when existing code is central to the point.

### Bucket II — Worth Discussing
- Put findings or recommendations here when they involve real tradeoffs, technical decisions, hidden constraints, or multiple plausible directions.
- For each item, include:
  - what the implementation revealed
  - why it became visible after implementation
  - the main options or decision points
  - the tradeoffs, risks, or uncertainty
- Support with a short embedded snippet when existing code is central to the point.

### Keep As-Is
- Call out odd-looking choices that should probably remain unchanged for now.
- Explain what evidence is missing for a change recommendation.

### Verdict
- End with one of:
  - `No meaningful improvement identified`
  - `Recommended targeted refactors, mostly straightforward`
  - `Recommended changes include technical decisions`
  - `Broader redesign may be warranted after clarifying constraints`
- If Bucket I or Bucket II is empty, say so explicitly instead of implying hidden findings.

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
- Default toward surfacing net-positive design or readability improvements, even when the gains are modest.
- Do not treat code churn alone as a reason to avoid recommending a refactor.
- Surface real tradeoffs when they exist.
- Keep Bucket I concise so the user can scan past the obvious items quickly.
- Put the user's attention on Bucket II.
- If there are no meaningful design flaws or worthwhile improvements, say so explicitly.
- If a concern is plausible but not yet well supported, put it in Bucket II or `Keep As-Is` rather than overstating confidence.
- Do not manufacture findings to make the review feel useful.

## Structure and Complexity Concerns

When a post-implementation review also raises questions about speculative structure, unnecessary complexity, over-engineering, flat organization, or mixed responsibilities, review those concerns inline here instead of delegating elsewhere.

Check structure in both directions:
- Over-structure: extra fields, config keys, DTO properties, wrappers, extension points, strategy objects, mode switches, or mapping layers that current behavior does not meaningfully use.
- Under-structure: flat directories mixing unrelated concerns, modules handling multiple capabilities at once, or related files scattered in ways that make one feature harder to follow.

Use the same posture here:
- Do not remove structure that already improves naming, local reasoning, ownership boundaries, or filesystem-level comprehension.
- Do not add nesting just because a directory has many files.
- Treat readability-oriented helpers, small composed objects, and clear subdirectories as legitimate value even when they are single-use.
- Recommend simplification or reorganization when the current shape adds recurring maintenance cost, obscures the real flow, or when a credible cleanup would materially improve readability or ownership clarity.
- Do not reject a structural recommendation just because the reorganization would touch many files; surface the cost and tradeoff instead.

Treat these as strong signals:
- A new field, option, or hook exists mostly for hypothetical future cases.
- A helper or abstraction adds another jump without improving naming, boundaries, invariants, or reuse.
- Callers still need to know internal details the abstraction claimed to hide.
- One concept now requires repeated mapping or translation just to preserve genericity.
- The filesystem or module layout no longer helps a reader predict where a concern lives.
- A small or medium reorganization would make one concern much easier to follow end to end.

Treat these as weak signals:
- The code is a bit inelegant but still locally understandable.
- A helper is slightly misnamed but still usable.
- There is some duplication, but it is small and isolated.
- A flat directory is still small and each file name clearly signals its purpose.

When these concerns matter, keep using the same report structure:
- Put straightforward structural fixes in `Bucket I — Straightforward / Recommended`.
- Put structural items with meaningful tradeoffs or unclear boundaries in `Bucket II — Worth Discussing`.
- Use `Keep As-Is` for structure that pays for itself now.
- If the implementation is structurally fine, say so plainly.

## Example Triggers

- "Now that this feature is implemented, do you see any design flaws we missed?"
- "Did this change reveal any abstraction problems?"
- "Should we refactor this now, or leave it alone?"
- "Review this implementation and tell me if the current design will age poorly."
- "Now that this is implemented, is the structure earning its keep or did we overbuild it?"
- "This feature works, but did the implementation reveal that the module layout is too flat or mixed?"
