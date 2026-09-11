# Cross-provider review

Large implementations require final independent review by the other provider. Claude-led work uses Codex; Codex-led work uses Claude. This is one of the normal final auditors. Spec reviewers may also use the alternate, but a spec verdict does not cover later code. Medium work uses judgment; small tasks do not inherit this requirement.

## Establish availability during planning

Inspect the host's exposed connector or CLI and existing session authorization. Use the configured model; do not invent a model name, assume an account, or change providers to evade a limit. A binary on PATH is not proof of authentication. A sandbox access error is not proof the user's account is logged out: distinguish missing tooling, denied access, authentication failure, and provider failure using permitted checks.

Honor actual permission boundaries for the private code and artifacts the reviewer would process. Existing authorization persists; do not ask again when it already covers the action. If authorization is missing, prepare the concrete review scope before requesting it and continue independent local work. Do not work around an explicit denial by changing tools, credentials, or payload packaging.

## Dispatch a terminal reviewer

Use a fresh, bounded read-only reviewer context. Supply the literal requirements, acceptance criteria, exact current diff/base plus relevant uncommitted inputs, and the surrounding code it may inspect. Include needed evidence without the worker's self-assessment. The reviewer returns findings and a verdict; it must not implement fixes, invoke orchestrate or council, or dispatch another cross-provider reviewer. Explicitly state these boundaries in its brief to prevent recursive orchestration.

Request concrete findings with trigger, expected/observed behavior, location, and reproduction or code-reading basis. Require acceptance coverage and unresolved limitations. Record the actual provider, model if exposed by the response, tool/session/run identifier, snapshot identity, and artifact path. A successful process exit, empty reply, or prose claim without a review result is not a pass.

Send confirmed findings to the owning worker. After fixes, have the alternate reviewer recheck the changed behavior on the integrated state. The normal two-broad-round cap still applies; use focused rechecks for unresolved findings.

## If the review cannot run

Record `pending` with the observed reason and the next required action. An independent auditor from the same provider may add useful evidence but does not satisfy the cross-provider requirement. Continue implementation and local checks that do not depend on the missing review. Do not silently downgrade the requirement or describe the full large-task pipeline as complete. An explicit user waiver permits wrapping with `waived by user` and the verification boundary stated; never relabel that as a passed review.
