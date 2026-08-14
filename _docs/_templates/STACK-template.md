---
type: "Architecture Note"
id: "STACK"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): this is a *live reference* document — unlike TASK/JOURNAL it has no history
> requirement and gets overwritten in place as the stack changes. Keep it short; link out
> to composer.json / package.json rather than duplicating version numbers that will drift.

# Stack — [Project Name]

## Runtime

| Item | Value | Note |
|------|-------|------|
| PHP version | | e.g. from `composer.json` `require.php` |
| Framework | | e.g. none / Laravel / Symfony / Slim |
| Web server | | e.g. Apache + mod_php / nginx + php-fpm |
| DB engine | | e.g. MySQL 8 / MariaDB / PostgreSQL |

## Key dependencies (composer.json)

> List only the dependencies that actually shape how the codebase is built
> (ORM, router, templating, auth) — not every transitive package.

| Package | Purpose | Note |
|---------|---------|------|
| | | |

## Frontend toolchain (if any)

| Item | Value |
|------|-------|
| Build tool | e.g. none / Vite / Webpack |
| JS approach | e.g. vanilla / Alpine.js / jQuery |
| CSS approach | e.g. plain / Sass / Tailwind |

## Autoloading / bootstrap

> How does a request start? PSR-4 autoload root, bootstrap file, front controller.

## Why this stack (link, don't duplicate)

> If the choice was a deliberate decision, link the ADR instead of re-explaining it here:
> [ADR-NNN](/DECISIONS-LOG.md#adr-nnn)
