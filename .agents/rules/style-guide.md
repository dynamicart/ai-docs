---
trigger: always_on
---

## AI-Driven Code & Docs Style Guide

Rules for project coding, code documentation, and maintenance routines. Goal: sustainability and AI-based searchability.

### 1. Coding & Inline Documentation

- **Style:** K&R (Kernighan & Ritchie) style.
- **Modularity:** Strive for modularity and reusability where appropriate.
- **"Why, not What":** Code describes what it does; comments explain why (business logic, constraints, workarounds).
- **File Header:** Mandatory on new or significantly modified files:
  ```
  /**
   * @file [Filename]
   * @description [1-sentence summary of the function]
   * @context [Module name, callers]
   */
  ```
- **Function Documentation:** Use standard blocks for complex functions (`@param`, `@returns`, `@throws`).
- **Visuals:** Use Mermaid.js diagrams where text or system flow is complex.
- **Decisions:** Record major architectural decisions as ADR-NNN in `_docs/DECISIONS-LOG.md`.

### 2. Knowledge Bundle & OKF Conformance

- `_docs/` is an OKF v0.2 knowledge bundle (`tasks/`, `journal/`, `arch/`, `modules/`, `spec/`).
- **Links:** Use bundle-relative markdown links (`[Title](/tasks/TASK-NNN_slug.md)`), never `[[wikilinks]]`.
- **Frontmatter:** Every concept frontmatter must include a `type` field (`Task`, `Journal`, `Module`, etc.).
- Detailed schemas, validation, and file rules live in the **Documentation Workflow Skill** (`.agents/skills/documentation/SKILL.md`).

### 3. End-of-Task Maintenance Routine

At the end of a task, offer appropriate closing actions:
- `/task-close` to archive the task and update board/log.
- `/journal-distill` to capture and close the session.
- `/project-sync` if project status moved (tasks, decisions, recent changes).
- `/arch-sync` if architectural specs, stack, routes, or schemas changed.
- `/agent-sync` only if stable facts changed (tech stack, layout, global conventions).

#### Exploration-Cost Trigger (`/module-sync`)
If understanding a code unit required disproportionate discovery effort that will be needed in future tasks, propose `/module-sync <module-name>` at the end of the task:
- **Proposal only:** Never run `/module-sync` automatically; it requires human invocation.
- **Seed sentence:** Name the module and explain in one sentence the expensive invariant or convention worth persisting.
- **Not for one-offs:** Do not propose it for a throwaway lookup, nor if an existing `arch/*.md` or `modules/*.md` file already covers it — check first.
- **No module file yet:** Say so; `/module-sync` will offer to create one from `MODULE-template.md`.
- **Once per module:** One proposal per module, at the end of the task — not mid-work, and not repeated for the same module in the same session.
