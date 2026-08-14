---
name: Documentation Workflow
description: Rules and logic for creating, managing, and closing project documentation (tasks, journals, ADRs), conformant with the Open Knowledge Format (OKF v0.2). Activate this skill when creating new tasks, distilling chat sessions into journals, closing tasks, or maintaining index.md/log.md files.
---

# Documentation Workflow Skill

This skill contains the common logic and ruleset required for the project's documentation processes.

**OKF conformance.** The `_docs/` folder is treated as an [OKF v0.2](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)-conformant knowledge bundle: every TASK / JOURNAL / ADR is a *concept* (one markdown file = one YAML-frontmatter + body document), cross-linked with standard markdown links, discoverable through `index.md`, and historized through `log.md`. This does not replace the existing workflow fields (`status`, `priority`, etc.) — OKF fields are added alongside them, never in place of them.

## When to use this skill?
- When creating a new task (TASK).
- When distilling the essence of a chat session (JOURNAL).
- When closing a task (TASK-CLOSE).
- When creating or updating a document in the `_docs/` folder.
- When updating the project overview (PROJECT-OVERVIEW), the decisions log (DECISIONS-LOG), or the bundle's `index.md` / `log.md` files.
- When creating or syncing a module document (MODULE) via `/module-sync` — see §11.

## How to use?

### 1. Filename and Slug generation
Every document (TASK, JOURNAL) filename contains a slug.
- **Format:** 2-5 words, separated by hyphens.
- **Characters:** Lowercase only, without accents (e.g., á -> a, ő -> o).
- **Example:** `database-migration`, `user-auth-fix`.
- **Full filename pattern:** `TASK-NNN_slug.md` / `JOURNAL-YYYY-MM-DD-slug.md`. Always resolve the *actual* filename by listing the target directory before linking to it — never guess it from the ID alone.

### 2. TASK numbering logic
TASK identifiers are in the format `TASK-NNN`.
1. List the `_docs/tasks/` folder.
2. Find the highest sequence number (NNN).
3. The next number: `highest + 1` (if empty: `TASK-001`).
4. Do not fill in numbering gaps.

### 3. JOURNAL numbering
Journals are date-based: `JOURNAL-YYYY-MM-DD-slug.md`.
If multiple Journals are created in a single day, mark the difference in the slug (e.g., `-2`, `-continuation`).

### 4. YAML Frontmatter schemas (OKF v0.2-conformant)

Every concept's frontmatter starts with the OKF-required `type` field — the concept's category, used by OKF-generic consumers for routing/filtering. This is distinct from the project's own `status` field (workflow state) and from JOURNAL's `journal_type` (session kind, see below).

**TASK schema:**
```yaml
type: Task                         # REQUIRED (OKF) — concept category, do not repurpose
id: TASK-NNN
title: "Title"
description: "One-sentence summary of what this task achieves."   # OKF recommended
project: "project-name"
status: "planning" # planning | in-progress | review | done | postponed
priority: "medium" # critical | high | medium | low
created: YYYY-MM-DD
updated: YYYY-MM-DD
completed:
resource: ""                       # optional (OKF) — canonical URI: PR / branch / issue link
tags: []
journals: []
related_tasks: []
complexity: "medium"
# complexity values: small | medium | large | epic
generated: { by: "agent:<agent-id>", at: "<ISO8601>" }   # OKF v0.2 — who/when produced this file
verified: []                       # OKF v0.2 — filled at /task-close on human approval, see §9
```

**JOURNAL schema:**
```yaml
type: Journal                      # REQUIRED (OKF) — concept category
id: JOURNAL-YYYY-MM-DD-slug
title: "Short topic description"   # OKF recommended, mirrors `topic`
description: "One-sentence summary of the session outcome."
date: YYYY-MM-DD
project: "project-name"
topic: "Topic"
journal_type: "planning"           # renamed from the old `type` field
# journal_type values: planning | debugging | review | decision | brainstorm
duration_approx: "N min"
related_tasks: []
participants: [human, ai-agent]
outcome: "Summary"
tags: []
generated: { by: "agent:<agent-id>", at: "<ISO8601>" }
```

> ⚠️ **Breaking rename:** the old `type: "planning"` field (session kind) is now `journal_type`. `type` is reserved for the OKF concept category (`Journal`). See §10 for migrating pre-existing journals.

**MODULE schema** (durable code-unit reference, see §11):
```yaml
type: Module                       # REQUIRED (OKF) — concept category
id: MODULE-slug
title: "Module Name"
description: "One-sentence summary of this module's responsibility."
project: "project-name"
status: "active"                   # active | deprecated | planned
owner: ""
created: YYYY-MM-DD
updated: YYYY-MM-DD
stale_after: 90                    # days since last full human verification before needs-review
tags: [module]
related_modules: []
related_tasks: []
related_adrs: []
generated: { by: "agent:<agent-id>", at: "<ISO8601>" }
verified: []                       # same shape as TASK.verified — never self-verified by an agent
```

### 5. Cross-linking (OKF-conformant — replaces Obsidian wikilinks)

Every reference between concepts MUST use a standard markdown link, bundle-relative to `_docs/` (leading `/`) — NOT the `[[wikilink]]` syntax:

- ✅ `[TASK-014](/tasks/TASK-014_add-auth.md)`
- ✅ `[ADR-003](/DECISIONS-LOG.md#adr-003)`
- ❌ `[[TASK-014]]` — Obsidian-only syntax; invisible to any generic OKF consumer (including the OKF reference visualizer), so it breaks the cross-link graph outside Obsidian.

Obsidian renders standard markdown links and still tracks them for backlinks, so this loses nothing on the Obsidian side while gaining OKF conformance elsewhere.

### 6. index.md maintenance (OKF reserved filename, §6 of the spec)

`index.md` carries no frontmatter — its body is a flat link list grouped under headings. Update the relevant directory's `index.md` whenever a concept is added, closed, or renamed:

- New/closed TASK → update `_docs/tasks/index.md`
- New JOURNAL → update `_docs/journal/index.md`
- New/deprecated MODULE → update `_docs/modules/index.md` (manually — `/module-sync` does not auto-maintain this, see §11)
- Either → update root `_docs/index.md` if it changes what's currently active

Format (see `_docs/_templates/INDEX-template.md`):
```markdown
# Tasks

* [TASK-014 — Add auth](TASK-014_add-auth.md) - in-progress, high priority
* [TASK-013 — Fix migration](TASK-013_fix-migration.md) - done
```

### 7. log.md maintenance (OKF reserved filename, §7 of the spec)

`_docs/log.md` is the OKF-reserved chronological history file, separate from `DECISIONS-LOG.md` (which stays the detailed ADR archive). Append one line per meaningful change, newest date-group first, on every `/task-new`, `/task-close`, and `/journal-distill` (see `_docs/_templates/LOG-template.md`):

```markdown
## 2026-08-06
* **Creation**: Opened [TASK-014](tasks/TASK-014_add-auth.md).
* **Update**: Closed [TASK-013](tasks/TASK-013_fix-migration.md).
```

> `/module-sync` does NOT append to `log.md` — module documents are durable references, not
> dated events; a module update is recorded in the module file's own "Change log" section
> instead (see §11).

### 8. Distillation rules (Journal)
During `/journal-distill`, the following must be extracted:
- **Context:** Why did the conversation start? (Status description, not "the user asked").
- **Options:** Investigated alternatives in a table (advantage, disadvantage, decision).
- **Decisions:** What was finalized? (**[Subject]:** Rationale).
- **Open Questions:** What remains unanswered? (In question form).
- **Next Steps:** Specific actions.
- **Sources (OKF v0.2):** If the journal references external material (docs, articles, code outside the repo), add a `sources` frontmatter entry instead of a loose link, and cite it in the body with `[^source-id]`:
```yaml
sources:
  - { id: okf-spec, resource: "https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md", title: "OKF v0.2 Spec", last_modified: 2026-07-24 }
```

### 9. Closing rules (Task Close)
During `/task-close`:
1. `status` -> `done`, `completed` -> today's date.
2. Fill in the `Result` section (in past tense, factually).
3. Fill in the "Agent summary" block at the end of the file.
4. If an ADR-level decision was made, update `DECISIONS-LOG.md` (per-entry `Status:` becomes `accepted` | `deprecated` | `superseded`).
5. **On human approval of the close**, append to `verified`: `{ by: "human:<username>", at: "<ISO8601>" }`. An agent must never self-verify its own task — this field exists specifically to make the project's "merges are always human-approved" rule queryable, not just documented.
6. Update `_docs/tasks/index.md` and append the corresponding `log.md` entry.
7. Ensure log-like entries (`log.md`, journal history) are not rewritten retroactively.
8. `/task-close` never triggers `/module-sync` — module documents are only touched by explicit, manual invocation (see §11).

### 10. Migrating existing documents (optional, non-blocking)

Files written before this OKF pass (missing `type`, `generated`, etc.) remain fully valid — OKF conformance (§9 of the spec) requires only a non-empty `type` field, and consumers MUST tolerate missing optional fields. Backfill `type` / `journal_type` opportunistically when a file is next edited by an agent; do not batch-rewrite the whole corpus in one pass.

### 11. Module documents (MODULE) and `/module-sync`

`_docs/modules/*.md` files are a durable, code-organization layer distinct from TASK/JOURNAL: one file per logical module (e.g. `Billing`, `Notifications`), describing what it's responsible for, where it lives in the code, its cross-module dependencies, and — most importantly for an agent — its **invariants** (implicit rules the code doesn't enforce with an explicit error, but whose violation breaks something elsewhere).

Key rules:
- **Manual only.** Module files are created and updated exclusively through the explicit `/module-sync <module-name> [TASK-NNN]` command (see `.agents/workflows/module-sync.md`). No other workflow — including `/task-close` — creates, updates, or references a module file automatically.
- **Delta, not regeneration.** `/module-sync` proposes section-level changes for human approval; it never rewrites a module file wholesale. The "Responsibility" and "Key decisions" sections are never agent-generated — human/ADR-sourced only.
- **Invariant conflicts block.** If a proposed change appears to violate a documented invariant, the sync cannot complete without an explicit human decision (update the invariant, or fix the code) — this is a hard stop, the same severity class as the `verified` gate in §9.
- **`verified` is file-level, not delta-level.** Approving a proposed delta does not mark the file `verified` — only a full, deliberate review of the entire module file does, appended in the same `{ by, at }` shape as `TASK.verified`.
- **`stale_after` (default 90 days):** if no full human verification has occurred within this window, the module should be treated as `needs-review` — checked at sync time, not enforced by a background job.
- **`_docs/modules/index.md` is maintained by hand**, not by `/module-sync` — see §6.
