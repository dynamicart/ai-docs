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

Installed for: `claude-code` | Installed: `2026-09-23`
<!-- AI_DOCS_BRIDGE_END -->
