# YAML Frontmatter Schemas (OKF v0.2-Conformant)

This document contains the detailed YAML frontmatter schemas for each concept category within the OKF v0.2-conformant `_docs/` knowledge bundle.

Every concept's frontmatter starts with the OKF-required `type` field — the concept's category, used by OKF-generic consumers for routing/filtering. This is distinct from the project's own `status` field (workflow state) and from JOURNAL's `journal_type` (session kind).

---

## 1. TASK Schema

Used for actionable development or documentation tasks (`_docs/tasks/TASK-NNN_slug.md`).

```yaml
type: Task                         # REQUIRED (OKF) — concept category, do not repurpose
id: TASK-NNN
title: "Title"
description: "One-sentence summary of what this task achieves."   # OKF recommended
project: "project-name"
status: "planning"                 # planning | in-progress | review | done | postponed
priority: "medium"                 # critical | high | medium | low
created: YYYY-MM-DD
updated: YYYY-MM-DD
completed:
resource: ""                       # optional (OKF) — canonical URI: PR / branch / issue link
tags: []
journals: []
related_tasks: []
related_modules: []                # module ids this task touches — read by modules/*.md Dataview
complexity: "medium"               # small | medium | large | epic
generated: { by: "agent:<agent-id>", at: "<ISO8601>" }   # OKF v0.2 — who/when produced this file
verified: []                       # OKF v0.2 — filled at /task-close on human approval
```

### Notes & Field Rules
- **Subtask Tracker:** The task body's `Approach / Plan` section must be a checkbox list (`- [ ] step`), not a numbered list. `/board-sync` reads it verbatim to render subtask progress on `_docs/BOARD.md`.
- **`verified` Gate:** Filled upon human approval at task closing. Agents must never self-verify tasks.

---

## 2. JOURNAL Schema

Used for session logs, distillations, and decision records (`_docs/journal/JOURNAL-YYYY-MM-DD-slug.md`).

```yaml
type: Journal                      # REQUIRED (OKF) — concept category
id: JOURNAL-YYYY-MM-DD-slug
title: "Short topic description"   # OKF recommended, mirrors `topic`
description: "One-sentence summary of the session outcome."
date: YYYY-MM-DD
project: "project-name"
topic: "Topic"
journal_type: "planning"           # renamed from legacy `type` field
# journal_type values: planning | debugging | review | decision | brainstorm
duration_approx: "N min"
related_tasks: []
participants: [human, ai-agent]
outcome: "Summary"
tags: []
generated: { by: "agent:<agent-id>", at: "<ISO8601>" }
sources: []                        # OKF v0.2 — external citations
```

### Notes & Field Rules
- ⚠️ **Breaking Rename:** The old `type: "planning"` field (session kind) is `journal_type`. `type` is strictly reserved for the OKF concept category (`Journal`).
- **Sources citation:** If external references are cited, format `sources` entries as:
  ```yaml
  sources:
    - { id: okf-spec, resource: "https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md", title: "OKF v0.2 Spec", last_modified: 2026-07-24 }
  ```
  Cite in markdown body via `[^okf-spec]`.

---

## 3. MODULE Schema

Used for durable code-unit documentation (`_docs/modules/MODULE-slug.md`).

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

### Notes & Field Rules
- Managed exclusively via `/module-sync` (manual invocation only).
- `stale_after` defaults to 90 days. If expired without a human verification, the module is flagged `needs-review`.

---

## 4. BRIEF Schema

Used for human-authored task requirements (`_docs/tasks/briefs/TASK-NNN_brief.md`).

```yaml
type: Brief                        # REQUIRED (OKF) — concept category
id: BRIEF-TASK-NNN
task_id: TASK-NNN
title: "Brief: [Task Name]"
project: "project-name"
author: human
status: "ready"                    # draft | ready | revised
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [brief]
```

---

## 5. PLAN Schema

Used for agent-generated technical plans (`_docs/tasks/plans/TASK-NNN_plan.md`).

```yaml
type: Plan                         # REQUIRED (OKF) — concept category
id: PLAN-TASK-NNN
task_id: TASK-NNN
title: "Implementation Plan: [Task Name]"
project: "project-name"
status: "proposed"                 # proposed | approved | in-progress | completed
created: YYYY-MM-DD
updated: YYYY-MM-DD
generated: { by: "agent:<agent-id>", at: "<ISO8601>" }
tags: [plan]
```
