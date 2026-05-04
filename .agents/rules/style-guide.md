---
trigger: always_on
---

## AI-Driven Code & Docs Style Guide

This document defines the rules for the project's coding, technical documentation, and code commenting.
Its goal is sustainability and AI-based searchability.

### 0. Coding Conventions (Detailed)

The project follows the K&R (Kernighan & Ritchie) style.

### 1. Code-level Documentation (Inline)

_"The Why, not the What"_
The code describes what it does. The comment describes why it does it that way (business logic, technical constraints, workarounds).

_File Header_
A short summary is mandatory at the beginning of every new or significantly modified file:
/**
 * @file [Filename]
 * @description [1-sentence summary of the function]
 * @context [Which module does it belong to, what calls it?]
 */

_Function Documentation_
Use standard blocks for complex functions:
@param: Type and short description.
@returns: What it returns and in what format.
@throws: What errors it might throw.

### 2. The _docs Folder Structure

All documents are prepared in Markdown format, organized into the following folders:

| Folder / File | Content | Update Frequency |
|--------------|----------|-----------|
| PROJECT-OVERVIEW.md | Project map, active tasks, tech stack. | At every change. |
| tasks/ | Individual tasks (TASK-NNN). | Continuously. |
| journal/ | Daily sessions, brainstorms (JOURNAL-YYYY-MM-DD). | Daily. |
| arch/ | API specifications, Database schemas, System diagrams. | At architecture changes. |
| DECISIONS-LOG.md | Repository of architectural decisions (ADR). | At new decisions. |
| spec/ | Business logic, specifications. | Before new features. |

### 3. ADR (Architecture Decision Record) Format

Every important decision must be recorded in `DECISIONS-LOG.md` with the appropriate sequence number (ADR-NNN), referencing the related TASK and JOURNAL.

### 4. Visualization (Mermaid.js)

Where the text is too complex, use Mermaid diagrams.

### 5. The Agent's "Maintenance" Routine

At the end of every task, the agent should offer (if it's clearly appropriate):
- The `/journal-distill` command to close the session.
- The `/task-close` command to archive the task.
- Checking the up-to-dateness of `PROJECT-OVERVIEW.md`.