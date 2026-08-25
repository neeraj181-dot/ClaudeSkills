---
name: project-drift
description: Detect gradual architectural and product drift by comparing the current project state against its documented intentions — README, architecture docs, project decisions, coding conventions, folder structure, dependency choices, UI patterns, naming conventions, and stated product direction. Classifies differences as DRIFT, INTENTIONAL DIFFERENCE, or UNKNOWN. Produces a Drift Map with evidence and risk levels. Use when the user suspects the project has slowly become inconsistent, or wants to verify the code still matches the intended architecture.
tools: Read, Glob, Grep, Bash
---

# Project Drift

> Creator: Neeraj
> Project: ClaudeSkills

Detect when a project has **gradually drifted** from its stated intentions — even when every individual change seemed reasonable.

**Core principle: projects drift silently.** Each commit makes sense in isolation. But over time, conventions diverge, patterns multiply, and the codebase slowly stops matching its own documentation. This skill finds those gaps before they become serious problems.

This skill is **not** a code audit (`code-auditor`), not a context documenter (`context-keeper`), and not a documentation writer (`documentation-engineer`). It is specifically about **comparing what the project says it is against what it actually is**.

---

## When to activate

Activate when:

- The user says "the project feels inconsistent" or "things have drifted".
- A project has been worked on by multiple people/sessions over time.
- Before a major refactor, to understand what's actually changed.
- The user wants to verify the code still matches the architecture docs.
- After a long period of rapid feature development.

---

## Phase 1 — Gather the "intended state"

Collect what the project **says** it should be:

- **README.md** — stated purpose, tech stack, setup instructions.
- **Architecture docs** — `docs/`, `ARCHITECTURE.md`, diagrams.
- **Project decisions** — `PROJECT_DECISIONS.md`, ADR files, `PROJECT_CONTEXT.md`.
- **Coding conventions** — `.editorconfig`, linting config, `CONTRIBUTING.md`.
- **Package manifests** — `package.json` description, dependencies.
- **Folder structure** — the intended organizational pattern.

For each source, extract the **claimed** state.

## Phase 2 — Observe the "actual state"

Inspect what the project **actually** is:

- **Folder structure** — how code is actually organized.
- **Dependency list** — what's actually installed.
- **Code patterns** — how components, routes, and logic are actually written.
- **Naming conventions** — how things are actually named.
- **UI patterns** — how the interface is actually built.
- **API patterns** — how endpoints are actually structured.
- **Testing patterns** — how tests are actually written.

For each area, extract the **observed** state from actual code.

## Phase 3 — Compare and classify

For each area, compare intended vs actual:

| Area | Expected (from docs) | Actual (from code) | Classification | Evidence |
|------|---------------------|--------------------|----|----------|
| Folder structure | Feature-based | Mixed feature/layer | DRIFT | src/components/ vs src/features/ |
| CSS approach | Tailwind | Tailwind + CSS modules | INTENTIONAL DIFFERENCE | Both used consistently |
| State management | Redux | Zustand | DRIFT | No Redux in package.json |

Classify each difference:

- **DRIFT** — the project has diverged from its stated intention without the documentation being updated. This is a problem.
- **INTENTIONAL DIFFERENCE** — the project consciously changed direction but documentation was not updated. Not a bug, but the docs are stale.
- **UNKNOWN** — the difference exists but it's unclear whether it was intentional or accidental. Needs investigation.

**Never assume a difference is drift without evidence.** Check git history, code comments, and documentation for explanations before classifying.

## Phase 4 — Detect specific drift patterns

Look for these common drift symptoms:

- **Duplicated approaches** — two different ways to do the same thing (e.g., two date formatters, two HTTP clients, two state managers).
- **Abandoned architecture** — architectural patterns started but not completed.
- **Inconsistent patterns** — similar code structured differently in different places.
- **Obsolete dependencies** — packages installed but no longer used.
- **Conflicting conventions** — different naming styles in different parts of the codebase.
- **Dead documentation** — docs describing features that no longer exist.
- **Product direction contradictions** — features that go against stated product goals.
- **Permanent temporaries** — "temporary" solutions that became permanent.
- **Competing implementations** — multiple ways to accomplish the same task.

## Phase 5 — Assess risk

For each finding:

- **Risk level** — how dangerous is this drift?
  - **High** — causes bugs, confusion, or maintenance burden.
  - **Medium** — creates inconsistency but doesn't break things.
  - **Low** — minor style difference, no functional impact.
- **Recommendation** — what should be done about it.

## Phase 6 — Produce the Drift Map

```markdown
# Project Drift Analysis

## Drift Found
| Area | Expected | Actual | Evidence | Risk | Recommendation |
|------|----------|--------|----------|------|----------------|

## Intentional Differences
| Area | Expected | Actual | Why |
|------|----------|--------|-----|

## Unknown Differences
| Area | Expected | Actual | Investigation needed |
|------|----------|--------|---------------------|

## Summary
- Total areas checked: N
- Drift found: N
- Intentional differences: N
- Unknown differences: N
```

---

## What this skill does NOT do

- Does **not** fix drift automatically (that is `refactor-engineer`).
- Does **not** document the codebase (that is `context-keeper`).
- Does **not** audit code quality (that is `code-auditor`).
- Does **not** create documentation (that is `documentation-engineer`).

This skill answers one question: **"Does this project still match what it claims to be?"**

---

## Final output

```markdown
# Drift Analysis Summary
# Intended State Sources
# Drift Map
# Drift Found
# Intentional Differences
# Unknown Differences
# Recommendations
```

---

**Guardrails:** always ground findings in actual code evidence; never assume drift without checking git history and comments; distinguish drift from intentional evolution; do not automatically rewrite the project to eliminate differences; keep findings specific and actionable; verify claims against actual files; and never claim documentation is "wrong" without understanding the context.
