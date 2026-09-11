# The user's council inside orchestrate

Use the custom `agent-council:council-orchestrator` to resolve material uncertainty during the pipeline, not just to review large designs. First investigate checkable facts and reuse already settled decisions. For remaining assumptions or competing approaches, formulate the proposed direction, alternatives, known evidence, and the goal the user's task serves. A council is unnecessary for factual lookups or ordinary mechanical choices.

## Resolve the real council

Prefer the available named council skill. If the host does not expose it, use the council source root declared in persistent guidance or the installed plugin's resolved location. Read `skills/council-orchestrator/SKILL.md` and the relevant `agents/` definitions, `docs/CONTRACT.md`, and `docs/LOGGING.md` from that root. Do not interpret plugin-root-relative paths as relative to the skill subfolder.

Use the council's tier and membership rules for the concrete decision, plus its actual member prompts and `agents/council-synthesizer.md`. On Claude, use registered council agents when available. On Codex, dispatch the same role instructions through the available internal subagent tools, using supported configured models rather than literal Claude model names or nonexistent Task tools. Distill relevant instructions and preserve verdict contracts. Do not substitute a generic panel merely because named agent types are not registered. If the source resources are unavailable, record that limitation and use fresh skeptical/assumption-focused perspectives as a fallback.

## Embedded mode: resolve, then continue

Inside orchestrate, the main agent owns phase transitions. Apply the following integration rules rather than the standalone council's operator-prompt and autopilot cascade:

- Prepare a concrete brief from the task and available context; do not ask the user to restate an ordinary decision. The main agent supplies its proposed answer and assumptions for challenge without steering members toward agreement.
- Verify checkable premises through available code, documentation, or tools. Do not ask “verify now or proceed-at-risk?” Label facts that remain unknown; the council evaluates whether a reversible assumption or further investigation can resolve them.
- Preserve the synthesizer's actual `PROCEED`, `PAUSE`, or `STOP` and its reasoning. These are evidence for the main agent, not automatic user interrupts. `PAUSE` or `STOP` means address the objection: investigate, implement a necessary guard within scope, or choose/revise the approach. Recheck changed premises or disputed concerns with the relevant members as needed. Do not blindly continue through an unresolved correctness or authorization blocker.
- Treat `ONE_WAY` as a requirement to assess actual effects, recovery limits, and existing authorization. It does not itself require renewed approval for an already authorized action. The council cannot invent permission, credentials, private facts, or a human-only action.
- Record the council recommendation, assumptions, and the main agent's decision separately. Never attribute the agent's choice to the user or fill the operator's call slot unless the user actually made that call. Keep council logging and its anti-mirroring separation intact.

Continue implementation after resolving the question. Avoid repeated whole-council loops over unchanged evidence. Escalate to the user only under orchestrate's autonomy default: a required fact, authorization, or human action remains unavailable and prevents safe progress after useful independent work. This embedded mode does not alter standalone `/council` behavior or change `~/.council-config.json`.
