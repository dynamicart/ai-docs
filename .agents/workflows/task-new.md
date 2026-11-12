---
description: Creates a new TASK file in `_docs/tasks/` from template, brief, or journal, and updates tracking.
---

## Documentation Workflow

Apply **Documentation Workflow Skill** rules and `_docs/_templates/TASK-template.md`.

### /task-new [title]

Creates a new TASK file without asking back.

#### 1. Execution Steps

1. Read `_docs/_templates/TASK-template.md`.
2. Determine next `TASK-NNN` and filename: `_docs/tasks/TASK-NNN_slug.md`.
3. Set frontmatter: `type: Task`, `status: "planning"`, `date: <YYYY-MM-DD>`, `title: [title]`, `generated: { by: "agent:<agent-id>", at: "<ISO8601>" }`.
4. Draft Goal and Context from session context.
5. **Auto-link Planning References** (bundle-relative links, never wikilinks):
   - If brief exists (`_docs/tasks/briefs/TASK-NNN_brief.md`): link in `Planning Reference`, align Goal & Acceptance Criteria.
   - If plan exists (`_docs/tasks/plans/TASK-NNN_plan.md`): link in `Planning Reference`, copy High-Level Milestones into `Approach / Plan` checklist.
6. Update `_docs/tasks/index.md` (add under "Active") and append `_docs/log.md` entry.
7. Run `/board-sync` to refresh `_docs/BOARD.md`.
8. Output created filename.

### /task-new from-brief [brief-file]

1. Read brief file (e.g. `_docs/tasks/briefs/TASK-NNN_brief.md` or user path).
2. Extract title, goal, context, acceptance criteria.
3. Reuse existing brief `task_id` or assign next `TASK-NNN` and update brief frontmatter.
4. Link brief in `Planning Reference`. If matching plan exists, link it and import milestones into `Approach / Plan`.
5. Complete standard steps 6–8 above.

### /task-new from-journal [journal-file]

1. Read referenced JOURNAL file.
2. Populate Goal and Plan from journal's next steps/decisions.
3. Link journal in `journals` frontmatter and `Planning Reference`.
4. Complete standard steps 6–8 above.
