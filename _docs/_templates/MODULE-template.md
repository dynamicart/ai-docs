---
type: "Module"
id: "MODULE-slug"
title: "Module Name"
description: "One-sentence summary of this module's responsibility."
project: "project-name"
status: "active"
# status values: active | deprecated | planned
owner: ""
created: 2025-01-01
updated: 2025-01-01
stale_after: 90
# days since last full human verification before the module should be treated as needs-review
tags:
  - module
related_modules: []
related_tasks: []
related_adrs: []
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
verified: []
---

# MODULE: Module Name

> Note (OKF): unlike TASK/JOURNAL, a MODULE file describes a durable code unit, not a
> single work session — it does not get archived when "done". It sits between the
> project-wide `arch/*.md` files and the work-scoped `tasks/`/`journal/` files: one file
> per logical module, kept current for as long as the module exists. `verified` here
> follows the same shape as the TASK schema (a list of human approval records) — an agent
> must never self-verify a module file.

## Responsibility

> 1-3 sentences: what this module's job is in the system. Do NOT describe HOW it does it
> (that belongs in the code) — only WHY it exists and WHAT it guarantees to other modules.

## Location in code

| Element | Path | Note |
|---|---|---|
| Namespace | `App\...` | PSR-4 root |
| Main directory | `app/...` | |
| Entry point(s) | | e.g. controller, service, CLI command |
| Configuration | | if the module has module-specific config |

> Give only the minimum needed for navigation — do not list files here, the agent will
> grep/glob further from this starting point.

## Dependencies

**This module builds on:**
- `modules/xxx.md` — why (1 sentence)

**Depended on by:**
- `modules/yyy.md` — what it uses from this module

> Only cross-module dependencies that are NOT already obvious from `use`/import
> statements — e.g. implicit ordering, runtime coupling, shared DB-level state.

## Key decisions

> Short, terse bullets — do not repeat the ADR, just point to it.

- **Decision:** ... — see [ADR-0xx](/DECISIONS-LOG.md#adr-0xx)
- **Deliberate constraint:** ... (e.g. "no caching, because...")

## Invariants — do not break

> The single most important section for an agent: rules the code does not enforce with an
> explicit error, but breaking them causes failures elsewhere. E.g. "Invoice.total is
> always stored in cents", "UserRepository never calls Billing directly".

-

## Known limitations / technical debt

> Things an agent might be tempted to "fix" but are deliberate, or known-incomplete
> without capacity to address yet.

-

## Related tasks

> Dataview query, kept for automation; explicit links still help an agent get context
> fast even where Dataview isn't available (e.g. a generic OKF consumer).

```dataview
TABLE status, journal_type
FROM "_docs/tasks"
WHERE contains(related_modules, "module-name")
SORT file.ctime DESC
```

## Change log (brief)

> Only SIGNIFICANT structural changes, not every commit. Details live in
> JOURNAL / `_docs/log.md` — this only records "what happened to this module" at a glance.

| Date | Change | Task/ADR |
|---|---|---|
| | | |
