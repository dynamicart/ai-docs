# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.1] - 2026-09-23

### Added
- README Quick Start: instructions for starting a new project from the `release` branch, and for adding the framework to an existing project without overwriting its `README.md`/`LICENSE`/`.gitignore`/`CHANGELOG.md`.
- `/project-sync` first-run mode: when `PROJECT-OVERVIEW.md` doesn't exist yet, it asks the user for the project's identity (name, purpose, goals, stack, owner) instead of deriving it from framework files.

### Changed
- `/install`:
  - New step 0 warns (never deletes) when `_docs/` still holds AI Docs' own docs, i.e. the repo was cloned from `main` instead of `release`.
  - The native-file table now covers `AGENTS.md`, `.cursor/rules/*.mdc` (`alwaysApply: true`) and `.windsurf/rules/` (`trigger: always_on`); `.cursorrules`/`.windsurfrules` are marked as legacy.
  - For Claude Code and Gemini CLI, the bridge now includes an `@agent.md` import, so `agent.md`'s own `@_docs/index.md` import actually resolves natively.
  - Native command wrappers forward `$ARGUMENTS`, and they carry an `<!-- AI_DOCS_WRAPPER -->` marker so re-runs can add, refresh and prune them without touching user-authored commands.
  - Re-running on an installed agent now refreshes the wrappers instead of stopping.
  - The bridge lists `PROJECT-OVERVIEW.md` among the generated files and states the first-time order (`/project-sync`, then `/agent-sync`).
- `/agent-sync` takes the project description from `PROJECT-OVERVIEW.md` (stable parts) and the root `README.md`, no longer from `_docs/README.md` (which describes the documentation system, not the project). On a fresh project it defers to `/project-sync` instead of describing the framework.

### Fixed
- README no longer claims that Claude Code, Antigravity and OpenCode natively auto-load `.agents/` and `agent.md`.

## [0.3.0] - 2026-09-06

### Changed
- **Reworked the division of responsibility between `agent.md`, `_docs/PROJECT-OVERVIEW.md`, and `_docs/index.md`** to remove the drift risk of two "current status" copies:
  - `agent.md` (`/agent-sync`) is now a *stable*, agent-facing digest only — about the project, tech stack, a "Where things live" directory map, and stable conventions — ending in an `@_docs/index.md` import. It no longer carries Active Tasks / Recent Journals / Key Decisions / Open Questions sections.
  - `/project-sync` is now the sole generator of dynamic project status (active tasks, decisions, recent journals) and of `_docs/index.md` itself, which becomes a generated, file-level bundle map (nested links into `arch/`, `spec/`, `tasks/index.md`, `journal/index.md`, `modules/index.md`, and all root documents) instead of a hand-maintained flat list.
  - `/task-close` and `/agent-sync` updated to match: task-close now offers `/project-sync` for a status refresh and `/agent-sync` only when a stable fact changed, instead of writing task/decision data into `agent.md` directly.
- `/arch-sync` now appends an entry to `_docs/log.md` on every run, and flags the root `_docs/index.md` as stale (suggesting `/project-sync`) whenever a new `arch/*.md` file is created for the first time.
- Added an "exploration-cost trigger" to the style guide's maintenance routine: agents should proactively propose `/module-sync <module-name>` at the end of a task when reconstructing an implicit convention required disproportionate discovery effort, so the reconstruction is captured once as a durable module doc.
- `_docs/_templates/AGENT-template.md` and `PROJECT-OVERVIEW-template.md` restructured to match the new stable/dynamic split; `INDEX-template.md` and `LOG-template.md` updated to document root-index ownership (`/project-sync`) and the wider set of commands appending to `log.md`.
- Added a `related_modules` frontmatter field to `TASK-template.md`, read by the `modules/*.md` Dataview blocks (updated to match) to list which tasks touch a given module.
- `agent.md` and `_docs/index.md` regenerated to the new format; `_docs/PROJECT-OVERVIEW.md` generated for this repository for the first time via `/project-sync`.

### Fixed
- A typo in the root `README.md`'s `/install` quick-start step.

## [0.2.1] - 2026-09-04

### Added
- `/install` workflow (`.agents/workflows/install.md`) — a one-time, per-agent bootstrap that detects the current AI agent's native auto-loaded file (`CLAUDE.md`, `.cursorrules`, `.windsurfrules`, `GEMINI.md`, etc.) and writes a bridge section into it, referencing `.agents/` (rules, skills, workflow commands) and `agent.md`. Lets agents that don't natively auto-load the framework pick it up automatically in every future session, without duplicating any file content.

### Changed
- Root `README.md`: added the `/install` step to Quick Start (as a new step 3, once-per-agent) and to the Workflow Commands table.
- `_docs/README.md`: added a "First-Time Agent Setup" note (§0) pointing to `/install`.

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
