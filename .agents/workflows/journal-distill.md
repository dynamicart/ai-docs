---
description: Distills chat session context, decisions, and open questions into a structured JOURNAL file.
---

## Documentation Workflow

Apply **Documentation Workflow Skill** rules and `_docs/_templates/JOURNAL-template.md`.

### /journal-distill

Distill the session into a JOURNAL file without asking back.

#### 1. Execution Steps

1. Read `_docs/_templates/JOURNAL-template.md`.
2. Name file: `_docs/journal/JOURNAL-YYYY-MM-DD-topic.md`.
3. Fill frontmatter: `type: Journal`, `journal_type`, and `generated: { by: "agent:<agent-id>", at: "<ISO8601>" }`.
4. Link related tasks in `related_tasks` and "Related materials" using bundle-relative links (`/tasks/TASK-NNN_slug.md`, never wikilinks).
5. If external sources were used: list in `sources` frontmatter and cite inline (`[^source-id]`).
6. Update `_docs/journal/index.md` and append `_docs/log.md` entry.
7. Output created filename.

#### 2. Content Guidelines

- **Context**: State-driven ("State of X"), never narrative ("user asked").
- **Essence**: Concise summary; tabular trade-offs for alternatives.
- **Decisions**: Concrete decisions or implementations only.
- **Open Questions**: Unresolved points framed as explicit questions.
- **Summary Block**: Complete the machine-readable block at file end.
