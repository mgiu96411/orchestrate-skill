# Worker execution and context

## Dispatch brief

Give each worker only what it needs to start safely:

- Objective, acceptance examples, constraints, and owned write paths.
- Checkout path and exact expected starting revision; for uncommitted dependencies, identify the included working-tree snapshot and required files explicitly.
- Relevant plan task and dossier paths, with symbols/sections or short ranges. Permit following dependencies when needed; line numbers are navigation aids, not a ban on inspecting surrounding code.
- Interfaces shared with other workers, integration order, and verification commands.
- Relevant distilled skill rules, report destination, and escalation conditions.

Use available model choices and the user's configured preference; do not invent model names or change providers to bypass a limit. Keep the main agent's decision context small without withholding evidence it needs.

## Checkout preflight

Resolve the intended base from current integrated work, not a stale session-start HEAD or an assumed `main`. Have the worker compare its actual starting revision and dependency files before edits. If they differ, stop that worker's edits and reconcile with the orchestrator. Preserve dirty changes; do not prescribe `reset --hard`, overwrite another worker's files, or discard user work. Recreate a clean isolated checkout from the correct base or integrate the required dependencies deliberately.

Different write paths can still share schema, generated files, database fixtures, or build outputs. Serialize or isolate those resources as needed; probes that commit state need disposable resources or verified cleanup beyond transaction rollback. Revalidate the integrated result after merging; worker-local success is not integration success.

Before a final integrated run, stabilize its application code, test sources, fixtures, and generated inputs together, or test an isolated snapshot. Record the revision plus relevant uncommitted inputs; restart long-lived processes when they cache changed sources. If covered inputs change during the run, record which versions are known and mark affected coverage as stale or indeterminate; rerun affected checks on the final state. Unrelated work may continue.

## Reports and follow-ups

Workers keep full evidence in a file. Return a compact completion summary: status, changed paths/commit, tests actually run and outcome, caveats/blockers, and report path. Aim for roughly 200–400 words; don't drop a critical failure or inventedly truncate at a fixed character count. Auditors may read the full evidence.

Return fixes to the same worker using the host's follow-up/message facility. Include finding IDs, reproduction, expected behavior, and scope. Ask for a report-only continuation first if an interrupted worker may already have finished. Resume state from artifacts rather than restarting an entire task. Use a replacement only when necessary, with the same handoff facts.

## Waiting and limits

Use completion notifications or bounded blocking waits, not repeated status messages. While agents work, perform useful integration, investigation, or documentation work without overlapping their write sets. Follow the host's progress-update cadence, and communicate changed findings, decisions, or blockers.

On a rate limit or failed worker, stop redundant dispatch into the same exhausted capacity. Preserve current state, completed/pending tasks, worker IDs, revisions, evidence, and next action in the plan. Continue unaffected work. Use an already authorized alternative if available and suitable; do not assume access to a different account/provider or spend. Mark failed or incomplete review as such. If blocked on the user, identify the concrete action needed rather than waiting silently. Do not create a recurring automation unless the user requests future execution.
