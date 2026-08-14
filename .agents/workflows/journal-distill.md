---
description: Saves the essence of the current chat session into a structured JOURNAL markdown file. Preserves context, decisions, and open questions — in a machine-processable format.
---

## Documentation Workflow

Follow the structure of `_docs/_templates/JOURNAL-template.md` exactly, and apply the rules of the **Documentation Workflow Skill**.

### /journal-distill

Distill the essence of the current chat session into a JOURNAL file.

1. Read the `_docs/_templates/JOURNAL-template.md` file.
2. Determine the filename based on the skill rules: `_docs/journal/JOURNAL-YYYY-MM-DD-topic.md`.
3. Fill in the YAML frontmatter and sections according to the skill's distillation rules — including `type: Journal`, `journal_type` (the old session-kind field), and `generated: { by: "agent:<this-agent-id>", at: "<now, ISO8601>" }`.
4. If there is a related TASK reference in the context, link it in the `related_tasks` field and under "Related materials" using a bundle-relative markdown link (`/tasks/TASK-NNN_slug.md`), never a `[[wikilink]]`.
5. If external material was used, add a `sources` frontmatter entry and cite it inline with `[^source-id]` instead of a loose link.
6. Update `_docs/journal/index.md` and append a `_docs/log.md` entry.
7. Do not ask back — create the file immediately.

### AI Distillation Guide (Briefly)

- **Context:** Describe the state, not that "the user asked".
- **Essence:** Concise summary, alternatives in a table.
- **Decisions:** Only what was specifically decided or implemented.
- **Open Questions:** Record uncertainties in question form.
- **Summary Block:** Fill in the machine-readable block at the end of the file.

---

*Note: This workflow replaces the previous `/session-distill` command.*
