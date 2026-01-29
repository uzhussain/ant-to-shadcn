---
title: Preserve analytics and error logging
impact: MEDIUM
impactDescription: Keep observability intact when swapping UI flows
tags: analytics, logging, observability
---

## Preserve analytics and error logging

When refactoring UI flows (dialogs, toasts, drawers, forms), ensure existing analytics/events and error logging remain intact.

### Checklist
- Carry over event emits/tracking from Ant flows to new shadcn components (e.g., submit, open/close, success/failure).
- Keep error logging for failed API calls; don’t drop logs when replacing notifications with toasts.
- If altering flow (modal → drawer), map events to the new triggers so dashboards remain accurate.

### Gotchas
- Silent failures can increase if toasts replace inline errors without logging—ensure both user feedback and logs. 
