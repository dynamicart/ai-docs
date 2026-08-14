---
type: "Architecture Note"
id: "ENTRYPOINTS"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): live reference, overwritten in place. This is the single most valuable file
> for a legacy/no-framework PHP project — it replaces "grep the routes file" with a
> maintained map. If the project uses a framework with a real router, this can mostly link
> to the route file(s) instead of duplicating every route.

# Entry Points — [Project Name]

## Routed (framework / router present)

| Method | Path | Controller / Handler | Note |
|--------|------|----------------------|------|
| | | | |

> Route source of truth: `path/to/routes.php` — update this table only when the routing
> pattern itself changes; for individual route additions, trust the routes file.

## Direct scripts (no router — file = endpoint)

> Typical for legacy PHP: every `.php` file under `public/` (or similar) is itself a
> reachable URL. List the ones that matter — skip pure includes/partials.

| File | Purpose | Auth required? | Note |
|------|---------|-----------------|------|
| | | | |

## Cron / CLI entry points

| Script | Trigger | Purpose |
|--------|---------|---------|
| | | |

## Webhooks / external callbacks

| Path | Caller | Purpose |
|------|--------|---------|
| | | |

## Notes

> Anything non-obvious about how a request flows from entry point to response
> (e.g. shared bootstrap file every entry point requires first, global auth check location).
