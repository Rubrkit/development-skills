# Responsive And Visual QA

Use this when rendered output changes.

## Browser Smoke Test

- Start the local dev server if the app needs it.
- Load the changed route in a browser.
- Check desktop and at least one mobile viewport. Add tablet/wide checks for complex layouts.
- Confirm no horizontal page overflow:
  `document.documentElement.scrollWidth <= document.documentElement.clientWidth + 1`
- Confirm the main interaction works, especially forms, dialogs, menus, and client state updates.
- Check browser console logs when behavior looks wrong.

## Breakpoint Checklist

Prefer these viewports when time allows:

- 320px narrow mobile.
- 375/390px common mobile.
- 768px tablet.
- 1024px small desktop/tablet landscape.
- 1280px desktop.
- 1440px/wide desktop when hero/media areas are prominent.

## Layout And Typography

- No horizontal overflow, clipping, or incoherent overlap.
- Containers have sensible max widths.
- Multi-column layouts stack in readable order.
- Sticky headers or CTAs do not cover content.
- Text wraps safely for long words, URLs, plan names, and button labels.
- Heading sizes remain proportional without viewport-width font scaling.
- Button and chip labels fit or wrap intentionally.

## Interaction And Accessibility

- Touch targets are at least 44x44px where practical.
- Focus states remain visible.
- Forms are usable on mobile keyboards.
- Hover-only affordances have mobile equivalents.
- Dialogs and menus can close by expected actions.
- Exactly one primary `h1` on public pages.
- Use meaningful link text and accessible button names.

## Automated Checks

- Run `npm run typecheck`, `npm run lint`, and `npm run build`.
- If an accessibility tool such as pa11y or axe is already installed, run it for changed public routes. Do not add heavy QA dependencies for a small change unless the user asks.
- Treat serious/critical accessibility violations as blockers unless documented as false positives.
