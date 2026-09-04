---
description: One-time bootstrap for a new AI agent on this project. Builds a native bridge so the agent picks up the AI Docs framework (`.agents/` + `agent.md`) automatically in every future session, without relying on framework-specific auto-loading it may not support.
---

## Why this workflow exists

`AI Docs` is agent-agnostic: its own instructions live in `.agents/` (rules, skills, workflow/`/`-command definitions) and its project knowledge lives in `_docs/`, distilled into `agent.md` at the repo root. Some agentic tools auto-load `.agents/` and `agent.md` natively; many don't — they only auto-load their own convention (e.g. a single memory/instructions file read at session start).

`/install` only needs to run **once per agent** (not once per session): the first time this project is opened with a given AI agent. It detects that agent's native auto-loaded file and writes a bridge section into it, so from then on that agent picks up the framework by itself. Re-run it only if the bridge section goes missing or the agent's native convention changes.

Do not run this automatically — only when the user explicitly asks to install/bootstrap the framework for the current agent.

## /install command execution

### 1. Identify yourself

Determine which agentic tool is running this session (e.g. Claude Code, Antigravity, OpenCode, Cursor, Windsurf, GitHub Copilot, Gemini CLI, or other). If genuinely unsure, ask the user which tool this is — do not guess silently, a wrong target file creates a dead bridge.

### 2. Resolve your native auto-loaded file

Every agentic tool has (at most) one file it reads into context automatically at session/project start, without being told to. Resolve it for the identified tool, for example:

| Agent | Native file |
|---|---|
| Claude Code | `CLAUDE.md` |
| Cursor | `.cursorrules` (or `.cursor/rules/*.mdc`) |
| Windsurf | `.windsurfrules` |
| GitHub Copilot | `.github/copilot-instructions.md` |
| Gemini CLI | `GEMINI.md` |
| Antigravity, OpenCode, or others with no fixed convention | ask the user, or check that tool's own docs for its instructions-file name |

If the tool has no such native file at all (nothing is ever auto-loaded), tell the user so and stop — there is nothing to bridge into; the user will need to paste `agent.md` into context manually each session.

### 3. Check for an existing bridge

Read the native file (create it if it doesn't exist yet). Look for a section delimited by:

```
<!-- AI_DOCS_BRIDGE_START -->
...
<!-- AI_DOCS_BRIDGE_END -->
```

- If found: this agent is already installed. Report that and stop (unless the user asked to force-reinstall, in which case replace only the content between the markers).
- If not found: continue to step 4. Never touch any other content in the file — append the bridge block, preserving everything else verbatim.

### 4. Write the bridge block

Append this block (translate none of the literal paths/markers; fill in `<agent-id>` with the tool identified in step 1):

```
<!-- AI_DOCS_BRIDGE_START -->
## AI Docs framework (bridge — do not remove)

This project uses the **AI Docs** framework. Its own rules, skills, and workflow commands live outside this file, in `.agents/`, and are not part of your native auto-loading — treat this section as your entry point into them:

- **At the start of every session**, read `agent.md` (repo root) — it is the "Shared Brain": a fresh, generated digest of `_docs/` (active tasks, recent journals, key decisions, open questions). If `agent.md` doesn't exist yet, run `/agent-sync` first to generate it.
- **Always apply** `.agents/rules/style-guide.md` as an always-on rule for this project (documentation conventions, OKF `_docs/` rules, coding/commenting conventions).
- **Load on demand** every `.agents/skills/*/SKILL.md` whose "When to use this skill?" section matches the current task, before acting on that task.
- **Treat every file in `.agents/workflows/*.md` as a `/`-command definition.** When the user types `/<filename-without-extension>` (with or without arguments), read that exact file and execute its instructions as written — do not paraphrase or skip steps. Do not invoke a workflow the user didn't ask for.
- Never edit `_docs/BOARD.md`, `_docs/index.md`, or `_docs/log.md` by hand — they're generated/maintained by the workflows above.

Installed for: `<agent-id>` | Installed: `<today's date, ISO8601>`
<!-- AI_DOCS_BRIDGE_END -->
```

### 4b. Bridge native command/skill directories (if the agent supports them)

Some agents have richer native extension points beyond the auto-loaded memory file — a custom-slash-command folder and/or a skill-discovery folder that the tool scans natively (showing up in its own `/help`, autocomplete, or skill listings — not just LLM-interpreted text). Resolve these too:

| Agent | Native commands folder | Native skills folder |
|---|---|---|
| Claude Code | `.claude/commands/*.md` | `.claude/skills/<name>/SKILL.md` |
| Cursor / Windsurf / others | check that tool's own docs; skip if it has no such convention |

If the agent has a native commands folder:
- For **every** `.agents/workflows/*.md`, create a matching thin wrapper file (e.g. `.claude/commands/agent-sync.md`) with the source workflow's own `description:` as frontmatter, and this body: "Read and execute `.agents/workflows/<name>.md` exactly as written — do not paraphrase or skip steps."
- Do not duplicate the workflow's actual instructions into the wrapper — it stays a pointer, so `.agents/` remains the single source of truth.

If the agent has a native skills folder:
- For **every** `.agents/skills/*/SKILL.md`, create a matching thin wrapper `.claude/skills/<name>/SKILL.md`, copying the original's `name:`/`description:` frontmatter verbatim (so native triggering/discovery behaves identically), with this body: "Load and follow `.agents/skills/<name>/SKILL.md` in full as this skill's actual instructions."

Skip this step entirely for agents with no such native folders — the memory-file bridge from step 4 is their only entry point.

### 5. Confirm

- Do not ask back before writing — only step 1 warrants a question, and only if the agent identity is genuinely ambiguous.
- Output: "AI Docs bridge installed for `<agent-id>` in `<native file>`. Read `agent.md` now to load project context."
- Then immediately read `agent.md` (or run `/agent-sync` if missing) to load context for the rest of this session.
