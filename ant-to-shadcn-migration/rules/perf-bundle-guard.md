---
title: Performance and bundle guard
impact: HIGH
impactDescription: Keep Ant out of the new bundle and avoid jank on large data sets
tags: performance, bundle, virtualization
---

## Performance and bundle guard

Verify the new shadcn shell stays lean and responsive, and that legacy Ant code isn’t sneaking into builds.

### Checklist
- Bundle hygiene: run build/bundle analyzer; confirm Ant CSS/icons absent; search for `antd/dist` and `@ant-design/icons` imports and remove from new shell.
- Icons: prefer named lucide imports; avoid wildcard/icon packs that bloat bundle.
- Data-heavy lists/tables/tree: add virtualization (e.g., `react-virtual`/`react-virtualized`) when row counts are large; avoid rendering hundreds of DOM nodes.
- Suspense/loaders: use skeletons or inline loading; avoid full-page spinners.
- Images: ensure explicit sizes/aspect to prevent CLS; lazy load below the fold.
- Mobile perf: test on mid-tier devices; watch for scroll jank on tables and drawers.

### Gotchas
- Dynamic imports of legacy modules can reintroduce Ant CSS; check code-split chunks.
- Avoid running heavy work on the client when it can stay on the server (App Router defaults). 
