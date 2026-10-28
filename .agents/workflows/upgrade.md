---
description: Upgrades the AI Docs framework in an existing project: refreshes the native agent bridge in-place, syncs native command/skill wrappers, ensures required directory structures exist, and runs /agent-sync.
---

## Why this workflow exists

As the AI Docs framework evolves (new conventions, stricter agent protocols, new workflow commands like `/plan-new` or subdirectories like `briefs/` and `plans/`), existing projects need a safe, one-command way to bring their configuration up to date without losing any project data or manual customizations.

`/upgrade` is idempotent and completely safe:
- Never touches user tasks, journals, ADRs, or project code.
- Only modifies content strictly inside the `<!-- AI_DOCS_BRIDGE_START -->` / `<!-- AI_DOCS_BRIDGE_END -->` markers in your native agent instructions file, leaving your own custom rules outside untouched.
- Refreshes generated command wrappers (`<!-- AI_DOCS_WRAPPER -->`), leaving custom commands untouched.
- Ensures missing framework directories exist with proper index files.
- Refreshes `agent.md` above the `<!-- AGENT_SYNC_BOUNDARY -->` marker.

## /upgrade command execution

### 1. Identify agent and native instructions file

Determine the current AI agent running this session (same resolution as `.agents/workflows/install.md` step 2):

| Agent | Native file | Notes |
|---|---|---|
| Claude Code | `CLAUDE.md` | Supports `@path` imports. |
| Gemini CLI | `GEMINI.md` | Supports `@path` imports. |
| Codex, OpenCode, and tools using `AGENTS.md` | `AGENTS.md` | Shared by all AGENTS.md-compatible tools. |
| Cursor | `.cursor/rules/ai-docs.mdc` | `alwaysApply: true` frontmatter. |
| Windsurf | `.windsurf/rules/ai-docs.md` | `trigger: always_on` frontmatter. |
| GitHub Copilot | `.github/copilot-instructions.md` | |
| Others | Consult tool documentation or ask the user. | |

### 2. In-place Bridge Update

Read the native instructions file:
1. Locate the bridge section delimited by:
   ```
   <!-- AI_DOCS_BRIDGE_START -->
   ...
   <!-- AI_DOCS_BRIDGE_END -->
   ```
2. Read the latest bridge template from `.agents/workflows/install.md` (Step 4), including the `MANDATORY PRE-ACTION PROTOCOL: Documentation First`.
3. If an existing bridge was found:
   - Extract any existing agents listed on the `Installed for:` line and merge the current `<agent-id>`.
   - Replace **strictly the content between the markers** with the latest bridge block.
   - **Never modify** any content before `<!-- AI_DOCS_BRIDGE_START -->` or after `<!-- AI_DOCS_BRIDGE_END -->`.
4. If no bridge section exists yet:
   - Append the complete bridge block to the file, preserving all existing content verbatim.

### 3. Sync Native Command and Skill Wrappers

If the agent supports native command or skill directories (e.g. Claude Code `.claude/commands/` and `.claude/skills/`):

1. **Commands:** For every `.agents/workflows/*.md` (including `/plan-new`, `/upgrade`, etc.), create or refresh its thin wrapper carrying `<!-- AI_DOCS_WRAPPER -->` (see `install.md` step 4b). Remove any wrapper carrying that marker whose source workflow no longer exists.
2. **Skills:** For every `.agents/skills/*/SKILL.md`, create or refresh its thin wrapper carrying `<!-- AI_DOCS_WRAPPER -->`. Remove orphaned wrappers.

### 4. Verify & Bootstrap Framework Directories

Check if the standard framework directories exist under `_docs/`. If any are missing, create them with their `.gitkeep` and `index.md`:

- `_docs/tasks/briefs/` (human-written task briefs)
- `_docs/tasks/plans/` (agent-generated technical plans)
- `_docs/modules/` (durable module references)
- `_docs/arch/` (architecture live references)
- `_docs/spec/` (business specifications)
- `_docs/journal/` (session journals)

Ensure `_docs/tasks/index.md` contains links to `briefs/` and `plans/` under a `# Subdirectories` heading if not present.

### 5. Refresh Shared Brain (`agent.md`)

If `agent.md` exists:
1. Run `.agents/workflows/agent-sync.md` to regenerate `agent.md` above the `<!-- AGENT_SYNC_BOUNDARY -->` marker.
2. This ensures `agent.md` reflects any updated rules (e.g. Documentation first, zero blind probing) and includes current stack details.

### 6. Confirm and Report

Output a clear, bulleted summary of what was upgraded:
- Native bridge status: updated in `<native file>` (protocol updated to latest).
- Command & skill wrappers: [N] refreshed, [N] added, [N] removed.
- Directories: verified [N] folders (created any missing ones).
- Shared Brain: `agent.md` synced.
