---
title: Asset and public folder hygiene
impact: MEDIUM
impactDescription: Ensure images/assets map correctly and avoid container conflicts
tags: assets, public, images, layout
---

## Asset and public folder hygiene

Clean up and map assets (public folder, images) to the new shadcn layouts without container conflicts.

### Steps

1) Inventory assets:
- List files under `public/` and image imports in the app.
- Note brand assets (logos, favicons), hero images, and per-feature images.

2) Organize:
- Group by usage: `public/brand/`, `public/marketing/`, `public/app/`.
- Ensure favicons/manifest are present and linked.

3) Update references:
- Replace Ant-era import paths with correct public paths or static imports.
- Avoid conflicting nested containers; wrap images in consistent sections (`relative`, `overflow-hidden`, `rounded-*`, `object-cover`).

4) Optimize layout usage:
- Use `max-w-*` and `mx-auto` for marketing content; avoid arbitrary width inline styles.
- For grids, prefer Tailwind grid/flex utilities instead of deeply nested containers.

### Why this matters

- Prevents broken asset paths after restructuring.
- Reduces layout conflicts caused by ad-hoc wrappers.

### Gotchas

- Check dark/light logo variants.
- Large hero images: ensure proper sizing and lazy-loading where appropriate.
