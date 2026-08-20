---
type: Task
id: TASK-002
title: "Multilingual UI for the Docs & Board Viewer (EN/HU toggle)"
description: "Convert the hardcoded Hungarian UI strings of tool/docs-viewer.html into a bilingual (English / Hungarian) display layer with a runtime language switch, defaulting to English."
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
related_tasks:
  - TASK-001
complexity: "small"
generated: { by: "agent:opencode", at: "2026-08-20T11:16:18Z" }
verified:
  - { by: "human:Szentgyörgyi János", at: "2026-08-20T11:38:19Z" }
---

# TASK-002: Multilingual UI for the Docs & Board Viewer (EN/HU toggle)

## Goal

> Make the viewer's display language switchable between English and Hungarian at runtime, without changing any behavior or the data layer. Default language is English (no saved preference yet), the choice is remembered in `localStorage`. Success criterion: every user-facing string renders correctly in both languages, switching re-renders the whole UI in place, and the canonical data values (status column names, priorities, type names) stay unchanged.

## Context and Background

> `tool/docs-viewer.html` (TASK-001) hardcodes ~30 UI strings in Hungarian — static HTML (toolbar, tabs, dropzone, sidebar filters, view headers, search placeholder) plus strings rendered from JS (meta line, toasts, empty states, board counters, card hints, reader badges, search messages). The repository is being published publicly in English (TASK-001 already translated), so the viewer needs an English display layer. Since the primary user is Hungarian, the UI keeps a Hungarian option too, switchable at runtime. The work is a pure display-layer refactor: an i18n dictionary with a `t()` lookup, a `setLang()` helper, `data-i18n` attributes on static elements, and a compact language `<select>` in the toolbar.

## Planning Reference

> Planning happened in this chat as a direct continuation of the TASK-001 close. Key agreed points: default language = English, persistence in `localStorage`, data-layer values (column names, priorities, type names) stay canonical English in both languages.

## Approach / Plan

> `/board-sync` reads this list verbatim for the subtask progress — keep it in short, checkable units.

- [x] Add an `I18N` dictionary (`en` + `hu` blocks, ~35 keys) and a `t(key, vars)` helper with `{placeholder}` substitution
- [x] Add `state.lang` (default `"en"`) + `setLang(lang)`: updates `<html lang>`, re-applies `data-i18n` static elements, re-runs `render()`, persists to `localStorage` (try/catch, safe under `file://`)
- [x] Tag static HTML with `data-i18n` attributes (toolbar buttons, tabs, dropzone texts, sidebar filter options, view headers, search placeholder, clear button)
- [x] Replace JS-rendered literals with `t()` calls (meta line, toasts, empty states, board counters, card-open title/hints, reader badges, search messages), including plural/format variants
- [x] Add a language `<select>` (🇬🇧 English / 🇭🇺 Magyar) to the toolbar next to the board-source toggle
- [x] Keep canonical data values unchanged (column names, priorities, type names) — identical in both languages
- [x] Verify: `node --check`, the 46-test harness stays green, plus i18n sanity tests (`t()` returns per-language strings, `setLang` changes the `lang` attribute)

## Decisions

### Decision 1: Default display language

- **Question:** Which language should the UI show when no preference is saved yet?
- **Options:** (a) Hungarian — the existing behavior; (b) English — since the repo is published publicly in English.
- **Chosen solution:** (b) English.
- **Rationale:** The repository is public and English-first; Hungarian remains available via the switch and is persisted once chosen.

### Decision 2: Localization mechanism

- **Question:** How should the string layer be implemented?
- **Options:** (a) a plain in-file `I18N` dictionary with `data-i18n` attributes + a `t()` helper; (b) a build step or external library.
- **Chosen solution:** (a) — consistent with the zero-dependency, single-file constraint of the viewer.
- **Rationale:** No build step, no network dependency; the viewer stays a portable single file.

## Implementation

> Implemented as a pure display-layer refactor of `tool/docs-viewer.html`. The data layer (parsers, board build, link resolution, search) is untouched — only the hardcoded Hungarian UI strings were moved into an i18n dictionary.

### Key Changes / Components

- `I18N` dictionary (`en` + `hu`, ~37 keys) with a `t(key, vars)` helper — `{placeholder}` substitution via simple split/join
- `state.lang` (default `"en"`) + `setLang(lang)`: sets `<html lang>`, re-applies `data-i18n` / `data-i18n-placeholder` / `data-i18n-html` static elements, re-runs `render()`, persists to `localStorage` under `docs-viewer-lang` (try/catch, safe under `file://`)
- Static HTML tagged with `data-i18n` attributes (toolbar buttons, tabs, dropzone texts, sidebar filter options, view headers, search placeholder, clear button)
- All JS-rendered literals replaced with `t()` calls — meta line, toasts (4), empty states (3), board counters, card-open title, subtask toggle hints, stale note, search messages, image placeholder
- Language `<select>` (🇬🇧 English / 🇭🇺 Magyar) in the toolbar next to the board-source toggle
- `updateStatusBar()` consolidates the filename tag + meta line rendering (used by load, index build and language switch)
- Canonical data values unchanged: column names (`Planning` → `Postponed`), priorities (`critical` → `low`), type names (`Task`/`Journal`/…) stay identical in both languages

### Technical Details

- **Pluralization:** handled per-language via whole-string formats (en `"{n} files"` / hu `"{n} fájl"`), avoiding a plural rule engine.
- **Dropzone paragraph with emphasis:** uses `data-i18n-html` (controlled strings only, no user input) to keep the `<b>` styling.
- **Persistence:** `localStorage` guarded with try/catch — under `file://` or blocked storage the viewer falls back to the default language silently.
- **Fallback:** `t()` falls back to the `en` entry, then to the key itself, so a missing key never renders empty.
- **Init order:** `initLang()` runs before the first `switchView("board")`, so the whole chrome is localized from first paint.

## Issues and Solutions

> Obstacles and errors encountered along the way, and how they were solved.

| Issue | Solution |
| -------- | -------- |
| Refactoring the meta line into `updateStatusBar()` removed the `var tasks` declaration from `buildIndex()`, which the board-source auto-switch still referenced — under `"use strict"` an undeclared variable read throws `ReferenceError`, breaking every folder/BOARD.md load | `var tasks = state.docs.filter(...)` restored in `buildIndex()`; regression tests added that actually call `buildIndex()` (no-throw + board-source auto-switch), which the harness previously never exercised |

## Result

> The viewer is now bilingual (English / Hungarian) with a runtime switch, defaulting to English. The language `<select>` sits in the top-right corner of the header (labeled `EN English` / `HU Magyar`), the choice persists in `localStorage`, and the whole UI — static chrome and JS-rendered strings — re-renders in place on switch. The data layer was untouched: board columns, priorities and type names stay canonical English in both languages. Tested by the user in a real browser (folder + BOARD.md loading, language switching) and by the harness: 61 checks green, `node --check` clean. One regression found during manual testing (the `buildIndex` `ReferenceError`, see Issues) was fixed and covered by tests.

- [x] Original goal achieved
- [x] Tested
- [x] Documented

## Open Questions / Next Steps

- Whether status/priority/type display names should ever be localized (currently canonical English in both languages) — revisit if the bundle itself ever becomes bilingual.

## Agent Summary

> This section is filled in by the closing agent when the task is closed.

```
TASK_ID: TASK-002
SUMMARY: The Docs & Board Viewer got a bilingual (EN/HU) display layer: an `I18N` dictionary with a `t()` helper, a `setLang()` that re-applies `data-i18n` static elements and re-renders JS strings, a top-right language `<select>` (default English), and `localStorage` persistence.
KEY_DECISIONS: Default language = English (public repo); localization mechanism = plain in-file `I18N` dictionary + `data-i18n` attributes, no build step or library; canonical data values (columns, priorities, types) stay English in both languages.
TECHNOLOGIES: Vanilla HTML5/JS (zero-dependency), `data-i18n` / `data-i18n-placeholder` / `data-i18n-html` attributes, `localStorage` (try/catch-guarded), per-language whole-string formats for plurals.
OUTCOME: 7/7 subtasks done; verified by the user in a real browser and by the harness (61 checks green); one load-path regression (`buildIndex` ReferenceError from a removed `var tasks`) found during manual testing, fixed and covered by a new regression test.
REUSABLE_PATTERNS: `data-i18n` attribute convention + `t(key, vars)` lookup with `{placeholder}` substitution; `setLang()` as a single re-render entry point for the whole chrome; try/catch-guarded `localStorage` persistence safe under `file://`; harness that exercises the real load path (`buildIndex`) to catch strict-mode ReferenceErrors.
```