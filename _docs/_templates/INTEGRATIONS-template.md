---
type: "Architecture Note"
id: "INTEGRATIONS"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): live reference, overwritten in place.

# External Integrations — [Project Name]

## Third-party services / APIs

| Service | Purpose | Called from | Auth method | Note |
|---------|---------|--------------|-------------|------|
| | | | | |

> "Called from" = a file or entry point (see `ENTRYPOINTS.md`).

## Outbound webhooks (this project calls out)

| Target | Trigger | Payload | Note |
|--------|---------|---------|------|
| | | | |

## Inbound webhooks (external service calls this project)

> Cross-ref `ENTRYPOINTS.md` § Webhooks: this adds "who"/"why", that has "where".

| Source | Endpoint | Purpose |
|--------|----------|---------|
| | | |

## Credentials / API keys — where they live

> Names/locations only, never values; full list in `CONFIG-ENV.md`.

| Service | Env var name(s) | Set where |
|---------|-------------------|-----------|
| | | |

## Rate limits / quirks worth remembering

-
