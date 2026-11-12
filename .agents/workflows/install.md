---
description: One-time bootstrap for a new AI agent on this project. Injects a native bridge into the agent's auto-loaded instruction file and sets up wrappers.
---

## /install

Run once per agent on a project (run `/upgrade` for maintenance). Never modify `_docs/` content.

### 0. Sanity Check: Release Template Check

If `_docs/PROJECT-OVERVIEW.md` or `_docs/DECISIONS-LOG.md` has `project: "ai-docs"`, or `_docs/tasks/` contains `TASK-001_standalone-docs-board-viewer.md`:
- If user is not developing AI Docs itself, **abort and warn**: Repo was cloned from `main` instead of `release`. Tell user to re-create project from `release` branch per README. Do not delete files yourself.

### 1. Identify Agent & Auto-Loaded File

Identify the current tool and its native instructions file (ask user if unsure):
- **Claude Code**: `CLAUDE.md` (supports `@path` imports)
- **Gemini CLI**: `GEMINI.md` (supports `@path` imports)
- **Codex / OpenCode / AGENTS.md tools**: `AGENTS.md`
- **Cursor**: `.cursor/rules/ai-docs.mdc` (`alwaysApply: true` frontmatter)
- **Windsurf**: `.windsurf/rules/ai-docs.md` (`trigger: always_on` frontmatter)
- **GitHub Copilot**: `.github/copilot-instructions.md`
- **Antigravity / others without a known convention**: Ask user or consult the tool's docs or ask user. If no auto-load file exists, inform user to include `agent.md` manually.

### 2. Check Existing Bridge

Read the target file (create with required frontmatter if missing):
- If `<!-- AI_DOCS_BRIDGE_START -->` ... `<!-- AI_DOCS_BRIDGE_END -->` exists: replace content strictly between markers with Step 3 block; append `<agent-id>` to `Installed for:`. Never edit outside content. Proceed to Step 4.
- If missing: append the bridge block from Step 3 to the file.

### 3. Bridge Block Template

Append or update between markers (keep `@agent.md` only for Claude Code & Gemini CLI; remove for others):

```markdown
<!-- AI_DOCS_BRIDGE_START -->
## AI Docs framework (bridge — do not remove)

<!-- Native import (Claude Code / Gemini CLI only): loads agent.md and its @_docs/index.md map. -->
@agent.md

### MANDATORY PRE-ACTION PROTOCOL: Documentation First (Zero Blind Probing)
Before executing ANY shell command to inspect the environment or versions (e.g. `php -v`, `node -v`, `composer`, checking extensions/tools), and before spawning exploration subagents or running broad searches:
1. **Consult `agent.md` first:** Tech stack, runtime requirements, directory layout, and key conventions are already distilled here.
2. **Consult `_docs/index.md` & `_docs/arch/` (e.g. `STACK.md`):** Check if the required architectural, environment, or configuration detail is documented in the knowledge bundle.
3. **DO NOT probe via shell or launch exploratory subagents** for information already present in `agent.md` or `_docs/`. Only execute discovery commands or spawn exploration agents if the required fact is demonstrably absent from the documentation.

This project uses the **AI Docs** framework. Its own rules, skills, and workflow commands live outside this file, in `.agents/`, and are not part of your native auto-loading — treat this section as your entry point into them:

- **At the start of every session / before taking action**, read `agent.md` (repo root) unless it was already imported above — it is the "Shared Brain": a generated digest of the *stable* facts about this project (what it is, tech stack, where things live, the conventions you must follow), ending with an `@_docs/index.md` import that maps the whole knowledge bundle.
- **Subagent / Exploration Guard:** When delegating tasks to subagents, supply `agent.md` and the relevant `_docs/` paths directly so the subagent does not re-explore or blindly probe the environment.
- **First-time setup:** if `_docs/PROJECT-OVERVIEW.md` doesn't exist yet, run `/project-sync` first (its first run asks the user about the project); if `agent.md` doesn't exist yet, run `/agent-sync` after that. The order matters: `/agent-sync` takes the project description from `PROJECT-OVERVIEW.md`.
- **For the current project status** — active tasks, recent journals, key decisions, open questions — open `_docs/PROJECT-OVERVIEW.md` (or the files `_docs/index.md` points at). This is deliberately *not* duplicated in `agent.md`.
- `_docs/modules/*.md` are durable, human-approved module references: read them for context, but only ever update one through an explicit `/module-sync` invocation.
- **Always apply** `.agents/rules/style-guide.md` as an always-on rule for this project (documentation conventions, OKF `_docs/` rules, coding/commenting conventions).
- **Load on demand** every `.agents/skills/*/SKILL.md` whose "When to use this skill?" section matches the current task, before acting on that task.
- **Treat every file in `.agents/workflows/*.md` as a `/`-command definition.** When the user types `/<filename-without-extension>` (with or without arguments), read that exact file and execute its instructions as written — do not paraphrase or skip steps. Do not invoke a workflow the user didn't ask for.
- Never edit `_docs/BOARD.md`, `_docs/index.md`, `_docs/PROJECT-OVERVIEW.md`, `_docs/log.md`, or `agent.md` above its boundary by hand — they're generated/maintained by the workflows above.

Installed for: `<agent-id>` | Installed: `<today's date, ISO8601>`
<!-- AI_DOCS_BRIDGE_END -->
```

### 4. Setup Native Wrappers (Claude Code & compatible tools)

If the agent supports native folders:
- **Commands** (e.g. `.claude/commands/<name>.md`): For each `.agents/workflows/<name>.md`, generate thin wrapper with source `description:` in frontmatter and body:
  ```markdown
  <!-- AI_DOCS_WRAPPER -->
  Read and execute `.agents/workflows/<name>.md` exactly as written — do not paraphrase or skip steps.

  Arguments: $ARGUMENTS
  ```
  Delete wrappers carrying `<!-- AI_DOCS_WRAPPER -->` whose source workflow no longer exists.
- **Skills** (e.g. `.claude/skills/<name>/SKILL.md`): For each `.agents/skills/<name>/SKILL.md`, generate wrapper copying frontmatter with body:
  ```markdown
  <!-- AI_DOCS_WRAPPER -->
  Load and follow `.agents/skills/<name>/SKILL.md` in full as this skill's actual instructions.
  ```
  Delete orphaned wrappers.

### 5. Finalize

- Do not ask back before writing (except steps 0/1 if needed).
- Output: "AI Docs bridge installed for `<agent-id>` in `<file>`." Report wrappers created/refreshed/removed.
- **Next steps**:
  - If `_docs/PROJECT-OVERVIEW.md` missing: prompt user to run `/project-sync` then `/agent-sync`.
  - If `agent.md` missing: run `/agent-sync`.
  - Else: read `agent.md` if not already imported natively.
