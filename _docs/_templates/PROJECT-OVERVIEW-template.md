---
type: "Project Overview"
project: "project-name"
description: "One-sentence summary of what the project is and who it's for."
status: "active"
# status values: planning | active | maintenance | closed | on-hold
created: 2025-01-01
updated: 2025-01-01
owner: "name"
stack:
  - php
  - javascript
  - mysql
tags:
  - web
  - custom
links:
  repo: ""
  live: ""
  docs: ""
---

# Project Name — Overview

> Note (OKF): rich, human-facing project map (Dataview-enabled). The lean OKF entry point is `_docs/index.md`.

## What is it for?

> One paragraph: what it is, who it's for.

## Goals

- **Primary Goal:** 
- **Secondary Goals:** 

## Technical stack

> Brief justification of choices.

| Layer | Technology | Note |
|-------|-------------|------------|
| Backend | | |
| Frontend | | |
| DB | | |
| Infra | | |

## Folder Structure

```
project-name/
├── agent.md
├── _docs/
│   ├── index.md               ← OKF bundle root index (§6)
│   ├── log.md                 ← OKF chronological history (§7)
│   ├── PROJECT-OVERVIEW.md    ← this file
│   ├── DECISIONS-LOG.md
│   ├── BOARD.md               ← generated kanban view (/board-sync)
│   ├── tasks/
│   │   └── index.md
│   ├── journal/
│   │   └── index.md
│   ├── arch/                  ← live architecture references (/arch-sync)
│   │   └── index.md
│   ├── spec/
│   │   └── index.md
│   ├── modules/               ← durable module references (/module-sync)
│   │   └── index.md
│   └── _templates/
└── ...
```

## Active tasks

```dataview
TABLE status, priority, updated
FROM "_docs/tasks"
WHERE status != "done" AND status != "postponed"
SORT priority ASC
```

## Closed tasks (last 10)

```dataview
TABLE completed, title
FROM "_docs/tasks"
WHERE status = "done"
SORT completed DESC
LIMIT 10
```

## Major decisions (summary)

> Only the most important; details in DECISIONS-LOG.

- 

## Known issues / Technical debt

- 

## Development directions

> Planned but not yet a task.

- 

## Log (recent changes)

> Date + one line; full history in `_docs/log.md`.

| Date | Change |
|-------|----------|
|       |          |
