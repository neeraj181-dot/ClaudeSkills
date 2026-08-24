---
name: safe-scope
description: Enforce strict project/folder boundaries for the whole task — Claude may only read, create, modify, rename, or delete files inside the exact folder(s) the user specified, and never touches sibling folders, other projects, other Git repos, or the whole drive. Establishes the allowed workspace, checks every file op and shell command against it, stops and asks before any out-of-scope change, and reports what changed to confirm it stayed in bounds. Use when the user wants work confined to a specific directory, is worried about changes leaking into other projects, names an explicit target folder, or has multiple projects on the same drive. The rule: work exactly where the user asked, nowhere else.
tools: Read, Glob, Grep, Bash, Edit, Write
---

# Safe Scope

Enforce strict project and folder boundaries for the entire task. **The most important rule: "Work exactly where the user asked. Nowhere else."**

## Core rule

Claude may **only** read, create, modify, rename, or delete files inside the **exact folder/project the user explicitly specified** for the current task. Never modify another folder or project. **Never assume a folder is part of the current project just because it's nearby or on the same drive.**

Example — if the user says `D:\website33`, the allowed workspace is `D:\website33` and its contents. Claude must **not** modify `D:\claude`, `D:\OurPdf`, `D:\Rentify`, `D:\Arcade`, or anything else, unless the user explicitly changes the allowed workspace.

This skill is a **governance overlay**: it constrains whatever other work is happening. Its guardrails take precedence over convenience — when in doubt, do nothing outside the allowed workspace.

## Strict path matching

When the user explicitly provides an **absolute path**, treat that **exact path** as the only authorized workspace. **Never substitute, infer, search for, or automatically select a similarly named folder.**

For example, if the user specifies `D:\website33` and it **does not exist**: **STOP.** Do **not** automatically use:

```
D:\claude\website33
D:\website33-old
D:\website33-copy
D:\other\website33
```

— even if one appears to be the intended project.

Report plainly:

```
Requested path does not exist: D:\website33
```

If another potentially relevant folder is discovered, mention it **only as a possible alternative** and **ask the user for explicit authorization** before reading or modifying it. **An explicitly supplied absolute path always takes precedence over filesystem discovery.**

## Phase 1 — Determine the allowed workspace

Before any change, establish the exact working directory from: an explicit path the user provided · the current working directory · a clearly-established project root. If the user explicitly provides a folder, **that folder becomes the allowed workspace**.

Example — "Work on `D:\website33`" → allowed: `D:\website33\*`; not allowed: all of `D:\*` except `D:\website33` and its contents.

## Phase 2 — Confirm scope

Before modifying files, internally fix the boundary:

```
ALLOWED:   <exact folder(s)>
FORBIDDEN: <everything else>
```

If the requested operation would require modifying something **outside** the allowed folder: **STOP.** Do not make the outside change automatically — explain what outside resource is needed and **ask the user for explicit permission** first.

## Phase 3 — File operations

**All** file operations stay inside the allowed workspace — read, write, edit, create, delete, rename, move, generate, extract, build output, temporary files. Never create files outside the workspace, and never place generated/build artifacts in another project.

## Phase 4 — Commands

Before running any command that can modify the filesystem, verify it operates **within** the allowed workspace. Be especially careful with: `rm` · `del` · `rmdir` · `move` · `copy` · `xcopy` · `robocopy` · `git` · `npm` · `pip` · package managers · build tools · scripts · Docker volume mounts · database commands.

**Do not run broad commands against an entire drive** when the task is scoped to one project. Avoid: deleting across `D:\`, searching/modifying the whole drive, mass replacement across unrelated projects, or recursive operations starting at the drive root. **Prefer commands scoped to the project directory** (pass the workspace path explicitly; run from inside it; avoid wildcards that escape it).

## Phase 5 — Git safety

If the allowed workspace is a Git repository, **Git operations are limited to that repository.** Before modifying Git state, inspect `git status`, the current branch, and the repository root (confirm it's the allowed workspace).

**Never** reset, commit into, push, re-branch, or delete files in **another** repository. Do not run Git commands that affect repos outside the allowed workspace.

## Phase 6 — Dependencies

Installing a dependency may change `package-lock.json`, `node_modules`, requirements files, lockfiles, or virtual environments — that's fine **only** when they belong to the current workspace. **Do not modify global packages or another project's environment** unless the user explicitly asks. Prefer project-local dependencies.

## Phase 7 — External resources

A project may reference another local project, a shared library, a database, an API, or an external service. **Reading** an external resource may be necessary and is allowed when required — but **READ access does not grant permission to MODIFY.** If another folder must be **modified**, **STOP and ask** for explicit permission.

## Phase 8 — Multiple projects

If the user explicitly gives multiple folders (e.g. `D:\frontend` **and** `D:\backend`), **all of them are allowed.** Do not automatically include a third (`D:\other-project`) — only explicitly-provided paths are in scope.

## Phase 9 — Scope changes

If the user says "Now also modify `D:\project2`", update the allowed workspace to **add** `D:\project2` — treat it as new explicit permission. If the user does **not** explicitly grant it, **keep the original scope.** Scope never broadens on its own.

## Phase 10 — Ambiguous requests

If the request is vague ("Fix my website") and multiple projects exist, **do not pick one at random.** Determine the current working directory first. If the target is genuinely ambiguous, **ask which project** to modify. **Never modify multiple projects "just to be safe."**

## Phase 11 — Before every modification

Before changing a file, verify:

1. **What is the absolute path?**
2. **Is it inside the allowed workspace?**
3. **Is this modification required by the user's request?**

If the answer to #2 is **no → DO NOT MODIFY IT.**

## Phase 12 — Final verification

Before finishing, review the files touched during the task and confirm **every** modified/created/deleted file is inside the allowed workspace.

If a tool or command modified an unexpected file **outside** the workspace: do **not** silently continue — **report it**, determine whether it can be safely reverted, and **ask for permission** before any further out-of-scope action.

---

## Final response

Report at the end:

```markdown
## Workspace
<exact allowed folder(s)>

## Files Changed
(only files that were modified)

## Files Created
(files created)

## Files Deleted
(files deleted, if any)

## Scope Verification
(confirm all intentional changes stayed inside the allowed workspace)
```

If something outside the workspace was **accessed but not modified**, mention that separately when relevant. List only real paths — don't pad the lists.

---

**Guardrails, always:** the user's explicitly specified folder is the boundary; an explicitly supplied absolute path takes precedence over filesystem discovery — never substitute a similarly named folder, and if the path doesn't exist, stop and report it; never modify another project automatically; never assume sibling folders belong to the project; never treat the whole drive as the workspace; never modify global configuration without permission; never modify another Git repository; never delete or move files outside the workspace; reading outside the workspace does not grant modification permission; if an outside modification is required, stop and ask; when uncertain, do nothing outside the allowed workspace; preserve user files and existing work; and never broaden scope automatically. **Work exactly where the user asked. Nowhere else.**
