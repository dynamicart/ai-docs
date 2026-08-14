# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.1] - 2026-08-14

### Added
- `/arch-sync` workflow (`.agents/workflows/arch-sync.md`) to regenerate the live `arch/*.md` reference files from the codebase (stack, directory map, entry points, DB schema, config/env, integrations).
- `/module-sync` workflow (`.agents/workflows/module-sync.md`) — a manual-only, delta-based sync for the durable per-module documents under `_docs/modules/`.
- Templates for the architecture reference files in `_docs/_templates/`: `STACK-template.md`, `DIRECTORY-MAP-template.md`, `ENTRYPOINTS-template.md`, `DATABASE-SCHEMA-template.md`, `CONFIG-ENV-template.md`, `INTEGRATIONS-template.md`.
- `MODULE-template.md` and the `_docs/modules/` directory with a hand-maintained `index.md`.

### Changed
- Documented the `Module` OKF concept category (schema + `module.status`) and the `/module-sync` rules in the documentation skill (`.agents/skills/documentation/SKILL.md`, §11).
- Updated `_docs/README.md` with the `modules/` layout, `Module` naming/status conventions, and the `/arch-sync` and `/module-sync` commands.
- `.gitignore` now excludes `test/**`.

## [0.1.0] - 2026-08-14

### Added
- Google Open Knowledge Format (OKF) v0.2 compatibility across the documentation layer:
  - `type`-based frontmatter and `generated` metadata on all concept files and templates.
  - Bundle-relative markdown cross-linking (replaces Obsidian wikilinks).
  - Reserved `index.md` / `log.md` directory indexes and logs (`_docs/index.md`, `_docs/log.md`, plus per-directory indexes).
  - `verified` gate on task close (human approval required, no agent self-verification).
- OKF-aligned updates to the agent workflows (`agent-sync`, `journal-distill`, `project-sync`, `task-close`, `task-new`) and the `style-guide.md`.
- Migration notes for pre-OKF documents (backfill `type` / `journal_type` opportunistically).

## [0.0.1] - 2026-05-04

### Added
- Initial documentation structure: `_docs/` layout (arch, journal, spec, tasks, `_templates`), templates, agent workflows, skill definition, and project guidelines.
- Agent workflows: `/agent-sync`, `/journal-distill`, `/project-sync`, `/task-close`, `/task-new`.
- Project scaffolding: `README.md`, `LICENSE`, `.agents/` rules, and the documentation skill.