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

> "Called from" should point at a file or entry point (see `ENTRYPOINTS.md`) so an agent
> can trace the actual call site instead of guessing.

## Outbound webhooks (this project calls out)

| Target | Trigger | Payload | Note |
|--------|---------|---------|------|
| | | | |

## Inbound webhooks (external service calls this project)

> Cross-reference with `ENTRYPOINTS.md` § Webhooks — this table adds the "who" and "why",
> that file has the "where".

| Source | Endpoint | Purpose |
|--------|----------|---------|
| | | |

## Credentials / API keys — where they live

> Names and locations only, never actual values — see `CONFIG-ENV.md` for the full env var
> list. This section is just the cross-reference from "service" to "where its key is set".

| Service | Env var name(s) | Set where |
|---------|-------------------|-----------|
| | | |

## Rate limits / quirks worth remembering

-
