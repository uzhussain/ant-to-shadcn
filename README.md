## Ant Design → shadcn/ui Migration Skill

Guidance and rule set to migrate Ant Design UIs (CRA/Vite/React) to shadcn/ui + Tailwind on Next.js App Router. Focuses on component mapping, token/theming (light/dark guardrails), responsive shells, auth/role-aware scaffolding, forms/tables/overlays, legacy CSS isolation, and shadcn-first installs (CLI/MCP only).

### What’s inside
- `SKILL.md` — overview, categories, migration order, scan→rule mapping.
- `rules/` — focused rules (setup, tokens, responsive layout, auth discovery, legacy parking, CSS cleanup, assets/fonts, forms, tables/components, overlays, icons/typography, dark-mode guardrails, performance, a11y/responsive/RTL, testing, gotchas including scroll/state and UX fallbacks, MCP setup).

### When to use
- You have an Ant Design-based app (CRA/Vite/React) moving to Next.js App Router with shadcn/ui + Tailwind.
- You need clean, responsive shells, while preserving auth/role flows and endpoint behavior.
- You want to run Ant and shadcn side-by-side during a phased migration without style bleed.

### Quick start (conceptual)
1) **Upgrade Ant to 6.x**: `npm install antd@latest`; fix warnings first.
2) **Baseline setup**: Tailwind + shadcn install, enable shadcn MCP server for component discovery/installs.
3) **Inventory**: scan `antd` imports/version, `ConfigProvider/theme.useToken`, global CSS/LESS, inline styles, overflow/scroll handlers, routes/guards/roles, public assets/fonts, analytics/logging.
4) **Park legacy UI**: move old Ant UI to `legacy-design/`; scope Ant CSS there.
5) **Tokens**: map Ant tokens → CSS vars/Tailwind theme; decide light-only vs dark opt-in; set brand colors/radius/shadows centrally.
6) **Layouts**: rebuild responsive shells (marketing vs authenticated shells), nav/breadcrumbs, role guards; add loading/error/not-found.
7) **Components**: swap forms to shadcn + RHF/zod, replace selects/date/upload, dialogs/toasts, tables via tanstack, icons via lucide; install via shadcn CLI/MCP (no hand-rolled copies if registry exists).
8) **Assets**: organize `public/` assets, fix paths, avoid container conflicts; set fonts explicitly.
9) **Coexistence**: keep Ant scoped; avoid mixed forms; align z-index/portals; use modern UX fallbacks when legacy patterns clash.
10) **Performance/A11y**: virtualize large tables/lists; honor reduced motion and contrast; ensure touch targets and mobile behavior; check RTL/locales if applicable.
11) **Verification**: compare new vs legacy for data/endpoint parity, auth/roles, loading/empty/error states, console noise, a11y/visual checks; keep analytics/logging intact.

### Key rule highlights
- **Setup**: Ant 6.x upgrade, shadcn install, MCP enablement, legacy parking, Tailwind config.  
- **Theming**: token bridge, dark-mode guardrails, CSS vars for brand colors/radius/shadows.  
- **Layout**: responsive shell, route scaffold, auth/role discovery, nav/drawer/breadcrumb patterns.  
- **Forms**: RHF + zod, API loading/success/failure, select/date/upload replacements.  
- **Data/Components**: tanstack table pattern, loading/empty/error/pagination/sort/filter states; pagination/result/empty; tabs/steps; tree/cascader; upload with progress.  
- **Overlays**: dialogs/sheets/toasts/alerts; console-noise prevention; UX fallbacks (modal→sheet, toast>notification).  
- **Styling**: CSS/LESS inventory, z-index scale, asset hygiene, icon/typography swap; fonts setup.  
- **Performance**: bundle guard (no Ant CSS/icons), virtualization guidance.  
- **A11y/Responsive/RTL**: reduced motion/contrast/focus, touch ergonomics, RTL/l10n checks.  
- **Gotchas**: coexistence boundaries, scroll/state reset, modern UX fallbacks.  
- **Testing/Observability**: visual/a11y/e2e smoke; keep analytics/error logging.

### MCP (shadcn) setup
- Configure `.cursor/mcp.json` (Cursor) to run `npx shadcn@latest mcp`.
- Ensure `components.json` lists registries if using custom namespaces.
- Use MCP to browse/install shadcn components instead of manual copy/paste. Install via shadcn CLI/MCP; avoid hand-rolled copies when registry components/blocks exist.

### Light/Dark mode guidance
- Light-only by default; keep dark tokens ready.  
- If enabling dark: toggle `<html>.dark`, ensure contrast for surfaces, borders, focus rings, icons, toasts/dialogs.  
- Remove stale Ant dark overrides; keep tokens centralized.

### Auth/roles safety
- Discover public vs protected routes, role mappings, and guards (Clerk/Supabase/cookies/hooks).  
- Reuse existing auth providers/guards; do not alter behavior.  
- Scaffold separate marketing vs app shells; role-aware nav/breadcrumbs.

### Assets and public folder
- Organize `public/` by brand/marketing/app; fix image paths; add favicons/manifest.  
- Use consistent containers (`relative`, `overflow-hidden`, `rounded-*`, `object-cover`) to avoid layout conflicts.

### Parity checks before cutover
- New shadcn pages match legacy endpoints/data, auth/roles, and UI states (loading/empty/error/success).  
- No console errors/unhandled rejections; overlays focus/ESC/return-focus verified; scroll/state stable.  
- Bundle: Ant CSS/icon bundle removed from new shell; legacy isolated; analytics/logging intact.

### License
MIT.
