# Testing Principles

Test the contract that users or callers can observe, not the current implementation technique.

## Choose the Boundary

Prefer an existing public seam owned by the behavior, such as a service method, parser, renderer, command, API route, or persisted state transition. Use a lower-level seam when the broader one is slow or nondeterministic, but keep the assertion semantic.

Expected values should come from a specification, fixture, protocol, or user-visible contract. Do not calculate the expected result with the same logic used by production code.

## Avoid Brittle Contracts

Avoid tests that primarily lock down:

- private method calls or internal data layout;
- exact collaborator call sequences when the outcome is what matters;
- prompt wording or volatile model output;
- incidental formatting outside a renderer or serialization contract;
- clarification history or a mistaken example that is not a durable input.

String assertions are appropriate when the exact string or serialization is itself the public contract. Otherwise prefer structured fields, status values, parsed output, durable side effects, or semantic error codes.

## Use TDD Selectively

Write a focused failing behavior test when it clarifies the requirement or prevents a reproduced defect. An existing failing test, CI signal, or minimal reproduction can serve as the red step.

Implement only enough to make the behavior pass. Refactor after green, keeping the refactor local and rerunning the focused test after each meaningful change.

Regression tests should cover defects that were reproduced and can plausibly recur. Replace or remove tests that preserve obsolete behavior or fail when internals change without a contract change.
