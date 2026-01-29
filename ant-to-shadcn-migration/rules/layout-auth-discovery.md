---
title: Discover routing, auth, and entry flows before migration
impact: HIGH
impactDescription: Preserve public/auth flows and roles while rebuilding shells
tags: layout, routing, auth, roles, discovery
---

## Discover routing, auth, and entry flows before migration

Inventory how users enter the app, which routes are public vs authenticated, and how roles/guards are enforced. Preserve behavior while rebuilding shadcn layouts; do not change auth.

### Steps

1) Map routes and entry points:
- Locate router config (React Router, custom config) and menu definitions.
- Identify landing/marketing pages vs authenticated app shell.
- Record public routes, protected routes, and role-gated paths.

2) Identify auth/guard mechanisms:
- Clerk/Supabase/auth hooks, HOCs (`RequireAuth`), cookie checks, `useEffect` redirects.
- Note where `redirect`/`navigate` logic lives; keep it intact.

3) Capture role metadata:
- Collect roles/permissions objects, menu-role mappings, guard functions.
- Plan to feed this into nav/breadcrumb generation and RoleGuard wrappers.

4) Plan dual shells:
- Marketing shell (public) separate from app shell (auth).
- Ensure new shadcn layouts mirror existing entry flow; reuse auth providers.

5) During migration:
- Do not change auth logic; wrap new pages with existing providers/guards.
- Keep old routes available under `legacy-design/` if needed for reference.

### Why this matters

- Prevents breaking login/role flows during a style migration.
- Ensures nav/breadcrumbs reflect role-based access.

### Gotchas

- `useEffect` redirects can mask missing server guards; document and preserve.
- Hash-based or query-param based flows (magic links, invites) must remain intact.
