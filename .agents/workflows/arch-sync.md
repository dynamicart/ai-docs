---
description: Regenerates `_docs/arch/*.md` live references (stack, directory-map, entrypoints, database-schema, config-env, integrations) from current codebase state.
---

## Documentation Workflow

Apply **Documentation Workflow Skill** rules. `arch/*.md` files are *live references* overwritten in place ("what is", not "what happened").

### /arch-sync [target]

`target` (optional): `stack` | `directory-map` | `entrypoints` | `database-schema` | `config-env` | `integrations`. Default: sync all six in order.

#### 1. Scope & Sources

If no `arch/*.md` files exist yet (first-time bootstrap): confirm full scan with user before proceeding.

Read per target on demand (never all at once):
- **STACK**: `composer.json`, `package.json`, framework config (`config/app.php`, `.env.example`).
- **DIRECTORY-MAP**: Top-level + 1 level deep folder tree from root.
- **ENTRYPOINTS**: Router files (`routes/*`) or `public/` web root.
- **DATABASE-SCHEMA**: `database/migrations/`, or request schema dump/credentials; never guess. Curate tables; do not raw-dump.
- **CONFIG-ENV**: `.env.example`, `config/*`, `*.env*` filenames. Names/purpose only; never read/record secret values.
- **INTEGRATIONS**: Grep HTTP clients (`curl_`, `Guzzle`, `Http::`, SDK imports) and webhook routes.

#### 2. Rules

1. **Read template & existing file first**: Read `_docs/_templates/<TARGET>-template.md` and current `arch/<TARGET>.md`. Preserve manual prose (e.g. "Known inconsistencies", "Known schema debt"); only regenerate scan-derived tables/structure.
2. **Signal over noise**: Curate for agent use. Omit vendor/logs/lookup tables and transitive dependencies. If unsure whether something is signal, ask rather than omit or over-include.
3. **Cross-link, don't duplicate**: Cross-reference headings between ENTRYPOINTS, CONFIG-ENV, and INTEGRATIONS.
4. **Frontmatter**: Update `updated` and `generated` (`by: agent:<id>`, `at: <ISO8601>`).
5. **Flag contradictions**: If scan contradicts manual notes, surface it to user for an ADR instead of silently overwriting.

#### 3. Execution Steps

1. Determine scope (target vs full; check if first-run bootstrap).
2. Scan source files, diff against current content, apply updates per rules.
3. If a new `arch/*.md` file was created:
   - Add link to `_docs/arch/index.md`.
   - Warn user that root `_docs/index.md` is stale and suggest `/project-sync` (never edit root index here).
4. Append to `_docs/log.md`: `**Update**: Arch-synced [targets].`
5. If first-run bootstrap: suggest creating `TASK-000` snapshot and retroactive ADRs via `/task-new`.
6. Output: "`arch/` synced: [N] files updated, [N] contradictions flagged for review." (Do not ask back unless bootstrap or contradictions require human input).

#### Related Files

- Templates: `_docs/_templates/{STACK,DIRECTORY-MAP,ENTRYPOINTS,DATABASE-SCHEMA,CONFIG-ENV,INTEGRATIONS}-template.md`
- Tracking: `_docs/arch/index.md`, `_docs/DECISIONS-LOG.md` (for ADRs), `.agents/workflows/task-new.md`
