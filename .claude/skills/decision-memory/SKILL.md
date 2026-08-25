---
name: decision-memory
description: Preserve important project decisions so future AI sessions and developers do not silently contradict previous architectural choices. Inspects existing documentation and code evidence to identify decisions, then creates or updates PROJECT_DECISIONS.md with structured decision records. Distinguishes DECISIONS from PREFERENCES from TEMPORARY DETAILS. Never invents historical decisions. Use when the user wants to document architectural choices, preserve context across sessions, or prevent future agents from undoing intentional decisions.
tools: Read, Glob, Grep, Bash, Write
---

# Decision Memory

> Creator: Neeraj
> Project: ClaudeSkills

Preserve important project decisions so that future AI sessions, new developers, or future you **do not silently contradict** previous choices.

**Core principle: decisions made without recording them are decisions that will be unmade.** Every project accumulates important choices — why PostgreSQL over MongoDB, why a monolith over microservices, why this folder structure. Without documentation, each new session starts from scratch and may "optimize" away intentional decisions.

This skill is a **decision recording and retrieval tool** — not a planning tool, not a documentation tool.

---

## When to activate

Activate when:

- The user says "record this decision", "document why we chose X", "save this context".
- A significant architectural choice was just made in the conversation.
- The user wants to prevent future agents from contradicting current choices.
- Starting work on a project that has no decision records.
- The user says "remember this" or "make sure future sessions know about this".

---

## Phase 1 — Search for existing decisions

Before creating anything, find what already exists:

- Check for `PROJECT_DECISIONS.md`, `docs/decisions/`, `ADR/`, `architecture/` directories.
- Read `README.md`, `CLAUDE.md`, `PROJECT_CONTEXT.md` for embedded decisions.
- Check git history for commit messages that describe decisions.
- Look for inline code comments that explain "why" a choice was made.

**Never overwrite existing decisions without comparing.**

## Phase 2 — Identify decisions from code evidence

Inspect the project for decisions that are **implied by the code** but not documented:

- **Framework choice** — which framework, why (infer from code if not stated).
- **Database choice** — which database, which ORM.
- **API conventions** — REST vs GraphQL, naming conventions, versioning approach.
- **UI conventions** — component library, styling approach, state management.
- **Security decisions** — authentication approach, session handling.
- **Dependency choices** — key libraries chosen and alternatives that exist.
- **Folder structure** — organizational pattern (feature-based, layer-based, etc.).
- **Testing approach** — test framework, coverage expectations, E2E strategy.

For each, note the **evidence** (file, code, config) that supports the inference.

## Phase 3 — Distinguish decision types

Classify each finding carefully:

- **DECISION** — an intentional choice with reasoning (e.g., "We chose X because Y").
- **PREFERENCE** — a style choice without strong reasoning (e.g., "Tabs over spaces").
- **TEMPORARY DETAIL** — a current state that may change (e.g., "Using v3.2.1").
- **UNKNOWN** — the choice exists but the reason is not evident.

Only record DECISIONS and significant PREFERENCES in the decision file. Do not record temporary details that will quickly become stale.

## Phase 4 — Create or update PROJECT_DECISIONS.md

If the file does not exist, create it with this structure:

```markdown
# Project Decisions

This document records important architectural and design decisions.
Future developers and AI agents should respect these choices unless
there is a clear reason to revisit them.

---

## [Decision Title]

**Decision:** [What was decided]

**Context:** [What situation prompted this choice]

**Reason:** [Why this choice was made]

**Alternatives Considered:** [What other options were evaluated]

**Consequences:** [What this choice implies for the project]

**Date:** [When the decision was made, if known]

**Status:** Accepted / Superseded / Under Review
```

If the file exists, **merge** new decisions into it. Do not overwrite existing content.

## Phase 5 — Identify superseded decisions

If a newer decision contradicts an older one:

- Mark the older decision as **Superseded**.
- Add a note linking to the newer decision.
- Do not delete the old decision — it provides context.

## Phase 6 — Note gaps

Explicitly list decisions that are **missing** — areas where the project has no documented choice:

- "No decision recorded for: error tracking approach"
- "No decision recorded for: deployment strategy"

This helps the user identify what should be documented.

---

## What this skill does NOT do

- Does **not** make architectural decisions (that is `product-architect`).
- Does **not** document the codebase (that is `context-keeper`).
- Does **not** create general documentation (that is `documentation-engineer`).
- Does **not** record every tiny choice — only significant architectural/design decisions.

---

## Final output

```markdown
# Decisions Found
# Decisions Recorded
# Decisions Updated
# Superseded Decisions
# Missing Decisions (recommendations)
```

---

**Guardrails:** never invent historical decisions; always ground decisions in code evidence; distinguish decisions from preferences from temporary details; do not overwrite existing decision records; merge new decisions into existing files; keep decisions concise and useful; and never claim a decision was made if the project does not support that fact.
