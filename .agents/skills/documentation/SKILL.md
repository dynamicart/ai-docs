---
name: Documentation Workflow
description: Rules and logic for creating, managing, and closing project documentation (tasks, journals, ADRs). Activate this skill when creating new tasks, distilling chat sessions into journals, or closing tasks to ensure consistency with the project's documentation standards.
---

# Documentation Workflow Skill

This skill contains the common logic and ruleset required for the project's documentation processes.

## When to use this skill?
- When creating a new task (TASK).
- When distilling the essence of a chat session (JOURNAL).
- When closing a task (TASK-CLOSE).
- When updating the project overview (PROJECT-OVERVIEW) or the decisions log (DECISIONS-LOG).

## How to use?

### 1. Filename and Slug generation
Every document (TASK, JOURNAL) filename contains a slug.
- **Format:** 2-5 words, separated by hyphens.
- **Characters:** Lowercase only, without accents (e.g., á -> a, ő -> o).
- **Example:** `database-migration`, `user-auth-fix`.

### 2. TASK numbering logic
TASK identifiers are in the format `TASK-NNN`.
1. List the `_docs/tasks/` folder.
2. Find the highest sequence number (NNN).
3. The next number: `highest + 1` (if empty: `TASK-001`).
4. Do not fill in numbering gaps.

### 3. JOURNAL numbering
Journals are date-based: `JOURNAL-YYYY-MM-DD-slug.md`.
If multiple Journals are created in a single day, mark the difference in the slug (e.g., `-2`, `-continuation`).

### 4. YAML Frontmatter schemas

**TASK schema:**
```yaml
id: TASK-NNN
title: "Title"
project: "project-name"
status: "planning" # planning | in-progress | review | done | postponed
priority: "medium" # critical | high | medium | low
created: YYYY-MM-DD
updated: YYYY-MM-DD
completed: 
tags: []
journals: [] 
related_tasks: []
complexity: "medium" # small | medium | large | epic
```

**JOURNAL schema:**
```yaml
id: JOURNAL-YYYY-MM-DD-slug
date: YYYY-MM-DD
project: "project-name"
topic: "Topic"
type: "planning" # planning | debugging | review | decision | brainstorm
duration_approx: "N min"
related_tasks: []
participants: [human, ai-agent]
outcome: "Summary"
```

### 5. Distillation rules (Journal)
During `/journal-distill`, the following must be extracted:
- **Context:** Why did the conversation start? (Status description, not "the user asked").
- **Options:** Investigated alternatives in a table (advantage, disadvantage, decision).
- **Decisions:** What was finalized? (**[Subject]:** Rationale).
- **Open Questions:** What remains unanswered? (In question form).
- **Next Steps:** Specific actions.

### 6. Closing rules (Task Close)
During `/task-close`:
1. `status` -> `done`, `completed` -> today's date.
2. Fill in the `Result` section (in past tense, factually).
3. Fill in the "Agent summary" block at the end of the file.
4. If an ADR-level decision was made, update the `DECISIONS-LOG.md`.
5. Ensure that log-like entries are not rewritten retroactively.
