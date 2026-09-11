# orchestrate

A shared Claude Code and Codex skill for complex implementations. The main agent owns decisions; workers implement; independent auditors return every product finding to the owning worker, including trivial fixes.

The entrypoint chooses small, medium, or large based on behavior and risk. Small edits bypass the pipeline. Medium tasks use investigation, a saved plan, implementation, and independent review. Large tasks add design council and spec review. Batch clusters execute in dependency order with their state saved in files.

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

The orchestrate entrypoint links execution, review, and learning details when needed. Optional council, planning, TDD, worktree, and reviewer skills retain inlined fallbacks. Only available, applicable dependencies are loaded, once per run. Large tasks require a final independent review by the authorized alternate provider (Claude → Codex, Codex → Claude), counted within the normal review panel. Unavailable or denied reviews remain pending unless explicitly waived by the user; local work continues. Medium tasks use judgment and small tasks retain their bypass. The plan records actual phase participants and evidence; Codex invoking Codex is not cross-provider review.

Vendored Superpowers and ECC references under `third_party/` remain frozen at the versions in [THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md). They are not automatically installed or loaded. Skill-specific host and permission requirements still apply.

## License

MIT — see [LICENSE](LICENSE). Vendored third-party components retain their licenses in [THIRD-PARTY-LICENSES.md](THIRD-PARTY-LICENSES.md).
