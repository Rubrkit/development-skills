---
name: frontend-development
description: Use when implementing, reviewing, or refactoring a Next.js App Router web app — React/MUI pages, templates, reusable components, SWR/local-state helpers, SEO metadata, responsive QA, accessibility checks, performance, and cleanup before handoff.
title: Frontend Development (Next.js + MUI)
tagline: Build and review production Next.js App Router UIs with MUI, JSDoc-typed JavaScript, SWR, and SEO/responsive/accessibility QA.
category: development
featured: true
---

# Frontend Development

Use this skill for production web work in a JavaScript-first Next.js App Router app with Material UI, SWR, `// @ts-check`, JSDoc, a local helper/store pattern, and a typed SQL data layer (for example Postgres with Drizzle).

## Conventions

- Use Next.js App Router conventions: thin `app/` route files, templates under `components/templates/`, reusable UI under `components/`, client/shared helpers under `lib/`, and backend code under `/server`.
- Use MUI primitives and theme tokens before custom HTML/CSS.
- Only attach hover/focus affordances (lift, border-glow, color shift, cursor changes) to genuinely interactive elements — links, buttons, inputs, controls. Do not add them to static display elements like content cards, chips, badges, or labels the user cannot click; it implies an interaction that is not there and misleads the user.
- Use `// @ts-check` and useful JSDoc on authored JavaScript files.
- Use SWR for repeated client reads and local cache semantics where it improves deduplication or revalidation.
- Make user-initiated mutations **optimistic** by default — reflect the change instantly, reconcile with the server response, and roll back on error. Do not block the UI on a network round-trip for ordinary CRUD/toggle actions. See `references/next-mui-patterns.md` (Optimistic mutations) for the SWR and local-state patterns and the exceptions (long-running jobs).
- Keep persistence behind a clear boundary; a typed SQL data layer (for example Postgres with Drizzle) is a good default.
- Avoid introducing unrelated tooling (CMS contracts, design-tool or ticketing workflows) unless the task explicitly calls for it.

## Inputs

Before starting, gather: the **repo path / target file or route**, the **change goal**, any **constraints** (design system, performance budget, browser support), and the **available scripts** (from `package.json`). If any are missing, **inspect the repo first** — nearby files, scripts, and conventions — and proceed on the established pattern; ask only when a critical, unguessable product decision remains.

## Workflow

1. Inspect the existing structure and nearby patterns before editing.
2. Classify the change:
   - **App/page implementation**: route/template/component work.
   - **Shared helper work**: hooks, store, SWR, theme, providers.
   - **Public route work**: metadata, sitemap, robots, semantic HTML, and SEO/AEO basics.
   - **Rendered UI change**: responsive and accessibility verification is required.
3. Implement in small focused files. Split components before they mix state orchestration, repeated render fragments, styling variants, responsive branches, and interaction handlers.
4. Verify with `npm run typecheck`, `npm run lint`, and `npm run build` when available. If a script is missing, note the gap and run the closest available equivalent check.
5. For rendered UI changes, run a browser smoke test at desktop and at least one mobile viewport. Check for horizontal overflow, broken interaction, console errors, and obvious accessibility regressions.
6. Cleanup before handoff: remove dead imports, temporary logs, generated artifacts, placeholder code that is not intentional, and unused dependencies. Leave a brief handoff note with the files changed, checks run, and any known follow-ups.

## Output contract

Deliver a handoff with these named sections:

1. **Summary** — what changed and why, in 1–3 sentences.
2. **Files changed** — each path with a one-line note (new / edited / deleted).
3. **Checks run** — `typecheck` / `lint` / `build` results, and for rendered UI, the viewports smoke-tested (desktop + ≥1 mobile) and what you verified.
4. **Risks & follow-ups** — known gaps, deferred work, anything the reviewer should watch.

Prefer a concise bulleted note over prose; another developer should be able to verify the result from it alone.

## Definition of done

A change is done only when **all** hold:

- `npm run typecheck` and `npm run lint` pass (or the closest available checks, with the substitution noted); `npm run build` passes when present.
- For rendered UI: no horizontal overflow, no new console errors, interactions work, and no obvious accessibility regressions at desktop + ≥1 mobile viewport.
- Cleanup done: no dead imports, stray logs, generated artifacts, placeholder code, or unused dependencies.
- The handoff note above is written, with any failing or skipped check called out rather than hidden.

## Edge cases & ambiguity

- **Unclear scope** → inspect the repo for the established pattern and follow it; if a genuinely ambiguous product decision remains, ask before implementing rather than guessing.
- **A required script is missing** (no `build` / `lint` / `typecheck`) → run the closest equivalent and state in the handoff which check was unavailable.
- **The request conflicts with repo conventions** → flag the conflict and prefer the existing convention unless the user explicitly wants to change it; never silently introduce a divergent pattern.
- **Rendered UI you can't verify** (no browser available) → say so explicitly instead of claiming visual/responsive QA passed.

## References

Load only the reference needed for the task:

- `references/js-ts-check-jsdoc.md` for JavaScript typing and React/Next performance rules.
- `references/next-mui-patterns.md` for App Router, MUI theme, GlobalStyles, component splitting, and state coverage.
- `references/responsive-qa.md` for browser, responsive, accessibility, and visual QA.
- `references/seo-aeo.md` for public route metadata, semantic HTML, sitemap/robots, and JSON-LD rules.
