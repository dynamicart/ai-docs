---
description: Regenerates `_docs/BOARD.md`, a markdown kanban view of all TASK files grouped by status with subtask progress.
---

## Documentation Workflow

Apply **Documentation Workflow Skill** rules. `_docs/BOARD.md` is a *live reference* overwritten in full on every sync (never hand-edited, no history).

### /board-sync

No arguments. Full rebuild (safe to call manually or from `/task-new`/`/task-close`).

#### 1. Source Files

1. `_docs/tasks/TASK-*.md`: Frontmatter (`status`, `priority`, `complexity`, `updated`, `title`) and "Approach / Plan" checkbox list.
2. `_docs/_templates/BOARD-template.md`: Column skeleton and header.

#### 2. Generation Rules

1. **Columns (fixed order)**: `Planning` → `In Progress` → `Review` → `Done` → `Postponed`. Empty column: `*(no tasks)*`.
2. **Card format**:
   - Checkbox: `- [ ]` (non-done) or `- [x]` (`status: done`).
   - Link: `TASK-NNN — <title>` (bundle-relative link to task).
   - Trailing metadata: `` `priority · complexity` ``.
   - Progress: `— X/Y subtasks` (if "Approach / Plan" has ≥1 item).
3. **Subtasks**: Nest checklist items verbatim from "Approach / Plan" (same text and checked state).
4. **Stale warning**: If `status: in-progress` and `updated` > 14 days ago, append: `- ⚠ stale — not updated in N days`.
5. **Sorting**: Within columns, sort by `priority` (critical → high → medium → low), then `updated` (newest first).
6. **Unknown status**: Skip tasks with missing/unknown status; report in output (never guess or silently drop).
7. **Header**: Update "Last synced: <YYYY-MM-DD>".

#### 3. Execution Steps

1. Read template and all `_docs/tasks/TASK-*.md` files.
2. Group, sort, and render cards per rules.
3. Overwrite `_docs/BOARD.md` in full. (Do not update `_docs/log.md` — board sync has no history).
4. Output: "`BOARD.md` synced: [N] tasks across [N] columns, [N] stale, [N] skipped (unrecognized status)."

#### Related Files

- Templates: `_docs/_templates/BOARD-template.md`, `TASK-template.md`
- Workflows calling `/board-sync`: `.agents/workflows/task-new.md`, `.agents/workflows/task-close.md`
