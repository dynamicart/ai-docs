---
description: Regenerates the `_docs/arch/*.md` live-reference files (STACK, DIRECTORY-MAP, ENTRYPOINTS, DATABASE-SCHEMA, CONFIG-ENV, INTEGRATIONS) from the current state of the codebase. Unlike agent-sync/project-sync, this reads the actual project files, not just `_docs/`.
---

## Documentation Workflow

Apply the rules of the **Documentation Workflow Skill** whenever updating architecture files. `arch/*.md` files are *live references* (see skill §4/§10 analogue): unlike TASK/JOURNAL they carry no history requirement and are overwritten in place — they describe "what is", not "what happened".

## /arch-sync [target]

`target` is optional: one of `stack` | `directory-map` | `entrypoints` | `database-schema` | `config-env` | `integrations`. Omitted → sync all six.

### 1. Determine scope

- No `target` given → run all six sections below in order.
- `target` given → run only that section.
- First run in a project (no `arch/*.md` files exist yet, only `.gitkeep`) → this is the **TASK-000 architecture snapshot** bootstrap. Confirm with the user this is a first-time full scan before proceeding (it reads the whole codebase, not just a diff).

### 2. Source files (read per section, not all at once)

| Section | Reads |
|---|---|
| STACK | `composer.json`, `package.json` (if any), framework config files (e.g. `config/app.php`, `.env.example` for framework hints) |
| DIRECTORY-MAP | top-level + one level deep folder listing of the project root |
| ENTRYPOINTS | route file(s) if a router exists (e.g. `routes/*.php`), else `public/` (or equivalent web root) file listing |
| DATABASE-SCHEMA | `database/migrations/` if present, else ask the user for a schema dump or DB credentials to introspect; never guess table structure |
| CONFIG-ENV | `.env.example`, `config/*.php`, any `*.env*` file names (never read actual `.env` values into the doc) |
| INTEGRATIONS | grep-style scan for known HTTP client usage (`curl_`, `Guzzle`, `Http::`, SDK imports) and webhook route patterns |

### 3. Generation rules

1. **Read before write.** Always read the existing `arch/*.md` file (if present) and the matching `_templates/*-template.md` first — preserve any manually-written notes (e.g. "Known inconsistencies", "Known schema debt") that can't be re-derived from a scan; only overwrite table rows and structural sections that come from the codebase itself.
2. **Curated, not exhaustive.** Skip vendor/lookup/log tables, pure includes/partials, transitive composer dependencies — the point is signal, not a full dump. If unsure whether something is signal, ask rather than omit or over-include.
3. **No secrets.** CONFIG-ENV and INTEGRATIONS list variable/key *names* and *purpose* only, never values — even if a `.env` file is readable.
4. **Cross-link, don't duplicate.** ENTRYPOINTS ↔ INTEGRATIONS (webhooks), CONFIG-ENV ↔ INTEGRATIONS (credential locations) reference each other by section heading instead of repeating rows.
5. **Update `updated` and `generated`** frontmatter fields on every touched file.
6. **Flag, don't resolve, inconsistencies.** If the scan finds something that contradicts an existing manual note (e.g. doc says "no framework" but a `symfony/*` package is now in `composer.json`), surface it to the user instead of silently overwriting — this may be an undocumented decision that needs an ADR, not just a doc fix.

### 4. /arch-sync command execution

1. Confirm scope (full sync vs single target; first-run bootstrap vs update).
2. Read source files per section (table above).
3. Read existing `arch/*.md` (if any) + matching template.
4. For each section: diff scan-derived facts against current file content; apply structural/table updates, preserve manual prose, surface contradictions instead of resolving them.
5. Update `_docs/arch/index.md` link list if a new arch file was created for the first time.
6. Append a `_docs/log.md` entry: `**Update**: Arch-synced [section list].`
7. If this was a first-run bootstrap, suggest to the user that a `TASK-000` (architecture snapshot) and any retroactive ADRs for undocumented-but-already-made decisions be created via `/task-new`.
8. Do not ask back beyond step 1 and any contradiction found in step 4 — otherwise create/update the files, then output: "`arch/` synced: [N] files updated, [N] contradictions flagged for review."

## What this workflow deliberately does NOT do

- Does not run on every commit or file save — it's a deliberate, occasional sync (manually invoked, or suggested after a `/task-close` that touched `arch`-relevant files).
- Does not replace `DATABASE-SCHEMA.md` content with a raw migration dump — always curated to what an agent actually needs.
- Does not overwrite hand-written "Known inconsistencies / legacy pockets" or "Known schema debt" sections — those are judgment calls, not scan output.
- Does not read or record actual secret values, even when technically accessible (`.env`, DB credentials).

## Related files

- `_docs/_templates/STACK-template.md`, `DIRECTORY-MAP-template.md`, `ENTRYPOINTS-template.md`, `DATABASE-SCHEMA-template.md`, `CONFIG-ENV-template.md`, `INTEGRATIONS-template.md`
- `_docs/arch/index.md` — directory index, updated on first-time file creation
- `.agents/workflows/task-new.md` — used for the TASK-000 bootstrap follow-up
- `_docs/DECISIONS-LOG.md` — where retroactive ADRs go when a contradiction turns out to be an undocumented decision
