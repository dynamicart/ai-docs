---
trigger: always_on
---

## AI-Driven Code & Docs Style Guide

This document defines the rules for the project's coding, technical documentation, and code commenting.
Its goal is sustainability and AI-based searchability.

### 0. Coding Conventions (Detailed)

The project follows the K&R (Kernighan & Ritchie) style.

### 1. Code-level Documentation (Inline)

_"The Why, not the What"_
The code describes what it does. The comment describes why it does it that way (business logic, technical constraints, workarounds).

_File Header_
A short summary is mandatory at the beginning of every new or significantly modified file:
/**
 * @file [Filename]
 * @description [1-sentence summary of the function]
 * @context [Which module does it belong to, what calls it?]
 */

_Function Documentation_
Use standard blocks for complex functions:
@param: Type and short description.
@returns: What it returns and in what format.
@throws: What errors it might throw.

### 2. The _docs Folder Structure

All documents are prepared in Markdown format, organized into the following folders:

| Folder / File | Content | Update Frequency |
|--------------|----------|-----------|
| PROJECT-OVERVIEW.md | Project map, active tasks, tech stack. | At every change (`/project-sync`). |
| index.md | Generated, file-level map of the bundle. | With PROJECT-OVERVIEW (`/project-sync`). |
| log.md | Chronological change history (OKF §7). | Appended, never rewritten. |
| BOARD.md | Generated plaintext kanban of all TASKs. | On every `/board-sync` — never hand-edited. |
| tasks/ | Individual tasks (TASK-NNN). | Continuously. |
| journal/ | Daily sessions, brainstorms (JOURNAL-YYYY-MM-DD). | Daily. |
| arch/ | API specifications, Database schemas, System diagrams. | At architecture changes (`/arch-sync`). |
| modules/ | Durable per-module references (responsibility, invariants). | Only via `/module-sync`, manually. |
| DECISIONS-LOG.md | Repository of architectural decisions (ADR). | At new decisions. |
| spec/ | Business logic, specifications. | Before new features. |

### 3. ADR (Architecture Decision Record) Format

Every important decision must be recorded in `DECISIONS-LOG.md` with the appropriate sequence number (ADR-NNN), referencing the related TASK and JOURNAL.

### 4. Visualization (Mermaid.js)

Where the text is too complex, use Mermaid diagrams.

### 5. The Agent's "Maintenance" Routine

At the end of every task, the agent should offer (if it's clearly appropriate):
- The `/journal-distill` command to close the session.
- The `/task-close` command to archive the task.
- `/project-sync` if the project *status* moved (tasks, decisions, recent changes) — this also
  refreshes `_docs/index.md`.
- `/agent-sync` only if a *stable* fact changed (tech stack, directory layout, a project-wide
  convention). `agent.md` carries no task/journal/decision lists — do not "update" it with status.
- `/arch-sync` if the work touched anything an `arch/*.md` file describes (stack, routes, schema,
  env vars, integrations).

#### Exploration-cost trigger — propose `/module-sync`

If, during the task, understanding a code unit required **disproportionate discovery effort** —
several exploration/search agents, reading many files across several folders, or reconstructing an
implicit convention by hand (e.g. *how templating works in this PHP framework and how the matching
css/js files are wired to it*) — and that same understanding will very likely be needed again in a
future task, then say so explicitly at the end of the task and propose:

> `/module-sync <module-name>` — so what was just reconstructed becomes a durable
> `_docs/modules/<module-name>.md` reference instead of being re-discovered from scratch next time.

Rules for this proposal:

- **It is a proposal only.** Never run `/module-sync` on your own initiative — it is
  manual-invocation-only by design (see the Documentation Workflow Skill §11).
- Name the concrete module, and state in one sentence what was expensive to reconstruct — the
  invariants, entry points, or implicit conventions worth writing down. That sentence is the seed
  of the module document.
- Do not propose it for a one-off, throwaway lookup, and do not propose it for something an
  existing `arch/*.md` or `modules/*.md` file already covers — check first.
- If no module file exists yet, say so: `/module-sync` will offer to create one from
  `MODULE-template.md`.
- One proposal per module, at the end of the task — not mid-work, and not repeated for the same
  module in the same session.

### 6. OKF Conformance

`_docs/` is an [Open Knowledge Format v0.2](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)-conformant knowledge bundle. Full rules live in the **Documentation Workflow Skill** (`.agents/skills/documentation/SKILL.md`); the short version:

- Every TASK/JOURNAL/PROJECT-OVERVIEW frontmatter block starts with a `type` field (`Task`, `Journal`, `Project Overview`, …) — this is the one OKF-required field.
- `index.md` and `log.md` are reserved filenames (spec §3.1) — never used for a concept document, always maintained per §6/§7 of the skill.
- Cross-references between concepts use standard bundle-relative markdown links (`[Title](/tasks/TASK-NNN_slug.md)`), never Obsidian `[[wikilinks]]`.
- OKF fields are additive: existing `status`, `priority`, and other project-specific frontmatter keys stay exactly as they are.