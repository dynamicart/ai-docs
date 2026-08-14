---
type: "Architecture Note"
id: "CONFIG-ENV"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): live reference, overwritten in place.
> ⚠️ Never record actual secret values here — names, purpose, and where they're set only.

# Configuration & Environment — [Project Name]

## Config files

| File | Purpose | Committed? |
|------|---------|------------|
| `config/app.php` | | yes/no |
| `.env` | | no (gitignored) |
| `.env.example` | | yes |

## Environment variables

| Variable | Purpose | Required | Default (if any) |
|----------|---------|----------|-------------------|
| | | | |

## Environments

| Environment | Where it runs | How config differs |
|-------------|----------------|---------------------|
| local | | |
| staging | | |
| production | | |

## Deployment notes

> How does config get from `.env.example` to a real environment? Manual? CI secret store?
> Anything that must be done manually after a fresh deploy (permissions, cache warmup,
> migrations)?

-
