---
name: minimal-change-engineering
description: Implement focused changes in an existing codebase with call-path analysis, minimal scope, behavior-oriented tests, and risk-scaled verification. Use when fixing a bug, adding a small feature, or refactoring while preserving established boundaries and unrelated user work.
---

# Minimal Change Engineering

Make the smallest coherent change that satisfies the current requirement and can be verified at the behavior-owning boundary.

## Understand Before Editing

1. Read the repository instructions and inspect the working tree before planning changes.
2. Trace the relevant entry point, callers, key functions, parameters, data shapes, and existing tests. Do not infer behavior from file names alone.
3. Normalize the requirement into stable behavior. Do not encode clarification history, mistaken examples, or negotiation artifacts in code or tests.
4. Identify the affected boundary and likely failure modes. For deletion, writes, authentication, type changes, persistence, or external APIs, state the trigger, impact, and fallback before editing.
5. Ask for clarification only when an unresolved semantic choice changes observable behavior, such as ordering, membership, deletion scope, or target inclusion.

## Design the Change

- Reuse existing helpers, modules, and conventions before adding dependencies or architecture.
- Keep the edit inside the modules that own the behavior. Avoid unrelated cleanup, broad renames, formatting churn, and cross-module refactors.
- Add an abstraction only when it removes current complexity, meaningful duplication, or an established source of variation.
- Prefer explicit upstream inputs over downstream inference when the caller already knows the branch or mode.
- Do not add compatibility branches or special terminology for a one-time misunderstanding. Compatibility must correspond to a durable input, persisted state, or external contract.
- Preserve unrelated modifications in a dirty worktree. Never make their removal part of the implementation.

## Keep the Implementation Legible

- Put the normal path in a positive branch and use guard clauses when they make exceptional paths clearer.
- Choose variables and branches that make inputs, outputs, and reasons apparent in one reading.
- Keep comments for non-obvious invariants and tradeoffs, not line-by-line narration.
- When a schema, header, or column definition is paired with row data, update both and preserve their ordering contract.
- Delete obsolete code only when its callers, persisted inputs, and fallback behavior are understood.

## Verify at the Right Boundary

Start with the smallest command that exercises the changed behavior, then broaden verification according to the change's blast radius.

- Test stable, observable behavior through the interface that owns it.
- For a reproduced bug, verify the original failing path plus the narrowest useful regression test.
- For an external API change, first validate the real response shape and types with a minimal probe, then run syntax and focused tests.
- Verify that configuration changes reached the running process when activation requires a reload or restart.
- Run wider tests for shared contracts, persistence, authentication, or cross-module behavior.

Read [testing principles](references/testing-principles.md) when adding, removing, or redesigning tests.

## Handle Repeated-Key Scans Deliberately

When synchronizing tables or lists with repeated keys, read [grouped scan performance](references/grouped-scan-performance.md). Avoid restarting a full target scan for every source item when a cursor or index can preserve progress.

## Deliver an Auditable Change

Before editing, summarize the intended files, behavior, and risk. After editing:

1. Show the key diff or describe the exact behavioral delta.
2. State when new files are untracked and therefore absent from ordinary `git diff` output.
3. Report verification commands and their results.
4. Explain remaining validation gaps and any fallback behavior.
5. Commit only when requested, staging only files that belong to the change.
