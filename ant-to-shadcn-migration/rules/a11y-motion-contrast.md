---
title: Reduced motion, focus, and contrast checks
impact: HIGH
impactDescription: Maintain accessibility across themes and motion preferences
tags: accessibility, motion, contrast, focus
---

## Reduced motion, focus, and contrast checks

Ensure new shadcn UI respects reduced motion, preserves focus/keyboard behavior, and maintains contrast in light/dark.

### Checklist
- Reduced motion: honor `prefers-reduced-motion` (disable/shorten transitions, toasts/dialog slide-ins).
- Focus: visible focus rings on all interactive elements; verify drawers/dialogs return focus; tabs/combobox/date picker/tree all keyboard-operable.
- Contrast: check brand tokens for WCAG contrast in light/dark; icons and focus rings remain visible in dark mode.
- Hover-only affordances: provide focusable triggers and visible states for keyboard users.
- Skeletons/spinners: ensure theme-aware colors; don’t flash bright backgrounds in dark mode.

### Gotchas
- Custom brand colors can break contrast; adjust vars, not component internals.
- Animations on toasts/drawers can be jarring; gate with reduced motion. 
