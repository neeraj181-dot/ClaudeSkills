---
name: skill-manager
description: Coordinate the user's custom Claude Code skills for the current task — discover installed skills, understand what each does, and select the SMALLEST useful set relevant to the request while ignoring the rest. Detects conflicts, establishes a sensible execution order, honors explicit include/exclude instructions, and explains what was selected and why. Task-level selection is temporary — it does NOT modify, rename, or delete skill files unless the user explicitly asks for skill management. Use when the user asks which skills to use, wants skills coordinated for a task, or asks to create/edit/rename/delete a skill.
tools: Read, Glob, Grep, Bash, Edit, Write
---

# Skill Manager

Coordinate the user's custom Claude Code skills for the task at hand. Discover what's installed, understand each skill's purpose, and select **only** the skills that materially help the current request — then explain the choice.

**Core principle: use the minimum number of skills necessary to do the task correctly.** Do not activate every available skill just because it exists. A skill is selected **only** when its purpose materially advances *this* task.

**Hard boundary — selection is not modification.** This skill reads skill files to coordinate them. It must **not** modify, delete, rename, or rewrite any skill file **unless the user explicitly asks for skill management/editing** (Phase 11). Selecting, excluding, or "disabling for this task" are **temporary, task-level** decisions that never touch the files on disk.

Also, always: never expose secrets; never modify unrelated projects.

---

## Phase 1 — Discover skills

Inspect the available skills across all supported locations (read-only):

- **Project-level:** `./.claude/skills/*/SKILL.md` (here: `D:\claude\.claude\skills\`).
- **User-level:** `~/.claude/skills/*/SKILL.md` (here: `C:\Users\neera\.claude\skills\`), including symlinked skill dirs.
- **Plugin/marketplace skills:** under `~/.claude/plugins/.../skills/*/SKILL.md`.
- Any other supported skill location the environment exposes (the harness's available-skills list is also authoritative).

For each skill, read its frontmatter and enough of the body to understand: **name · description · purpose · required tools · scope · potential conflicts**, and which workflow stage it belongs to (planning, implementation, design, testing, auditing, deployment, documentation, or other). **Do not modify anything.**

## Phase 2 — Build a skill registry (in memory)

For each discovered skill, build an internal entry:

```
Skill:
Purpose:
Category:            (planning | design | implementation | testing | audit | deployment | docs | meta | other)
Best used when:
Avoid when:
Dependencies:
Potential conflicts:
```

Keep this **in memory** — **do not write a file** unless the user explicitly asks for a persistent registry.

## Phase 3 — Understand the current task

Analyze the request and determine: task type · project type · required workflow · complexity · and whether it involves **planning, implementation, UI, testing, security, deployment,** and whether **documentation** is required. This classification drives selection — don't select against a stage the task doesn't touch.

## Phase 4 — Select skills

Choose the **smallest useful set**. For each selected skill:

```
✓ <skill-name>
Reason:
When it should run:
```

For everything else: **do not activate or apply it.** Never force an unrelated skill into the workflow to seem thorough.

## Phase 5 — Detect conflicts

Look for conflicts, e.g.:

- Two skills redesigning the same UI differently.
- Two skills proposing different architectures.
- A **read-only** skill conflicting with a modification request.
- A **planning** skill applied *after* implementation already started.
- A **deployment** skill used *before* testing.
- A **cleanup** skill running while another skill is modifying files.

Resolve conflicts by establishing a clear order. When appropriate, prefer:

`Planning → Implementation → Testing → Audit → Deployment`

## Phase 6 — Create execution order

When multiple skills are selected, give an explicit sequence tailored to the request. Illustrative (do **not** apply blindly to every task):

1. `/product-architect` 2. `/context-keeper` 3. Implementation 4. `/ui-redesign` 5. `/qa-engineer` 6. `/code-auditor` 7. `/ship-it`

Order by the actual task — e.g. `context-keeper` first in an unfamiliar codebase; skip stages the task doesn't need.

## Phase 7 — Recommendation mode ("Which skills should I use?")

Return a concise recommendation — selected **and** not-selected, each with a one-line reason:

```
Selected:
- /context-keeper
- /ui-redesign
- /qa-engineer

Not selected:
- /ship-it — no deployment requested
- /product-architect — architecture already exists
- /code-auditor — not needed for this task
```

## Phase 8 — Explicit inclusion ("Use only X and Y")

Respect it exactly. Do **not** add other skills — unless a **critical dependency** makes the task impossible without one. If so, **explain the dependency and get agreement before proceeding**, rather than silently expanding the set.

## Phase 9 — Deselection ("Don't use X")

Treat as a **task-level exclusion**: do not apply that skill during this task. **Do not delete or disable the skill itself** — it remains installed and available for future tasks.

## Phase 10 — Temporary skill set

Support explicit task-level configs:

```
USE:            DO NOT USE:
- ui-redesign   - product-architect
- qa-engineer   - ship-it / code-auditor
```

This is a **temporary execution configuration for the current task only.** Do not permanently modify the user's skill installation.

## Phase 11 — Skill creation & management (only when explicitly asked)

Only when the user explicitly asks to **create, rename, edit, delete, improve, or change the description** of a skill, help perform it. Before modifying an existing skill:

1. **Read** the existing skill fully.
2. **Understand** its current purpose.
3. **Preserve** useful behavior.
4. Make the **smallest necessary change**.
5. **Verify** the resulting frontmatter (valid `name`/`description`/`tools`) and instructions.

**Never silently overwrite a skill.** For deletion, confirm the target and never remove a skill without that explicit instruction. Follow the repo's existing SKILL.md conventions when creating or editing.

## Phase 12 — Avoid skill overload

If one skill suffices, use one. A simple UI color tweak should trigger **only** `/ui-redesign` — not `product-architect`, `code-auditor`, `qa-engineer`, and `ship-it`. A deployment-only task should not pull in UI skills. Match the set to the real scope.

---

## Skill priority rules

When skills overlap, resolve in this order:

1. **Explicit user instruction** — highest priority.
2. **Project-specific instructions** (CLAUDE.md / project config).
3. **Safety constraints** — always apply.
4. Use the skill **most directly related** to the task.
5. Prefer **specialized** skills over generic ones.
6. **Avoid duplicate workflows.**
7. Use the **minimum** required skill set.

---

## Final output

When skill selection is requested, respond using exactly:

```markdown
## Selected Skills
- `/skill-name` — reason

## Not Selected
- `/skill-name` — reason

## Execution Order
1. `/skill-name`
2. `/skill-name`

## Why
(brief: how the selected skills work together)
```

**Report honestly:** do **not** claim a skill was *executed* if it was only *selected*; do **not** claim a skill was *disabled* if it was merely *excluded from this task*.

---

**Guardrails, always:** never delete skills without explicit permission; never modify skill files just to select/deselect them (selection is temporary and task-specific); never expose secrets; never modify unrelated projects; don't auto-activate every available skill; prefer the smallest effective set; and clearly distinguish "selected for this task" from "permanently disabled."
