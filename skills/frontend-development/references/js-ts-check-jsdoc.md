# JS, JSDoc, React, And SWR

Use these rules when authoring JavaScript in a JavaScript-first Next.js app.

## Type Checking

- Add `// @ts-check` to authored `.js` and `.mjs` source/config files or use typescript if the repo allows it.
- Add JSDoc to exported components, hooks, route handlers, scripts, schema/helpers, and non-trivial local helpers.
- Keep JSDoc accurate and useful. Do not add comments that only restate obvious assignments.
- Prefer focused typedefs for shared object shapes when a file uses the same shape more than once.
- Keep route files thin: export metadata and render a template or small server wrapper.

## React And Next Performance

- Do not define components inside components. Extract child render pieces and pass props.
- Derive values during render when they can be computed from current props/state; do not mirror them into state through effects.
- Avoid setting state synchronously inside effects solely to transform props or initialize static values.
- Use Suspense boundaries around client hooks that read URL/search state or around async subtrees where the shell can render first.
- Minimize server-to-client serialization. Pass only fields used by client components.
- Use dynamic imports for heavy or client-only components that are not needed for initial render.
- Keep import paths statically analyzable.

## SWR And Local Data

- Use SWR for repeated client-side reads that benefit from deduplication, cache, and revalidation.
- Use stable keys. Use `null` keys to pause reads until required inputs exist.
- For local/prototype data, keep the store shape small and versionable; avoid unbounded localStorage growth.
- For mutations, update the source of truth and call `mutate` with `revalidate: false` when the next value is already known.

## JavaScript Hygiene

- Return early from validation and classification functions.
- Hoist reusable RegExp values when they do not depend on render-time values.
- Use `Set`/`Map` for repeated lookups.
- Avoid mutating props/state arrays with `.sort()`; use `.toSorted()` or copy first.
