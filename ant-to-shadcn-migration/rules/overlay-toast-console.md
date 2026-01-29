---
title: Prevent console noise and align toasts with API flows
impact: HIGH
impactDescription: Clean feedback UX and no console errors during interactions
tags: overlays, toasts, console, errors
---

## Prevent console noise and align toasts with API flows

Replace Ant `notification`/`message` and imperative modals with toasts and dialogs that don’t generate console warnings. Ensure API failures are caught and surfaced to users, not the console.

### Steps

1) Wrap all async UI actions in try/catch; surface errors with toast + optional inline text.

2) Centralize toast helpers:
```ts
export const notify = {
  success: (title: string, description?: string) => toast.success(title, { description }),
  error: (title: string, description?: string) => toast.error(title, { description }),
  info: (title: string, description?: string) => toast(title, { description }),
}
```

3) Console hygiene:
- No unhandled promise rejections.
- Gate debug logs to development only.
- Avoid React act/warning spam from imperative Ant APIs; prefer component-based dialogs.

4) Submission flows:
- Disable buttons while loading.
- On success: toast + optionally optimistic UI update.
- On failure: toast + inline message; keep retry button where relevant.

5) Alert/confirmation flows:
- Prefer `AlertDialog` for confirms; ensures focus trap and aria.
- Remove `Modal.confirm` usage.

### Why this matters

- Users see meaningful feedback; engineers see fewer noisy console warnings.
- Reduces flakes in tests caused by unhandled rejections.

### Gotchas

- Avoid stacking multiple toasts for the same failure; coalesce.
- Check dark mode contrast for toast background/foreground.
