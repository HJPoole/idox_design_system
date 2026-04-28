# Idox Design System

Enterprise design system for Idox Software products — built for public sector and local government applications including planning, licensing, and case management.

## Authoritative files

| File | Purpose |
|---|---|
| [component-examples.html](component-examples.html) | Canonical component library — reference for all UI patterns and markup |
| [page-example.html](page-example.html) | Reference page implementation demonstrating full layout composition |

## Documentation

| File | Purpose |
|---|---|
| [design.md](design.md) | Token values, component specs, and UI rules |
| [rationale.md](rationale.md) | Design decision log — explains why rules exist and what constraints they impose |

## Stack

- Tailwind CSS v4 (browser CDN)
- Tailwind Elements (`@tailwindplus/elements`)
- Font Awesome (icon library — `fa-light` as default weight)
- DM Sans (Google Fonts — the sole typeface)

## Key conventions

- Use `@theme inline` in page HTML to expose tokens as Tailwind utility classes.
- Never use arbitrary value syntax (`bg-[#4F46E5]`). Raise as a token gap.
- Sentence case for all UI text. See [design.md](design.md) for writing guidelines.
