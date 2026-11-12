---
type: "Architecture Note"
id: "ENTRYPOINTS"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): live reference, overwritten in place. Replaces "grep the routes file" with a maintained map; with a real router, mostly link to the route file(s).

# Entry Points — [Project Name]

## Routed (framework / router present)

| Method | Path | Controller / Handler | Note |
|--------|------|----------------------|------|
| | | | |

> Route source of truth: `path/to/routes.php` — update only when the routing pattern changes.

## Direct scripts (no router — file = endpoint)

> Legacy PHP: each `.php` under `public/` is a URL. List relevant ones; skip includes/partials.

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

> Non-obvious request flow (shared bootstrap, global auth check location).
