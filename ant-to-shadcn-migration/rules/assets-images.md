---
title: Images and responsive sizing
impact: MEDIUM
impactDescription: Prevent broken assets and layout jank after migration
tags: assets, images, responsive
---

## Images and responsive sizing

Ensure images referenced in Ant-era code map cleanly to the new shadcn/Tailwind layouts with correct paths, sizes, and loading behavior.

### Steps

1) Inventory images:
- List `public/` assets and imported images in components/pages.
- Note hero/marketing vs app imagery; identify light/dark variants if present.

2) Fix paths:
- Replace Ant-era relative paths with correct `public/` references or static imports.
- Keep favicons/manifest in place; update links if app structure changed.

3) Responsive usage:
- Wrap images with consistent containers: `relative overflow-hidden rounded-*` and `object-cover`.
- Constrain widths: use `max-w-*`, `mx-auto` for marketing content; grids for galleries/cards.
- Provide explicit width/height or aspect ratios to avoid layout shift.

4) Loading states:
- Use `Skeleton` or placeholder blocks where images load slowly.
- For large heroes, consider lazy loading if below the fold.

5) Background images:
- Prefer utility classes or inline `style` with CSS vars for dynamic URLs; avoid deep nested containers that fight layout.

### Why this matters

- Prevents broken asset paths and maintains responsive, stable layouts.

### Gotchas

- Check dark/light logo variants.
- Avoid stretching icons/logos; keep aspect ratio intact.
