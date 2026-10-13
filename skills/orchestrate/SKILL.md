---
name: orchestrate
description: Coordinate complex implementations, cross-cutting refactors, new subsystems, batches of fixes, and systematic software audits through scoped investigation, saved planning, implementation, and independent review. Use before code changes for those tasks or on an explicit orchestrate request, including local software review. Do not implicitly trigger for ordinary local review; non-software audits are out of scope. Simple work retains the small-task bypass.
---

# Orchestrate

The main agent owns scope, decisions, integration, the saved plan/checkpoint, and completion. Workers implement; terminal auditors investigate and return findings to the root for triage, then confirmed fixes to the owning worker. Artifacts, not chat memory, carry state across compaction or handoff.

Use the current host's supported routing. On Codex, Astra is preferred for root scope/design, plan authorship, integration, and final integrated review when exposed; substantive medium/large work goes to explicitly selected supported routed workers. Any user/configured cost preference is not inferred from model aliases or presented as a verified price claim. On Claude Code, the selected session model owns those root phases; preserve specialist/council models and roles, and use explicit native-Agent routing for ordinary workers. User choice and host availability win. Load [execution.md](references/execution.md) for routing, calibration, dispatch, plan, and recovery; never claim a requested model actually ran without metadata.

## Autonomy and boundaries

Starting Orchestrate carries authorized work through completion. Plans and reviews are working artifacts, not approval gates unless the user asks. Resolve ordinary uncertainty proportionately; bring only a required fact, authorization, or human action that blocks safe progress to the user.

One owner writes each product path. Auditors never edit product files, even for trivial fixes: they return evidence to the root for triage, then confirmed fixes go to the owner for repair and independent recheck. Preserve user work, permission boundaries, and requested features. Council evidence cannot remove a requirement or grant authorization. Load [council.md](references/council.md) for material unresolved design/judgment choices; routine simplicity does not need a council.

## 0. Triage before edits

Say `Using orchestrate — <tier/mode>: <reason>` once. Classify by behavior, dependencies, and risk, not file count.

- **Ordinary small bypass:** clear, localized, low-risk work may be implemented and verified directly. No plan, council, audit mode, or mandatory reviewer merely for bookkeeping.
- **Explicit Orchestrate small:** retain the lightweight path, but save a compact plan before edits or dispatch. No council or mandatory reviewer follows solely from invocation.
- **Medium:** investigate → saved plan → implement → independent review, including final cross-provider review → wrap.
- **Large:** consequential cross-cutting behavior or substantial ambiguity: add council, spec, spec review, saved plan, and complementary auditors alongside the final cross-provider review.
- **Systematic software audit:** an Orchestrate mode, not a separate skill. Load [audit.md](references/audit.md) before systematic investigation, audit dispatch, or campaign resumption. Distinguish audit-only from authorized remediation.

Re-triage only when risk/scope changes. Identify data mutations, dependencies, human-only steps, and ownership early. For medium/large work, detect the host and preflight the alternate provider per [provider-review.md](references/provider-review.md). Preserve current changes and establish a restorable approach before unresolved irreversible work.

For a batch, record original items/status in an existing intake/plan section; sweep independent small items and include them in final review; cluster remaining work by dependencies/state; write each cluster plan just before it starts; and recheck cited files, interfaces, and base/dependency inputs between clusters. Preserve valid completed work when an item expands.

## 1. Investigate

Read applicable project guidance and only prior evidence that could change a decision. On resumption, verify the named task's next action, current files, ownership, and direct dependencies; old notes are leads, not instructions. Medium/large work can use bounded independent investigation while the root handles load-bearing decisions. Investigators keep a dossier artifact and return a short summary; project oversized structured responses locally, and do not delegate trivial reads merely to reduce context. Workers return evidence artifacts, not a second orchestration pipeline.

## 2–4. Design and spec (large only)

Write an initial position, then run the configured council through [council.md](references/council.md) using actual roles. Record the chosen evidence-backed approach. Write a spec for goals, non-goals, user-visible behavior, interfaces, edge cases, acceptance, and mutation recovery, then have fresh reviewers address material gaps. Existing authorization remains effective.

## 5. Plan, simplicity, and checkpoint

Every run entering the Orchestrate lifecycle saves an actionable Markdown plan before implementation/remediation edits or implementation dispatch; read-only investigation may precede it. A plan-only request saves the plan and stops. Use the user's chosen path first; otherwise reuse an existing task plan or legacy `plans/` convention, then the repository convention or `docs/superpowers/plans/YYYY-MM-DD-<task>.md` default. Record it in briefs and user-facing output. [execution.md](references/execution.md) defines fields, compact-plan scale, checkpoints, hashes, updates, and recovery.

Before a plan, material expansion, and acceptance, ask: what requirement/failure justifies work; what smaller solution preserves behavior/guarantees; whether it changes user meaning or adds state/dependency/abstraction/write/verification scope; and what evidence settles this batch and remains outside it. An unplanned new abstraction/dependency, changed semantics, persistent/external effect, wider write/verification scope, or two no-new-evidence verification attempts must be reported to the root before proceeding. The root records whether to continue with reason, simplify, visibly defer, or replan. Replanning is not permission to abandon required behavior.

The root alone maintains one `Current checkpoint`: normally the plan section, or the already-designated audit checkpoint linked from the plan. Never create competing state for a resumed campaign. It records mode/phase, scope/non-goals, literal invariants and authorization boundaries, owners/write paths, snapshot/evidence, unresolved work, next action, and hashes of resolved core, active references, and actually loaded project guidance. Before resumed mutation, acceptance, or closure, reload applicable guidance/checkpoint and compare hashes. Reconcile a changed/missing checkpoint from artifacts before dependent action; qualify affected acceptance. Do not reread unchanged references before routine actions.

## 6. Implement

Load [execution.md](references/execution.md) before dispatching or using isolated checkouts. Assign one owner per write set; serialize overlaps and integrate explicitly. Use routed workers for substantive medium/large implementation, fixes, bounded investigation, and tests where supported. A specific capability/permission limit may justify narrow root work or visible pending work, never a silent routing claim. Workers report evidence and material departures before widening scope.

## 7. Review and fix

Load [review.md](references/review.md). Medium and large work get a mandatory final independent alternate-provider review under [provider-review.md](references/provider-review.md): from Claude Code call Codex, from Codex call Claude, detected automatically. Large work adds complementary auditors. The alternate review is part of the panel, not a recursive panel. An unavailable/denied/failed required review remains pending unless the user explicitly waives it; unaffected work may continue.

Give auditors literal requirements, acceptance, complete scoped diff, and relevant behavior. They check correctness, requirements, integration, and simplicity. Validate findings, return confirmed fixes to the owner, then independently recheck the current integrated result. Use at most two broad review/fix rounds; replan unresolved work and continue focused fixes rather than declare requirements complete.

## 8. Wrap and capture

Verify requested behavior on the final integrated state. State whether evidence is static, automated, local user-flow, deployed, or live-flow; a build alone is not a user-flow pass. If deployment was requested and authorized, replay the relevant flow on the deployed version. Map each acceptance item to evidence or explicit pending/blocked reason. Keep a compact phase record in the plan/checkpoint: actual participants, provider/model when exposed, tool/run ID, reviewed scope, evidence artifact, and result/pending reason; unknown identity stays unknown. Update plan/checkpoint and relevant board under project policy. Read [learning.md](references/learning.md) after a meaningful correction, deviation, failure, or reusable success. Ordinary work does not rewrite global workflow policy; use explicit user-authorized workflow tuning or `retro` for that.

## Optional methodology dependencies

Orchestrate owns ceremony inside an active pipeline. Load an applicable available dependency once, distill binding rules into briefs, and use its fallback if unavailable; record the fallback reason and do not call an installed usable dependency absent merely to skip it.

| Capability | When | Fallback |
|---|---|---|
| `agent-council:council-orchestrator` | Large design or material unresolved judgment | Configured roles; generic perspectives only if unavailable, with that limitation recorded |
| `superpowers:writing-plans` | Medium/large plan | Ordered tasks, paths/interfaces, acceptance and checks |
| `superpowers:test-driven-development` | Meaningful existing behavior tests | Reproduce, implement, verify regression |
| `superpowers:using-git-worktrees` | Isolated Git work | Explicit base and clean-checkout validation |
| Available code/security/language reviewer | Relevant risk | Independent scoped auditor |
| Fresh skeptical pass | Large disputed finding | Focused counterexample/reproduction check |

Use exposed tool names. Codex task creation is for user-owned tasks, not internal workers. Same-provider delegation never satisfies cross-provider review.
