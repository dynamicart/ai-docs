---
type: Journal
id: JOURNAL-YYYY-MM-DD-topic
title: "Short topic description"
description: "One-sentence summary of the session outcome."
date: YYYY-MM-DD
project: "project-name"
topic: "Short topic description"
journal_type: "planning"
# journal_type values: planning | debugging | review | decision | brainstorm
duration_approx: "30 min"
related_tasks:
  - TASK-000
participants:
  - human
  - ai-agent
tags:
  - planning
outcome: "short summary in one sentence"
generated: { by: "agent:manual", at: "YYYY-MM-DDT00:00:00Z" }
# sources:                              # optional (OKF v0.2) — external material
#   - { id: source-slug, resource: "https://...", title: "...", last_modified: YYYY-MM-DD }
---

# JOURNAL: YYYY-MM-DD — Topic Name

## Starting Point / Context

> State at session start; trigger or question.

## Essence of the Session

> Concise essence, not verbatim; machine-processable.

### Questions Raised

-
-

### Explored Approaches / Options

| Option | Advantage | Disadvantage | Decision |
| ----- | ----- | ------- | ------ |
|       |       |         |        |

## Decision Points

> What was decided.

1. **[Subject of Decision]:** [What was decided and why]
2.

## Result / Conclusion

> Outcome and next step.

## Open Questions

> Raised but undecided — for the next session/task.

-

## Related Materials

- Task: [TASK-000](/tasks/TASK-000.md)
- Previous journal: [JOURNAL-YYYY-MM-DD-previous](/journal/JOURNAL-YYYY-MM-DD-previous.md)
- Spec:

<!-- If `sources:` is used, cite inline: "...per policy[^source-slug]." -->

## AI Design Summary

> Agent-generated distillation at session end.

```
JOURNAL_ID: JOURNAL-YYYY-MM-DD-topic
CONTEXT:
EXPLORED:
DECIDED:
RATIONALE:
NEXT_ACTIONS:
OPEN_QUESTIONS:
```
