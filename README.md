# orchestrate

A shared Claude Code and Codex skill for complex implementations and systematic software audits. The main agent owns decisions; workers implement; independent auditors return findings to the root for triage and every confirmed product finding to the owning worker, including trivial fixes. In Codex's routing profile, Astra is preferred for root reasoning, planning, integration, and final review when live identity confirms it; explicit supported routed workers handle substantive medium/large work. User/configured cost preference is not inferred from model aliases or presented as a verified price claim. In Claude Code, the selected session model owns root phases while native Agent workers use Haiku for bounded mechanical work, Sonnet for normal implementation and ordinary audits, and Opus for difficult or correctness-critical tasks. User model choices, enforcement, cost constraints, and actual host availability take precedence.

The entrypoint chooses small, medium, or large based on behavior and risk. Ordinary small edits bypass the pipeline; explicit small Orchestrate runs save a compact plan. An explicit Orchestrate request may cover local software review at the appropriate tier, while ordinary local review does not implicitly trigger the skill. Medium tasks use investigation, a saved plan, implementation, and independent review. Large tasks add design council and spec review. A systematic software audit is an Orchestrate mode with a named coverage denominator and one readable Markdown trail that records live progress, evidence, handoffs, and resumption. Non-software audits are outside scope.

## Prerequisites for cross-provider review

Medium and large runs call the *other* agent's CLI for a read-only final review, so install and sign in to both:

```sh
# Claude Code CLI
claude --version && claude auth status     # sign in: claude auth login
# Codex CLI
codex --version && codex login status      # sign in: codex login
```

If the alternate CLI is missing or signed out, the review is recorded as `pending` with the fix to apply; the run is not reported complete unless you explicitly waive it.

## Local installation

Use this repository as the canonical source. Link the skill folders into the host's discovery directory so edits cannot drift between copies. If a destination already exists, inspect and back it up before replacing it.

```sh
# Run from this repository. These commands expect unused destination names.
mkdir -p "$HOME/.agents/skills" "$HOME/.claude/skills"
ln -s "$PWD/skills/orchestrate" "$HOME/.agents/skills/orchestrate"
ln -s "$PWD/skills/retro" "$HOME/.agents/skills/retro"
ln -s "$PWD/skills/orchestrate" "$HOME/.claude/skills/orchestrate"
ln -s "$PWD/skills/retro" "$HOME/.claude/skills/retro"
```

Invoke `orchestrate` or `retro` by name in the host's skill selector or prompt (`$orchestrate` / `$retro` in Codex; `/orchestrate` / `/retro` in Claude Code). When Codex recognizes the repository plugin namespace, the selector names are `orchestrate:orchestrate` and `orchestrate:retro` (verified on this installation). A concise global `AGENTS.md` or `CLAUDE.md` rule can request orchestrate for complex implementation work. If discovery does not refresh, restart the host. Verify the skill appears in the actual available-skills list; readable files alone do not prove discovery.

The existing Claude plugin packaging remains available:

```text
/plugin marketplace add mgiu96411/orchestrate-skill
/plugin install orchestrate@orchestrate-skill
```

Plugin installations use the published version, which may differ from local uncommitted edits. Avoid duplicate active copies of the same skill.

## Learning loop

Orchestrate appends meaningful lessons to the private shared journal at `~/.local/share/orchestrate/RETRO-NOTES.md`. `retro` reviews unprocessed notes and representative session evidence on request, makes scoped changes when authorized, tests them against realistic scenarios, and records consumed evidence in `RETRO-LOG.md`. It preserves notes and a backlog instead of discarding records or pretending every session was reviewed.

Private transcripts, client data, retrospective reports, and backups stay outside this repository. No recurring background job is installed. Memory connectors are optional; a host with search-only access uses the local journal for writes.

## Conditional references and dependencies

The orchestrate entrypoint links execution, audit, review, and learning details when needed. Codex collaboration workers and Claude native Agent workers select an explicit supported model instead of relying on inherited routing; registered Claude specialist/council roles retain their configured models unless the user overrides them. Plans carry the root-owned current checkpoint, including hashes of active policy and actually loaded project-guidance files, so a resumed run can reload and reconcile changed instructions. Plans also record actual phase participants, exposed provider/model identity, tool/run ID, review scope, evidence, and pending results. Council, TDD, worktree, and reviewer skills retain inlined fallbacks where applicable. Only available, applicable dependencies are loaded once per run. Medium and large tasks require a final independent review by the alternate provider (Claude → Codex, Codex → Claude), counted within the normal review panel. The skill detects its host and calls the other CLI in read-only mode automatically. Unavailable or denied reviews remain pending unless explicitly waived by the user; local work continues. Ordinary small tasks retain their bypass. Same-provider delegation is not cross-provider review.

Vendored Superpowers and ECC references under `third_party/` remain frozen at the versions in [THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md). They are not automatically installed or loaded. Skill-specific host and permission requirements still apply.

## License

MIT — see [LICENSE](LICENSE). Vendored third-party components retain their licenses in [THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md).
