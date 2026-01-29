---
title: Pagination patterns
impact: MEDIUM
impactDescription: Replace Ant pagination with shadcn components and URL/state wiring
tags: components, pagination, data
---

## Pagination patterns

Replace Ant `Pagination` with shadcn pagination controls wired to table/list state (client or server).

### Pattern (server-aware)

```tsx
import { Pagination, PaginationContent, PaginationItem, PaginationLink, PaginationNext, PaginationPrevious } from '@/components/ui/pagination'

export function Pager({ page, pageCount, onPageChange }: { page: number; pageCount: number; onPageChange: (p: number) => void }) {
  return (
    <Pagination>
      <PaginationContent>
        <PaginationItem>
          <PaginationPrevious href="#" onClick={(e) => { e.preventDefault(); onPageChange(Math.max(1, page - 1)) }} />
        </PaginationItem>
        {Array.from({ length: pageCount }).map((_, i) => (
          <PaginationItem key={i}>
            <PaginationLink href="#" isActive={i + 1 === page} onClick={(e) => { e.preventDefault(); onPageChange(i + 1) }}>
              {i + 1}
            </PaginationLink>
          </PaginationItem>
        ))}
        <PaginationItem>
          <PaginationNext href="#" onClick={(e) => { e.preventDefault(); onPageChange(Math.min(pageCount, page + 1)) }} />
        </PaginationItem>
      </PaginationContent>
    </Pagination>
  )
}
```

### Wiring tips
- For server data, sync page to search params; trigger fetch on change.
- For client data, update table state directly (tanstack pagination state).
- Keep `aria-label` on controls; ensure focus styles remain.

### Why this matters

- Provides Ant-like pagination UX without Ant dependencies.

### Gotchas

- Avoid rendering huge page lists; add ellipses for large counts.
- Keep page bounds clamped to [1, pageCount]. 
