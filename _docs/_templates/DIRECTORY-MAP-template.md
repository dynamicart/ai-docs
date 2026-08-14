---
type: "Architecture Note"
id: "DIRECTORY-MAP"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): live reference, overwritten in place. Regenerate whenever the top-level
> folder structure changes meaningfully (new module, renamed root folder) — not on every
> file add. Depth: stop at the level that's still meaningful; don't enumerate every file.

# Directory Map — [Project Name]

```
project-root/
├── public/             ← web root / front controller(s)
│   └── index.php
├── src/ (or app/)       ← application code
│   ├── Controllers/
│   ├── Models/
│   └── ...
├── config/              ← configuration files
├── includes/            ← legacy shared includes (if applicable)
├── templates/ (or views/)
├── vendor/               ← composer dependencies (not tracked in detail)
├── composer.json
└── ...
```

## Folder-by-folder notes

> For each top-level (and meaningful second-level) folder: what lives there, what the
> naming/organizing convention is, and anything an agent must NOT assume (e.g. "looks like
> MVC but Models also do direct output — see TASK-003").

### `public/`

-

### `src/` (or `app/`)

-

### `config/`

-

## Known inconsistencies / legacy pockets

> Be honest here — PHP codebases often mix conventions across eras. Flag anything that
> looks like a pattern but isn't, so an agent doesn't over-generalize from one example.

-
