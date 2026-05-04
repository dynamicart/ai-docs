---
type: "meta-docs"
---

# \_docs — Development Documentation

This folder records the project's development process: design decisions, task solutions, and distilled summaries of chat sessions with the AI agent. This knowledge base serves as the foundation for the `agent.md` (Shared Brain) file located in the root directory.

## Folder Structure

```
_docs/
├── README.md                    ← this file
├── PROJECT-OVERVIEW.md          ← [OPTIONAL/GENERATED] project map, active tasks
├── DECISIONS-LOG.md             ← ADR log (why and what decisions, ADR-NNN format)
├── tasks/                       ← Task-specific documents
│   ├── TASK-001_name.md
│   ├── TASK-002_name.md
│   └── ...
├── journal/                     ← Distilled summaries of chat sessions
│   ├── JOURNAL-YYYY-MM-DD-topic.md
│   └── ...
├── arch/                        ← architecture, schemas, diagrams
├── spec/                        ← business specifications, stories
└── _templates/
    ├── TASK-template.md
    ├── JOURNAL-template.md
    ├── PROJECT-OVERVIEW-template.md
    └── DECISIONS-LOG-template.md
```

## Automated Workflow

Documentation is primarily managed by the agent (Antigravity) through `/` commands. The goal of the process is to ensure that the design occurring in the chat is not lost but remains in a structured format.

### 1. Starting a New Task
- Use the `/task-new` command. This creates a `tasks/TASK-NNN_name.md` file based on the template.
- Task goals and YAML parameters (priority, complexity) are recorded at initiation.

### 2. Recording a Design Session (Chat)
- The term "session" refers to the current chat conversation with the agent.
- If important decisions or plans were made in the chat, use the `/journal-distill` command.
- This saves the essence of the conversation into a `JOURNAL-YYYY-MM-DD-topic.md` file (context, options, decisions, open questions).
- Always link the JOURNAL file in the related TASK file.

### 3. Closing a Task and Archiving
- At the end of work, the `/task-close` command closes the task, updates statuses, and records the final result.

### 4. Synchronization (Maintenance)
- `/agent-sync`: Updates the `agent.md` file in the root so that every agent (e.g., Antigravity, OpenCode) sees the same updated context.
- `/project-sync`: Generates or updates the `PROJECT-OVERVIEW.md` file if necessary (conditionally).

## Documentation Rules

Detailed rules are contained in `style-guide.md`.

- **Decisions:** Record every major technical decision in the form of an ADR (Architecture Decision Record) in the `DECISIONS-LOG.md` file (e.g., `ADR-001`).
- **Code Comments:** "The Why, not the What". Use file headers and standard function documentation.
- **Obsidian:** The `_docs/` folder can also be used as an Obsidian vault (with Dataview support).

## File Naming Conventions

| File     | Pattern                      | Example                           |
| -------- | ---------------------------- | --------------------------------- |
| Task     | `TASK-NNN_name.md`           | `TASK-001_user-auth.md`           |
| Journal  | `JOURNAL-YYYY-MM-DD-topic.md` | `JOURNAL-2025-01-15-auth-plan.md` |
| ADR      | `ADR-NNN`                    | `ADR-001`                         |

## YAML Status Values

**task.status:** `planning` · `in-progress` · `review` · `done` · `postponed`  
**task.priority:** `critical` · `high` · `medium` · `low`  
**journal.type:** `planning` · `debugging` · `review` · `decision` · `brainstorm`
