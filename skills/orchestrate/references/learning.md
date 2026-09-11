# Capture lessons while evidence is fresh

Use the shared private journal at `~/.local/share/orchestrate/RETRO-NOTES.md`, unless the user has configured another location in persistent guidance. Both Claude and Codex use the same journal. Keep session excerpts, client details, and private paths outside the shareable skill repository.

Append a short record after a meaningful correction, process deviation, failure, or reusable success. The main agent is the sole writer for a run. Preserve existing records; use a structured file API or a safely quoted heredoc. If the journal is not writable, save the entry with the project plan and report its path for later ingestion.

Record:

`ID | date | task/session + message/line or artifact reference | phase | observation | impact | candidate lesson | confidence | applied/pending`

Use a stable unique ID, for example task ID plus event number. Distinguish observed evidence from interpretation. Include one tiny sanitized excerpt only if it is necessary to understand the lesson. Do not store credentials or duplicate full transcripts. Record positive evidence as well as failures. If usage/time figures are available, state their source and scope; estimates are estimates, unavailable values are unknown.

An explicit user preference can justify a change immediately. Repeated failures strengthen a proposed rule; a single consequential demonstrated failure can also justify a narrowly scoped fix. A one-off inconvenience is not automatically a global prohibition. Do not infer savings from a token counter without a comparable baseline.

The companion `retro` skill reviews unprocessed notes and representative recent sessions, updates the appropriate skill/instructions within the user's request, and records what it consumed. Ordinary implementation captures evidence; it does not independently change global workflow rules.

MemPalace can provide related evidence or store a compact index when the current host exposes the needed tool. Follow existing project/wing conventions. If only search is available, retain the local journal and report that no memory write occurred; do not claim a saved diary or invent a write tool.
