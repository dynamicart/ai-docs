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
8. **Follow-up:** If open questions remain, suggest a new task.

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
    ├─ Does agent.md exist?
    │   ├─ yes → update active tasks + recent changes
    │   └─ no → skip (agent-sync will handle it)
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
    │
    └─ Output summary
```
