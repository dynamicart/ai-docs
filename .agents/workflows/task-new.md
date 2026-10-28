---
description: Creates a new TASK file in the `_docs/tasks/` folder based on the template. Automatically fills the YAML frontmatter from the provided data and information extracted from the context.
---

## Documentation Workflow

Apply the rules of the **Documentation Workflow Skill** whenever creating a new task.

### /task-new [title]

Create a new TASK file.

1. Read the `_docs/_templates/TASK-template.md` file.
2. Determine the next `TASK-NNN` number and the filename based on the skill rules.
3. Fill in the YAML frontmatter (`type: Task`, status: "planning", date: today, title: [title], `generated: { by: "agent:<this-agent-id>", at: "<now, ISO8601>" }`).
4. Fill in Goal and Context sections based on the current chat context (or existing brief).
5. **Brief & Plan auto-linking:**
   - If a matching brief exists (`_docs/tasks/briefs/TASK-NNN_brief.md`), link it in `Planning Reference`, and align the Goal / Acceptance criteria.
   - If a matching plan exists (`_docs/tasks/plans/TASK-NNN_plan.md`), link it in `Planning Reference`, and populate the "Approach / Plan" checklist from the plan's High-Level Milestones.
6. Use bundle-relative markdown links (`/tasks/...`, `/journal/...`), never `[[wikilinks]]`, when referencing other concepts.
7. Update `_docs/tasks/index.md` (add the new task under "Active") and append a `_docs/log.md` entry.
8. Run `/board-sync` to reflect the new task on `_docs/BOARD.md`.
9. Do not ask back — create the file, then output the filename.

### /task-new from-brief [brief-file]

Create a new TASK based on an existing human-written brief.

1. Read the referenced brief file (e.g. `_docs/tasks/briefs/TASK-NNN_brief.md` or a path supplied by user).
2. Extract title, goal, background, and acceptance criteria from the brief.
3. If the brief already has a `task_id` (e.g. `TASK-003`), use that ID; otherwise determine the next `TASK-NNN` and update the brief's frontmatter `task_id`.
4. Link the brief under the "Planning Reference" section.
5. If a matching implementation plan (`_docs/tasks/plans/TASK-NNN_plan.md`) already exists, link it and extract its milestones into the "Approach / Plan" checklist.
6. Complete task creation per steps 6–8 above.

### /task-new from-journal

Create a new TASK based on a closed JOURNAL.

1. Read the referenced JOURNAL file.
2. Fill in the TASK based on the plan and next steps listed in the journal.
3. Link the journal in the `journals` field and under the "Planning reference" section.
4. Save the file according to the skill rules.

---

| Section            | Who fills it        | When              |
| ----------------- | ------------------- | ----------------- |
| YAML frontmatter  | agent               | at task-new       |
| Goal              | agent (draft) + you | at task-new (or from brief) |
| Planning ref.     | agent               | links brief/plan/journal |
| Approach/Plan     | you or agent        | from plan or discussion |
| Implementation    | you                 | during development|
| Result            | you + agent         | at task-close     |
