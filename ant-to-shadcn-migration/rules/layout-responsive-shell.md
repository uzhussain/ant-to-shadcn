---
title: Build responsive shells (desktop/tablet/mobile)
impact: HIGH
impactDescription: Ensure modern, responsive layouts replace Ant Layout/Sider patterns
tags: layout, responsive, navigation, shell
---

## Build responsive shells (desktop/tablet/mobile)

Replace Ant Layout/Sider/Header with shadcn + Tailwind shells that adapt to desktop/tablet/mobile, including responsive navigation (menu vs drawer) and consistent spacing/typography.

### Steps

1) Define breakpoints and spacing scale (Tailwind defaults are fine; ensure tokens for gaps/padding).

2) Shell pattern:
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

3) Responsive nav (menu → drawer on mobile):
```tsx
// components/nav.tsx
'use client'
import Link from 'next/link'
import { usePathname } from 'next/navigation'
import { Sheet, SheetContent, SheetTrigger } from '@/components/ui/sheet'
import { Button } from '@/components/ui/button'
import { Menu } from 'lucide-react'
import { cn } from '@/lib/utils'

const links = [
  { href: '/dashboard', label: 'Dashboard' },
  { href: '/settings', label: 'Settings' },
]

function NavLinks({ onSelect }: { onSelect?: () => void }) {
  const pathname = usePathname()
  return (
    <>
      {links.map(link => (
        <Link
          key={link.href}
          href={link.href}
          className={cn(
            'rounded-md px-3 py-2 text-sm font-medium hover:bg-muted',
            pathname === link.href && 'bg-muted text-foreground'
          )}
          onClick={onSelect}
        >
          {link.label}
        </Link>
      ))}
    </>
  )
}

export function Nav() {
  return (
    <header className="flex items-center justify-between border-b px-4 py-2">
      <div className="text-base font-semibold">App</div>
      <nav className="hidden items-center gap-3 md:flex">
        <NavLinks />
      </nav>
      <Sheet>
        <SheetTrigger asChild className="md:hidden">
          <Button variant="outline" size="icon"><Menu className="h-5 w-5" /></Button>
        </SheetTrigger>
        <SheetContent side="left">
          <div className="flex flex-col gap-2 mt-4">
            <NavLinks onSelect={() => {}} />
          </div>
        </SheetContent>
      </Sheet>
    </header>
  )
}
```

4) Breadcrumbs: use segments; ensure truncation for deep paths on mobile.

5) Content area: use responsive grid/flex utilities; prefer `max-w-6xl` for readable content widths.

6) Spacing/typography: enforce consistent `space-y-*`, `gap-*`, and heading classes via Tailwind utilities; avoid inline styles.

### Why this matters

- Provides polished, responsive behavior out of the box (desktop/tablet/mobile).
- Replaces Ant Sider/Menu assumptions with modern shadcn primitives.
- Reduces reliance on legacy CSS and hardcoded widths/z-index.

### Gotchas

- Ensure drawer focus trap works; test ESC/overlay click.
- Keep nav link data in config to avoid duplication.
- Add `loading.tsx` and `error.tsx` to maintain UX parity during suspense/errors.
