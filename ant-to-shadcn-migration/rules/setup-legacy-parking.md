---
title: Park legacy Ant UI safely
impact: CRITICAL
impactDescription: Preserve old UI while building new shadcn layouts
tags: setup, migration-strategy, legacy
---

## Park legacy Ant UI safely

Keep legacy Ant Design code intact while generating new shadcn/Tailwind layouts and pages. Do not delete legacy code; move it into a scoped folder to avoid style bleed and keep a reference.

### Steps

1) Move legacy UI into a parking folder:
- Create `legacy-design/` at the repo root.
- Move old Ant-first layouts/pages/components there (or copy if needed for reference).
- Keep entry points runnable (optional) for diffing behavior.

2) Prevent style bleed:
- Ensure Ant CSS is only imported within `legacy-design/` entry points.
- Do not import Ant global CSS in the new App Router tree.

3) Wire new App Router:
- Generate fresh `app/` layouts/pages using shadcn components and Tailwind.
- Point primary routes to new pages; keep legacy routes behind a toggle if needed (e.g., `/legacy/*`).

4) Document the switch:
- Add README notes on how to run the legacy shell vs new shell.
- Mark cutover steps for final removal once parity is confirmed.

### Why this matters

- Avoids accidental deletion of reference code.
- Stops Ant globals from affecting new shadcn styles.
- Allows side-by-side comparison during migration.

### Gotchas

- Watch build tooling: if Ant LESS build steps exist, keep them only for `legacy-design/` until removal.
- Do not import legacy components into new layouts; keep boundaries clean.
