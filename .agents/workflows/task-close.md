---
description: Closes a TASK file, verifies with user, updates tracking/board, and offers relevant sync commands.
---

## Documentation Workflow

Apply **Documentation Workflow Skill** closing rules.

### /task-close [TASK-NNN]

Close the active task.

#### 1. Execution Steps

1. **Locate Task**: Identify active `_docs/tasks/TASK-NNN_slug.md`.
2. **Update Content**:
   - Set frontmatter: `status: "done"`, `completed: <YYYY-MM-DD>`.
   - Complete `Result` section and `Agent summary` block.
3. **Decisions (ADR)**: If an architectural/stack/trade-off decision was made, update `_docs/DECISIONS-LOG.md` (`accepted` | `deprecated` | `superseded`).
4. **Human Verification Gate**: Ask user for explicit confirmation before finalizing close.
   - On confirmation, append to frontmatter: `verified: [{ by: "human:<username>", at: "<ISO8601>" }]`.
   - Never self-verify on agent authority.
5. **Bookkeeping**:
   - Move task entry in `_docs/tasks/index.md` from "Active" to "Done".
   - Append to `_docs/log.md`: `**Close**: TASK-NNN closed.`
   - Run `/board-sync` to update `_docs/BOARD.md`.
6. **Follow-up & Maintenance Offers**:
   - If open questions remain, propose a follow-up task.
   - Offer `/project-sync` to refresh `PROJECT-OVERVIEW.md` and `_docs/index.md` (never write dynamic status to `agent.md`).
   - Offer `/agent-sync` only if *stable* facts changed (stack, directory layout, conventions).
   - If understanding a code unit required disproportionate discovery effort, propose `/module-sync <module-name>` with a 1-sentence seed.

### /task-close with-journal [TASK-NNN]

1. Run `/journal-distill`.
2. Link the generated JOURNAL in the TASK file.
3. Execute standard `/task-close` steps above.
