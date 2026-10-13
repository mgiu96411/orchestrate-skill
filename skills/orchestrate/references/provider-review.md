# Cross-provider review

Medium and large implementations require final independent review by the other provider. Claude-led work uses Codex; Codex-led work uses Claude. This is one of the normal final auditors. Spec reviewers may also use the alternate, but a spec verdict does not cover later code. Ordinary small tasks do not inherit this requirement.

## Detect the host

Identify the current host from the session's own system/developer instructions and exposed tools (Claude Code vs Codex). Shell hints corroborate but do not override them: Claude Code shells normally set `CLAUDECODE=1`; Codex shells normally expose `CODEX_*` variables. Record the detected host and the alternate in the checkpoint. If the host is still unknown, record that and ask the user once which provider should review; do not guess or review with the same provider.

| Current host | Alternate reviewer | Preflight (read-only) |
|---|---|---|
| Claude Code | Codex CLI | `codex --version` and `codex login status` |
| Codex | Claude Code CLI | `claude --version` and `claude auth status` |

Run the preflight during planning, before review depends on it. A binary on PATH is not proof of authentication. Use the configured default model; do not invent a model name, assume an account, or change providers to evade a limit. A sandbox access error is not proof the user's account is logged out: distinguish missing tooling, denied access, authentication failure, and provider failure using permitted checks.

Honor actual permission boundaries for the private code and artifacts the reviewer would process. Existing authorization persists; do not ask again when it already covers the action. Inside a Codex sandbox the Claude CLI usually needs network access, so request the normal sandbox escalation/approval for that one command; never use a bypass flag. If authorization is missing, prepare the concrete review scope before requesting it and continue independent local work. Do not work around an explicit denial by changing tools, credentials, or payload packaging.

## Dispatch a terminal reviewer

Write the reviewer brief to a file in the plan's evidence directory. Supply the literal requirements, acceptance criteria, the exact base revision, a saved diff file covering committed and relevant uncommitted changes, and the surrounding paths it may inspect. Include needed evidence without the worker's self-assessment. The reviewer returns findings and a verdict; it must not implement fixes, invoke orchestrate or council, or dispatch another cross-provider reviewer. Explicitly state these boundaries in its brief to prevent recursive orchestration.

Run from the repository root, non-interactive and read-only, as a background or long-timeout command (reviews can take several minutes):

```sh
# Claude Code host -> Codex reviewer
codex exec --sandbox read-only -c approval_policy=never --ephemeral \
  --json -o "$EVIDENCE/provider-review.md" - < "$EVIDENCE/provider-brief.md" \
  > "$EVIDENCE/provider-review-events.jsonl"

# Codex host -> Claude reviewer
claude -p --permission-mode plan --tools "Read,Grep,Glob" --output-format json \
  < "$EVIDENCE/provider-brief.md" > "$EVIDENCE/provider-review.json"
```

The Claude reviewer has no shell, so the brief must point to the saved diff file rather than ask it to run `git`. Add `--skip-git-repo-check` for Codex only when reviewing outside a Git repository. Stderr noise such as unrelated MCP connector errors is not a failure; a non-zero exit, missing output file, or empty result is.

Request concrete findings with trigger, expected/observed behavior, location, and reproduction or code-reading basis. Require acceptance coverage and unresolved limitations. Record the actual provider, model if exposed (`modelUsage` keys in Claude JSON output; Codex event metadata), session/thread identifier (Claude `session_id`, Codex `thread_id`), snapshot identity, and artifact path. A successful process exit, empty reply, or prose claim without a review result is not a pass.

Send confirmed findings to the owning worker. After fixes, have the alternate reviewer recheck the changed behavior on the integrated state with a fresh brief and the new diff. The normal two-broad-round cap still applies; use focused rechecks for unresolved findings.

## If the review cannot run

Record `pending` with the observed reason and the next required action (for example, "install Codex CLI and run `codex login`"). An independent auditor from the same provider may add useful evidence but does not satisfy the cross-provider requirement. Continue implementation and local checks that do not depend on the missing review. Do not silently downgrade the requirement or describe the medium/large pipeline as complete. An explicit user waiver permits wrapping with `waived by user` and the verification boundary stated; never relabel that as a passed review.
