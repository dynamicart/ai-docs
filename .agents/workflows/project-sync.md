---
description: Regenerates the `_docs/PROJECT-OVERVIEW.md` file from the project's current state. This file is a professional extract for project managers and external observers.
---

Regenerate the `_docs/PROJECT-OVERVIEW.md` file. This document summarizes the project's current status in a clear, professional format.

## Division of responsibility (read this first)

This is now the **only** place that renders dynamic project status (active tasks, decisions,
recent journals/changes) as content. `agent.md` (`/agent-sync`) deliberately does not duplicate
any of this anymore — it only holds stable facts plus a link map into `_docs/`. Keep that
boundary: don't push stable, rarely-changing facts (tech stack rationale aside) back out of here
into `agent.md`, and don't trim this file's status sections down to pointers — the whole point
of `PROJECT-OVERVIEW.md` is to spell them out in full, readable form.

## First run (no `_docs/PROJECT-OVERVIEW.md` yet)

On a fresh project there are no tasks, journals, or ADRs to summarize, so the project's identity
can't be derived from `_docs/`. Never fill it in from the framework's own files either
(`README.md` placeholder, `_docs/README.md`, `.agents/`): those describe AI Docs, not this
project. Instead, in the same question round as the Dataview/Static choice below, **ask the user**
for:

- project name (frontmatter `project`, used as a slug) and a one-sentence description,
- what it is for and its primary/secondary goals,
- the tech stack. Offer to infer it from the codebase instead (manifest files, `_docs/arch/STACK.md`
  if `/arch-sync` already ran). For an empty repo, take the user's planned stack.
- owner, and the repo/live URLs if any.

Then generate the file from the template with these answers. Empty status sections get their
placeholder rows (e.g. *(no active tasks)*), not invented content.

## Preparation:

Before starting the generation, **ask the user** whether to:

- Use **Obsidian Dataview** tables for dynamic display,
- Or write **Static Markdown** tables into the file (which are readable everywhere).

## Source files (read in order):

1. **_docs/_templates/PROJECT-OVERVIEW-template.md** — The basis of the document.
2. **_docs/PROJECT-OVERVIEW.md** (if it exists) — Keep its project identity (name, description,
   goals, owner, links) unless the user asks to change it; only the status sections are regenerated.
3. **_docs/tasks/*.md** — Task status (Active and Done).
4. **_docs/journal/*.md** — For summarizing recent changes.
5. **_docs/DECISIONS-LOG.md** — Most important milestones and decisions.

Any of 3–5 may be empty or missing on a young project. That's normal, not an error.

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

1. User approval/choice (Dataview vs Static) — plus, on the first run, the project questions above.
2. Data collection and processing.
3. Create or update `_docs/PROJECT-OVERVIEW.md`.
4. Create or update `_docs/index.md` — the lean, frontmatter-minimal OKF bundle entry point (spec §6). Keep the section list (`tasks/`, `journal/`, `arch/`, `spec/`, `modules/`) and root-document links (`PROJECT-OVERVIEW.md`, `DECISIONS-LOG.md`, `log.md`, `BOARD.md`, `README.md`) in sync with what actually exists; this file stays a plain link list, no Dataview, no prose beyond one intro line. Go one level deeper than just the folder links: under `arch/` and `spec/`, list the actual files that currently exist in each (e.g. `STACK.md`, `DIRECTORY-MAP.md`, `DATABASE-SCHEMA.md`/`database/`, `ENTRYPOINTS.md`, `CONFIG-ENV.md`, `INTEGRATIONS.md`), and add direct links to `tasks/index.md`, `journal/index.md`, and `modules/index.md`. Still just nested links, no prose — the point is that `agent.md`'s `@_docs/index.md` import gives any agent a complete, file-level map of `_docs/` at near-zero token cost, without ever having to `Glob` the folder to find the right file.
5. Feedback: "Project overview updated. [N] active tasks, [N] closed milestones displayed. `_docs/index.md` refreshed."