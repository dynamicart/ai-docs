---
type: "Decisions Log"
project: "project-name"
updated: 2025-01-01
---

# Decisions Log — Project Name

> This file records all major architectural and design decisions.
> Each ADR (Architecture Decision Record) preserves context: what the question was,
> what alternatives were considered, what was chosen and why.
> The distilling agent can use this to answer "why" type questions.
>
> Note (OKF): this file aggregates many decision records under one path, so it does not
> map one-to-one onto a single OKF concept the way TASK/JOURNAL files do. It plays the
> same role as OKF's reserved `log.md` (§7) but with a domain-specific, per-ADR structure;
> `_docs/log.md` stays reserved for the generic, flat chronological history (see skill §7).

---

## ADR-001: [Subject of Decision]

**Date:** 2025-01-01
**Status:** accepted
<!-- Status values: accepted | deprecated | superseded -->
**Stale after:** <!-- optional (OKF-inspired), e.g. 2026-12-31 — set for time-bound decisions (pricing, policy, indicator recalibration) -->
**Journal:** [JOURNAL-2025-01-01-topic](/journal/JOURNAL-2025-01-01-topic.md)
**Task:** [TASK-000](/tasks/TASK-000.md)

### Context

> What was the situation? What constraints or needs led to this decision point?

### Question

> What exactly was the question to be decided?

### Alternatives

**Option A: [Name]**
- Description: 
- Advantage: 
- Disadvantage: 

**Option B: [Name]**
- Description: 
- Advantage: 
- Disadvantage: 

### Decision

> What was chosen?

### Rationale

> Why this and not the others? What factors decided?

### Consequences

> What does this decision entail? What becomes harder / easier?

### Review Criteria

> When should it be reconsidered? (optional)

---

## ADR-002: [Subject of Decision]

**Date:**
**Status:** accepted
**Stale after:**
**Journal:**
**Task:**

### Context

### Question

### Alternatives

### Decision

### Rationale

### Consequences

---

<!-- 
AGENT PROCESSING GUIDE:
This file contains ADR blocks with ## ADR-NNN headers.
Each block can be processed independently.
Machine summary format:

ADR_ID: ADR-001
PROJECT: project-name
DECISION: 
CONTEXT: 
ALTERNATIVES_CONSIDERED: 
RATIONALE: 
DATE: 
STATUS: 
-->
