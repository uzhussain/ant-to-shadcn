---
title: Build shadcn data tables with TanStack
impact: HIGH
impactDescription: Match Ant Table features with shadcn + tanstack table
tags: data, table, tanstack, pagination, sorting, filtering
---

## Build shadcn data tables with TanStack

Replace Ant `Table` with tanstack table + shadcn UI to cover sorting, filtering, pagination, selection, and state handling (loading/empty/error).

### Steps

1) Install deps:
```bash
npm install @tanstack/react-table
```

2) Define columns/config in one place; avoid inline table definitions.

3) State coverage:
- Loading → skeleton rows.
- Empty → empty state component.
- Error → inline alert with retry.
- Success → render table with controls.

4) Minimal pattern sketch:
```tsx
import { useReactTable, getCoreRowModel, getSortedRowModel, getPaginationRowModel } from '@tanstack/react-table'
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from '@/components/ui/table'

const table = useReactTable({
  data,
  columns,
  state: { sorting, pagination },
  onSortingChange: setSorting,
  onPaginationChange: setPagination,
  getCoreRowModel: getCoreRowModel(),
  getSortedRowModel: getSortedRowModel(),
  getPaginationRowModel: getPaginationRowModel(),
})

return (
  <div className="space-y-3">
    <Table>
      <TableHeader>
        {table.getHeaderGroups().map(hg => (
          <TableRow key={hg.id}>
            {hg.headers.map(header => (
              <TableHead key={header.id}>
                {header.isPlaceholder ? null : header.column.columnDef.header}
              </TableHead>
            ))}
          </TableRow>
        ))}
      </TableHeader>
      <TableBody>
        {table.getRowModel().rows.map(row => (
          <TableRow key={row.id}>
            {row.getVisibleCells().map(cell => (
              <TableCell key={cell.id}>{cell.renderValue()}</TableCell>
            ))}
          </TableRow>
        ))}
      </TableBody>
    </Table>
    {/* add pagination controls */}
  </div>
)
```

5) Sorting/filtering:
- Use column definitions with accessor keys; add sortable headers and filter inputs.
- For server-side data, wire sorting/filter/pagination to fetch params.

6) Selection (if needed):
- Add a checkbox column; manage selected row IDs; align with a11y (aria-checked).

7) Replace Ant features:
- `Table` pagination → shadcn pagination component bound to tanstack state.
- `columns` renderers → column cell render functions.
- Sticky headers/scroll → wrap in `ScrollArea`.

### Why this matters

- Delivers Ant-like functionality with modern, tree-shakeable pieces.
- Keeps state explicit and testable.

### Gotchas

- Ensure header cells are buttons for sortable columns with focus styles.
- Test narrow viewports; consider responsive stacking for simple tables.
