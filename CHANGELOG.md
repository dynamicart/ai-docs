# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - 2026-08-20

### Added
- **Docs & Board Viewer** (`tool/docs-viewer.html`, TASK-001): a standalone, server-less, zero-dependency HTML/JS tool that loads a `_docs/` folder (drag&drop or folder picker) or a single `BOARD.md`, and renders three views — a kanban board (built from TASK frontmatter, with a switchable BOARD.md fallback), a markdown reader with in-app internal-link navigation (relative, `../` and root-relative links), and a global wildcard search with result highlighting. All processing happens client-side; nothing is uploaded.
- **Bilingual UI (EN/HU)** (TASK-002): an in-file `I18N` dictionary with a `t()` helper and `data-i18n` attributes, a runtime language switch (default English) in the header's top-right corner, and `localStorage` persistence of the choice.
- **DECISIONS-LOG.md** with the first architecture decision records: ADR-001 (kanban board data source), ADR-002 (folder-loading API choice), ADR-003 (localization approach).
- `_docs/_archive/` directory for archived, non-indexed document backups.

### Changed
- The viewer's board source auto-switches: loading only a `BOARD.md` switches the kanban to BOARD.md mode; a BOARD.md-only source without a BOARD.md falls back to the TASK-file-based view.
- The board-sync rules (5 fixed columns, subtask progress from the "Approach / Plan" checklist, 14-day stale flag) are now also reproduced client-side in the viewer's kanban view.
- `TASK-001` and `TASK-002` completed and recorded as done on `_docs/BOARD.md`.

### Fixed
- A load-path regression in the viewer (`buildIndex` `ReferenceError` from a removed `var` declaration under strict mode) that prevented folder/BOARD.md loading; covered by new regression tests.

## [0.1.2] - 2026-08-18

### Added
- `/board-sync` workflow (`.agents/workflows/board-sync.md`) — fully rebuilds `_docs/BOARD.md`, a plaintext markdown kanban view of all TASK files grouped by `status` (`Planning` → `In Progress` → `Review` → `Done` → `Postponed`), each card showing priority/complexity and subtask progress. Pure read+render with no approval gate; safe to run automatically.
- `BOARD-template.md` in `_docs/_templates/`, and the initial `_docs/BOARD.md`.

### Changed
- `TASK-template.md`: the "Approach / Plan" section is now a checkbox list (`- [ ] step`) instead of a numbered list — it doubles as the task's subtask tracker, read verbatim by `/board-sync`.
- `task-new.md` and `task-close.md` now run `/board-sync` as their closing step, so the board always reflects the latest task state.
- Documented the board schema and rules in the documentation skill (`.agents/skills/documentation/SKILL.md`, §12).
- Updated `_docs/README.md` with the `BOARD.md` entry, the `/board-sync` command, and the updated naming conventions.
- Updated the root `README.md` (folder structure, workflow command table, quick-start diagram) to catch it up with the `arch/`, `modules/`, and `BOARD.md` additions from this and the previous release.

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
