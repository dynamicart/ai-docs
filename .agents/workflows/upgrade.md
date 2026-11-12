---
description: Upgrades AI Docs in an existing project: refreshes native bridge in-place, syncs wrappers, verifies directory structure, and runs /agent-sync.
---

## /upgrade

Idempotent and safe: never touches user tasks, journals, ADRs, or project code.

### 1. Identify Agent & Native File

Identify current agent and its auto-loaded instructions file:
- **Claude Code**: `CLAUDE.md` (supports `@path` imports)
- **Gemini CLI**: `GEMINI.md` (supports `@path` imports)
- **Codex / OpenCode / AGENTS.md tools**: `AGENTS.md`
- **Cursor**: `.cursor/rules/ai-docs.mdc` (`alwaysApply: true` frontmatter)
- **Windsurf**: `.windsurf/rules/ai-docs.md` (`trigger: always_on` frontmatter)
- **GitHub Copilot**: `.github/copilot-instructions.md`
- **Other**: Check tool conventions or ask user.

### 2. In-Place Bridge Update

Read the native instructions file:
1. Locate markers: `<!-- AI_DOCS_BRIDGE_START -->` ... `<!-- AI_DOCS_BRIDGE_END -->`.
2. Get latest bridge template from `.agents/workflows/install.md` (Step 3).
3. If bridge exists:
   - Preserve existing `Installed for:` agents and append `<agent-id>`.
   - Replace **strictly the content between the markers**. Never touch content outside.
4. If bridge is missing: append the bridge block to the end of the file.

### 3. Sync Command & Skill Wrappers

If the agent supports native folders (e.g. Claude Code `.claude/`):
- **Commands**: For each `.agents/workflows/*.md`, create/refresh a thin wrapper with `<!-- AI_DOCS_WRAPPER -->` (see `install.md` step 4). Remove wrappers with this marker if source workflow was deleted.
- **Skills**: For each `.agents/skills/*/SKILL.md`, create/refresh thin wrapper carrying `<!-- AI_DOCS_WRAPPER -->`. Remove orphaned wrappers.

### 4. Verify & Bootstrap Directories

Ensure standard folders exist under `_docs/` (create with `.gitkeep` and `index.md` if missing):
- `_docs/tasks/briefs/`
- `_docs/tasks/plans/`
- `_docs/modules/`
- `_docs/arch/`
- `_docs/spec/`
- `_docs/journal/`

Ensure `_docs/tasks/index.md` contains links to `briefs/` and `plans/` under a `# Subdirectories` heading.

### 5. Refresh Shared Brain (`agent.md`)

If `agent.md` exists, run `.agents/workflows/agent-sync.md` to refresh content above the `<!-- AGENT_SYNC_BOUNDARY -->` marker.

### 6. Report

Output summary:
- Native bridge updated in `<file>`.
- Wrappers: [N] refreshed, [N] added, [N] removed.
- Directories: verified [N] folders.
- `agent.md` synced.
