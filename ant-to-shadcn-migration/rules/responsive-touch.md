---
title: Responsive and touch ergonomics
impact: MEDIUM
impactDescription: Ensure mobile/tablet usability and touch targets
tags: responsive, mobile, touch
---

## Responsive and touch ergonomics

Keep the migrated UI comfortable on mobile/tablet with adequate spacing and touch targets.

### Checklist
- Touch targets: min ~40px height/width for buttons/controls; avoid tiny icons without padding.
- Navigation: drawer/sheet works on mobile; close on navigation; focusable and keyboard-friendly.
- Tables on mobile: allow horizontal scroll with clear affordance or stack key fields; keep pagination/filter controls reachable.
- Carousels/drag: don’t rely on hover; ensure swipe works or provide arrows/buttons.
- Spacing/density: use consistent `gap/space` utilities; avoid cramped inline controls on mobile.

### Gotchas
- Avoid nested scroll areas that trap touch scroll.
- Ensure sticky headers/footers don’t block content on small screens. 
