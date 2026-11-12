---
type: "Architecture Note"
id: "STACK"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): live reference, overwritten in place. Keep short; link to composer.json / package.json instead of duplicating versions.

# Stack — [Project Name]

## Runtime

| Item | Value | Note |
|------|-------|------|
| PHP version | | e.g. from `composer.json` `require.php` |
| Framework | | e.g. none / Laravel / Symfony / Slim |
| Web server | | e.g. Apache + mod_php / nginx + php-fpm |
| DB engine | | e.g. MySQL 8 / MariaDB / PostgreSQL |

## Key dependencies (composer.json)

> Only dependencies shaping the build (ORM, router, templating, auth); no transitive packages.

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

> How a request starts: PSR-4 root, bootstrap, front controller.

## Why this stack (link, don't duplicate)

> Link the ADR instead of re-explaining:
> [ADR-NNN](/DECISIONS-LOG.md#adr-nnn)
