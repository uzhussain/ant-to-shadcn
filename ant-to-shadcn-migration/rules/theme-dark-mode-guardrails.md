---
title: Dark mode guardrails and theme cleanup
impact: CRITICAL
impactDescription: Prevent light/dark drift; keep tokens clean and optional
tags: theme, dark-mode, tokens
---

## Dark mode guardrails and theme cleanup

Keep light/dark support sane, optional, and token-driven. Remove stale dark-mode fragments from legacy Ant code while preserving a clean path to enable dark mode in shadcn.

### Steps

1) Decide support level:
- If dark mode is NOT required now: keep `.dark` token definitions ready, but ship with light only; remove stale toggles and CSS that partially force dark.
- If dark mode IS required: ensure toggler sets `.dark` on `<html>`; verify all key surfaces (background, foreground, border, focus) have dark tokens.

2) Inventory legacy dark code:
- Search for `dark:` classes, `.dark` selectors, theme toggles, and Ant token overrides for dark.
- Remove/disable legacy Ant dark overrides that bleed into new layouts.

3) Centralize tokens:
- Define light/dark CSS vars in `globals.css` (colors, radius, shadow, focus ring).
- Extend Tailwind to use vars (already set in theme-bridge-tokens). Keep primary/secondary/neutral scales consistent.

4) Component checks:
- Buttons, inputs, cards, nav, table header/body, toasts/dialogs: verify contrast in dark.
- Icons and borders: ensure `text-muted-foreground`/`border-border` use vars that differ by theme.

5) Toggle pattern (if enabled):
- Client-only theme switch that sets class on `<html>`; persist in localStorage; no flicker (use `data-theme`/`suppressHydrationWarning` pattern if needed).

6) Comments for users:
- Document where to change primary/secondary brand colors and surface backgrounds.
- Point to vars for quick brand updates without touching components.

### Why this matters

- Prevents broken mixed-theme remnants from Ant.
- Keeps a clear opt-in path to dark mode using shadcn defaults.

### Gotchas

- Avoid mixing Ant dark tokens with shadcn vars—pick one system.
- Check focus rings in dark mode; they often disappear if not tokenized.
