---
description: Regenerates the `agent.md` file from the current state of the `_docs/` folder. This file is the "shared brain" — every agent (Antigravity, OpenCode) reads this as context before starting work.
---

Read the contents of the `_docs/` folder and regenerate the `agent.md` file in the project root.

## Source files (read in order):

1. **_docs/_templates/AGENT-template.md** — Preserving the basic structure and manual sections.
2. **_docs/index.md** — OKF bundle root index; sanity-check that nothing listed there is missing below.
3. **_docs/tasks/*.md** (and `_docs/tasks/index.md`) — All tasks (status, priority, Agent summary).
4. **_docs/journal/*.md** (and `_docs/journal/index.md`) — The last 3 journals (newest by date).
5. **_docs/DECISIONS-LOG.md** — ADR blocks (only the last 10-15).
6. **_docs/log.md** — Last 5-10 entries, for a quick "what changed recently" cross-check against the journals.
7. **_docs/arch/*.md** and **_docs/spec/*.md** — Architectural and business context.
8. **_docs/README.md** — Project description and tech stack basis.

Note: `/agent-sync` never writes to `_docs/index.md` or `_docs/log.md` — those are maintained by `/task-new`, `/task-close`, `/journal-distill`, and `/project-sync`. It only reads them.

## Generation Rules:

1. **Boundaries:** Always respect the `<!-- AGENT_SYNC_BOUNDARY -->` marker. Only overwrite the part above it!
2. **About the Project:** Summarize the essence based on `README.md` and `arch/`/`spec/` files (2-3 sentences).
3. **Tech Stack:** Collect technologies (source: README and `TECHNOLOGIES` field in TASKs).
4. **Current Status:** Count tasks by status (done, in-progress, planning).
5. **Active Tasks:** In tabular format, in priority order.
6. **Recent Journals:** The last 3 journals: `JOURNAL-ID (date): [Summary]`.
7. **Key Decisions:** Highlight the most important ones from `DECISIONS-LOG.md` (max 10).
8. **Open Questions:** Collect from TASKs and JOURNALs (without duplicates).

## /agent-sync command execution:

- Do not ask back.
- Find or create `agent.md` in the root.
- If it already exists, preserve the manual part below the boundary.
- Output: "`agent.md` successfully updated. [N] active tasks, [N] new decisions recorded."

---

## AGENT.MD SIZE LIMITS

`agent.md` should remain under ~150 lines for efficient readability.

- Active tasks: max 10 (the rest: "+ N more").
- Key decisions: max 10.
- Open questions: max 10.
- Journals: max 3.
