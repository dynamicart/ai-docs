---
description: Regenerates the `_docs/PROJECT-OVERVIEW.md` file from the project's current state. This file is a professional extract for project managers and external observers.
---

Regenerate the `_docs/PROJECT-OVERVIEW.md` file. This document summarizes the project's current status in a clear, professional format.

## Preparation:

Before starting the generation, **ask the user** whether to:

- Use **Obsidian Dataview** tables for dynamic display,
- Or write **Static Markdown** tables into the file (which are readable everywhere).

## Source files (read in order):

1. **_docs/_templates/PROJECT-OVERVIEW-template.md** — The basis of the document.
2. **_docs/tasks/*.md** — Task status (Active and Done).
3. **_docs/journal/*.md** — For summarizing recent changes.
4. **_docs/DECISIONS-LOG.md** — Most important milestones and decisions.

## Generation Rules:

1. **Style:** Use professional, concise language. Avoid technical jargon where unnecessary.
2. **Data Sources:**
   - **What is it for? / Goals:** Summarize based on the read files (not just copy-paste, but in complete sentences). Project basic data (name, technologies, goals).
   - **Active tasks:** If static mode was chosen, generate a table (ID, Title, Priority, Status).
   - **Done tasks:** List of the last 10 closed tasks.
   - **Log:** Based on the last 5-10 journals, create a "Recent changes" table.
3. **Folder Structure:** Always present the project's current, real structure (if there is a deviation from the template, correct it).
4. **Dates:** Always update the `updated` field in the frontmatter.

## /project-sync command execution:

1. User approval/choice (Dataview vs Static).
2. Data collection and processing.
3. Create or update `_docs/PROJECT-OVERVIEW.md`.
4. Create or update `_docs/index.md` — the lean, frontmatter-minimal OKF bundle entry point (spec §6). Keep the section list (`tasks/`, `journal/`, `arch/`, `spec/`) and root-document links in sync with what actually exists; this file stays a plain link list, no Dataview, no prose beyond one intro line.
5. Feedback: "Project overview updated. [N] active tasks, [N] closed milestones displayed. `_docs/index.md` refreshed."