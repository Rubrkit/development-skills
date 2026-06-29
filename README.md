# Development skills

[![Grade skills](https://github.com/Rubrkit/development-skills/actions/workflows/grade.yml/badge.svg)](https://github.com/Rubrkit/development-skills/actions/workflows/grade.yml)

The development skills we use to build Rubrkit ([https://www.rubrkit.com](https://www.rubrkit.com)), open-sourced.

Skills for building production web apps — frontend and backend.

## Install

These skills are compatible with the open agent-skills ecosystem ([vercel-labs/skills](https://github.com/vercel-labs/skills)). Install the whole set into any supported agent (Claude Code, Cursor, Codex, and more) with:

```bash
npx skills add Rubrkit/development-skills
```

Or copy any `SKILL.md` below straight into your agent's `skills/` directory.

## Skills

- [Backend Development (Next.js)](https://www.rubrkit.com/skills/backend-development) — Structure Next.js backend code as route → controller → service → repository with validation, security checkpoints, and testable boundaries.
- [Frontend Development (Next.js + MUI)](https://www.rubrkit.com/skills/frontend-development) — Build and review production Next.js App Router UIs with MUI, JSDoc-typed JavaScript, SWR, and SEO/responsive/accessibility QA.

## Graded by Rubrkit

Every skill in this repo is graded by [Rubrkit](https://www.rubrkit.com) in CI on each push. A skill that scores below 75 fails the build — the same quality gate you can run on your own instructions with `npx rubrkit test … --fail-under 75`. See [CI quality gates](https://www.rubrkit.com/ci-quality-gates).

## About

Browse the full, rendered collection at [https://www.rubrkit.com/skills](https://www.rubrkit.com/skills).

Licensed under the MIT License.
