---
description: Regenerates `_docs/BOARD.md`, a plaintext markdown kanban view of all TASK files, grouped by `status` with each card showing the "Approach / Plan" checklist progress. Pure read+render, no interpretation or judgment involved — safe to run automatically from `/task-new` and `/task-close`, and also callable manually at any time.
---

## Documentation Workflow

Apply the rules of the **Documentation Workflow Skill** whenever updating the board. `_docs/BOARD.md` is a *live reference* (same convention as `arch/*.md`): fully regenerated in place on every sync, never hand-edited, never carries history.

## /board-sync

No arguments. Always does a full rebuild — there is no partial/delta mode, unlike `/module-sync`, because rendering the whole board is cheap and there is no judgment call to preserve.

### 1. Source files (read in order)

1. Every `_docs/tasks/TASK-*.md` file — frontmatter (`status`, `priority`, `complexity`, `updated`, `title`) and the "Approach / Plan" section's checkbox list.
2. `_docs/_templates/BOARD-template.md` — column skeleton and header text.

### 2. Generation rules

1. **Columns are fixed**, in this order, one per `task.status` value: `Planning` → `In Progress` → `Review` → `Done` → `Postponed`. Do not reorder, merge, or add columns.
2. **One card per TASK file**, rendered as a top-level checkbox list item:
   - `- [ ]` for any non-`done` status, `- [x]` for `status: done`.
   - Link text: `TASK-NNN — <title>`, bundle-relative link to the task file.
   - Trailing metadata: `` `priority · complexity` `` from frontmatter.
   - If the "Approach / Plan" checklist has at least one item, append `— X/Y subtasks` (done count / total count).
3. **Subtask lines** are nested under the card exactly as they appear in the source TASK's "Approach / Plan" checklist — same text, same checked state. Do not summarize or truncate them.
4. **Stale flag:** if `status: in-progress` and `updated` is more than 14 days before today, append a line under the card: `- ⚠ stale — not updated in N days`. This is informational only — it never blocks the sync.
5. **Empty column:** render `*(no tasks)*` under a column heading with no matching tasks (see `BOARD-template.md`).
6. **Update the "Last synced" line** in the header to today's date (ISO8601).
7. Tasks with a missing or unrecognized `status` value are skipped and reported in the sync output, never silently dropped or guessed into a column.

### 3. Command execution

1. Read `_docs/_templates/BOARD-template.md` for the header/column skeleton.
2. List `_docs/tasks/` and read every `TASK-*.md` file's frontmatter and "Approach / Plan" section.
3. Group tasks by `status` into the five fixed columns; sort within a column by `priority` (critical → high → medium → low), then by `updated` (newest first).
4. Render each card and its subtask lines per the rules above.
5. Write the result to `_docs/BOARD.md`, overwriting it in full.
6. Do not update `_docs/log.md` — a board refresh is not a dated event, it has no history to record (same reasoning as `module-sync` not touching `log.md`).
7. Output: "`BOARD.md` synced: [N] tasks across [N] columns, [N] stale, [N] skipped (unrecognized status)."

## What this workflow deliberately does NOT do

- Does not ask for confirmation or human approval — this is pure read+render with no interpretation, unlike `/module-sync`'s delta proposals or `/task-close`'s `verified` gate.
- Does not modify any TASK file — it is strictly a reader of `_docs/tasks/`.
- Does not track history — `BOARD.md` always reflects only the current moment; look at `_docs/log.md` or individual TASK files for history.
- Does not introduce a second source of truth — every value shown is read live from the TASK files on each sync, never cached or hand-adjusted.

## Related files

- `_docs/_templates/BOARD-template.md` — header and column skeleton
- `_docs/_templates/TASK-template.md` — "Approach / Plan" section is the subtask-checklist source
- `.agents/workflows/task-new.md`, `.agents/workflows/task-close.md` — both call `/board-sync` as a closing step
