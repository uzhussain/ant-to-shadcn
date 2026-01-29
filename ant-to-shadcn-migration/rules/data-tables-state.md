---
title: Migrate tables/lists with loading, empty, and error states
impact: HIGH
impactDescription: Preserve UX while replacing Ant Table/List with shadcn patterns
tags: data, tables, lists, states
---

## Migrate tables/lists with loading, empty, and error states

Replace Ant `Table`/`List` with shadcn patterns (often tanstack table + shadcn UI) and enforce full state coverage: loading, empty, error, and interactive controls (pagination/sort/filter).

### Steps

1) Choose a table pattern (e.g., tanstack table + shadcn UI components). Keep columns/config in a single place.

2) State handling:
- `loading`: show `Skeleton` rows or spinner with aria-live.
- `empty`: render an empty state with icon/CTA.
- `error`: show inline alert + retry action.
- `success`: support pagination, sort, filter; keep controls accessible.

3) Example skeleton:
```tsx
// pseudo-pattern
if (isLoading) return <TableSkeleton />
if (error) return <Alert variant="destructive">Failed to load <Button onClick={refetch}>Retry</Button></Alert>
if (!rows.length) return <EmptyState title="No data" action={<Button onClick={onCreate}>Create</Button>} />
return <DataTable columns={columns} data={rows} />
```

4) Empty state pattern:
```tsx
function EmptyState({ title, action }) {
  return (
    <div className="flex flex-col items-center justify-center gap-2 py-10 text-center">
      <p className="text-sm text-muted-foreground">{title}</p>
      {action}
    </div>
  )
}
```

5) Pagination/sort/filter:
- Wire table state to URL search params when needed for shareable state.
- Keep controls keyboard-accessible; ensure focus order is logical.

6) Replace Ant features:
- `Table` pagination → shadcn pagination component + server/client handlers.
- `List` item layout → Tailwind flex/grid + `Card`/`Separator`.
- `Descriptions` → definition list pattern with `flex`/`grid`.
- `Tag`/`Badge` → shadcn `Badge`.

### Why this matters

- Maintains UX coverage users expect from Ant out of the box.
- Prevents regressions on empty/error/loading paths.

### Gotchas

- Ensure column headers remain clickable with clear focus styles.
- Test with long text, narrow viewports, and keyboard-only navigation.
