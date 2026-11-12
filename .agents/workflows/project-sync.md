---
description: Regenerates `_docs/PROJECT-OVERVIEW.md` (dynamic project status) and refreshes `_docs/index.md` bundle map.
---

## Division of Responsibility

`_docs/PROJECT-OVERVIEW.md` is the **canonical** place for dynamic status (active/done tasks, decisions, recent journals). `agent.md` strictly holds *stable* facts and never duplicates this status.

### /project-sync

#### 1. User Prompts (Preparation)

- **Table Mode**: Ask user: **Obsidian Dataview** or **Static Markdown** tables.
- **First-run Bootstrap** (if `PROJECT-OVERVIEW.md` missing): Ask for project name, 1-sentence description, purpose/goals, owner, and URLs. Offer to infer stack from code or `_docs/arch/STACK.md`. Never copy AI Docs framework details.

#### 2. Source Files (Read in order)

1. `_docs/_templates/PROJECT-OVERVIEW-template.md` (structure reference).
2. `_docs/PROJECT-OVERVIEW.md` (if exists: preserve identity/goals/owner; regenerate only status sections).
3. `_docs/tasks/*.md` (active tasks and last 10 closed tasks).
4. `_docs/journal/*.md` (recent changes from last 5–10 journals).
5. `_docs/DECISIONS-LOG.md` (milestones and accepted ADRs).

#### 3. Generation Rules

- **Content**: Concise, professional prose. No jargon filler.
- **Active Tasks**: Render table (ID, Title, Priority, Status) if static mode selected.
- **Done Tasks**: List up to 10 most recently closed tasks.
- **Log / Changes**: Recent changes table from latest 5–10 journals.
- **Structure**: Match current directory layout.
- **Frontmatter**: Update `updated: <YYYY-MM-DD>`.

#### 4. Execution Steps

1. Collect user preference (Dataview vs Static; first-run answers if new).
2. Gather sources and write `_docs/PROJECT-OVERVIEW.md`.
3. Regenerate `_docs/index.md`:
   - Keep lean, plain markdown link list (no prose beyond 1 intro line, no Dataview).
   - Link root docs (`PROJECT-OVERVIEW.md`, `DECISIONS-LOG.md`, `log.md`, `BOARD.md`, `README.md`).
   - Link subfolder indices: `tasks/index.md`, `journal/index.md`, `modules/index.md`.
   - List actual existing files under `arch/*.md` and `spec/*.md`.
4. Output: "Project overview updated: [N] active tasks, [N] closed tasks. `_docs/index.md` refreshed."
