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
# days since last full human verification before needs-review
tags:
  - module
related_modules: []
related_tasks: []
related_adrs: []
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
verified: []
---

# MODULE: Module Name

> Note (OKF): durable code-unit reference, never archived; one file per logical module, kept current while it exists. `verified` = human approval records (same shape as TASK); an agent must never self-verify.

## Responsibility

> 1-3 sentences: WHY it exists and WHAT it guarantees to other modules — not HOW (that's the code).

## Location in code

| Element | Path | Note |
|---|---|---|
| Namespace | `App\...` | PSR-4 root |
| Main directory | `app/...` | |
| Entry point(s) | | e.g. controller, service, CLI command |
| Configuration | | if the module has module-specific config |

> Minimum for navigation; don't list files.

## Dependencies

**This module builds on:**
- `modules/xxx.md` — why (1 sentence)

**Depended on by:**
- `modules/yyy.md` — what it uses from this module

> Only non-obvious cross-module coupling (implicit ordering, runtime coupling, shared DB state).

## Key decisions

> Terse bullets; point to the ADR, don't repeat it.

- **Decision:** ... — see [ADR-0xx](/DECISIONS-LOG.md#adr-0xx)
- **Deliberate constraint:** ... (e.g. "no caching, because...")

## Invariants — do not break

> Most important section: rules the code doesn't enforce but whose breakage fails elsewhere. E.g. "Invoice.total is always stored in cents".

-

## Known limitations / technical debt

> Deliberate or known-incomplete things an agent might be tempted to "fix".

-

## Related tasks

> Dataview query; add explicit links where Dataview isn't available.

```dataview
TABLE status, priority, updated
FROM "_docs/tasks"
WHERE contains(related_modules, "module-name")
SORT file.ctime DESC
```

> Matches TASK `related_modules` — add this module's id there, or the table stays empty.

## Change log (brief)

> Only significant structural changes; details live in JOURNAL / `_docs/log.md`.

| Date | Change | Task/ADR |
|---|---|---|
| | | |
