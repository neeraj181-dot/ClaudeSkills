# Claude Code Skills

A collection of **reusable [Claude Code](https://claude.com/claude-code) skills** for real development workflows — planning, understanding, designing, auditing, testing, optimizing, securing, and shipping software.

![Skills](https://img.shields.io/badge/skills-50-blue) ![Claude Code](https://img.shields.io/badge/Claude%20Code-Skills-8A2BE2)

Each skill is **self-contained and modular** — a single `SKILL.md` that teaches Claude Code how to act like a specialized senior engineer for one job. Install only the skills you need, or use them together as a full lifecycle.

> **Use the right skill for the right job.**

Every skill in this repository follows the same principles: it **inspects before it acts**, **preserves existing functionality**, **stays inside the project you point it at**, **never fabricates results**, and **never exposes secrets**. Many are **read-only by default** and only modify code when you explicitly ask.

---

## ✨ Features

- **Modular skills** — each skill does one job well; install one, some, or all.
- **Project-aware workflows** — skills detect your stack (React, Next.js, Vite, Django, FastAPI, Node.js, PHP, Java/Spring Boot, Flutter, Docker, …) and work with your existing conventions.
- **Safe project boundaries** — `safe-scope` confines all file operations and commands to the exact folder you specify.
- **UI/UX improvements** — `ui-redesign` for visual design, `motion-designer` for purposeful animation and interaction.
- **Automated QA workflows** — `qa-engineer` tests the *running* app; `code-auditor` statically reviews the source.
- **Performance analysis** — `performance-optimizer` finds real bottlenecks, fixes them minimally, and verifies the gain.
- **Dependency management** — `dependency-doctor` audits, cleans, and safely upgrades packages.
- **Environment & secrets protection** — `env-guard` configures credentials via `.env` and makes accidental Git exposure very hard.
- **Release workflows** — `ship-it` verifies production readiness and deploys only with explicit permission.
- **Terminal visualization** — `terminal-visualizer` presents output as a clean developer console.
- **Skill coordination** — `skill-manager` selects the smallest useful set of skills for a task and orders them.

*Every capability listed above maps to an actual skill in this repository — nothing is aspirational.*

---

## 📦 Available Skills

Descriptions are taken directly from each skill's `SKILL.md`.

| Skill | Description | Best For |
|---|---|---|
| **safe-scope** | Enforces strict project/folder boundaries — reads, writes, and commands stay inside the exact folder(s) you specify; stops and asks before anything out of scope. | Confining work to one project; multi-project drives |
| **skill-manager** | Discovers installed skills, selects the smallest useful set for the task, detects conflicts, and sets a sensible execution order — selection is temporary and never edits skill files unless asked. | Choosing and coordinating which skills to use |
| **context-keeper** | Maps an unfamiliar codebase into a `PROJECT_CONTEXT.md` (structure, architecture, entry points, feature/route/API/DB maps, data flows, conventions, unknowns). Read-only except that one file. | Onboarding; understanding a project before changing it |
| **product-architect** | Turns a vague idea or feature request into a realistic product plan — problem, users, scope, architecture, DB schema, API design, security review, roadmap — before any code. | Planning a new app or feature |
| **ui-redesign** | Redesigns an existing page into a polished, human-designed interface that doesn't look AI-generated, while preserving all existing functionality. | Restyling / modernizing an existing UI |
| **motion-designer** | Adds professional, purposeful animation and interaction to an existing site with a coherent motion system, `prefers-reduced-motion` support, and preserved behavior. | Tasteful animations, transitions, micro-interactions |
| **code-auditor** | Strict static audit of an existing codebase for bugs, security, architecture, performance, reliability, and maintainability, with a severity-ranked report. Read-only until you say fix. | Finding problems in source code |
| **qa-engineer** | Tests the *actual running application* — real user flows, invalid/edge inputs, console/network/API, responsive layouts — and reports reproduced bugs. Read-only until you say fix. | Finding real user-facing bugs by running the app |
| **performance-optimizer** | Finds real bottlenecks (renders, bundles, N+1/slow queries, network waterfalls, assets, caching), fixes them with the smallest useful change, and verifies the speedup. Never fabricates numbers. | Diagnosing and fixing slowness |
| **dependency-doctor** | Audits, cleans, and safely upgrades dependencies across ecosystems (npm/pnpm/yarn, pip/Poetry/uv, Maven/Gradle, Composer, Dart), runs official security audits, and plans minimal controlled upgrades. Read-only until you say update. | Dependency audits, vulnerabilities, safe upgrades |
| **ship-it** | Senior release engineer — verifies production readiness and secret safety, picks a provider-agnostic deploy strategy, and deploys only with explicit permission, then verifies the live app. | Preparing and shipping to production |
| **terminal-visualizer** | Presents Claude Code's output as a polished developer console (Unicode boxes, tables, trees, status/progress blocks, dashboards) for clarity — modifies nothing. | Clearer, more structured terminal output |
| **env-guard** | Configures API credentials via environment variables and local `.env`, protects `.env` via `.gitignore`, keeps a secret-free `.env.example`, and scans for hardcoded secrets — never printing or committing a secret. | Setting up API keys safely; preventing secret leaks |

---

## 🧩 Skill Categories

### 🛡️ Safety & Security
- **safe-scope** — keep work inside the specified folder
- **env-guard** — protect API keys and secrets from Git

### 🧭 Coordination
- **skill-manager** — pick and order the right skills for a task

### 🧠 Planning & Understanding
- **product-architect** — plan before building
- **context-keeper** — understand and document a codebase

### 🎨 Design & UX
- **ui-redesign** — static visual design (layout, type, color)
- **motion-designer** — motion & interaction design

### 🔍 Quality & Testing
- **code-auditor** — static source audit
- **qa-engineer** — test the running application
- **performance-optimizer** — find & fix real bottlenecks

### 📦 Maintenance
- **dependency-doctor** — audit & safely upgrade dependencies

### 🚀 Release
- **ship-it** — verify and deploy to production

### 🖥️ Developer Experience
- **terminal-visualizer** — polished, readable terminal output

---

## 🚀 Installation

Claude Code discovers skills in a `.claude/skills/` directory — either **project-level** (`<your-project>/.claude/skills/`) or **user-level** (`~/.claude/skills/`, available in every project).

```bash
# 1. Clone this repository
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

**Install a single skill** into a project (copy just the folder you want):

```bash
# macOS / Linux
cp -r .claude/skills/code-auditor /path/to/your-project/.claude/skills/

# Windows (PowerShell)
Copy-Item -Recurse .claude\skills\code-auditor C:\path\to\your-project\.claude\skills\
```

**Install every skill for your user** (available in all projects):

```bash
# macOS / Linux
mkdir -p ~/.claude/skills
cp -r .claude/skills/* ~/.claude/skills/

# Windows (PowerShell)
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse .claude\skills\* "$HOME\.claude\skills\"
```

Restart or reopen Claude Code so it picks up the new skills.

---

## ▶️ Usage

Once installed, invoke a skill explicitly by name:

```
/code-auditor
/ui-redesign
/ship-it
```

Or simply describe your task — Claude Code will match it to the relevant skill based on the skill's description. Not sure which to use? Ask:

```
/skill-manager   →  which skills should I use to build and ship a login page?
```

### Using skills together

The skills are designed to compose across a project's lifecycle. A typical flow:

```
context-keeper  →  product-architect  →  (implement)  →  ui-redesign
                                                          motion-designer
                                             │
                                             ▼
                    code-auditor  →  qa-engineer  →  performance-optimizer
                                             │
                                             ▼
                            dependency-doctor  →  env-guard  →  ship-it
```

`safe-scope` and `terminal-visualizer` are cross-cutting — they apply on top of whatever else you're doing (boundaries and presentation), and `skill-manager` helps you choose and order the rest.

---

## 🗂️ Repository Structure

Skills live under `.claude/skills/`, one folder per skill, each containing a single `SKILL.md`:

```
.claude/skills/
├── safe-scope/            SKILL.md
├── skill-manager/         SKILL.md
├── context-keeper/        SKILL.md
├── product-architect/     SKILL.md
├── ui-redesign/           SKILL.md
├── motion-designer/       SKILL.md
├── code-auditor/          SKILL.md
├── qa-engineer/           SKILL.md
├── performance-optimizer/ SKILL.md
├── dependency-doctor/     SKILL.md
├── ship-it/               SKILL.md
├── terminal-visualizer/   SKILL.md
└── env-guard/             SKILL.md
```

---

## 🔒 Shared principles

Every skill in this collection is built around the same guardrails:

- **Inspect first, act second** — understand the project before changing anything.
- **Preserve functionality** — changes are minimal, focused, and behavior-safe.
- **Stay in scope** — never modify unrelated projects or files outside the target folder.
- **Never expose secrets** — credentials are redacted, never printed or committed.
- **Be honest** — never claim a test passed, a build succeeded, or a file changed unless it actually did.
- **Read-only by default where it matters** — audit/test/dependency skills only modify code when you explicitly ask.

---

## 🤝 Contributing

New skills are welcome. Each skill is a folder under `.claude/skills/<skill-name>/` with a `SKILL.md` that has YAML frontmatter (`name`, `description`, `tools`) followed by clear, phase-based instructions. Follow the conventions of the existing skills:

- Give the skill **one clear job** and a discovery-friendly `description`.
- Grant only the **tools it needs**.
- State its **guardrails** explicitly (scope, safety, honesty).

---

## 📄 License

Licensed under the Apache License 2.0.
You are free to use, modify, and redistribute this project
while preserving the required copyright and license notices.
