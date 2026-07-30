# orchestrate

A Claude Code skill that turns the main model into an **orchestrator** for
complex implementations: it triages every request by complexity, and — when the
work is big enough to deserve it — runs a full pipeline of investigation,
design council, spec, spec review, planning, multi-agent implementation, and
adversarial review, with every decision kept in the orchestrator's hands.

Small tasks pass through untouched. That's the point: the triage gate makes it
cheap to invoke on borderline requests.

## The pipeline

```
Phase −1  Batch intake     — sweep trivial items, cluster the rest, order by information
Phase 0   Triage gate      — small (skip pipeline) / medium (slim path) / large (full)
Phase 1   Investigate      — parallel read-only subagents map the territory
Phase 2   Council          — adversarial design review of the key decisions   (large only)
Phase 3   Spec             — written to plans/, survives context compaction   (large only)
Phase 4   Spec review      — 2–5 critic subagents                             (large only)
Phase 5   Plan             — ordered tasks a context-free subagent can execute
Phase 6   Implement        — one subagent per task, TDD, parallel where safe
Phase 7   Review & fix     — reviewer panel + security pass + optional codex
                             cross-vendor review; findings adversarially verified
Phase 8   Wrap             — end-to-end verification with evidence, then commit
```

Key design rules:

- **The orchestrator never outsources decisions.** Subagents supply evidence
  and drafts, not verdicts.
- **Specs and plans are files, not chat.** Everything load-bearing lives under
  `plans/` and survives `/clear` and context compaction.
- **Batches run sequentially per cluster.** Parallel clusters on shared files
  are merge hell; sequential is the design.
- **Review loops are capped.** Two review-fix rounds, then a report.

## Install

Copy the skill into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills
cp -R skills/orchestrate ~/.claude/skills/
```

Then either let it self-trigger on complex requests, or invoke it explicitly
with `/orchestrate`.

## Dependencies (all optional)

The skill's sub-skill registry references a few plugins. **Every row has a
built-in fallback** — the pipeline runs fine with none of them installed; the
plugins just make each phase stronger.

| Dependency | Used for | Install | Without it |
|---|---|---|---|
| [agent-council](https://github.com/mgiu96411/agent-council) | Phase 2 design council: tiered adversarial members, deterministic synthesizer, reversibility gate | `/plugin marketplace add mgiu96411/agent-council` | 3 fresh subagents (Skeptic / Pragmatist / Critic) |
| [Superpowers](https://github.com/obra/superpowers) | Plan-writing conventions, TDD discipline, worktree isolation | `/plugin install superpowers` | Inlined fallback rules per registry row |
| [ECC](https://github.com/affaan-m/ECC) | Code review format, adversarial verification (santa-method), security reviewer, per-language reviewers | See ECC repo | Fresh generalist reviewer subagents |
| [codex CLI](https://github.com/openai/codex) | Optional phase-7 cross-vendor review round (large tier only) | `npm i -g @openai/codex` | Skipped — Claude reviewers cover phase 7 |

Unmodified copies of the specific Superpowers and ECC pieces the registry
references are vendored under `third_party/` (MIT, see
[THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md)) so you can read exactly
what the registry expects — or copy them into `~/.claude/skills/` — without
installing the full plugins. They are frozen at the versions noted there;
install the upstream plugins for the latest.

### A note on the codex row

The external-review row comes from real use: a 26-round audit where the codex
CLI reviewed each fix cycle read-only and returned `VERDICT: ACCEPT/REJECT`
until the diff came back clean. Cross-vendor review catches defects a
same-family reviewer misses. Caveat baked into the skill: codex reviews code
only — it never runs tests, so test/build verification stays on the Claude
side.

## Task board integration

The skill mentions mirroring batch items and linking plans to a task board.
Any board works (or none) — the intake file under `plans/` is the canonical
tracker either way.

## License

MIT — see [LICENSE](LICENSE). Vendored third-party portions keep their
upstream MIT licenses — see [THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md).
