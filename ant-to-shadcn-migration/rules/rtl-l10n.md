---
title: RTL and localization checks
impact: LOW
impactDescription: Prevent regressions for RTL or localized layouts
tags: rtl, l10n, i18n
---

## RTL and localization checks

If the app supports RTL or multiple locales, validate layouts and navigation after migration.

### Checklist
- Enable RTL toggle and verify nav, tabs, drawers/sheets, and tables (scroll direction, alignments).
- Avoid hardcoded paddings/margins that assume LTR; use logical properties where possible.
- Date/number formats: ensure components respect locale formatting.
- Breadcrumbs and pagination: confirm order and icons are appropriate in RTL.

### Gotchas
- Overflow/scroll areas can behave differently in RTL; test table scroll and drawers.
- Iconography that implies direction may need mirroring for RTL. 
