# SEO, AEO, And Public Route Basics

Use this for public routes and marketing pages.

## Metadata

- Each public page should have a unique, specific title and useful description.
- Canonical URLs should use `NEXT_PUBLIC_SITE_URL` or a single shared base URL.
- Open Graph data should match visible page content.
- Do not put private, user-specific, or draft content in metadata.

## Semantic HTML

- Use exactly one primary `h1`.
- Keep headings in logical order.
- Use `header`, `nav`, `main`, `section`, and `footer` where appropriate.
- Use `article` for editorial/detail content.
- Link text should describe the destination or action.

## Structured Data

- Add JSON-LD only when it matches visible, real page content.
- Prefer shared builders if schema grows beyond a tiny static object.
- Valid schema candidates may include `WebSite`, `Organization`, `SoftwareApplication`, `FAQPage`, or `BreadcrumbList`, but only when supported by visible content.
- Do not invent ratings, pricing, claims, addresses, team details, or FAQs.

## Sitemap And Robots

- Keep `app/sitemap.js` deterministic.
- Include only public, indexable routes.
- Keep `public/robots.txt` aligned with the canonical site URL.
- Exclude future private dashboard/account routes unless explicitly public.

## Answer And Generative Engine Content

- Public pages should state what the product or page does directly near the top.
- Prefer specific, source-like claims over vague marketing filler.
- Structure steps, features, examples, and pricing as lists or tables when useful.
- Keep product terminology consistent across headings, metadata, nav, and body copy.
