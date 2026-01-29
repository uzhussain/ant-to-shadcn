---
title: Scroll areas and state reset gotchas
impact: HIGH
impactDescription: Prevent useEffect/state resets and scroll glitches after migration
tags: gotchas, scroll, state, useeffect
---

## Scroll areas and state reset gotchas

Legacy Ant scroll areas or section steppers can break after migration when effects reset state or scroll positions.

### Patterns to avoid
- `useEffect` that runs on every render because dependencies change (new function refs), resetting config/state and wiping scroll.
- Imperative scroll tied to stale refs or missing guards, causing flicker or wrong positions.
- Mixed Ant scroll containers with new layouts causing z-index/overflow clashes.

### Fixes
- Stabilize effects: wrap callbacks in `useCallback`; guard initialization with a ref/flag to avoid re-running initial setState.
- Preserve scroll: use `ScrollArea` (shadcn) or a single overflow container; store/restore scroll position when needed.
- Imperative scroll: use `element.scrollIntoView({ behavior: 'smooth', block: 'start' })` on known refs after layout; avoid chaining setState + scroll in the same tick without guarding.
- Ensure only one scroll container owns scrolling; remove nested overflow that fights the root container.

### Quick checklist
- Does an effect reset state on dependency changes? Add an `initialized` ref or move to a stable data source.
- Are scroll handlers referencing stale elements? Update refs and guard against null.
- Is Ant scroll styling still applied? Replace with `ScrollArea` and scoped styles.

### Why this matters

- Prevents “state snaps back” and scroll-jank when users navigate sections or forms.

### Gotchas

- Avoid coupling scroll to changing function identities; memoize handlers.
- When mixing legacy and new UI, ensure only one overlay/scroll layer sets `overflow`. 
