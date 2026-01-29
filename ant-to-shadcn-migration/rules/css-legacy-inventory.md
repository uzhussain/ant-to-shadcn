---
title: Inventory and isolate legacy CSS/LESS
impact: MEDIUM
impactDescription: Prevent legacy styles from breaking shadcn/Tailwind
tags: css, less, cleanup, z-index, specificity
---

## Inventory and isolate legacy CSS/LESS

Audit legacy global CSS/LESS and inline styles before introducing shadcn/Tailwind to avoid bleed-through and z-index conflicts.

### Steps

1) Find style sources:
- `*.css`, `*.scss`, `*.less` in `src/`, `styles/`, `public/`.
- Inline `style=` usage.
- `@import` chains and vendor CSS.

2) Classify:
- Keep: required brand tokens (colors, typography), print styles.
- Migrate: layout/spacing overrides, components that map to shadcn.
- Drop: unused resets, legacy grid, Ant theme overrides if not needed.

3) Isolation strategy:
- Scope legacy Ant overrides under `.ant-` prefixed containers where coexistence is required.
- Move necessary globals into `app/globals.css` after Tailwind base but before component-level styles.
- For pages still using Ant, lazy-load their CSS in route segments; do not import into root layout.

4) Z-index and layers:
- Define a z-index scale in `globals.css` or tokens (e.g., toast 40, tooltip 50, modal 60, drawer 70).
- Replace ad-hoc `z-9999` with tokenized classes.

5) Inline style cleanup:
- Replace inline sizes/margins/padding with Tailwind classes.
- For dynamic widths/heights, use style vars (e.g., `style={{ '--col-span': 3 }}` with `grid` utilities) sparingly.

### Before → After (legacy global)

```css
/* Before: global.css */
.ant-btn {
  border-radius: 2px !important;
}
.card {
  box-shadow: 0 0 10px rgba(0,0,0,0.2);
}
```

```css
/* After: scoped */
.ant-app .ant-btn { border-radius: 2px; }
:root { --shadow-card: 0 10px 15px -3px rgb(0 0 0 / 0.1); }
```

```tsx
// After: shadcn card
<Card className="shadow-[var(--shadow-card)]">...</Card>
```

### Why this matters

- Prevents legacy overrides from breaking shadcn styles and focus states.
- Clarifies what to delete vs migrate, reducing bundle size and conflicts.

### Gotchas

- Ant icon font: ensure it is removed when components are replaced; otherwise keep scoped to Ant routes.
- LESS variables: note them for token mapping; remove LESS build steps when Ant usage ends.

### Before → After (scoping legacy CSS)

```css
/* Before: global overrides leaking everywhere */
.ant-btn {
  border-radius: 2px !important;
}
.card {
  box-shadow: 0 0 10px rgba(0,0,0,0.2);
}
```

```css
/* After: scoped and tokenized */
.ant-app .ant-btn { border-radius: 2px; }
:root { --shadow-card: 0 10px 15px -3px rgb(0 0 0 / 0.1); }
```

```tsx
// After: shadcn card
<Card className="shadow-[var(--shadow-card)]">...</Card>
```
