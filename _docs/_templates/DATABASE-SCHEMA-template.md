---
type: "Architecture Note"
id: "DATABASE-SCHEMA"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): live reference, overwritten in place. If the project has migrations, this
> file should be a curated summary generated FROM them (don't hand-maintain both as
> separate sources of truth) — regenerate after migrations change the schema. If there are
> no migrations, this file IS the source of truth and must be kept accurate by hand.

# Database Schema — [Project Name]

## Overview

- **Engine:** e.g. MySQL 8 / MariaDB / PostgreSQL
- **Migration tool:** e.g. none (hand-maintained) / Phinx / Laravel migrations
- **Schema source of truth:** e.g. `database/migrations/` — this file is a curated summary

## Tables

> One subsection per table that agents actually touch. Skip pure lookup/log tables unless
> they matter for a task. Full authoritative schema always lives in the DB / migrations.

### `table_name`

| Column | Type | Note |
|--------|------|------|
| id | | PK |
| | | |

**Relations:** e.g. `belongs_to users.id`, `has_many order_items`

**Notes:** anything non-obvious (soft deletes? JSON columns? denormalized fields kept in sync manually?)

---

## Entity relationship summary

> Short prose or a simple list of the core relations — full ERD as a diagram belongs in
> `arch/` as a separate asset if one exists, linked here.

## Known schema debt

> Denormalization, naming inconsistencies, columns that are effectively deprecated but
> still present — anything that would mislead an agent inferring intent from the schema.

-
