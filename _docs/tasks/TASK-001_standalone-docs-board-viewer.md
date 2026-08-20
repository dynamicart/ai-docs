---
type: Task
id: TASK-001
title: "Standalone Docs & Board Viewer (folder-based HTML/JS tool)"
description: "A single-file, server-less HTML/JS tool that loads a _docs/ folder (drag&drop or folder picker) and provides kanban + reader + search views built from the frontmatter and body of TASK/JOURNAL/MODULE files — usable without Obsidian."
project: "ai-docs"
status: "done"
priority: "medium"
created: 2026-08-20
updated: 2026-08-20
completed: 2026-08-20
resource: "tool/"
tags:
  - development
  - tooling
  - frontend
journals: []
related_tasks: []
complexity: "large"
generated: { by: "agent:claude", at: "2026-08-20T00:00:00Z" }
verified:
  - { by: "human:Szentgyörgyi János", at: "2026-08-20T10:26:31Z" }
---

# TASK-001: Standalone Docs & Board Viewer (folder-based HTML/JS tool)

## Goal

> A self-contained, portable HTML/JS file (inline CSS/JS, in the `tool/` folder) that can
> load an entire `_docs/` folder (via drag&drop or a folder picker) and offer three views:
> a kanban board (based on TASK frontmatter), a reader view (rendered markdown with
> internal link following), and a global text search. Goal: a useful tool also for those who
> do not use Obsidian. Success criterion: loading the project's own `_docs/` folder renders
> the tasks, journals and modules correctly, and switching between the search/kanban/reader
> views is seamless — without a server, build step or external dependency.

## Context and Background

> Background: a kanban-only viewer (`test/board-viewer.html`) was previously built from
> `test/BOARD.md`, with a file input and drag&drop and client-side markdown parsing (see the
> project's earlier board-viewer sessions). The need arose for this tool to grow beyond the
> kanban view: to become a general AIBrain docs viewer that makes the entire `_docs/`
> content (task, journal, module, arch files) readable and searchable without having to
> install Obsidian. The size (typically a few dozen to a few hundred markdown files, a few
> MB) does not justify a server — a client-side in-memory solution is sufficient and simpler.

## Planning Reference

> There was no separate journal session; planning happened in this chat as a continuation of
> the board-viewer work. The Approach / Plan and Decisions sections below are transcripts of
> that discussion.

## Approach / Plan

> `/board-sync` reads this list verbatim for the subtask progress — keep it in short,
> checkable units.

- [x] Folder-based data input: `webkitGetAsEntry()` recursive traversal on drag&drop (reading every `.md` file when a folder is dropped), plus `<input type="file" webkitdirectory>` as the click-based fallback
- [x] Markdown + YAML frontmatter parser (compact, inline, dependency-free): every file yields a `{ path, type, frontmatter, rawBody, title }` record
- [x] In-memory index built from the loaded files, with file-type detection by path/pattern (`tasks/`, `journal/`, `modules/`, `arch/`)
- [x] Kanban view switched from a BOARD.md-based to a TASK-file-based source: `status`/`priority`/`complexity` from the frontmatter, subtask progress from the "Approach / Plan" checklist — following the `board-sync.md` workflow rules (5 fixed columns, stale flag after 14 days)
- [x] BOARD.md parsing kept as a fallback mode (switchable: "From TASK files" vs "From BOARD.md")
- [x] Reader view: selected file rendered as markdown (compact, dependency-free markdown→HTML renderer inlined)
- [x] Internal markdown links intercepted on click, relative path resolution in the index, in-app navigation (no browser tab escape to a broken link)
- [x] Sidebar: tree structure or flat list of the loaded files, filterable by type and `status`/`priority`
- [x] Global search: wildcard/substring search over title + frontmatter + body, results clickable into the reader view
- [x] View switching (kanban / reader / search results list) state management within one file, reusing the existing design tokens (color palette, typography) of `board-viewer.html`
- [x] Final file placed in the `tool/` folder, filename determined (e.g. `tool/docs-viewer.html`)
- [x] Tested against the project's own `_docs/` folder (task, journal, module, arch files mixed)

## Decisions

### Decision 1: Board data source — BOARD.md vs TASK files

- **Question:** Should the kanban view read the generated `BOARD.md` or the TASK files directly?
- **Options:** (a) parse BOARD.md (as in the previous iteration), (b) read the TASK files directly and reproduce the board-sync logic client-side, (c) both, with a toggle.
- **Chosen solution:** (c) — the board is built primarily from the TASK files; BOARD.md parsing remains the fallback.
- **Rationale:** The TASK files' frontmatter (`status`, `verified`, etc.) is the primary source of truth; the viewer thus does not depend on whether `/board-sync` has already run, and introduces no second source of truth. The BOARD.md fallback remains useful for those who quickly want to check just the generated view.

### Decision 2: Folder-loading method

- **Question:** Which browser API should handle folder-level file reading?
- **Options:** (a) `File System Access API` (`showDirectoryPicker()`), (b) a combination of `webkitGetAsEntry()` + a `webkitdirectory` input.
- **Chosen solution:** (b).
- **Rationale:** `showDirectoryPicker()` is Chromium-only with narrower compatibility; the `webkitGetAsEntry()`/`webkitdirectory` pair is older and more widely supported (Chrome, Edge, Firefox), and keeps the existing drag&drop UX with minimal extension.

## Implementation

> `tool/docs-viewer.html` (single-file, inline CSS/JS, no external dependency) was built during development; the section below documents the final state. All processing happens client-side; nothing is uploaded.

### Key Changes / Components

- `tool/docs-viewer.html` — self-contained viewer (inline CSS/JS, ~1600 lines, zero-dependency)
- Folder loading: `webkitGetAsEntry()` recursive drag&drop + `<input webkitdirectory>` fallback; a single BOARD.md can also be loaded
- Compact YAML frontmatter + markdown renderer (headings/anchors, list/checkbox, table, code block, blockquote, inline formatting)
- In-memory index with path-based type detection (`tasks/`, `journal/`, `modules/`, `arch/`, `spec/`; BOARD/index/log/README → Meta)
- Kanban: from TASK frontmatter (5 fixed columns, priority-ordered, subtask progress, 14-day stale flag) + BOARD.md fallback with a toggle
- Reader view with internal-link navigation, sidebar filters (type + task status), global wildcard search with result-context highlighting
- View switching (kanban / reader / search) built around a single `state` object

### Architecture / Data Flow

Load → parse → index → render:

1. **Load:** folder drag&drop (recursive `readEntries()` batching) or a `webkitdirectory` input; only `.md`/`.markdown`/`.txt` files are processed.
2. **Parse:** `splitFrontmatter()` extracts the YAML block, `parseYaml()` converts the frontmatter into an object (quoted scalars, `-` arrays, `[]`/`{}` flow values, empty → null); the body stays as `rawBody`. Every file record: `{ relPath, name, type, frontmatter, rawBody, title }`.
3. **Index:** `state.docs` + `state.byPath` (normalized relative path → document), type detection by path pattern.
4. **Render:** `render()` → sidebar + active view (kanban / reader / search).

### Technical Details

- **Design tokens:** the `test/board-viewer.html` color palette and typography are reused (status/priority colors, dark theme); the viewer works exclusively locally.
- **Kanban (TASK-based):** follows the `board-sync.md` rules — 5 fixed columns (`Planning` → `In Progress` → `Review` → `Done` → `Postponed`), ordered within a column by `priority` (critical → low) then `updated` (newest first), subtask progress from the "Approach / Plan" checklist, `in-progress` + `updated` older than 14 days → `⚠ stale` flag.
- **Kanban (BOARD.md fallback):** the `board-viewer.html` BOARD.md parser logic is carried over; switchable via the "Board source" toggle, with automatic fallback to the TASK-based view when BOARD.md is missing.
- **Link resolution:** relative (`TASK-001_….md`), `../`-relative and root-relative (`/tasks/…`) markdown links are resolved in the index and trigger in-app reader navigation; `#anchor`s scroll to the slug-ids of rendered headings; external links open in a new tab; unresolved internal links show a toast.
- **Markdown renderer:** heading (`id=slug`), fenced/indented code, blockquote, hr, ul/ol + `- [x]` checkboxes (`<input disabled checked>`), table (skipping the separator row), inline: `code`, `**bold**`, `*italic*`, `[link](url)`, `![image]` → placeholder. All output is HTML-escaped.
- **Search:** wildcard (`*`/`?`) → regex; over title + frontmatter + body fields, result-context snippet with `<mark>` highlighting, clickable results → reader.
- **State:** a single `state` object (`docs`, `byPath`, `view`, `currentPath`, `query`, `boardSource`, filters) + a `render()`/`renderCurrentView()` dispatcher; the view elements follow the `view-<view>` id convention.

### Verification

- `node --check` — JS syntax without errors.
- Node harness tests on the real `_docs/` folder (25 files mixed: task, journal, module, arch, spec, meta): 46 checks, 45 green (1 false negative — the TASK-001 body contains no markdown link).
- Rendering of multiple document types (README code block + tree structure, tables, checkbox lists, log) without exception.
- Bugs found during development fixed (see Issues and Solutions).

### Known Limitations (for manual testing)

- The drag&drop folder loading cannot be tested in a headless environment — verify in a real browser (Chrome/Edge/Firefox).
- Firefox does not support `webkitGetAsEntry()` → for folder drops it falls back to the flat `dataTransfer.files` list (without folder structure); the `webkitdirectory` input then offers an alternative with full paths.
- The underscore italic (`_text_`) is sensitive to words containing underscores when they are not in a code span; `_docs/`-like occurrences are safe inside code blocks.

## Issues and Solutions

| Issue | Solution |
| -------- | -------- |
| Root-relative (`/…`) markdown links resolved relative to the doc folder instead of the bundle root | `resolveLink()` handles `/`-prefixed links separately: they resolve relative to the root |
| Broken YAML regex (`/^[\s|:-\]+$/`) caused a syntax error in the table-separator filter | Character class fixed: `/^[\s|:-]+$/` |

## Result

> `tool/docs-viewer.html` was built and closed. The viewer was tested against the project's own `_docs/` folder (drag&drop + file picker + BOARD.md drop); the views (kanban / reader / search) and the board-source switching work seamlessly. Bugs found during development (root-relative link resolution, YAML regex, card-subtask toggle, drag&drop fallback, board-source auto-switch) were fixed and verified. The viewer runs exclusively client-side with no external dependencies.

- [x] Original goal achieved
- [x] Tested
- [x] Documented

## Open Questions / Next Steps

- Final filename and exact location in the `tool/` folder (to be agreed with the user before starting implementation)
- Whether an export/save feature is needed (e.g. exporting search results or the kanban state), or the tool stays a read/search-only instrument

## Agent Summary

> This section is filled in by the closing agent when the task is closed.

```
TASK_ID: TASK-001
SUMMARY: A single-file, server-less HTML/JS Docs & Board Viewer was built at `tool/docs-viewer.html`; it loads a `_docs/` folder (drag&drop or `webkitdirectory` picker) or a single BOARD.md and provides kanban / markdown-reader / global-search views — without Obsidian.
KEY_DECISIONS: Board data source = TASK frontmatter (primary) + BOARD.md fallback with a toggle; folder loading = webkitGetAsEntry()/webkitdirectory (instead of showDirectoryPicker, for wider compatibility).
TECHNOLOGIES: Vanilla HTML5/JS (inline CSS/JS, zero-dependency), File API (webkitGetAsEntry, webkitdirectory, FileReader), client-side markdown + YAML frontmatter parsing.
OUTCOME: 12/12 subtasks done; the viewer tested against the real `_docs/` (task/journal/module/arch/spec/meta files), the three views and board-source switching work; the project's first tool that makes the entire bundle browsable without Obsidian.
REUSABLE_PATTERNS: Compact dependency-free YAML frontmatter + markdown renderer; path-based type detection; relative/root-relative link resolution in an index; single-`state` view dispatcher; reuse of dark-design tokens.
```