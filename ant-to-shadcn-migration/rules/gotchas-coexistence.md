---
title: Coexistence: Ant and shadcn side-by-side
impact: HIGH
impactDescription: Prevent style bleed and runtime conflicts during phased migration
tags: gotchas, coexistence, css, icons
---

## Coexistence: Ant and shadcn side-by-side

During phased migration, keep Ant and shadcn from interfering with each other.

### Practices

- Scope Ant CSS: import Ant styles only inside legacy routes or the `legacy-design/` shell; avoid importing into the new App Router tree.
- Container scoping: wrap legacy screens with `.ant-app` and scope overrides to that container.
- Z-index and portals: ensure modals/sheets/toasts don’t collide—define a shared z-index scale and test overlays.
- Icons: drop Ant icon font when not needed; if both exist, ensure only one set is loaded per shell.
- CSS order: load Tailwind base + globals first, then shadcn component styles; keep Ant CSS last and scoped.
- Packages: avoid mixing Ant form components with shadcn form fields in the same form; migrate a form as a unit.

### Gotchas

- Hydration/classname mismatches if Ant CSS loads in server-rendered shadcn routes—keep imports out of new layouts.
- Global resets from Ant can override shadcn focus/outline styles—scope or remove.
- `Modal.confirm` can create stray portals; replace with `AlertDialog`.
