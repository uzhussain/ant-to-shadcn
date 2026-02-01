---
title: Avoid component and page sprawl
impact: HIGH
impactDescription: Keep a single source of truth for core UI and routes
tags: gotchas, components, naming, duplication
---

## Avoid component and page sprawl

During migration, duplicate “custom” variants create inconsistent UX and confusion. Enforce a single canonical shadcn component per primitive and clean route naming.

### Practices
- Replace Ant components outright; never wrap Ant components.
- For core primitives (Button, Sheet, Dialog, Tabs, Input, Card), keep exactly one canonical component import (shadcn).
- Centralize shared styling in `cva` variants or `className` utilities; allow page-level tweaks via `className`/composition instead of new components.
- If a custom component encodes domain logic (e.g., `BillingPlanCard`), keep it; if it is just a restyled primitive (`NewButton`, `CustomSheet`), consolidate into the canonical component.

### Detection
- Search for `NewButton`, `CustomButton`, `ButtonV2`, `NewPage`, `PageV2`, duplicate `*Sheet*`/`*Dialog*` components.
- Check usage counts and imports to find the live component.

### Resolution
- Identify the live version by usage in routes/nav/tests.
- Move unique styling into variants on the canonical component.
- Remove or rename duplicates; update imports.

### Why this matters
- Reduces UI drift and styling conflicts.
- Makes testing and maintenance reliable.

### Gotchas
- If a placeholder page like `NewPage` exists, confirm which route is live and remove/rename the stale one.
- Keep data endpoints and props flexible when swapping Ant → shadcn; do not change API contracts.
