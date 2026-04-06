---
name: lean-review
description: Review code for unnecessary structure, speculative design, and low-value complexity without attacking readability. Use whenever the user asks whether code is overengineered, too abstract, too configurable, too future-proof, whether extra DB columns/fields/helpers should exist, whether a change feels heavier than necessary, or wants help simplifying code while keeping readability benefits such as clear names, small object boundaries, or helpful file structure.
---

# Lean Review

Review code with one question in mind: does each piece of structure earn its keep right now?

This skill is for conservative simplification review, not minimalism for its own sake. Do not flatten or remove structure that materially improves readability, naming, local reasoning, or ownership boundaries. A single-use helper, nested directory, or small composed object can be worth keeping when it makes the code easier to understand.

## Workflow

1. Anchor the review in the real code.
- Read the changed files, nearby interfaces, and affected tests or docs.
- Distinguish between:
  - necessary structure serving current behavior
  - readability-oriented structure that pays for itself now
  - speculative or low-value structure added mostly for hypothetical future use

2. Look for lean-review signals.
- Extra DB columns, JSON fields, DTO properties, or config keys that current behavior does not meaningfully use.
- Helpers or wrappers that mostly forward calls without improving naming, boundaries, or reuse.
- Extension points, strategy objects, plugin hooks, or flags added for imagined future cases.
- Generic abstractions that force repeated mapping or translation without reducing real duplication.
- Multiple layers of indirection around a concept that still has only one concrete path.
- Options, enums, or mode switches where only one case is valid in practice.
- State or persistence added before there is a concrete consumer, producer, or invariant that needs it.
- Module splits or directory nesting that increase hunting rather than understanding.

3. Check whether the structure earns its cost.
Ask what the extra layer buys now:
- clearer names or call sites
- a stronger type boundary
- a sharper ownership boundary
- reduced repeated branching or glue code
- simpler invariants
- isolation of ugly external details
- clearer file or directory-level comprehension

If the answer is weak or hypothetical, treat it as a simplification candidate.

4. Keep the simplification bar high.
- Do not recommend removal just because something is abstract.
- Do not punish code for being explicit.
- Do not treat readability-oriented structure as waste.
- Recommend simplification only when the extra structure adds recurring maintenance cost, obscures the real flow, or mainly exists for futures the code does not currently need.

## Review Standard

Treat these as strong signals that code may be too heavy:

- A new field, column, option, or hook was added for future flexibility, but current behavior barely touches it.
- A helper or abstraction adds another jump without improving naming, reuse, invariants, or boundaries.
- Callers still need to know internal details the abstraction claimed to hide.
- One concept now requires extra mapping objects or translation layers purely to preserve genericity.
- A single implementation is wrapped in extension machinery that has no concrete second use in sight.
- The current feature became harder to follow because the design optimized for hypothetical later work instead of today's path.

Treat these as signs to keep the current structure:

- A single-use helper materially improves the signature, name, or readability of the call site.
- A small object or composed value groups related data in a way that makes invariants easier to see.
- Nested modules or directories help a reader predict where responsibility lives.
- A wrapper isolates awkward dependency details or side effects behind a clearer seam.
- A little duplication is cheaper than a shared abstraction, but the current structure is still locally clear.

## Output

Use this structure when reporting:

### Findings
- List only concrete speculative or low-value complexity issues supported by the code.
- For each issue, explain why the structure does not appear to earn its cost right now.
- Support each issue with a short embedded snippet when the code itself is central to the point.

### Keep As-Is
- Call out choices that may look heavy at first glance but are worth keeping for readability, naming, or boundary clarity.
- Say what benefit they provide now.

### Simplification Candidates
- Include only changes that clear the bar above.
- For each one, state:
  - the smallest reasonable simplification
  - what it would remove or collapse
  - what would be gained
  - what could be lost
  - why now is or is not the right time

### Open Questions
- List missing constraints or future requirements that could justify the current shape.

### Verdict
- End with one of:
  - `Already lean enough`
  - `Small simplification is justified`
  - `Broader simplification may help, but only after clarifying constraints`

## Snippet Rules

- Prefer embedded snippets over editor-style line references.
- Put the source file path on the first line of each snippet as a comment.
- Keep snippets tight: signatures, schema definitions, wrappers, config shapes, and translation layers are usually enough.

## Communication Rules

- Be direct and specific.
- Default to the smallest useful recommendation.
- Separate readability wins from speculative complexity; they are not the same thing.
- If the code is already appropriately lean, say so plainly.
- If a concern is plausible but weakly supported, label it as a watch item rather than a recommendation.
- Do not manufacture simplification opportunities just to make the review feel useful.

## Example Triggers

- "Is this overengineered?"
- "Do we actually need these extra fields and helpers?"
- "Can you review this diff for speculative code or unnecessary abstractions?"
- "I want this implementation to stay lean. What should we remove, if anything?"
- "Check whether this new schema and helper layer is earning its keep."
