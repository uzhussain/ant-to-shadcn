---
title: Prefer modern UX fallbacks over 1:1 Ant copies
impact: HIGH
impactDescription: Resolve UX/conflicts by switching to better shadcn patterns
tags: gotchas, ux, overlays, tables, accordions
---

## Prefer modern UX fallbacks over 1:1 Ant copies

When Ant-era patterns cause z-index conflicts, duplicate overlays, or clunky UX, choose a modern shadcn approach instead of forcing a 1:1 recreation.

### Overlays and feedback
- Modal storms or z-index clashes → switch to `Sheet`/drawer for non-blocking contexts; reserve `Dialog`/`AlertDialog` for critical/confirm flows.
- `notification`/`message` → use `toast`/`sonner`; keep copies short, include action when useful.
- Loading spinners → prefer inline or `Skeleton`; avoid full-page blocking overlays unless truly necessary.

### Lists, accordions, and tables
- Deep nested `Collapse`/accordion stacks → flatten with `Accordion` or simple cards; avoid excessive nesting.
- Tables that overload modals for detail → use drawers or inline expansion; keep table pagination/sort/filter clear.

### Decision tips
- Ask: what data must be visible? Can it be inline or in a non-blocking surface?
- If z-index conflicts repeat, swap the pattern (e.g., modal → drawer, toast instead of inline notification pop).
- Use shadcn Blocks/MCP to pull updated patterns rather than cloning legacy UI.

### Why this matters

- Reduces layering bugs and improves clarity during migration.

### Gotchas

- Keep focus/aria behavior intact when changing patterns (drawer vs modal).
- Avoid mixing Ant overlays with shadcn ones in the same flow; migrate the flow as a unit.
