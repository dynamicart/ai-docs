---
project: "project-name"
type: "project-overview"
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
├── _docs/
│   ├── tasks/
│   ├── journal/
│   ├── PROJECT-OVERVIEW.md   ← this file
│   └── DECISIONS-LOG.md
├── ...
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

| Date | Change |
|-------|----------|
|       |          |
