---
title: Accessibility and responsive checks
impact: MEDIUM
impactDescription: Ensure shadcn replacements meet a11y and viewport requirements
tags: accessibility, responsive, testing
---

## Accessibility and responsive checks

Ensure migrated UI meets accessibility and responsive expectations across desktop/tablet/mobile. Focus on keyboard nav, focus management, aria roles, and viewport behavior.

### Checklist

- Keyboard: all interactive controls focusable; visible focus ring; logical order.
- Dialogs/sheets: focus trap, ESC close, return focus to trigger.
- Tooltips/popovers: triggered on hover/focus; dismiss on ESC/blur.
- Forms: labels associated, `aria-invalid` on error, inline errors near fields.
- Tables/lists: header focusable; row hover/focus styles; responsive stacking or horizontal scroll with aria labels.
- Nav: drawer on mobile; traps focus; close on navigation; active state visible.
- Breakpoints: verify at mobile, tablet, desktop; spacing/typography scale appropriately.
- Dark mode: contrast, focus ring visible, toasts/alerts legible.
- Screen readers: landmarks (main, nav), aria labels on icons, breadcrumbs readable.

### Why this matters

- Prevents regressions when replacing Ant’s built-in a11y defaults.
- Ensures modern responsive UX that matches shadcn/Vercel guidance.

### Gotchas

- Do not rely on hover-only affordances; ensure focusable triggers.
- Ensure reduced motion settings respected if animations are added.
