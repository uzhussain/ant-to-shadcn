---
title: Migrate TreeSelect and Cascader
impact: MEDIUM
impactDescription: Replace Ant tree/cascader inputs with shadcn patterns
tags: components, select, tree, cascader
---

## Migrate TreeSelect and Cascader

Use grouped options or simple tree data with shadcn Combobox/Command; maintain keyboard/a11y and clear selection behavior.

### Pattern

```tsx
// Flattened options with grouping
const options = [
  { label: 'Europe', options: [{ label: 'France', value: 'fr' }] },
  { label: 'Americas', options: [{ label: 'USA', value: 'us' }] },
]

<Popover>
  <PopoverTrigger asChild>
    <Button variant="outline" role="combobox" className="justify-between">
      {value ? labelFor(value) : 'Select'}
    </Button>
  </PopoverTrigger>
  <PopoverContent className="p-0">
    <Command>
      <CommandInput placeholder="Search..." />
      <CommandList>
        {options.map(group => (
          <CommandGroup key={group.label} heading={group.label}>
            {group.options.map(opt => (
              <CommandItem key={opt.value} onSelect={() => onChange(opt.value)}>
                {opt.label}
              </CommandItem>
            ))}
          </CommandGroup>
        ))}
      </CommandList>
    </Command>
  </PopoverContent>
</Popover>
```

### Tree specifics
- For nested levels, either flatten with headings or show indented labels (`pl-*`).
- If multi-select is needed, add checkboxes inside `CommandItem`.

### Why this matters

- Avoids heavy Ant TreeSelect/Cascader JS while keeping searchable selection.
- Preserves keyboard navigation and aria via shadcn Command/Popover.

### Gotchas

- Keep selection visible; close on select unless multi-select.
- For large trees, consider virtualized list or async search. 
