---
description: Generates or updates a detailed technical implementation plan in `_docs/tasks/plans/` based on chat discussion and any existing brief.
---

## Documentation Workflow

Apply the rules of the **Documentation Workflow Skill** whenever creating or updating an implementation plan.

## /plan-new [TASK-NNN | title]

Generate a technical implementation plan for a task.

### 1. Resolve Target Task & Scope

- If `TASK-NNN` is supplied (e.g. `/plan-new TASK-003`): target that specific task.
- If a title or description is supplied: determine the next available `TASK-NNN` identifier.
- If omitted: infer from the active task in current context, or ask the user which task this plan is for.

### 2. Gather Context (Documentation First)

1. Read `_docs/tasks/briefs/TASK-NNN_brief.md` if it exists — understand the business requirements and acceptance criteria.
2. Read `agent.md` (Tech stack, architecture) and relevant `_docs/arch/` files (`STACK.md`, `DIRECTORY-MAP.md`, `ENTRYPOINTS.md`, `DATABASE-SCHEMA.md`).
3. If an existing `TASK-NNN.md` exists, read its Goal, Context, and Decisions sections.

### 3. Generate Implementation Plan

1. Read `_docs/_templates/PLAN-template.md`.
2. Draft `_docs/tasks/plans/TASK-NNN_plan.md`:
   - Fill frontmatter (`type: Plan`, `id: PLAN-TASK-NNN`, `task_id: TASK-NNN`, status: "proposed").
   - **Technical Summary & Approach:** Architecture, trade-offs, patterns.
   - **Affected Files & Components:** Exact filepaths, actions (Create/Modify), and purposes.
   - **Step-by-Step Technical Plan:** Concrete implementation details (code structure, signatures, queries).
   - **Verification & Testing Strategy:** Automated tests and manual check steps.
   - **Risks & Mitigation:** Edge cases and potential gotchas.
   - **High-Level Milestones:** 3–8 concise, checkable milestone items (suitable for `Approach / Plan` in `TASK-NNN.md`).
3. Update `_docs/tasks/plans/index.md` with the new plan link.

### 4. Sync with TASK File (if it exists)

If `_docs/tasks/TASK-NNN_slug.md` already exists:
1. Ensure the plan link is listed under `## Planning Reference`:
   `- Implementation Plan: [TASK-NNN Plan](/tasks/plans/TASK-NNN_plan.md)`
2. If the task's `## Approach / Plan` checklist is empty or has only template placeholders, populate it with the High-Level Milestones from the plan.


### 5. Confirm

- Do not ask back before writing the draft plan.
- Output: "Implementation plan created: `_docs/tasks/plans/TASK-NNN_plan.md`."
- Highlight the key technical choices and milestones for human review.