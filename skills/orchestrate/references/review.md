# Independent review and feedback

## Auditor contract

Review product changes without editing product source, tests, configuration, or documentation. You may run non-destructive checks and create throwaway fixtures/build outputs in an isolated test environment. Do not modify production data, deploy, install dependencies, or run mutating integration tests solely because the auditor role exists; use the task's actual authorization and environment.

Read the literal request and acceptance examples, the full scoped diff, and relevant surrounding behavior. Include requirement coverage and integration risks, not only stylistic review. Where useful, execute a focused reproduction with synthetic inputs. Placeholder credentials are suitable only when the check does not need a real integration; otherwise report that boundary. If execution is unavailable, distinguish a code-reading finding from a reproduced failure.

For large tasks, include the required alternate-provider reviewer according to [provider-review.md](provider-review.md). Judge stateful behavior across transitions and the executed data path; a node having no outgoing edges does not establish that earlier or parallel effects are safe. Use the original acceptance criteria, not a worker's preferred interpretation.

For each actionable finding report:

- Stable ID, severity, exact path/symbol/line.
- Input/trigger, expected versus observed behavior, and supporting evidence.
- Impact and a suggested direction or regression case, without applying the fix.

Even a one-character fix returns to the worker. This maintains one write owner and keeps the reviewer independent. A reviewer can dispute another reviewer's claim without changing code.

## Triage and recheck

The main agent accepts, rejects, or requests evidence for each finding and records why. Validate claims before assigning fixes; fix size is not evidence of correctness. Check that the reproduction represents the requested contract: for stateful or cumulative examples, specify opening state, dates, prior actions, and reset boundaries, and distinguish action deltas from lifetime totals. A fixture defect is not a product defect; current code is not by itself the authority for intended behavior.

For parsing, normalization, identity detection, or other heuristic changes, prefer the actual data contract over guessing from shape. Probe both error directions and counterexamples that change the context or composition while preserving the apparent match. For context-sensitive language, include the reversed context, multiple candidates, and an explicit switch; inspect structured actions as well as wording. Preserve the intended fallback when evidence is ambiguous. Use a fresh skeptic for large-task disputes; for medium work, a focused independent counterexample check is enough.

Send confirmed findings to the owning worker. The worker reports the fix, regression evidence, and any residual issue by finding ID. Re-review the changed behavior on the current integrated revision, including side effects of the fix. A prior rejection remains unresolved until the relevant evidence is rechecked; silence, a crashed agent, or tests run before the last fix do not count as acceptance.

After two broad rounds, replan the unresolved findings and continue narrowly. Deferred nonblocking issues should be explicitly scoped and recorded; correctness, requested behavior, and authorization blockers must not disappear into a task board while the task is called complete.

## Model and scenario evidence

For model-driven behavior, retain the request, raw response, post-validation result, source/config/catalog identities, and actual model metadata available from the run. Score each original criterion against the observed turns, including refusals, ambiguity, prior unanswered questions, and resets. A valid JSON response or validator status `ok` is not a business pass; a correction made by the validator is not evidence that the model complied unaided. Preserve failed samples and historical artifacts. Separate replay of recorded outputs, synthetic regression tests, and new live model runs, and derive reported case/turn totals from the evidence rather than hand-written counts.
