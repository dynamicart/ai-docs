---
description: Closes the current TASK file, updates DECISIONS-LOG.md, and optionally runs journal-distill.
---

## Documentation Workflow

Apply the closing rules of the **Documentation Workflow Skill**.

### /task-close [TASK-NNN]

Close the active task.

1. Identify the active TASK file.
2. Update the header: `status: "done"`, `completed: [today's date]`.
3. Fill in the "Result" section based on the chat context.
4. Fill in the "Agent summary" block according to the skill requirements.
5. **Decisions:** If an ADR-level decision was made, update `DECISIONS-LOG.md` (`Status:` accepted | deprecated | superseded).
6. **Verification (OKF v0.2):** Ask the user to confirm the close explicitly. Only on their confirmation, append to `verified`: `{ by: "human:<username>", at: "<now, ISO8601>" }`. Never fill this in on the agent's own authority — it exists to make the "merges are always human-approved" rule queryable.
7. **Bundle bookkeeping:** Move the task's entry in `_docs/tasks/index.md` from "Active" to "Done", and append a `_docs/log.md` entry.
8. **Board:** Run `/board-sync` to reflect the closed task (and its final subtask state) on `_docs/BOARD.md`.
9. **Follow-up:** If open questions remain, suggest a new task.
10. **Sync offers:** Refreshing the project status after a close is `/project-sync`'s job (it
    regenerates `_docs/PROJECT-OVERVIEW.md` and `_docs/index.md`), never `agent.md`'s — `agent.md`
    holds no task/decision/journal lists any more. Offer `/agent-sync` only if a *stable* fact
    changed (tech stack, directory layout, a project-wide convention). See the Maintenance Routine
    in `.agents/rules/style-guide.md` §5 for the full end-of-task offer list.

### /task-close with-journal [TASK-NNN]

Close the task AND distill a final journal as well.

1. Run the `/journal-distill` command.
2. Link the newly created JOURNAL in the TASK.
3. Execute the `/task-close` process.

---

### Decision Aid: What goes into DECISIONS-LOG?

- Architectural decisions (how something is built).
- Technology choice (why this and not that).
- Trade-offs (security vs. performance).
- Hard-to-reverse decisions.

---

## 8. FULL PROCESS SUMMARY

```
/task-close
    │
    ├─ Read TASK file
    ├─ Fill Result section
    ├─ Fill Agent summary block
    ├─ status → done, completed → today
    │
    ├─ Is there a new ADR-level decision?
    │   ├─ yes → update DECISIONS-LOG.md
    │   └─ no → skip
    │
    ├─ Status refresh needed?
    │   └─ offer /project-sync (PROJECT-OVERVIEW.md + index.md)
    │      — never write task/journal/decision lists into agent.md
    │
    ├─ Did a STABLE fact change (stack, layout, convention)?
    │   ├─ yes → offer /agent-sync
    │   └─ no → skip
    │
    ├─ Is there an open question?
    │   ├─ yes → suggest follow-up task
    │   └─ no → skip
    │
    ├─ User confirms the close?
    │   ├─ yes → append `verified: [{by: human:<user>, at: <now>}]`
    │   └─ no  → leave `verified` empty, do not close silently
    │
    ├─ Update _docs/tasks/index.md (Active → Done) + append _docs/log.md entry
    ├─ Run /board-sync
    │
    └─ Output summary
```
