---
description: Generates a technical implementation plan in `_docs/tasks/plans/` and syncs milestones to the TASK file.
---

## Documentation Workflow

Apply **Documentation Workflow Skill** rules and `_docs/_templates/PLAN-template.md`.

### /plan-new [TASK-NNN | title]

#### 1. Scope & Context Gathering

- **Target**: Resolve `TASK-NNN` from argument, title, or active session context.
- **Read Context** (Documentation First):
  1. `_docs/tasks/briefs/TASK-NNN_brief.md` (if present) for requirements.
  2. `agent.md` and relevant `_docs/arch/*.md` files (`STACK`, `DIRECTORY-MAP`, `ENTRYPOINTS`, `DATABASE-SCHEMA`).
  3. `_docs/tasks/TASK-NNN_slug.md` (if present) for Goal, Context, and Decisions.

#### 2. Plan Generation

1. Read `_docs/_templates/PLAN-template.md`.
2. Create `_docs/tasks/plans/TASK-NNN_plan.md`:
   - **Frontmatter**: `type: Plan`, `id: PLAN-TASK-NNN`, `task_id: TASK-NNN`, `status: "proposed"`.
   - **Technical Summary & Approach**: Architecture, trade-offs, patterns.
   - **Affected Files & Components**: Exact file paths, actions (Create/Modify), purposes.
   - **Step-by-Step Technical Plan**: Concrete logic, signatures, queries.
   - **Verification & Testing Strategy**: Automated and manual checks.
   - **Risks & Mitigation**: Edge cases and fallbacks.
   - **High-Level Milestones**: 3–8 concise, checkable items.
3. Update `_docs/tasks/plans/index.md`.

#### 3. Task File Synchronization

If `_docs/tasks/TASK-NNN_slug.md` exists:
1. Link plan under `## Planning Reference`:
   `- Implementation Plan: [TASK-NNN Plan](/tasks/plans/TASK-NNN_plan.md)`
2. If `## Approach / Plan` checklist is empty or placeholder, populate it with High-Level Milestones from the plan.

#### 4. Output

Create file without asking back. Output:
"Implementation plan created: `_docs/tasks/plans/TASK-NNN_plan.md`." Highlight key technical choices and milestones for user review.
