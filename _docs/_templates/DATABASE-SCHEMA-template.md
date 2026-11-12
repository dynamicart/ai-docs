---
type: "Architecture Note"
id: "DATABASE-SCHEMA"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): live reference, overwritten in place. With migrations: curated summary generated FROM them (regenerate when they change). Without: this file IS the source of truth, keep it accurate by hand.

# Database Schema — [Project Name]

## Overview

- **Engine:** e.g. MySQL 8 / MariaDB / PostgreSQL
- **Migration tool:** e.g. none (hand-maintained) / Phinx / Laravel migrations
- **Schema source of truth:** e.g. `database/migrations/` — this file is a curated summary

## Tables

> One subsection per table agents touch; skip lookup/log tables unless relevant. Authoritative schema lives in DB/migrations.

### `table_name`

| Column | Type | Note |
|--------|------|------|
| id | | PK |
| | | |

**Relations:** e.g. `belongs_to users.id`, `has_many order_items`

**Notes:** anything non-obvious (soft deletes? JSON columns? denormalized fields kept in sync manually?)

---

## Entity relationship summary

> Core relations; link a full ERD from `arch/` if one exists.

## Known schema debt

> Denormalization, naming inconsistencies, effectively deprecated columns — anything misleading.

-
