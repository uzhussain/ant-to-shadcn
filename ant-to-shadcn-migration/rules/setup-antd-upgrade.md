---
title: Upgrade Ant Design to latest 6.x before migration
impact: CRITICAL
impactDescription: Align components/token model with current Ant docs for accurate mapping
tags: setup, antd, upgrade, prerequisites
---

## Upgrade Ant Design to latest 6.x before migration

Ensure the codebase is on current Ant Design 6.x so component APIs and tokens match the official docs, making mappings to shadcn/ui accurate.

### Steps

1) Check current version:
```bash
npm ls antd
```
If missing or < 6.x, plan an upgrade.

2) Upgrade:
```bash
npm install antd@latest
```

3) Verify peer deps:
- React 18+ (prefer 19).
- dayjs/date-fns if used by DatePicker/TimePicker.

4) Run codemods if applicable (Ant provides codemods for major upgrades; review release notes).

5) Align with docs:
- Compare usage against latest components list: https://ant.design/components/overview/
- Note any deprecated components (e.g., old List patterns) to replace with current equivalents before mapping.

6) Re-run the app and fix any breaking changes before starting shadcn migration.

### Why this matters

- Component props and token names differ across major versions; upgrading avoids mismatches when mapping to shadcn.
- Reduces surprises when replacing components or tokens.

### Gotchas

- Icons: ensure `@ant-design/icons` version matches `antd`.
- LESS variables may change; recompile and note overrides before removal.
- Fix warnings after upgrade so migration starts from a clean baseline.
