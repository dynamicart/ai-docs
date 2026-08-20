# _docs Update Log

> OKF reserved file (spec §7) — flat, date-grouped chronological history, newest first.
> Appended by `/task-new`, `/task-close`, `/journal-distill`, `/agent-sync`.
> Separate from `DECISIONS-LOG.md`, which stays the detailed, per-ADR archive.

## 2026-08-20

* **Update**: Closed [TASK-002](tasks/TASK-002_multilingual-viewer.md) — the viewer is bilingual (EN/HU, default English, persisted), with a top-right language switch; one load-path regression (`buildIndex` ReferenceError) found and fixed during manual testing.
* **Update**: Recorded [ADR-003](/DECISIONS-LOG.md) — localization approach for the Docs & Board Viewer (in-file `I18N` dictionary, default English).
* **Creation**: Opened [TASK-002](tasks/TASK-002_multilingual-viewer.md) — a runtime EN/HU language switch for the Docs & Board Viewer UI (default English, persisted in `localStorage`).
* **Update**: Closed [TASK-001](tasks/TASK-001_standalone-docs-board-viewer.md) — the `tool/docs-viewer.html` was completed and human-validated (12/12 subtasks, 3 decisions worth of ADRs: board source, folder loading).
* **Creation**: Opened [DECISIONS-LOG.md](DECISIONS-LOG.md) — ADR-001 (board data source) and ADR-002 (folder loading) recorded.
* **TASK-001 created**: [Standalone Docs & Board Viewer](tasks/TASK-001_standalone-docs-board-viewer.md) — plan for a folder-based, server-less HTML/JS tool for the kanban/reader/search views of the `_docs/` bundle, as a continuation of the earlier `board-viewer.html` work. Goal: a viewer usable without Obsidian, final location in the `tool/` folder.

## Not started yet

* **Initialization**: `_docs/` scaffold created from the ai-docs template.
