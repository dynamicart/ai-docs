---
type: "meta-docs"
---

# \_docs — Development Documentation

This folder records the project's development process: design decisions, task solutions, and distilled summaries of chat sessions with the AI agent. This knowledge base serves as the foundation for the `agent.md` (Shared Brain) file located in the root directory.

Since v0.2, `_docs/` is also an [Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)-conformant knowledge bundle: every TASK/JOURNAL/ADR is a self-describing concept (YAML frontmatter + markdown body), cross-linked with plain markdown links, and browsable through `index.md` files — so, besides Obsidian, any generic OKF consumer (e.g. the [reference visualizer](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf)) can read it without translation.

## Folder Structure

```
_docs/
├── README.md                    ← this file
├── index.md                     ← [OKF] bundle root index (reserved filename, no frontmatter except okf_version)
├── log.md                       ← [OKF] chronological history (reserved filename)
├── BOARD.md                     ← [GENERATED] plaintext kanban view of all tasks (via /board-sync)
├── PROJECT-OVERVIEW.md          ← [OPTIONAL/GENERATED] rich, human-facing project map (Dataview)
├── DECISIONS-LOG.md             ← ADR log (why and what decisions, ADR-NNN format)
├── tasks/                       ← Task-specific documents
│   ├── index.md                 ← [OKF] directory index
│   ├── TASK-001_name.md
│   ├── TASK-002_name.md
│   └── ...
├── journal/                     ← Distilled summaries of chat sessions
│   ├── index.md                 ← [OKF] directory index
│   ├── JOURNAL-YYYY-MM-DD-topic.md
│   └── ...
├── modules/                     ← Durable per-module reference docs (manual, via /module-sync)
│   ├── index.md                 ← directory index, hand-maintained
│   ├── billing.md
│   └── ...
├── arch/                        ← architecture, schemas, diagrams
├── spec/                        ← business specifications, stories
└── _templates/
    ├── TASK-template.md
    ├── JOURNAL-template.md
    ├── MODULE-template.md
    ├── BOARD-template.md
    ├── PROJECT-OVERVIEW-template.md
    ├── DECISIONS-LOG-template.md
    ├── INDEX-template.md        ← [OKF] template for any index.md
    └── LOG-template.md          ← [OKF] template for any log.md
```

## Automated Workflow

Documentation is primarily managed by the agent (Antigravity) through `/` commands. The goal of the process is to ensure that the design occurring in the chat is not lost but remains in a structured format.

### 1. Starting a New Task
- Use the `/task-new` command. This creates a `tasks/TASK-NNN_name.md` file based on the template.
- Task goals and YAML parameters (priority, complexity) are recorded at initiation.
- Ends by running `/board-sync`, so the new task appears on `BOARD.md` immediately.

### 2. Recording a Design Session (Chat)
- The term "session" refers to the current chat conversation with the agent.
- If important decisions or plans were made in the chat, use the `/journal-distill` command.
- This saves the essence of the conversation into a `JOURNAL-YYYY-MM-DD-topic.md` file (context, options, decisions, open questions).
- Always link the JOURNAL file in the related TASK file.

### 3. Closing a Task and Archiving
- At the end of work, the `/task-close` command closes the task, updates statuses, and records the final result.
- Also ends by running `/board-sync`.

### 4. Synchronization (Maintenance)
- `/agent-sync`: Updates the `agent.md` file in the root so that every agent (e.g., Antigravity, OpenCode) sees the same updated context.
- `/project-sync`: Generates or updates the `PROJECT-OVERVIEW.md` file if necessary (conditionally).
- `/arch-sync`: Regenerates the `arch/*.md` live-reference files (stack, directory map, entry points, DB schema, config/env, integrations) from the actual codebase.
- `/module-sync`: On-demand, **manual-only** delta sync for a single `modules/*.md` file against a given TASK's changes — never runs automatically. See `.agents/workflows/module-sync.md`.
- `/board-sync`: Fully rebuilds `BOARD.md`, the plaintext kanban view of all tasks, from live TASK data. Pure read+render, no approval gate — runs automatically at the end of `/task-new` and `/task-close`, and is also callable manually. See `.agents/workflows/board-sync.md`.

## Documentation Rules

Detailed rules are contained in `style-guide.md` and `.agents/skills/documentation/SKILL.md`.

- **Decisions:** Record every major technical decision in the form of an ADR (Architecture Decision Record) in the `DECISIONS-LOG.md` file (e.g., `ADR-001`).
- **Code Comments:** "The Why, not the What". Use file headers and standard function documentation.
- **Obsidian:** The `_docs/` folder can also be used as an Obsidian vault (with Dataview support).
- **OKF:** every concept file's frontmatter starts with `type`; cross-references use bundle-relative markdown links (`/tasks/TASK-001_name.md`), never `[[wikilinks]]`; `index.md` and `log.md` are reserved filenames, never used for a concept document.
- **Modules:** `modules/*.md` are durable, code-organization references (not archived like TASK/JOURNAL). They exist and update only through the explicit `/module-sync` command — no other workflow touches them automatically.
- **Board:** `BOARD.md` is a generated view, never a source of truth — it is fully rebuilt from `_docs/tasks/*.md` on every `/board-sync`. Subtask progress on the board comes directly from each TASK's "Approach / Plan" checkbox list; check items off in the TASK file, not on the board.

## File Naming Conventions

| File     | Pattern                      | Example                           |
| -------- | ---------------------------- | --------------------------------- |
| Task     | `TASK-NNN_name.md`           | `TASK-001_user-auth.md`           |
| Journal  | `JOURNAL-YYYY-MM-DD-topic.md` | `JOURNAL-2025-01-15-auth-plan.md` |
| Module   | `slug.md`                    | `billing.md`, `notifications.md`  |
| ADR      | `ADR-NNN`                    | `ADR-001`                         |
| Bundle index | `index.md`                | `_docs/index.md`, `_docs/tasks/index.md` |
| Bundle log   | `log.md`                  | `_docs/log.md`                    |
| Board        | `BOARD.md`                | `_docs/BOARD.md` (singular, generated) |

## YAML Status Values

**\*.type (OKF, required):** `Task` · `Journal` · `Module` · `Project Overview` · `Decisions Log`
**task.status:** `planning` · `in-progress` · `review` · `done` · `postponed` (also the fixed `BOARD.md` column order)
**task.priority:** `critical` · `high` · `medium` · `low`
**journal.journal_type** *(renamed from `journal.type` — see SKILL.md §4)*: `planning` · `debugging` · `review` · `decision` · `brainstorm`
**adr.Status:** `accepted` · `deprecated` · `superseded`
**module.status:** `active` · `deprecated` · `planned` (see SKILL.md §11)
