---
title: Visual and a11y verification
impact: LOW
impactDescription: Catch regressions after replacing Ant components
tags: testing, visual, accessibility
---

## Visual and a11y verification

Add quick checks to validate the migrated UI. Favor lightweight steps that catch console noise, missing states, and contrast issues.

### Steps

1) Run lint and type checks; ensure no unhandled promise rejections in console.

2) Spot visual checks:
- Compare key routes at desktop/mobile widths.
- Verify loading/empty/error states for forms and tables.
- Check overlays (dialogs/sheets/tooltips/toasts) open/close without console warnings.

3) A11y quick pass:
- Use browser devtools a11y checker or axe extension on key pages.
- Verify focus order and focus return on dialogs/sheets.
- Confirm aria labels on icon-only buttons and nav toggles.

4) Optional automation:
- Add simple Playwright smoke for nav, form submit happy-path, and dialog open/close.
- Add Percy/Loki snapshots if available.

5) Parity check (new vs legacy):
- Compare new shadcn pages against legacy `legacy-design/` for required data, endpoint usage, and key UI states.
- Ensure auth/role-protected routes still render or redirect as before.

### Why this matters

- Prevents regressions that Ant’s defaults previously masked.
- Catches console errors and missing states early.

### Gotchas

- Keep tests resilient to styling changes; assert presence/roles, not exact text unless necessary.
