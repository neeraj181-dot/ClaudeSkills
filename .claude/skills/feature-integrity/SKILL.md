---
name: feature-integrity
description: Determine whether a feature is actually complete end-to-end by tracing it through the full stack — UI, interaction, state, API, business logic, database/external services, response handling, and error states. Identifies broken links in the chain where a feature appears to exist but does not actually work. Never marks a feature complete based only on source code presence. Use when the user wants to verify a feature is genuinely working, or wants to know if a partially-built feature is ready for users.
tools: Read, Glob, Grep, Bash
---

# Feature Integrity

> Creator: Neeraj
> Project: ClaudeSkills

Determine whether a feature is **actually complete end-to-end** — not just partially wired up.

**Core principle: a button existing is not a feature working.** A feature is only complete when the full chain works: UI → interaction → state → API → business logic → data → response → UI update → error handling. A break at *any* link means the feature is incomplete — even if 9 out of 10 links are solid.

This skill is **not** a QA test (`qa-engineer`), not a code audit (`code-auditor`), and not a completeness checker for documentation. It traces a **specific feature** through the **full implementation chain** and reports where the chain breaks.

---

## When to activate

Activate when:

- The user says "is this feature done?" or "is this ready?"
- A feature was partially built and the user wants to know what's missing.
- The user says "does the export actually work?" or "can users really do X?"
- Before marking a ticket/story as complete.
- After a rapid build session to verify nothing was missed.

---

## Phase 1 — Identify the feature

Clearly define what feature is being checked:

- What is the feature name?
- What is the user-facing entry point? (button, link, form, route)
- What is the expected end-to-end behavior?
- What data does it read? What data does it write?

## Phase 2 — Trace the full chain

For the feature, trace every link in this chain:

```
UI Element (button, link, form)
  ↓
Click/Submit Handler
  ↓
State Update (local or global)
  ↓
API Call (if any)
  ↓
Route/Controller
  ↓
Business Logic / Service Layer
  ↓
Database / External Service
  ↓
Response Construction
  ↓
State Update (response)
  ↓
UI Update (display result)
  ↓
Error Handling (all failure paths)
```

For **each link**, verify:

- ✅ **Exists** — the code for this link is present.
- ✅ **Connected** — the link is properly wired to adjacent links.
- ✅ **Functional** — the logic does what it's supposed to.
- ❌ **Missing** — the code does not exist yet.
- ⚠️ **Broken** — the code exists but is not connected or not working.

## Phase 3 — Check error paths

For every link, check what happens on failure:

- Network error — does the UI show an error?
- Invalid data — does validation catch it?
- Unauthorized — does the user see a clear message?
- Timeout — does the user know what happened?
- Server error — is there a fallback or retry?

A feature that works on the happy path but crashes on errors is **not complete**.

## Phase 4 — Check edge cases

For the feature, verify:

- Empty state — what does the user see when there's no data?
- Loading state — what does the user see while waiting?
- Success state — is feedback clear and helpful?
- Boundary conditions — max length, zero items, single item, many items.

## Phase 5 — Classify completeness

For the overall feature:

```
FULLY COMPLETE
  All links exist, connected, and functional.
  Error paths handled. Edge cases covered.

MOSTLY COMPLETE
  Core chain works but minor gaps remain.
  One or two links missing or weak.

PARTIALLY COMPLETE
  Core chain has breaks.
  Feature cannot be used end-to-end.

BARELY STARTED
  Only UI or only backend exists.
  No connected chain yet.

NOT STARTED
  No code for this feature exists.
```

## Phase 6 — Report broken links

For each broken or missing link:

| Link | Status | File | Issue | Impact |
|------|--------|------|-------|--------|
| API Call | ❌ Missing | — | No API route for this action | Feature cannot save data |
| Error Handling | ⚠️ Broken | route.ts | Catches error but shows generic message | User confused on failure |

---

## What this skill does NOT do

- Does **not** test the running application (that is `qa-engineer`).
- Does **not** audit all code quality (that is `code-auditor`).
- Does **not** find bugs in existing features (that is `incident-debugger`).
- Does **not** check performance (that is `performance-optimizer`).

This skill answers one question: **"Is this specific feature actually complete from click to completion?"**

---

## Final output

```markdown
# Feature Integrity Report
# Feature: [name]
# Chain Trace
# Links Verified
# Broken Links
# Edge Cases
# Completeness Classification
# What Remains
```

---

**Guardrails:** trace through actual code, not assumptions; verify each link exists AND is connected; check error paths, not just happy paths; never mark a feature complete based on source code presence alone; distinguish "code exists" from "code works"; keep the focus on one feature at a time; and never claim completeness without verifying the full chain.
