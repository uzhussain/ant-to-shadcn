---
title: E2E and visual regression smoke
impact: LOW
impactDescription: Catch auth/nav/form/dialog/table regressions early
tags: testing, e2e, visual
---

## E2E and visual regression smoke

Add lightweight Playwright (or similar) smoke tests plus optional visual snapshots to catch regressions in auth, navigation, forms, dialogs, and tables after migration.

### Smoke checklist (suggested)
- Auth: login flow succeeds; protected route redirects when unauthenticated.
- Nav: primary nav links work; mobile drawer opens/closes and navigates.
- Forms: happy-path submit shows success toast; invalid submit shows inline errors.
- Dialogs/Sheets: open/close, ESC, focus trap, return focus to trigger.
- Tables: pagination/sort/filter controls respond; empty/loading/error states render without console errors.
- Console: no errors/unhandled rejections during flows.

### Visuals (optional)
- Capture desktop and mobile snapshots for key pages (dashboard, form, table, modal open).
- Compare against baseline; allow small diffs, flag large layout shifts.

### Implementation hints
- Keep tests short and resilient: target roles/labels, not brittle selectors.
- Gate console errors: fail tests on console.error/UnhandledRejection.
- Use test users/fixtures; avoid real external calls (mock or seed).

### Why this matters

- Ensures migrated shadcn UI didn’t break critical flows that Ant previously handled.

### Gotchas

- Handle timezones/dates deterministically in tests.
- Debounce network/mock delays to avoid flaky assertions. 
