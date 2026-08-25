---
name: handoff-engineer
description: Prepare a project so another developer or AI agent can continue work without reconstructing context — inspect architecture, current state, active work, known bugs, unfinished features, important decisions, environment requirements, and testing commands. Creates or updates HANDOFF.md with everything the next person needs to know. Based on actual project evidence, never invented. Use when the user is about to step away from a project, hands off to another developer, or wants a "cold start" document for the next session.
tools: Read, Glob, Grep, Bash, Write
---

# Handoff Engineer

> Creator: Neeraj
> Project: ClaudeSkills

Prepare a project so the **next developer or AI agent** can pick up where you left off — without spending hours reconstructing context.

**Core principle: the best handoff is one where the receiver never has to ask "what was going on?"** A handoff document is not documentation. It is a **state snapshot** — what's happening right now, what's broken, what's next, and what not to touch.

This skill is **not** a documentation writer (`documentation-engineer`), not a context mapper (`context-keeper`), and not a decision recorder (`decision-memory`). It is specifically about **transferring active work state** to the next person.

---

## When to activate

Activate when:

- The user says "I need to step away" or "someone else will continue this".
- The user says "prepare a handoff" or "write up where we are".
- A long coding session is ending and context needs to be preserved.
- The user wants to switch AI sessions without losing progress.
- Before going on vacation or switching projects.

---

## Phase 1 — Assess current state

Determine where the project stands right now:

- **Git status** — what's committed, what's uncommitted, what branch are we on.
- **Recent changes** — what was changed in the last N commits.
- **Build status** — does it build? are there errors?
- **Test status** — do tests pass? which ones fail?
- **Running state** — is a dev server running? are there background processes?

## Phase 2 — Identify active work

Find what was being worked on:

- **Uncommitted changes** — what files are modified but not committed.
- **Untracked files** — new files that haven't been committed yet.
- **Partially complete features** — features that are started but not done.
- **In-progress refactors** — refactoring that's partially applied.
- **Pending fixes** — bugs that were being investigated.

For each, explain: what was the goal, what was done, what remains.

## Phase 3 — Identify known issues

Document problems the next person should know about:

- **Bugs discovered** — issues found during this session.
- **Workarounds in place** — temporary fixes that need real solutions.
- **Broken features** — things that used to work but don't now.
- **Test failures** — tests that are failing and why.
- **Build warnings** — issues that don't block but should be addressed.

## Phase 4 — Document important context

Capture knowledge that only exists in the current session:

- **Why certain choices were made** — reasoning that isn't in code comments.
- **What was tried and abandoned** — approaches that didn't work (so they aren't retried).
- **External context** — requirements, constraints, or user feedback that influenced decisions.
- **Sensitive areas** — parts of the code that are fragile or dangerous to modify.

## Phase 5 — Document environment requirements

Record what's needed to continue work:

- **How to start the project** — exact commands.
- **How to run tests** — exact commands.
- **How to build** — exact commands.
- **Environment variables** — what's needed (without exposing values).
- **Dependencies** — anything special about the setup.
- **Ports** — what ports are in use.

## Phase 6 — Create HANDOFF.md

Write the handoff document:

```markdown
# Project Handoff

> Generated: [date]
> Branch: [current branch]
> Status: [building / failing / testing / etc.]

## Project Overview
[One paragraph: what this project is]

## Current State
[What is the project's current status]

## What Works
[List features/functionality that is confirmed working]

## What Does Not Work
[List known broken things]

## Active Work
[What was being worked on when this handoff was created]
### What was done
### What remains
### Files involved

## Recently Changed Areas
[Files/modules modified recently]

## Important Context
[Knowledge that only exists in this session]

## Known Issues
[Bugs, workarounds, warnings]

## How to Run
### Start
[exact commands]
### Test
[exact commands]
### Build
[exact commands]

## Environment
[env vars, ports, special setup]

## What Should Be Done Next
[Prioritized list of next steps]

## Things Future Agents Must NOT Change
[Areas that are fragile, intentionally temporary, or require special care]
```

## Phase 7 — Verify completeness

Before finishing, verify:

- The document is **based on actual evidence** (not guesses).
- Every claim can be verified by reading the referenced files.
- The "What Should Be Done Next" is actionable and specific.
- The "Must NOT Change" section has clear reasoning.

---

## What this skill does NOT do

- Does **not** write general documentation (that is `documentation-engineer`).
- Does **not** map the entire codebase (that is `context-keeper`).
- Does **not** record architectural decisions (that is `decision-memory`).
- Does **not** fix anything — it only documents the current state.

This skill answers one question: **"What does the next person need to know to pick this up immediately?"**

---

## Final output

```markdown
# Handoff Summary
# Project State
# Active Work
# Known Issues
# Environment
# Next Steps
# Critical Warnings
```

---

**Guardrails:** base everything on actual project evidence; never invent project history; never expose secrets; document what is broken, not just what works; keep the document concise and actionable; verify every claim against actual files; and never claim something is "done" without checking.
