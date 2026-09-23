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

> Note (OKF): this is the rich, human-facing project map (Dataview-enabled). The lean,
> frontmatter-minimal OKF entry point for the bundle is `_docs/index.md` — see there for
> the plain link-list a generic OKF consumer would start from.

## What is it for?

> One paragraph: what is this, who is it for, what is its essence.

## Goals

- **Primary Goal:** 
- **Secondary Goals:** 

## Technical stack

> Briefly justified why these were chosen.

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

> DECISIONS-LOG provides details, here only the most important ones.

- 

## Known issues / Technical debt

- 

## Development directions

> What is planned but not yet recorded as a task.

- 

## Log (recent changes)

> Brief entries on what changed in the project. Date + one line.
> For the full, OKF-conformant chronological record see `_docs/log.md`.

| Date | Change |
|-------|----------|
|       |          |
