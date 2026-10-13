# Systematic software audit mode

Load this reference before starting or resuming a systematic software audit. It is for software/product audits with a defined coverage request, not ordinary local review or non-software audits. Audit-only work never authorizes product writes, data mutation, deployment, or remediation; it permits only authorized plan/trail/evidence artifact writes. Record separate remediation authority at intake.

## Intake, plan, and coverage

Before systematic investigation or audit dispatch, save the Orchestrate plan and create/designate one readable Markdown trail. Reuse the campaign's `AUDITORIA.md`, `AUDIT.md`, or equivalent when present; otherwise use `docs/audits/YYYY-MM-DD-<scope>/AUDIT.md` within the already authorized filesystem scope. If repository artifact writes are explicitly disallowed, use an authorized private artifact location and put its exact path in the brief. Put the trail path in the plan and every audit brief, and link it to the user. The root is the only trail writer; workers return evidence/proposed updates. Keep sensitive mappings/evidence private; shareable artifacts use a clear pointer while a private checkpoint may retain exact resolved policy paths/hashes.

Record purpose, accepted product behavior, audit-only/remediation authority, target snapshot, requested coverage and exclusions, risk order, owners, bounded batch exit conditions, and named denominator. If the request says every function, retain that full denominator; vary depth/order by risk without silently shrinking coverage. Exclude audit scaffolding from product coverage. Track source review separately from behavioral verification.

Each finding has a stable ID and moves through `candidate`, `reproduced/qualified`, `accepted` or `rejected`, `corrected`, `independently rechecked`, and `integrated/user-flow verified` as applicable. Blocked/deferred findings keep reason, owner, and next action. Candidate count is not confirmed-bug count; a test pass count is not campaign completion.

## Readable live trail

Keep a mutable overview plus append-only journal. The overview gives last-update time and timezone, authorized purpose/scope, current batch, denominator coverage, accepted/rejected/open/blocked findings, last verified effect, integration boundary, next action, and a link to the checkpoint or detailed ledger. Do not duplicate raw logs or maintain a second generated dashboard.

Append an entry before the next dependent action for a batch start/completion; finding state change/reopen; material scope, simplicity, or routing escalation decision; failed/blocked verification; owner hand-back and independent recheck; integration; planned pause/handoff/resume; or closure. Do not log every tool call or routine pass. Each entry includes time/timezone, stable finding/batch ID, actor/role, observation or decision and reason, source snapshot/evidence link, verification state, and next owner/action. Preserve historical failures; append linked corrections instead of rewriting them. Keep secrets and unnecessary sensitive payloads out.

Example:

```md
## Current overview
Updated: 2026-09-30 10:40 UTC-03:00. Batch B3; coverage: 18/42 payment paths.
Findings: accepted F-12; open F-13; blocked none. Last verified effect: none for F-12; fix pending.
Integration: local branch only. Next: owner fix for F-12, then independent recheck. Checkpoint: <plan path>#current-checkpoint.

## Journal
- 2026-09-30 10:40 UTC-03:00 — F-12 / auditor: reproduced duplicate capture from snapshot `abc123`; expected one ledger entry, observed two; evidence `evidence/F-12.md`; accepted, owner `payments-worker` to fix then independent recheck.
```

On handoff, transfer trail ownership explicitly. If an interruption leaves a gap, reconcile artifacts read-only, label reconstructed entries and unknown times, and keep unresolved verification open. If historical checkpoints conflict, reconcile the latest applicable user instructions and evidence, designate one current checkpoint, preserve valid completed work, and avoid a blanket re-audit. On resume compare the current audited target to its recorded snapshot; for changed paths, qualify affected coverage and accepted/rejected finding evidence as stale/indeterminate, then requeue only those items and their dependent checks. A failed plan/trail write must be reported and repaired or visibly redirected before dependent work; never claim it was saved otherwise. On resume, read the overview, current checkpoint, and relevant entries/evidence rather than the whole journal by default.

## Batches, remediation, and closure

Prioritize risk, but use bounded repair batches. Tier authorized remediation normally as small, medium, or large; medium and large remediation inherit the required final alternate-provider review. Auditors return findings to the root for triage; the root sends confirmed findings to the owner, who fixes only when remediation is authorized; an independent auditor rechecks. Keep the review evidence record from [review.md](review.md), including exact before/after snapshots and same-scenario effect. Record integration status separately from corrected status.

Apply the proportionate risk-triggered controls in [review.md](review.md), rather than a universal test or mutation matrix.

Close only when coverage, every accepted/deferred/blocked finding, required independent checks, and the integration boundary are explicit. A paused campaign retains its denominator, audit trail, checkpoint, findings, and next action; resume the same campaign rather than starting a competing one or losing findings.
