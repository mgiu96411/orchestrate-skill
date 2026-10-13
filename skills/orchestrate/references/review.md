# Independent review and feedback

## Auditor contract

The root's integrated review is never independent. On Codex, Astra is preferred for that root review only when caller metadata confirms `gpt-6-astra`; otherwise record the limitation. On Claude Code, the selected session model performs it. Route independent audits by [execution.md](execution.md): standard explicit worker for ordinary review and strong profile for difficult/correctness-critical disputes; requested identity is not observed identity. If no standard independent auditor is available, a fresh explicitly selected `gpt-6-astra` auditor is a narrow exception only when host and user model/cost constraints permit it; otherwise independent review remains pending. Retain medium/large alternate-provider review exactly as [provider-review.md](provider-review.md) requires.

Auditors do not edit product source, tests, configuration, or documentation. They may run non-destructive checks and isolated throwaway outputs under task authorization. Do not deploy, install dependencies, mutate production data, or run mutating integrations solely for review.

Read literal request, acceptance, full scoped diff, relevant behavior, and integration path. Do not prime any auditor with the worker's claim that the result is correct or tests pass. Check requirement coverage, user-visible effects, state transitions, and scoped dependencies. When useful, reproduce with synthetic inputs; distinguish reading evidence from reproduced failure. Each finding gives stable ID/severity, exact location, trigger, expected/observed behavior, impact, evidence, and direction without applying the fix.

## Simplicity and evidence challenge

Independently check the live simplicity decision: requirement/credible failure, smaller sufficient option, added user meaning/state/dependencies/abstractions, widened write/verification scope, and batch-settling evidence. A smaller option that loses behavior is not valid. Flag unreported material expansion, unrelated cleanup, or two no-evidence attempts; return it to root/owner for continue-with-reason, simplify, visible deferral, or replanning. Do not recurse on reports or this check.

For an accepted change keep compact evidence: requirement/failure trigger; exact old/new snapshots; independently stated expected result; same-scenario before/after effect; unchanged controls; proportionate negative/concurrent/recovery cases; review disposition; and integration boundary. Use risk-triggered controls, not a universal matrix: durable retry needs intent/payload/receipt; failed read is not empty/zero; numeric work needs exact boundaries; concurrent writers may require all writers, lock order, and re-read; guards belong before side-effect seams; UI changes need the actual consumer. Fixture repair is not product fix; isolated fix is not deployed improvement; build is not user-flow pass. Performance claims need comparable workload, equality, and measurement.

## Triage and recheck

Root accepts, rejects, or requests evidence and records why. Reconcile review coverage across every changed path, including early sweep changes; an omitted shard cannot pass a file. Verify contract context: opening state, dates/prior actions, reset boundaries, and deltas where relevant. Distinguish product, fixture/test, environment, and insufficient-evidence failures. For heuristic/context changes, test both error directions and counterexamples; preserve intended fallback when ambiguous.

Return confirmed fixes to owner. Owner reports fix/evidence/residual issue by ID; independently recheck on current integrated revision, including side effects. Silence, crash, or pre-fix tests is not acceptance. After two broad rounds, replan blockers and use focused rechecks; deferred nonblocking work stays visible, while requested behavior, correctness, and authorization blockers cannot disappear.

## Model and scenario evidence

For model-driven behavior retain request, raw response, post-validation result, source/config identities, and exposed model metadata. Score original criteria against observed turns, refusals, ambiguity, unanswered questions, and resets. A valid JSON response or validator `ok` is not a business pass, and validator correction is not unaided model compliance. Separate replayed outputs, synthetic regressions, and new live runs; preserve failures and derive counts from evidence.
