---
description: Proposes delta updates for a single `_docs/modules/*.md` file based on a task diff or user description. Manual invocation only; requires human approval.
---

## Documentation Workflow

Apply **Documentation Workflow Skill** rules. `_docs/modules/*.md` are durable references (never archived on task close). Never run automatically.

### /module-sync <module-name> [TASK-NNN]

#### 1. Scope & Sources

- `module-name` (required): If `_docs/modules/<module-name>.md` does not exist, offer creation from `MODULE-template.md`.
- `TASK-NNN` (optional): If provided, inspect task git diff and linked journal. If omitted, ask user for what changed (never infer silently from dirty working tree).

Read in order:
1. `_docs/modules/<module-name>.md`
2. `_docs/_templates/MODULE-template.md`
3. Task git diff / user description
4. Linked journal (if any)

#### 2. Per-Section Update Policy

| Section | Policy | Handling |
|---|---|---|
| **Responsibility** | Manual only | Never generate or overwrite. |
| **Location in code** | Proposed | Update path/namespace from diff. |
| **Dependencies** | Proposed | Update from `use`/import statements in diff. |
| **Key decisions** | Manual only | Link to ADRs only; never generate prose. |
| **Invariants** | Flag only | If diff conflicts with invariant, **block sync** until human decides. Never auto-modify. |
| **Known limitations** | Proposed | Propose removal only if task explicitly resolved it. |
| **Related tasks** | Untouched | Dataview block; leave as-is. |
| **Change log** | Proposed | Append row: `| <YYYY-MM-DD> | <summary> | <TASK/ADR> |`. |

#### 3. Execution & Approval Rules

1. **Proposal only**: Present diff proposal in chat. Never edit file before explicit user confirmation. Example format:
   ```
   modules/billing.md

   + ## Dependencies — new line:
   + **Depended on by:**
   + - modules/notifications.md — sends invoice-issued notifications

   + ## Change log — new row:
   + | 2026-08-10 | Added notifications integration | TASK-052 |

   ⚠ Invariant check: the diff calls the Billing service directly from the
     Notifications controller — conflicts with the existing invariant
     ("services only communicate through jobs"). Human decision required:
     update the invariant, or fix the code.
   ```
2. **Invariant conflict gate**: Any invariant violation is a hard blocker requiring explicit user direction.
3. **Verification (`verified`)**:
   - Applying a delta keeps existing `verified` frontmatter unchanged.
   - Only a full manual review of the whole file appends `{ by: "human:<username>", at: "<ISO8601>" }`. Never self-verify.
   - `stale_after`: if that many days passed since the last full human verification, treat the file as `status: needs-review` for this sync (checked at sync time, not by a background process).
4. **Apply on approval**: Apply only confirmed lines to `_docs/modules/<module-name>.md`.
5. **Output**: "`modules/<module-name>.md` updated: [N] sections proposed, [N] approved, [N] invariant conflicts flagged."

#### Related Files

- Template: `_docs/_templates/MODULE-template.md`
- Decisions: `_docs/DECISIONS-LOG.md` (for ADRs if invariants change)
