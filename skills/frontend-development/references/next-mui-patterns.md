# Next.js And MUI Patterns

Use these rules for page, template, component, theme, and provider work in a Next.js App Router + MUI app.

## App Router

- Keep `app/` route files thin. They should export metadata and render `RootTemplate` or a route-specific template.
- Put reusable page surfaces in `components/templates/`.
- Put client-only interactions behind explicit `'use client'` files.
- Do not create dynamic client wrappers for whole pages unless the route truly needs client behavior.
- Use semantic HTML through MUI `component` props: `header`, `nav`, `main`, `section`, `article`, `footer`.

## MUI

- Use MUI primitives before custom HTML.
- Prefer theme tokens, `palette`, `typography`, `shape`, spacing, and component overrides before repeating raw values.
- Keep `GlobalStyles` mounted with `CssBaseline` inside `ThemeProvider`.
- Global styles should cover only durable app defaults: `html`, `body`, `a`, `button/input/textarea`, `::selection`, and necessary third-party widgets.
- Do not add a second styling system unless the project already uses it.

## Component Splitting

Split a component before final QA when it mixes:

- State orchestration.
- Repeated render fragments.
- Styling variants.
- Responsive branches.
- Interaction handlers.
- Data adaptation.

Keep parent components focused on orchestration. Move cards, list items, CTA groups, panels, buttons, badges, and repeated fragments into named sibling components.

## States

For reusable UI, cover:

- Default.
- Hover.
- Focus/focus-visible.
- Active/current.
- Disabled.
- Loading.
- Empty/error when relevant.
- Long text and narrow viewport behavior.

## Optimistic mutations

User-initiated mutations are **optimistic by default**: update the UI immediately, reconcile with the server response, and roll back on error. Never gate an ordinary CRUD or toggle action behind a spinner/disabled state that waits for a network round-trip — that latency is what optimism removes. (A disabled control while waiting is treating the symptom; remove the wait instead.)

**When the data lives in an SWR cache**, use the bound `mutate` from the hook with a promise plus options:

```js
const { data: labels, mutate } = useLabels();

// create
await mutate(
  client
    .createLabel({ name, color })
    .then((created) => [...(labels || []), created]),
  {
    optimisticData: [...(labels || []), { id: `temp-${name}`, name, color }],
    rollbackOnError: true,
    populateCache: true, // resolved promise value becomes the cache
    revalidate: false, // server returned the canonical list
  },
).catch((error) =>
  showSnackbar({ message: getApiErrorMessage(error), variant: "error" }),
);
```

Same shape for update (`map` the changed item) and delete (`filter` it out). Set `revalidate: true` instead when the server response is not authoritative enough to trust.

**When the data lives in local `useState`** (e.g. a value seeded from a parent payload and owned by the component), capture the previous value, apply the optimistic value, fire the request, reconcile with its response, and restore the previous value on error:

```js
const previous = labels;
onChange([...labels, label]); // optimistic
try {
  onChange(await client.attachLabel(bundleId, label.id)); // reconcile
} catch (error) {
  onChange(previous); // rollback
  showSnackbar({ message: getApiErrorMessage(error), variant: "error" });
}
```

**Surface errors with a snackbar** (and the rollback), not an inline blocking state, so the optimistic close/update can happen immediately. Keep client-side validation (e.g. required fields) inline and pre-network.

**Exceptions — do not fake these optimistically:**

- **Long-running jobs** (uploads, async processing, anything that enqueues work) — these report real progress via polling. Show the actual job/progress state, not an optimistic result.
- **Create-then-navigate** flows where success routes elsewhere (e.g. creating a record and pushing to its page) — there is no list to optimistically update in place.
- **Mutations whose resulting state isn't known client-side** — e.g. restoring an item from a list reseeds content the client doesn't hold. Close the dialog promptly and reconcile via revalidation rather than faking the post-restore content.
- Anything where a wrong optimistic guess would mislead the user about a destructive or irreversible outcome.

## Product Surface Guidance

- SaaS and operational tools should feel quiet, direct, and work-focused.
- Avoid marketing-only decoration on product screens.
- Use clear controls: buttons for commands, selects for option sets, chips for compact labels, dialogs for blocking choices, and tabs for parallel views.
- Do not use visible in-app text to explain obvious UI mechanics.
