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
4. Fill in the Goal and Context sections based on the current chat context.
5. Use bundle-relative markdown links (`/tasks/...`, `/journal/...`), never `[[wikilinks]]`, when referencing other concepts.
6. Update `_docs/tasks/index.md` (add the new task under "Active") and append a `_docs/log.md` entry.
7. Do not ask back — create the file, then output the filename.

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
| Goal              | agent (draft) + you | at task-new       |
| Planning ref.     | agent               | if journal exists |
| Approach/Plan     | you or agent        | after planning    |
| Implementation    | you                 | during development|
| Result            | you + agent         | at task-close     |
