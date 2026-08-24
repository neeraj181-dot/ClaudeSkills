---
name: ui-redesign
description: Redesign an existing website page into a polished, professional, human-designed interface that does not look AI-generated — while preserving all existing functionality. Works across React, Next.js, plain HTML/CSS/JS, Django templates, and similar web projects. Use when the user asks to redesign, restyle, revamp, modernize, clean up, or improve the look of an existing page or UI.
tools: Read, Glob, Grep, Bash, Edit, Write, WebFetch
---

# UI Redesign

Redesign an **existing** page so it looks like a competent human designer made deliberate choices — not like a template or an AI generator filled in defaults. The bar is not "modern"; the bar is "intentional." Two hard constraints govern everything below:

1. **Preserve behavior.** The redesign is visual and structural-in-markup only. Functionality, data, and logic must work exactly as before.
2. **Stay in scope.** Only touch the current project. Never modify unrelated projects or files outside the working directory.

Work through the phases in order. Do not skip inspection to jump to code.

---

## Phase 1 — Inspect before changing anything

Understand what exists before altering a single line.

- Detect the stack and entry points. Look for `package.json`, `next.config.*`, `vite.config.*`, `index.html`, `manage.py` / `templates/`, or static `*.html`/`*.css`. Identify the framework, styling approach (CSS/Sass, CSS Modules, Tailwind, styled-components, plain stylesheets), and component conventions already in use.
- Locate the exact page(s) in scope and read them fully, plus the components, layouts, and stylesheets they pull in.
- Inventory existing design assets: fonts, color tokens/variables, logo, images/SVGs, spacing scales, shared components (buttons, inputs, cards, nav). You will reuse these where possible.
- Note the tooling: dev server command, build command, linting. You'll need these in Phase 6.

Do not edit yet.

## Phase 2 — Understand purpose and content

- What is this page *for*? Who uses it, and what is the single most important thing they came to do?
- Map the real content hierarchy — what must be seen first, what is secondary, what is chrome.
- Catalog existing functionality that must survive untouched: **routing, links, API calls, data fetching, authentication/authorization, form submission and validation, state management, event handlers, and business logic.** Note every `id`, `class`, `data-*`, `name`, `htmlFor`/`for`, `aria-*`, ref, and hook the code depends on. These are contracts — preserve the ones behavior relies on.

## Phase 3 — Diagnose the current UI

List concrete, specific problems (not vague adjectives) in each area:

- **Hierarchy** — is the primary action/content actually dominant?
- **Spacing** — inconsistent gaps, cramped or arbitrary padding, no rhythm.
- **Typography** — too many sizes/weights, poor scale, weak line-height/measure, default system stack used thoughtlessly.
- **Layout & alignment** — nothing lines up to a grid, centered-everything, no structure.
- **Responsiveness** — breakpoints missing or broken; mobile is an afterthought.
- **Consistency** — buttons/inputs/colors differ across the page for no reason.
- **Usability** — unclear affordances, poor contrast, tiny tap targets, ambiguous states.

## Phase 4 — Establish a design direction (before implementing)

Decide the direction first, then execute it. Define a small, coherent system:

- **Type**: a deliberate pairing (display + body, plus a utility/mono role if data-heavy) and a clear modular scale with intentional weights and line-heights. Prefer refining the existing typeface choices over adding new ones unless they're genuinely wrong.
- **Color**: a restrained palette — one or two real accents plus a neutral ramp. Reuse existing brand tokens/variables when they exist.
- **Spacing & grid**: one spacing scale (e.g. 4/8-based) and a grid or column structure that content aligns to.
- **Layout concept**: how the page is sectioned, where whitespace does the work, and where an asymmetric or editorial composition serves the content better than yet another centered stack.

Sanity-check the direction against the brief: if any part reads like the default you'd produce for *any* page, revise it and make it specific to this product's purpose and content.

## Phase 5 — Implement the redesign

Actually build it — do not merely propose changes.

**Preserve behavior while restyling.** Change markup structure, classes, and styles freely, but keep the hooks logic depends on: routes and hrefs, API/fetch calls, auth gates, form `name`/validation/submit handlers, state bindings, event handlers, and the attributes/selectors they key off. When you restructure an element that carries a behavioral contract, carry the contract with it and verify the wiring still resolves.

**Design principles — make it look human-made:**

- Lead with strong typography, generous and *consistent* whitespace, real alignment to the grid, and clear visual hierarchy. Restrained color.
- Design each breakpoint intentionally — **mobile, tablet, desktop** are distinct compositions, not one layout that merely shrinks. Ensure adequate tap targets and readable measure on small screens.
- Use subtle animation *only* where it aids usability or feedback (state changes, focus, meaningful transitions). Respect `prefers-reduced-motion`.
- Reuse existing components and assets; extend rather than duplicate. Introduce a new component only when it genuinely doesn't exist.

**Avoid the generic-AI aesthetic. Do not:**

- Wrap every section in a card — most sections are better defined by whitespace, a heading, and alignment than by a bordered box.
- Lean on excessive glassmorphism, gradients, or blur.
- Over-round everything, scatter decorative blobs, or stack drop-shadows for depth that isn't there.
- Sprinkle icons on every item or animate things that don't need it.
- Reach for these defaults: cream-background + serif + terracotta; near-black + one acid accent; hairline-ruled "broadsheet" columns. Fine if the brief truly calls for one; not fine as an autopilot choice.

**Prefer instead:** intentional sectioning, dividers and rules used sparingly, typographic contrast, whitespace as structure, and asymmetric/editorial layouts where they fit the content.

**Keep dependencies lean.** Don't add UI libraries, icon packs, or font bundles unless clearly justified and lightweight. Don't rewrite working code that isn't part of the redesign.

## Phase 6 — Run and test

- Start the dev server (or build) using the project's own commands. If a browser-automation/verify skill is available, use it to load the page and capture the rendered result and console output.
- Check for: **build errors, console errors/warnings, broken imports or missing assets, layout at mobile/tablet/desktop breakpoints, accessibility (contrast, focus visibility, semantic markup, labels/alt, keyboard nav), and visual consistency** across states.
- **Fix every problem you find** before moving on. Re-run until clean.

## Phase 7 — Final visual review

Look at the result critically, as if reviewing someone else's work. Anything that still feels generic, templated, or AI-generated — a needless card, a default gradient, misaligned spacing, an unmotivated animation — refine or remove it. Confirm the two hard constraints held: behavior preserved, nothing outside the project touched.

## Phase 8 — Report

Summarize concisely:

- **What was redesigned** — pages/sections addressed.
- **Major design decisions** — type, color, layout, spacing direction and the reasoning.
- **Components changed / created** — with file paths.
- **Responsive improvements** — what changed per breakpoint.
- **Remaining issues** — anything unresolved, deferred, or needing the user's input.

---

**Guardrails, always:** preserve existing functionality; keep the change visual/structural, not behavioral; reuse before adding; add no unnecessary dependencies; and never modify unrelated projects or files outside the current project.
