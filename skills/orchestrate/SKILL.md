---
name: orchestrate
description: Coordinate complex implementations, cross-cutting refactors, new subsystems, and batches of fixes through scoped investigation, planning, implementation, and independent review. Use before code changes for these tasks, or on explicit orchestrate requests. Simple tasks use the small-task bypass.
---

# Orchestrate

The main agent owns scope, decisions, integration, and completion. Workers implement; auditors investigate findings and report them back. Specs, plans, and evidence live in files so work can resume after context loss. Use the current host's tools and configured models; do not assume Claude is the orchestrator or Codex is only a reviewer.

## Autonomy default

Starting orchestrate means carry the requested work through to completion within its authorized scope. Do not ask for permission to start, approve the tier/spec/plan, move between phases, choose ordinary implementation details, or apply confirmed reviewer fixes. Plan and review files are working artifacts, not approval gates, unless the user explicitly requested a gate.

Investigate uncertainty, choose reasonable assumptions and reversible defaults, record consequential choices, and continue. Worker questions and reviewer disagreements go to the orchestrator. Resolve factual gaps through investigation; bring material unresolved assumptions, competing approaches, and judgment calls to the user's agent council via [council.md](references/council.md), at whichever phase they arise. The council challenges the options; the orchestrator adopts an evidence-backed decision and continues. Interrupt the user only for a required fact, authorization, or human action that cannot be resolved from available evidence and prevents safe progress; continue useful independent work first. Existing authorization persists, and the host's actual permission rules still apply.

## 0. Triage before edits

Say `Using orchestrate — <tier>: <reason>` once. Classify by behavior, dependencies, and risk; file counts are hints, not thresholds.

- **Small:** clear, localized, low-risk edit. Implement and verify directly. No council, plan file, sub-skill loading, or mandatory reviewer. If an auditor is already involved, its findings still return to the worker.
- **Medium:** known patterns with several connected changes. Investigate → plan → implement → review → wrap.
- **Large:** new subsystem, substantial ambiguity, or consequential cross-cutting behavior. Add design council, spec, and spec review before planning.

Re-triage follow-ups when scope or risk changes, not on every message. Resolve ordinary uncertainty with a proportionate investigation and reasonable assumptions under the autonomy default above.

Locate the owning subproject, Git root, current changes, and existing plan conventions. Identify persistent-data mutations, integration dependencies, and human-only steps early. Preserve the user's requested scope and existing authorization. A council cannot silently remove a requested capability or grant permission for an external action. Prepare a concrete reversible or restorable approach before any unresolved irreversible operation.

## Batch intake

For several distinct requests:

1. Record the original items and status in `plans/YYYY-MM-DD-<slug>-intake.md` (or the project's existing planning location).
2. Sweep independent small edits and verify directly. If an item expands, preserve valid work and reclassify it; do not revert other work. Include sweep changes in the eventual review.
3. Investigate the remaining items together, then cluster by shared model/subsystem. Run cheap probes before decisions that depend on them.
4. Execute clusters sequentially when they share state or files. Write each cluster's plan just before it starts; do not speculate every later spec upfront.
5. Between clusters, re-check cited files, interfaces, and base revisions. Update intake and checkpoint under the project's commit policy. Mirror status to an available task board with file links, not copied plan bodies. Continue within the authorized scope.

## 1. Investigate

Read relevant project guidance and retrieve a small amount of related session/memory evidence when it can change a decision. Old notes are leads to verify against current code, not current instructions.

When resuming a named task, plan, or handoff, use its next actionable item as the initial candidate and verify the current files, relevant board state, ownership, and direct dependencies. Check standing guardrails and reopened work without turning intake into an audit of every historical reference or unrelated subproject. If a structured tool response is oversized, project the relevant fields locally before reading them; simple parsing/filtering does not warrant a subagent. Continue independent work while any necessary interpretation runs; missing information blocks only the actions it can affect.

For medium/large tasks, delegate bounded independent investigations when useful while doing substantive local work. Investigators return a dossier file with paths, symbols, observed behavior, unknowns, and evidence; send only a short summary into the main context. The main agent reads the load-bearing code needed for decisions. Do not delegate trivial reads solely to reduce its context.

## 2–4. Design and spec (large only)

Write your initial position, then run the user's `agent-council:council-orchestrator` through [council.md](references/council.md), using its actual role definitions, tier selection, and synthesizer. Do not replace an available custom council with a generic reviewer panel. This scheduled design round is for large tasks; a focused council can also resolve a material question that arises during a medium task. A localized task whose uncertainty warrants council should be re-triaged first. Council verdicts are evidence; resolve their concerns, record the chosen approach, and continue under the autonomy default.

Write a spec covering goals, non-goals, user-visible behavior, data/interfaces, edge cases, acceptance criteria, and recovery for consequential mutations. Have 2–3 fresh reviewers critique it, then resolve the material gaps. Previously authorized design choices do not require another approval merely because a council calls them irreversible. Resolve scope details through investigation and reasonable assumptions; escalate only a genuine blocker under the autonomy default above.

## 5. Plan

Before implementation, save and verify `plans/YYYY-MM-DD-<slug>-plan.md`, or use the project's existing equivalent. Include ordered tasks, owning worker, precise files/symbols, interfaces, acceptance examples, verification, and prerequisites. Link the spec when one exists. A plan written after code changes is a retrospective record, not evidence of planning before implementation. Keep tasks small enough to complete with a focused brief; split at dependency boundaries, not arbitrary token counts.

Load [execution.md](references/execution.md) before dispatching workers or preparing isolated checkouts. It defines briefs, base checks, concise reports, and interruption handling.

## 6. Implement

Assign one owner per write set. Independent tasks may run concurrently; overlapping edits serialize or use separate checkouts with an explicit integration order. One worker may own several tightly coupled tasks. The main agent may implement when delegation adds no value, but independent review still applies to medium/large work. State a material departure from the plan and why.

When existing test infrastructure supports a meaningful behavioral check, reproduce the failure before fixing it. Do not bootstrap test infrastructure or add tests that merely mirror a cosmetic edit unless the plan calls for it. Workers report evidence and blockers rather than silently changing scope.

## 7. Review and fix

Load [review.md](references/review.md). Medium tasks normally use one independent auditor plus targeted validation of disputed findings; add another for distinct uncovered risk. Large tasks normally use 2–3 complementary auditors. Give them the literal request, acceptance criteria, complete review diff including sweep changes, and relevant code; do not prime them with the worker's claim that it is correct.

**Large tasks require an independent cross-provider review of the final changes.** Use the authorized alternate provider when available: Claude-led work uses Codex; Codex-led work uses Claude. Count this reviewer within the panel, not as another mandatory panel. Resolve availability and authorization during planning using [provider-review.md](references/provider-review.md). Medium tasks may use it when risk warrants; small tasks retain their bypass. An unavailable, denied, or failed review stays visibly pending unless the user explicitly waives it; continue unaffected work and do not report the full pipeline complete.

**Auditors never edit product files, including trivial fixes.** They report findings and evidence. The main agent triages them and sends confirmed findings to the owning worker, preferably the same worker, who fixes and reports verification. The auditor rechecks the changed result. A replacement worker gets a concise handoff if the original is unavailable. If the main agent was the implementer, it receives the feedback as the worker; it must not claim its own check was independent.

Use at most two broad review/fix rounds. After that, stop repeated whole-diff panels: replan remaining blockers and use focused fixes/rechecks. The cap does not permit declaring unresolved required behavior complete. Report any actual blocker and the next action.

## 8. Wrap and capture

Verify the requested behavior on the final integrated state. Distinguish static checks, automated tests, local user-flow checks, deployment confirmation, and live-flow checks. A successful build or served version string is not proof the user flow works. If deployment was requested and authorized, replay the relevant flow on the deployed version; otherwise state the actual verification boundary. Discover missing credentials or human-only steps during planning, and surface any remaining blocker clearly.

Keep a compact phase record in the plan: actual participants, provider/model when exposed, tool/run identifier, evidence artifact, and result or pending reason. Unknown model identity stays unknown; a role label or requested model is not proof of the model used. Map every original batch acceptance item to final evidence, including failures and omissions; passing a subset does not complete the batch.

Update intake/plan, relevant board, and commit according to the project's policy. Report outcome, meaningful checks, and remaining limitations. Keep skill usage/skip reasons in the plan for triggered dependencies, not a user-facing receipt for every possible skill.

When a correction, failed approach, process deviation, or reusable success occurred, read [learning.md](references/learning.md) and capture a short evidence-linked lesson. No automatic rewrite of global policy during ordinary implementation. Use the companion `retro` skill when the user requests workflow improvement.

## Optional methodology dependencies

Inside an active pipeline, orchestrate's tier owns ceremony. Load an available relevant skill once, extract its binding rules into briefs, and avoid distributing whole skill bodies to each worker. If a dependency is absent, incompatible with the host, or temporarily unavailable, use the fallback and record the reason. Do not call an installed usable dependency absent merely to skip it.

| Capability / known skill | When | Fallback |
|---|---|---|
| `agent-council:council-orchestrator` | Large design round; material unresolved assumptions or judgment calls at any phase | Resolve the configured source and dispatch its actual role definitions; generic perspectives only if those resources are unavailable, with that limitation recorded |
| `superpowers:writing-plans` | Medium/large plan | Ordered tasks, exact symbols, interfaces, per-task checks |
| `superpowers:test-driven-development` | Meaningful behavior tests with existing infrastructure | Reproduce failure, implement, verify regression |
| `superpowers:using-git-worktrees` | Isolated Git work | Explicit base revision + clean checkout validation per execution reference |
| `ecc:code-review` or equivalent | Medium/large review | Independent auditor over the full scoped diff |
| `ecc:security-reviewer`, language reviewers | Relevant risk or unfamiliar runtime | Auditor focused on the affected trust boundary/runtime |
| `ecc:santa-method` | Large-task disputed findings | Fresh skeptical reproduction/counterexample pass |

Use currently exposed tool names. Codex task creation tools are for user-requested standalone tasks, not internal subagents. Launching Codex from Codex does not create cross-provider independence. Missing tooling reduces the available evidence, never turns an unperformed check into a pass.
