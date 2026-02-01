---
title: Generate role-aware App Router layouts
impact: HIGH
impactDescription: Fresh skeletons reduce legacy coupling and clarify access control
tags: layout, routing, roles, navigation, breadcrumbs
---

## Generate role-aware App Router layouts

Create clean layout/page skeletons per route with shadcn primitives, mapping Ant Layout/Menu/Breadcrumb patterns to App Router files and role metadata.

### Steps

1) Discover routes/roles:
- Scan router config, menu definitions, guard HOCs/hooks (e.g., `RequireAuth`, `useAuth`), and permissions maps.
- Extract: path, title, breadcrumb labels, required roles, nav grouping (sections).
- Flag duplicate/placeholder routes (e.g., `NewPage`, `PageV2`) and identify the live path.

2) Scaffold App Router structure:
```
app/
  (app)/           // optional route group
    layout.tsx     // shell with nav + provider
    page.tsx       // dashboard/root
    settings/
      layout.tsx
      page.tsx
    users/
      [id]/page.tsx
    not-found.tsx
    error.tsx
    loading.tsx
```

Naming rules:
- Use consistent folder naming for routes (kebab-case).
- Avoid “New*” or “*V2” route folders; rename to canonical routes before wiring nav/tests.
- Ensure new routes are linked from nav/breadcrumbs and covered in smoke tests.

3) Layout shell with shadcn primitives:
```tsx
// app/(app)/layout.tsx
import { Nav } from '@/components/nav'
import { Breadcrumbs } from '@/components/breadcrumbs'

export default function AppLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="min-h-screen bg-background text-foreground">
      <Nav />
      <div className="px-6 py-4 space-y-4">
        <Breadcrumbs />
        <main className="">{children}</main>
      </div>
    </div>
  )
}
```

4) Nav from route metadata (replace Ant `Menu`/`Sider`):
```tsx
// components/nav.tsx
'use client'
import Link from 'next/link'
import { usePathname } from 'next/navigation'
import { cn } from '@/lib/utils'

const links = [
  { href: '/dashboard', label: 'Dashboard', roles: ['admin', 'user'] },
  { href: '/settings', label: 'Settings', roles: ['admin'] },
]

export function Nav() {
  const pathname = usePathname()
  return (
    <nav className="flex items-center gap-3 border-b px-4 py-2">
      {links.map(link => (
        <Link
          key={link.href}
          href={link.href}
          className={cn(
            'rounded-md px-3 py-2 text-sm font-medium hover:bg-muted',
            pathname === link.href && 'bg-muted text-foreground'
          )}
        >
          {link.label}
        </Link>
      ))}
    </nav>
  )
}
```

5) Breadcrumbs from segments:
```tsx
// components/breadcrumbs.tsx
'use client'
import { useSelectedLayoutSegments } from 'next/navigation'
import { Breadcrumb, BreadcrumbItem, BreadcrumbLink, BreadcrumbList, BreadcrumbSeparator } from '@/components/ui/breadcrumb'

export function Breadcrumbs() {
  const segments = useSelectedLayoutSegments()
  const parts = ['dashboard', ...segments]
  const hrefFor = (index: number) => '/' + parts.slice(0, index + 1).join('/')
  return (
    <Breadcrumb>
      <BreadcrumbList>
        {parts.map((part, i) => (
          <BreadcrumbItem key={part}>
            <BreadcrumbLink href={hrefFor(i)}>{part}</BreadcrumbLink>
            {i < parts.length - 1 && <BreadcrumbSeparator />}
          </BreadcrumbItem>
        ))}
      </BreadcrumbList>
    </Breadcrumb>
  )
}
```

6) Role guard (lightweight):
```tsx
// components/role-guard.tsx
'use client'
import { useUser } from '@/lib/auth'
import { redirect } from 'next/navigation'

export function RoleGuard({ children, roles }: { children: React.ReactNode; roles: string[] }) {
  const user = useUser()
  if (!user) redirect('/login')
  if (!roles.some(r => user.roles.includes(r))) redirect('/403')
  return <>{children}</>
}
```

Wrap pages that require roles, or apply at layout level.

### Why this matters

- Breaks free from Ant Layout/Menu assumptions; uses lightweight shadcn primitives.
- Centralizes nav/breadcrumbs and role checks for consistent UX.
- Generates clean skeletons to drop legacy CSS and inline styles.

### Gotchas

- Ensure `loading.tsx`, `error.tsx`, and `not-found.tsx` exist for UX parity.
- Keep nav data in config for testability; avoid hardcoding in multiple places.
- When coexisting with Ant, isolate Ant CSS; do not import Ant global CSS into the new layout tree.
