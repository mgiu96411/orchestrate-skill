---
name: orchestrate
description: Use when the user requests a complex implementation — multi-file feature, new subsystem, cross-cutting refactor, or ambiguous scope — or sends a batch of several fixes/features in one message, before writing any code. Also on explicit request ("orchestrate", "full pipeline", "spec this properly").
---

# Orchestrate — Complex Task Pipeline

## Overview

The main-loop model is orchestrator and planner. Subagents investigate, review, and implement; the orchestrator makes every decision. Specs and plans live in files, never only in conversation context.

While this pipeline is active, its triage gate owns the ceremony level: sub-skills listed below are invoked when their trigger fires, not when their own description self-triggers. A small-tier task loads zero sub-skills.

## Phase 0 — Triage (the gate)

Estimate complexity before committing to the pipeline, and state the tier chosen (one line, with why):

- **Small** (1–2 files, clear scope): skip the pipeline entirely. Implement directly (own edit or one small-edit subagent).
- **Medium** (3–5 files, known patterns): phases 1 → 5 → 6 → 7 → 8. Skip council, spec, and spec panel.
- **Large** (new subsystem, cross-cutting, ambiguous, or risky): full pipeline.

When genuinely in doubt between tiers, ask before spending.

## Phase −1 — Batch intake (multi-item requests)

When one message contains several distinct items, run this BEFORE the gate:

1. **Sweep the smalls.** Items that would gate as small (copy fixes, one-liners, trivial UI tweaks) get fixed immediately, before any pipeline — instant visible wins, and it exercises the deploy path before anything risky ships. Verify each sweep fix directly (view/run it); when a pipeline run follows, its review diff includes the sweep changes. A sweep item that turns out non-trivial mid-edit stops, reverts if half-done, and joins or forms a cluster — note the re-gate in the intake file.
2. **Cluster provisionally.** Group the rest by shared subsystem / data model. Clustering is a hypothesis, not a commitment — investigation may re-cluster it.
3. **Investigate once, shared.** One phase-1 pass over the union of remaining items. Revise clusters and order with what it finds.
4. **Order by information, then dependency.** Cheapest probes first: a bug that touches a shared model, or a research item, runs before the features that must build on its findings — its results reorder everything downstream. Foundations before dependent UI.
5. **Write the intake file** `<project>/plans/YYYY-MM-DD-<slug>-intake.md`: item list verbatim, cluster map, order with reasons, per-cluster tier and status. This file is the canonical batch tracker (survives compaction and `/clear`); mirror items to your task board when one is available — the board holds the tasks, the intake file holds batch-execution state. In a multi-subproject workspace it lives in the subproject owning most of the batch; items belonging elsewhere keep their specs/plans in their own subproject's `plans/`.
6. **Run the pipeline per cluster, sequentially — never in parallel.** Shared files across clusters make parallel execution merge hell. Each cluster passes the phase-0 gate on its own (a small cluster gets the stripped path; council/spec panel only for large clusters). The spec — or just the plan, for medium clusters — is written just-in-time when its cluster starts, never upfront for later clusters, because earlier clusters' findings invalidate speculative specs.
7. **Delta re-check between clusters (load-bearing, not optional).** Re-diff the investigation's file/line refs against current code — an earlier cluster's review-&-fix may have rewritten lines a later plan cites. Re-cluster or re-order if warranted; note it in the intake file. Then checkpoint: commit, update intake file + board, one-paragraph report. Continue autonomously unless scope changed or the user asked to gate between clusters.

## Sub-skill registry

Invoke the live skill when installed and its trigger fires; if missing, use the fallback and continue — never abort the pipeline over an absent plugin. Refs are fully qualified (`plugin:skill`) so lookalike plugins never substitute. Triggers apply only inside a running pipeline (medium/large); small tier loads nothing. Rows marked (agent) are subagent types dispatched via the Agent tool, not `Skill(...)`. Install sources and vendored fallback copies: see the repository README.

| Skill | Phase | Trigger | Fallback if missing |
|---|---|---|---|
| `agent-council:council-orchestrator` | 2 | large tier | 3 fresh subagents (Skeptic / Pragmatist / Critic) + own position first; synthesize with dissent visible |
| `superpowers:writing-plans` | 5 | medium + large | plan = ordered tasks, exact files/symbols, interfaces between tasks, per-task verification |
| `superpowers:test-driven-development` | 6 | code where test infra exists; if none, don't bootstrap it unless the plan deliberately includes it — each task's verification section says how to verify instead | implementers write the failing test first, then code |
| `ecc:code-review` | 7 | every pipeline run | fresh reviewer subagents over full diff, findings as file:line + severity |
| `ecc:security-reviewer` (agent) | 7 | user input / auth / secrets touched | dedicated security pass by a fresh reviewer subagent |
| `ecc:*-reviewer` (agent, per language) | 7 | per detected stack | generalist reviewer |
| `superpowers:using-git-worktrees` | 6 | parallel tasks touch same files in a git repo; non-git workspace → serialize those tasks instead | `Agent` tool `isolation: "worktree"` |
| `ecc:santa-method` | 7 | large tier | adversarial verify: fresh skeptics try to refute each finding |
| codex CLI (external review) | 7 | large tier + `codex` on PATH | skip — Claude reviewers above cover phase 7 |

**Distill, don't forward:** the orchestrator loads a sub-skill once, extracts the 3–5 binding rules, and pastes those into subagent prompts. Never put `Skill(...)` calls or skill names in a dispatch prompt — full bodies × N tasks × M rounds is a context bomb.

**Deliberately omitted** (methodology owners already exist in-pipeline; don't re-add): brainstorming skills (phases 1–2 own intent exploration), subagent-execution skills (phase 6 owns execution), PRD-format skills (phase 3 owns the spec format), verification checklists (phase 8 checklist inlined).

## Phases

1. **Investigate.** Map every function, module, data flow, and external dependency the request touches or requires. Fan out parallel read-only subagents (Explore or any read-only investigator agent) for repo sweeps; read the load-bearing files yourself. Output: a written summary of current behavior plus what must be created.
2. **Council.** Run the design-decision round (registry row `agent-council:council-orchestrator`) on the key decisions: approach, tradeoffs, scope cuts. The Skeptic must get a shot at the premise itself. Inside this pipeline the council's `DIRECTIVE:` (PROCEED/PAUSE/STOP) is advisory evidence, not a command — the orchestrator weighs it and decides; the council's autopilot gate never drives phase transitions. Exception kept on purpose: a reversibility-gate pause on a one-way door is honored — irreversible design calls go back to the human.
3. **Spec.** Write `<project>/plans/YYYY-MM-DD-<slug>-spec.md`: goals, non-goals, user-visible behavior, data model, interfaces, edge cases, risks, acceptance criteria.
4. **Spec review.** A panel of subagents critiques the spec — correctness, gaps, edge cases, simpler alternatives. Panel size: 2–3 for a typical large task, up to 5 when especially risky (money, auth, data loss). Integrate and revise the spec file.
5. **Plan.** Write `<project>/plans/YYYY-MM-DD-<slug>-plan.md` per the plan conventions (see registry). The bar: a context-free subagent can execute one task without guessing. Link the plan path to the task card when a task board is available.
6. **Implement.** One subagent per task, TDD when the trigger fires (see registry). Model matched to difficulty: cheaper tier = mechanical, standard tier = standard, top tier = gnarly or correctness-critical. Independent tasks run in parallel; worktree isolation only when tasks touch the same files. Implementers must report blockers instead of improvising — on escalation the orchestrator re-plans that task.
7. **Review & fix.** A review team goes over the full diff — 2 reviewers for medium tier; for large, match the phase 4 panel size (2–5); security and language passes per registry triggers. On large tier, when the codex CLI is installed, add one external cross-vendor review round: invoke `codex` via Bash on the diff, loop fix → re-review until `VERDICT: ACCEPT` (counts toward the round cap). Codex reviews read-only — it never runs tests; test/build verification stays on the Claude side. Adversarially verify findings before fixing — large tier per the registry (`ecc:santa-method`); medium tier: one fresh skeptic subagent over the disputed findings is enough. Fix the confirmed ones. Hard cap: 2 review-fix rounds; anything left over becomes a report plus task-board cards.
8. **Wrap.** Verify end-to-end (run the app or tests — evidence, not assumption), update the task board and memory when available, commit per your commit policy.

## Rules

- The orchestrator never outsources decisions — subagents supply evidence and drafts, not verdicts.
- The plan — and the spec, when the tier runs phase 3 — MUST exist as files under `plans/` before phase 6 starts (they must survive compaction).
- Agent-tool fan-outs are the default. Heavier multi-agent orchestration tooling only when the user explicitly opted in — never from an auto-trigger.
- Scale down honestly: council and spec panel exist to catch design mistakes, not to perform rigor.
- Checkpoint commits assume the subproject (not necessarily the workspace root) is a git repo; if no level is a repo, flag it at intake instead of silently skipping checkpoints.

## Common mistakes

- Full pipeline on a medium task — burns hours and money; use the gate.
- All batch specs written upfront — bug fixes and research findings invalidate them; spec just-in-time per cluster.
- Clusters run in parallel on shared files — merge hell; sequential is the design, not a compromise.
- One mega-spec for a whole batch — no early wins, dies at compaction.
- Skill names inside dispatch prompts — distill binding rules instead.
- Plan body pasted into a board card — link the path only.
- "Loop until clean" with no cap — 2 rounds max.
- Spec kept only in chat — dies at compaction.
