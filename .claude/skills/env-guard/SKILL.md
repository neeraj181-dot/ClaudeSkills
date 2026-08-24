---
name: env-guard
description: Safely configure a project's API credentials via environment variables and local .env files while making accidental Git exposure extremely difficult. Detects the stack, sets the right env var name, protects .env via .gitignore, maintains a secret-free .env.example, scans for hardcoded secrets, and checks Git tracking/history — never printing, committing, or hardcoding a secret, and never rewriting Git history automatically. Only touches the specified project. Works across JS/Node, Python/Django, and similar stacks. Use when the user wants to set up an API key/token/secret, add .env handling, stop secrets leaking into Git, or check for exposed credentials.
tools: Read, Glob, Grep, Bash, Edit, Write
---

# Env Guard

Safely configure project API credentials using **environment variables** and local **`.env`** files, while preventing secrets from ever reaching Git. The goal: **"Configure credentials automatically while making accidental Git exposure extremely difficult."**

**Core principle:** API keys, tokens, passwords, and secrets must **NEVER** be hardcoded into source or committed to Git. The actual secret value must never be written into source code · README/docs · Git commits · GitHub repos · committed config files · logs · **or terminal output**. Use env vars and local `.env` files instead.

This skill only touches the **specified project** (see Phase 12) and never rewrites Git history on its own (Phase 8).

## Phase 1 — Inspect the project

Detect (don't modify yet): framework · language · package manager · existing `.env` files · `.env.example` · `.gitignore` · whether it's a Git repo · existing env-var conventions · existing API configuration.

## Phase 2 — Determine the env variable name

When the user provides a key and asks to configure it, determine the correct variable name — e.g. `ANTHROPIC_API_KEY`, `ANTHROPIC_AUTH_TOKEN`, `OPENAI_API_KEY`, `GOOGLE_API_KEY`, `GITHUB_TOKEN`, `STRIPE_SECRET_KEY`.

If the required name is **ambiguous, ask.** Never guess a provider's auth variable when a wrong guess could make the app fail (e.g. `_API_KEY` vs `_AUTH_TOKEN` + `_BASE_URL`).

## Phase 3 — Protect `.env`

If the project uses `.env`, ensure it's git-ignored:

- No `.gitignore`? **Create one** in the current project.
- Existing `.gitignore`? **Preserve its contents**; add only the missing entries.

Common entries:

```
.env
.env.local
.env.*.local
```

**Do not blindly ignore `.env.example`** — that file is meant to be committed.

## Phase 4 — Create/update `.env.example`

Create or update `.env.example` with **variable names only, never real secrets**:

```
ANTHROPIC_API_KEY=your_api_key_here
# or:
ANTHROPIC_BASE_URL=https://example.com
ANTHROPIC_AUTH_TOKEN=your_token_here
```

**Never copy a real token into `.env.example`.**

## Phase 5 — Configure the local environment

When the user explicitly provides a secret and asks to configure it, store it in the appropriate **local** config — prefer **`.env`** for project-local application credentials. For Claude Code-specific env vars, use the appropriate Claude Code config or shell environment per the user's request.

**Never** reach for **system-wide** environment variables when project-local config is sufficient. Write the value only to the local `.env` (which Phase 3 has ignored) — and never echo it back.

## Phase 6 — Hardcoded-secret detection

Search the project for likely hardcoded secrets — API keys, tokens, bearer tokens, secret keys, passwords, private credentials (common prefixes like `sk-`, `ghp_`, `AKIA`, `Bearer `, long base64/hex blobs).

**Do not print any discovered secret.** Report only the location:

```
⚠ Potential secret found in <file>:<line>
```

Never display the actual value.

## Phase 7 — Git tracking safety

Run `git status` and determine whether `.env` is already tracked.

- **Untracked and ignored** → good.
- **Already tracked** → **STOP.** Explain: *"The `.env` file is already tracked by Git."* Ask permission **before** running `git rm --cached .env`. **Never delete the user's local `.env`** (only untrack it, and only with consent).

## Phase 8 — Git history safety

If a secret appears to have **already been committed**, **do NOT attempt automatic history rewriting.** Explain that removing a secret from the working tree does **not** remove it from history. Recommend, in order:

1. **Rotate/revoke** the exposed credential (most important).
2. Remove the secret from current files.
3. Clean Git history separately if necessary (e.g. `git filter-repo` / BFG) — as a deliberate, user-driven step.

Never expose the secret in the report.

## Phase 9 — Framework integration

Where appropriate, wire the app to **read** the env var — without inventing application logic:

```
JavaScript   process.env.API_KEY
Python       os.getenv("API_KEY")
Django       os.environ.get("API_KEY")
```

**Only modify application logic if the user asked for API integration/configuration.** Otherwise, stop at env/ignore/example setup.

## Phase 10 — Verification (masked)

Verify: `.env` exists where appropriate · `.env` is ignored · `.env.example` has placeholders only · app config references the env var · the secret is **not** in tracked source · `git status` doesn't show `.env` as untracked.

**Never print the secret while verifying.** Prefer a plain status — `API key: CONFIGURED` — or masked form at most — `sk-****…****`.

## Phase 11 — Multiple environments

Support: **Development** → `.env`; **Production** → the platform/host secret manager; **Testing** → `.env.test` or test env vars. **Never recommend committing production secrets.**

## Phase 12 — Scope safety

Only modify the **exact project folder** the user specified. **Never** modify another project, another repository, global Git configuration, or system environment variables — unless explicitly requested. (Pairs with `safe-scope`.)

## Phase 13 — Final report

```
╭─ Environment Security ───────────────────────╮
│ API variable   │ CONFIGURED                  │
│ .env           │ PROTECTED                   │
│ .gitignore     │ UPDATED                     │
│ .env.example   │ SAFE                        │
│ Git status     │ CLEAN                       │
╰───────────────────────────────────────────────╯
```

Then:

```markdown
## Files Changed
(files changed — never revealing secrets)

## Security Status
(is the credential protected?)

## Git Status
(could the secret be committed? is .env tracked?)

## Remaining Issues
(anything needing user action — e.g. rotate an exposed key)
```

Report real states only — if `.env` is still tracked or a secret was already committed, say so plainly rather than showing a green dashboard.

---

**Critical security rules — always:** NEVER print API keys or tokens; NEVER put secrets in `.env.example`; NEVER commit `.env`; NEVER hardcode secrets into source; NEVER push secrets to GitHub; NEVER expose secrets in error messages, logs, or terminal output; NEVER put secrets in README files; NEVER rewrite Git history automatically; NEVER delete a user's `.env`; NEVER modify another project; if a secret was exposed publicly, **recommend rotating it**; and if `.env` is already tracked, **ask before** removing it from tracking.
