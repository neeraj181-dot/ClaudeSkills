---
name: terminal-visualizer
description: Present Claude Code's own responses and command output as a polished developer console — using terminal-compatible text graphics (Unicode boxes, tables, trees, progress/status blocks, ASCII architecture & workflow diagrams, change summaries, compact dashboards) for clarity, not decoration. Controls only how Claude formats its output; it does NOT modify Claude Code, terminal settings, or any files. Keeps output ~60–90 cols, ASCII-fallback safe, honest (never fakes progress/tests/changes), and copy-safe for commands. Use when the user wants clearer, more structured, more visual terminal output, dashboards, diagrams, or status/progress displays.
tools: Read, Glob, Grep
---

# Terminal Visualizer

Make Claude Code's terminal output more **visual, structured, readable, and professional** using terminal-compatible text graphics. The output should feel like a **polished developer console** — clear, structured, visual, compact, useful, readable. **Never sacrifice usability for decoration.**

**This skill controls only how Claude *presents* its responses and command output.** It does **not** — and must never try to — modify, replace, patch, or recreate Claude Code's own terminal UI.

**Core goal:** visual hierarchy without bloat. Prefer clean, useful terminal graphics over decorative ASCII art. Structure earns its place only when it makes the content clearer.

---

## Visual vocabulary

Use terminal-compatible: Unicode box drawing · tables · trees · progress bars · status indicators · section headers · separators · compact dashboards · ASCII architecture/workflow diagrams · checklists · numbered steps.

```
╭──────────────────────────────────────────────╮
│              PROJECT ANALYSIS                  │
├──────────────────────────────────────────────┤
│ Project    │ Website33                         │
│ Framework  │ Next.js                           │
│ Status     │ ✓ Ready                           │
╰──────────────────────────────────────────────╯
```

## Status indicators (consistent vocabulary)

```
✓ Success    ✗ Failure    ⚠ Warning    ● Running
○ Pending    → Next step   ◆ Important   ℹ Information
```

Use them consistently — and **don't overuse symbols.** One clear marker per line beats a row of glyphs.

## Progress displays

When a task has multiple **meaningful** stages, show a compact progress block:

```
╭─ Progress ───────────────────────────────────╮
│ ✓ Inspect project                             │
│ ✓ Analyze architecture                        │
│ ● Implement redesign                          │
│ ○ Test application                            │
│ ○ Final review                                │
╰──────────────────────────────────────────────╯
```

Only show progress when it's genuinely informative. **Never fake progress percentages** — if you don't have a real measure, use stage markers (✓/●/○), not invented numbers.

## Architecture diagrams

Use when they improve understanding, kept within normal terminal width:

```
User
 │
 ▼
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Frontend   │ ──▶ │     API      │ ──▶ │   Database   │
└──────────────┘     └──────────────┘     └──────────────┘
```

## Project trees

Show only the **relevant** sections — never dump an enormous directory tree:

```
website33/
├── src/
│   ├── components/
│   ├── pages/
│   └── styles/
├── public/
├── package.json
└── README.md
```

## Command presentation

Separate commands from prose so they stay **copyable**. **Never put decorative characters inside a command** — a box around a command is fine, but the command text itself must be clean and paste-ready. When copy-safety matters most, prefer a plain fenced code block over a box:

```
cd /d D:\website33
npm run dev
```

## Build / test results

```
╭─ Test Results ───────────────────────────────╮
│ ✓ Build          PASS                         │
│ ✓ Unit tests     PASS                         │
│ ✓ API tests      PASS                         │
│ ⚠ E2E tests      NOT CONFIGURED               │
╰──────────────────────────────────────────────╯
```

**Never claim a test passed unless it was actually run.** Unknown/!run states get ⚠ or ○, never a fabricated ✓.

## Errors

Surface the real error, then explain:

```
╭─ ✗ ERROR ────────────────────────────────────╮
│ Module not found: ./Navbar                    │
│ File: src/App.jsx   Line: 12                   │
╰──────────────────────────────────────────────╯
```

→ **Cause** · → **Fix** · → **Next command**

**Do not hide or paraphrase away the original error** — show it, then interpret it.

## File changes

Compact summary using `+` created · `~` modified · `-` deleted:

```
╭─ Changes ────────────────────────────────────╮
│ + src/components/Hero.tsx                     │
│ ~ src/styles/global.css                       │
│ + src/components/AnimatedButton.tsx           │
╰──────────────────────────────────────────────╯
```

**Never list a file as changed if it wasn't actually changed.**

## Workflow diagrams

For multi-stage work, only when it clarifies:

```
Requirement ─▶ Analysis ─▶ Implementation ─▶ Testing ─▶ Review ─▶ Complete ✓
```

## Final summaries

For substantial tasks, a compact dashboard — then the important remaining issues **below** it:

```
╭──────────────────────────────────────────────╮
│                TASK COMPLETE ✓                 │
├──────────────────────────────────────────────┤
│ Files changed    7                             │
│ Files created    2                             │
│ Tests passed     18                            │
│ Tests failed     0                             │
│ Build            PASS                          │
╰──────────────────────────────────────────────╯
```

Every number must be real.

---

## Constraints

**Terminal width** — design for ordinary terminals, roughly **60–90 characters** per line. No extremely wide boxes; don't assume a specific terminal size.

**Color** — if ANSI color is available, use it **sparingly** and semantically: green = success, red = error, yellow = warning, cyan/blue = info, neutral = normal. **Never rely on color alone** — symbols and text must carry the meaning, and output must stay readable with no color at all.

**Animation** — do **not** create fake redrawing animations, excessive spinners, or game-like output. If a real command already emits progress, don't duplicate it.

**Compatibility** — output must stay readable in Windows CMD, PowerShell, Windows Terminal, Linux terminals, macOS Terminal, and the VS Code integrated terminal. Prefer standard Unicode box characters; **if Unicode rendering may be unreliable, fall back to ASCII** (`+`, `-`, `|`, `=`).

## When to use structure (interaction style)

- **Simple questions** → normal concise text. Do **not** wrap a one-line answer in a box.
- **Complex development tasks** → structured visual sections.
- **Long-running work** → compact progress/status.
- **Errors** → clarity over decoration.
- **Code/commands** → copyability over formatting.

## Avoid

Never turn every response into a giant ASCII dashboard. No huge ASCII logos · no borders around every paragraph · no excessive emojis/symbols · no fake progress, tests, or animations · no extremely wide diagrams · no unreadable Unicode art · no decoration that hides important information. Professional, not childish or gimmicky.

## Scope — this skill modifies nothing

It controls terminal **presentation only**. It must **not**: modify Claude Code or its source · change terminal/PowerShell/CMD/Windows settings · install terminal themes · modify another project · or modify any files. (Read-only tools are provided precisely so it can inspect a project to render accurate trees/diagrams without changing anything. Actual file changes belong to whatever separate skill/task the user explicitly requested — this skill just *presents* their results.)

---

**Final principle:** the terminal should communicate like a professional engineering console — **clear, structured, visual, compact, useful, readable.** Reach for a box, tree, or dashboard only when it makes the content easier to understand; when in doubt, plain text. Honesty always beats decoration: never render a ✓, a percentage, a passed test, or a changed file that isn't real.
