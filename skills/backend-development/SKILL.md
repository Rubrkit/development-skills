---
name: backend-development
description: "Use when implementing, reviewing, or refactoring backend code in a Next.js app: API routes, server actions, controllers, services, repositories, database access, auth/session handling, validation, security-sensitive flows, Postgres, Drizzle, or any code that belongs under the /server root."
title: Backend Development (Next.js)
tagline: Structure Next.js backend code as route → controller → service → repository with validation, security checkpoints, and testable boundaries.
category: development
featured: true
---

# Backend Development

Use this skill for backend work in a Next.js app. Favor reusable, testable server code with explicit boundaries and security review built into the workflow.

## Architecture Rules

- Keep reusable backend code under the `/server` root folder.
- Treat Next.js API routes and server-action entrypoints as the thin edge. They should parse framework inputs, call controllers, and return framework responses.
- Keep route handlers in `app/api/**/route.js` or `app/api/**/route.ts` lean; all reusable validation, orchestration, business logic, persistence, and security helpers should live under `/server`.
- Put request orchestration in controllers. Controllers validate inputs, call services, map errors, and shape responses.
- Put business logic in services. Services coordinate use cases and should not depend on Next.js request/response objects.
- Put persistence behind repositories. Repositories own database queries, transactions, and database-specific details.
- Keep shared backend utilities under `/server` by concern, such as `server/security`, `server/validation`, `server/errors`, or `server/db`.
- Do not import server modules into client components.

Recommended shape:

```text
app/api/<resource>/route.js
server/controllers/<resource>Controller.js
server/services/<resource>Service.js
server/repositories/<resource>Repository.js
server/db/client.js
server/db/schema.js
server/errors/
server/security/
server/validation/
```

## Engineering Standards

- Promote code reuse, but avoid abstractions that only hide one call site.
- Apply DRY where duplication creates maintenance risk; keep small duplication when it preserves clarity.
- Prefer repository, service, controller, mapper, validator, and policy patterns when they fit the problem.
- Keep modules focused around use cases and ownership boundaries.
- Use dependency injection by parameters or factories when it improves testability without adding ceremony.
- Prefer explicit return objects and typed JSDoc over implicit shape coupling.
- Prefer TypeScript file extensions when the codebase uses TypeScript; keep the same layering either way.
- Use database constraints, transactions, and indexes for invariants that matter.

## Security Standard

Security is a top priority. Pause and discuss with the user before implementing a path that creates meaningful risk or changes trust boundaries. For low-risk CRUD changes, refactors, or bug fixes that stay within existing auth and data-access patterns, proceed normally.

Push back or ask for a decision when work touches:

- Authentication, authorization, roles, tenants, sessions, or ownership checks.
- Secrets, environment variables, token handling, webhooks, or third-party credentials.
- User-generated content, file uploads, exports, imports, prompts, or stored AI outputs.
- SQL construction, dynamic filters, raw queries, migrations, or destructive database operations.
- Rate limits, abuse prevention, audit trails, logging, privacy, or data retention.

Default expectations:

- Validate all external input at the boundary before calling services.
- Authorize every read and write against the acting user, resource ownership, and tenant scope.
- Keep secrets server-only and out of logs, client bundles, and serialized responses.
- Prefer parameterized queries over raw SQL.
- Return safe error messages to clients; keep detailed diagnostics in server logs.
- Treat migrations and destructive operations as review-worthy changes.

## Inputs

Before starting, gather: the **target resource / boundary** (route, controller, service, repository), the **change goal**, the **data and auth model** it touches (ownership, tenant, roles), and the **available scripts** (from `package.json`). If any are missing, **inspect the existing patterns first** — nearby route → controller → service → repository, schema, validation — and follow them; ask clarifying questions only when a critical detail remains unknown, and never guess on auth, persistence, or destructive operations.

## Backend Workflow

1. Inspect existing route, controller, service, repository, schema, and validation patterns before editing.
2. Identify the backend boundary being changed and keep files in `/server` unless the code is purely Next.js routing glue.
3. Design the flow route -> controller -> service -> repository, skipping layers only when the feature is trivial and the boundary remains clear.
4. Define validation, authorization, error handling, and transaction needs before writing persistence code.
5. Implement the smallest reusable modules that solve the use case.
6. Add or update focused tests when behavior, security checks, or persistence contracts change.
7. Run relevant verification, usually `npm run typecheck`, `npm run lint`, and targeted tests/build commands when available.

## Output contract

Deliver a handoff with these named sections:

1. **Summary** — the use case and the layers touched (route / controller / service / repository).
2. **Files changed** — each path with a one-line note; confirm reusable logic landed under `/server`.
3. **Rationale** — key design choices (layering, validation, transactions) and any abstraction tradeoffs.
4. **Security & risk notes** — which trust boundaries were touched and how input was validated/authorized, or "none" with why.
5. **Verification** — `typecheck` / `lint` / tests run and their results.

Keep it a concise bulleted note a reviewer can verify against.

## Definition of done

A change is done only when **all** hold:

- All reusable validation, orchestration, business logic, persistence, and security helpers live under `/server`; route files stay thin.
- Every new read/write is validated at the boundary and authorized against the acting user, resource ownership, and tenant scope.
- Auth-, secret-, SQL-, migration-, or destructive-touching work was explicitly gated by review/clarification with the user, not silently implemented.
- `npm run typecheck` and `npm run lint` pass (or the closest available checks, noted); focused tests added/updated when behavior, security, or persistence contracts changed.
- The handoff note above is written, with any failing or skipped check called out.

## Edge cases & ambiguity

- **Unclear scope or missing detail** → inspect the existing patterns first; if a blocking decision remains, ask rather than guess.
- **Auth, persistence, or destructive operations** → never guess. Surface the risk and get an explicit decision before proceeding (see Security Standard).
- **The request conflicts with the layering or security rules** → flag it and prefer the established boundary; don't bypass `/server` layering or auth checks to satisfy a request.
- **A required script or test is missing** → run the closest equivalent and state in the handoff what couldn't be verified.

## Next.js Route Pattern

Keep route files boring and thin:

```js
export async function POST(request) {
  return createThingController({ request });
}
```

The controller should own body parsing, validation, calling services, and response mapping. Services and repositories should remain portable server modules that can be tested without Next.js route machinery.
