---
description: On-demand delta sync for a single `_docs/modules/*.md` file, proposed against a specified TASK's changes (or a user-supplied description of what changed). Manual invocation only — never runs automatically on `/task-close` or any other workflow. Proposes deltas for human approval; never rewrites a module file outright.
---

## Documentation Workflow

Apply the rules of the **Documentation Workflow Skill** whenever updating module documentation. `_docs/modules/*.md` files are durable references (same convention as `arch/*.md`): unlike TASK/JOURNAL they are not archived when a task closes — they stay current for as long as the module exists.

## /module-sync <module-name> [TASK-NNN]

Manual command only. Never triggered automatically by `/task-close` or any other workflow — this is a deliberate decoupling, see "What this workflow deliberately does NOT do" below.

### 1. Identify scope

- `module-name` is required — resolve to `_docs/modules/<module-name>.md`. If it doesn't exist, offer to create it from `MODULE-template.md` instead of running a sync.
- `TASK-NNN` is optional. If given, read that task's diff / changed-file list as the source of the delta. If omitted, ask the user what changed (diff, file list, or plain description) — never infer silently from the current state of the working tree.

### 2. Source files (read in order)

1. `_docs/modules/<module-name>.md` — current content, read in full first.
2. `_docs/_templates/MODULE-template.md` — section shape reference.
3. The specified TASK's diff / changed files (via git, if `TASK-NNN` given), or the user-supplied description.
4. The TASK's linked JOURNAL, if any — for decision context.

### 3. Generation rules — per-section update policy

| Section | Auto-update? | Rationale |
|---|---|---|
| Responsibility | No | intent-level, human-authored only |
| Location in code | Yes, proposed | namespace/path changes are mechanically verifiable |
| Dependencies | Yes, proposed | new `use` / import statements are visible in the diff |
| Key decisions | No | only ever a link to an ADR, never generated prose |
| Invariants | No, flag only | if the diff appears to violate an existing invariant → **warning**, never silent removal |
| Known limitations | Proposed | if the TASK explicitly closes a known issue, propose removing it |
| Related tasks | Automatic | Dataview block, no action needed |
| Change log | Yes, new row | date + one-line summary + TASK/ADR reference |

### 4. Proposal format

Present a diff-style proposal in the chat — never edit the module file directly:

```
modules/billing.md

+ ## Dependencies — new line:
+ **Depended on by:**
+ - modules/notifications.md — sends invoice-issued notifications

+ ## Change log — new row:
+ | 2026-08-10 | Added notifications integration | TASK-052 |

⚠ Invariant check: the diff calls the Billing service directly from the
  Notifications controller — this conflicts with the existing invariant
  ("services only communicate through jobs"). Human decision required:
  update the invariant, or fix the code.
```

### 5. Human approval

- The proposal is never committed to the file automatically.
- The user confirms per-line or in full, or edits manually before it's applied.
- An invariant conflict **blocks** the sync from completing without an explicit human decision — this is a blocking condition, not a warning-level one, same severity class as a `verified` gate on `/task-close`.

### 6. `verified` handling

- If the module file received an update from an approved proposal, `verified` stays as-is (unchanged) until someone explicitly reviews the *entire* file, not just the delta.
- On a full manual review, append `{ by: "human:<username>", at: "<ISO8601>" }` to `verified` — same shape as the TASK schema's `verified` field; an agent must never self-verify a module file.
- If `stale_after` days have passed since the last full human verification, treat the file as `status: needs-review` for the duration of this sync — this is checked at sync time, not enforced by a background process.

## /module-sync command execution

1. Resolve `module-name`; if the module file doesn't exist, offer `MODULE-template.md`-based creation instead of a sync.
2. Resolve the change source: the given `TASK-NNN`'s diff, or an explicit description from the user.
3. Read the current module file and the template.
4. Build the per-section delta per the rules in §3; never touch Responsibility or Key decisions.
5. Present the proposal; wait for explicit approval before writing anything.
6. On approval, apply only the approved lines to `_docs/modules/<module-name>.md`; leave `verified` untouched.
7. Do not ask back beyond scope resolution (step 1-2) and any invariant conflict found in step 4 — otherwise present the proposal, apply on approval, then output: "`modules/<module-name>.md` updated: [N] sections proposed, [N] approved, [N] invariant conflicts flagged."

## What this workflow deliberately does NOT do

- Does not run automatically on `/task-close` or any other workflow — manual invocation only, by design, so a module file is never touched without someone deciding to look at it.
- Does not regenerate the Responsibility or Key decisions sections — these are always human- or ADR-sourced.
- Does not remove an Invariants entry automatically, even if the code no longer appears to violate it — removal is itself a decision.
- Does not mark `verified` on a delta approval alone — only a full-file human review counts.

## Related files

- `_docs/_templates/MODULE-template.md` — the module document skeleton
- `_docs/DECISIONS-LOG.md` — where a new ADR goes if an invariant conflict resolves in favor of changing the invariant
- `.agents/workflows/task-close.md` — intentionally NOT a caller of this workflow (see above)
