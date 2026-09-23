---
description: One-time bootstrap for a new AI agent on this project. Builds a native bridge so the agent picks up the AI Docs framework (`.agents/` + `agent.md`) automatically in every future session, without relying on framework-specific auto-loading it may not support.
---

## Why this workflow exists

`AI Docs` is agent-agnostic: its own instructions live in `.agents/` (rules, skills, workflow/`/`-command definitions) and its project knowledge lives in `_docs/`, distilled into `agent.md` at the repo root. Agentic tools generally do **not** auto-load `.agents/` or `agent.md` — each only auto-loads its own convention (e.g. `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`). Note that `agent.md` is not the `AGENTS.md` open standard: the names differ (and on case-sensitive filesystems they are different files), so no tool picks `agent.md` up by accident.

`/install` only needs to run **once per agent** (not once per session): the first time this project is opened with a given AI agent. It detects that agent's native auto-loaded file and writes a bridge section into it, so from then on that agent picks up the framework by itself. Re-running it is safe: it never duplicates the bridge, and it refreshes the native command/skill wrappers (step 4b) so workflows added since the last run show up too.

`/install` never creates, deletes, or resets anything in `_docs/` — project content is not its job. A project starts with an empty bundle only when it was created from the clean `release` branch (see the root `README.md`); step 0 catches the case where it wasn't.

Do not run this automatically — only when the user explicitly asks to install/bootstrap the framework for the current agent.

## /install command execution

### 0. Sanity check: release template, not the framework's own docs

The framework repository's `main` branch documents AI Docs itself (dogfooding), so a plain clone of `main` ships AI Docs' own `agent.md`, `PROJECT-OVERVIEW.md`, tasks, ADRs, log, and board. Before installing, check for that:

- `_docs/PROJECT-OVERVIEW.md` or `_docs/DECISIONS-LOG.md` has `project: "ai-docs"` in its frontmatter, or
- `_docs/tasks/` contains `TASK-001_standalone-docs-board-viewer.md`.

If either is true **and** the user is not working on the AI Docs framework itself, stop and warn: "This `_docs/` bundle holds AI Docs' own project documentation, not yours: the repo was cloned from `main` instead of the clean `release` branch. Re-create the project from the `release` branch as described in the AI Docs README (Quick Start, step 2), then run `/install` again." Never delete or reset those files yourself. Continue only if the user explicitly says this *is* the AI Docs repository.

### 1. Identify yourself

Determine which agentic tool is running this session (e.g. Claude Code, Antigravity, OpenCode, Codex, Cursor, Windsurf, GitHub Copilot, Gemini CLI, or other). If genuinely unsure, ask the user which tool this is — do not guess silently, a wrong target file creates a dead bridge.

### 2. Resolve your native auto-loaded file

Every agentic tool has (at most) one file it reads into context automatically at session/project start, without being told to. Resolve it for the identified tool, for example:

| Agent | Native file | Notes |
|---|---|---|
| Claude Code | `CLAUDE.md` | Supports `@path` imports (see step 4). |
| Gemini CLI | `GEMINI.md` | Supports `@path` imports (see step 4). |
| Codex, OpenCode, and other tools following the `AGENTS.md` convention | `AGENTS.md` | One bridge here serves every such tool at once. |
| Cursor | `.cursor/rules/ai-docs.mdc` | Needs `alwaysApply: true` frontmatter, or it is not loaded every session. `.cursorrules` is legacy. |
| Windsurf | `.windsurf/rules/ai-docs.md` | Needs `trigger: always_on` frontmatter. `.windsurfrules` is legacy. |
| GitHub Copilot | `.github/copilot-instructions.md` | |
| Antigravity, or others with no documented convention | ask the user, or check that tool's own docs for its instructions-file name | |

If a tool supports several files, prefer its most specific native one (e.g. `CLAUDE.md` over `AGENTS.md` for Claude Code). If the tool has no such native file at all (nothing is ever auto-loaded), tell the user so and stop — there is nothing to bridge into; the user will need to paste `agent.md` into context manually each session.

### 3. Check for an existing bridge

Read the native file (create it if it doesn't exist yet, with the frontmatter from step 2's Notes column where one is required). Look for a section delimited by:

```
<!-- AI_DOCS_BRIDGE_START -->
...
<!-- AI_DOCS_BRIDGE_END -->
```

- If found: the bridge is already installed. Do not rewrite it (unless the user asked to force-reinstall, in which case replace only the content between the markers). If the `Installed for:` line doesn't list the current agent yet (e.g. a second tool sharing `AGENTS.md`), append it to that line. Then skip to step 4b — the wrapper refresh still runs.
- If not found: continue to step 4. Never touch any other content in the file — append the bridge block, preserving everything else verbatim.

### 4. Write the bridge block

Append this block (translate none of the literal paths/markers; fill in `<agent-id>` with the tool identified in step 1). For agents whose native file supports `@path` imports (Claude Code, Gemini CLI), keep the `@agent.md` line: it makes the tool load `agent.md`, and through it `agent.md`'s own `@_docs/index.md` import, natively at every session start. A plain "read `agent.md`" instruction can't do that, because a nested import only resolves when the importing file was itself loaded through an import. For every other agent, delete the `@agent.md` line and its comment.

```
<!-- AI_DOCS_BRIDGE_START -->
## AI Docs framework (bridge — do not remove)

<!-- Native import (Claude Code / Gemini CLI only): loads agent.md and its @_docs/index.md map. -->
@agent.md

This project uses the **AI Docs** framework. Its own rules, skills, and workflow commands live outside this file, in `.agents/`, and are not part of your native auto-loading — treat this section as your entry point into them:

- **At the start of every session**, read `agent.md` (repo root) unless it was already imported above — it is the "Shared Brain": a generated digest of the *stable* facts about this project (what it is, tech stack, where things live, the conventions you must follow), ending with an `@_docs/index.md` import that maps the whole knowledge bundle.
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

### 4b. Bridge native command/skill directories (if the agent supports them)

Some agents have richer native extension points beyond the auto-loaded memory file — a custom-slash-command folder and/or a skill-discovery folder that the tool scans natively (showing up in its own `/help`, autocomplete, or skill listings — not just LLM-interpreted text). Resolve these too:

| Agent | Native commands folder | Native skills folder |
|---|---|---|
| Claude Code | `.claude/commands/*.md` | `.claude/skills/<name>/SKILL.md` |
| Others | check that tool's own docs; skip if it has no such convention | |

This step runs on every `/install`, including re-runs where the bridge already existed, so it doubles as the sync for workflows/skills added or removed since the last run. Every wrapper it writes carries the marker line `<!-- AI_DOCS_WRAPPER -->` in its body — that marker is how later runs tell generated wrappers apart from the user's own commands/skills, which must never be modified or deleted.

If the agent has a native commands folder:
- For **every** `.agents/workflows/*.md`, create or refresh a matching thin wrapper file (e.g. `.claude/commands/agent-sync.md`) with the source workflow's own `description:` as frontmatter, and this body:
  ```
  <!-- AI_DOCS_WRAPPER -->
  Read and execute `.agents/workflows/<name>.md` exactly as written — do not paraphrase or skip steps.

  Arguments: $ARGUMENTS
  ```
  The `$ARGUMENTS` line is what forwards e.g. `title="..."` from `/task-new title="..."` into the workflow. Without it, the arguments are silently dropped. Use the argument placeholder syntax of the agent at hand if it differs.
- Do not duplicate the workflow's actual instructions into the wrapper — it stays a pointer, so `.agents/` remains the single source of truth.
- Delete any file in the commands folder that carries the `<!-- AI_DOCS_WRAPPER -->` marker but whose source workflow no longer exists.

If the agent has a native skills folder:
- For **every** `.agents/skills/*/SKILL.md`, create or refresh a matching thin wrapper `.claude/skills/<name>/SKILL.md`, copying the original's `name:`/`description:` frontmatter verbatim (so native triggering/discovery behaves identically), with this body:
  ```
  <!-- AI_DOCS_WRAPPER -->
  Load and follow `.agents/skills/<name>/SKILL.md` in full as this skill's actual instructions.
  ```
- Delete any wrapper skill folder carrying the marker whose source skill no longer exists.

Skip this step entirely for agents with no such native folders — the memory-file bridge from step 4 is their only entry point.

### 5. Confirm

- Do not ask back before writing — only steps 0 and 1 warrant a question, and only when they apply.
- Output: "AI Docs bridge installed for `<agent-id>` in `<native file>`." (or "already installed — wrappers refreshed"), plus the number of command/skill wrappers created, refreshed, and removed in step 4b.
- Then load context for the rest of this session:
  - `_docs/PROJECT-OVERVIEW.md` missing (fresh project) → tell the user the next step is `/project-sync` (it asks about the project), then `/agent-sync`. Don't run them unprompted: `/project-sync` needs the user's answers.
  - `agent.md` missing but `PROJECT-OVERVIEW.md` present → run `/agent-sync`.
  - Otherwise → read `agent.md` now (if it wasn't already imported natively).
