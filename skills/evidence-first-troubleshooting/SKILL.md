---
name: evidence-first-troubleshooting
description: Diagnose local runtime, extension, CLI, authentication, networking, and integration failures from observable evidence. Use when symptoms are ambiguous, logs contain multiple warnings, environments behave differently, or a suspected cause needs validation before configuration changes.
---

# Evidence First Troubleshooting

Find the first verified break in the failing path, then apply the smallest fix at the component that owns it.

## Establish the Failure

1. State the exact user-visible symptom and the condition that would prove recovery.
2. Record the relevant timestamps, versions, runtime arguments, configuration scope, and execution location.
3. Map the path across process and machine boundaries, such as UI, extension host, child process, proxy, and upstream service.
4. Reproduce the failure with the narrowest available action before changing state.

Treat a working environment as a control. Compare it with the failing environment and find the first observable divergence instead of listing every difference.

## Classify Evidence

Assign each observation one role:

- **Primary failure:** directly prevents the requested behavior.
- **Consequence:** occurs because an earlier step failed.
- **Background warning:** unrelated work that can fail without blocking the requested behavior.
- **Unknown:** needs a targeted test before classification.

Attribute errors to the component that emitted them. A network, authentication, or dependency warning does not establish causality unless the failed request belongs to the user-visible path.

## Test Hypotheses

For each plausible cause, state:

- the evidence supporting it;
- the observation it predicts;
- the smallest reversible test;
- the result that would falsify it.

Change one variable at a time. Prefer read-only inspection and process-scoped experiments before persistent configuration changes, reinstallations, credential changes, or data deletion.

If a setting only takes effect at process startup, verify the new process arguments after restarting. Do not infer that a configuration edit was loaded merely because the file changed.

## Fix and Verify

Apply the narrowest change at the failing boundary. Preserve unrelated user changes and avoid broad environment cleanup.

After the fix:

1. Re-run the original reproduction.
2. Verify the expected behavior, not only the absence of an error line.
3. Check that the process and configuration actually reflect the change.
4. Report remaining warnings separately unless they affect the acceptance condition.
5. State any verification gap that requires another machine, account, or external service.

## Report Format

Keep the conclusion auditable:

1. **Observed failure** - exact symptom and first failing boundary.
2. **Evidence** - relevant logs, versions, process state, and comparison results.
3. **Ruled out** - hypotheses disproved by a concrete observation.
4. **Fix** - minimal change and why it targets the failure.
5. **Verification** - original behavior retest and residual risk.

Redact secrets from commands, logs, screenshots, and reports.
