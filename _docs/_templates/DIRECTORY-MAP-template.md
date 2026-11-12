---
type: "Architecture Note"
id: "DIRECTORY-MAP"
project: "project-name"
updated: 2025-01-01
generated: { by: "agent:manual", at: "2025-01-01T00:00:00Z" }
---

> Note (OKF): live reference, overwritten in place. Regenerate on meaningful top-level structure changes, not every file add; don't enumerate files.

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

> Per meaningful folder: contents, naming convention, and what an agent must NOT assume (e.g. "looks like MVC but Models also output directly").

### `public/`

-

### `src/` (or `app/`)

-

### `config/`

-

## Known inconsistencies / legacy pockets

> Flag anything that looks like a pattern but isn't, so agents don't over-generalize.

-
