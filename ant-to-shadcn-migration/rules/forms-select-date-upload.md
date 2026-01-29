---
title: Migrate Ant Select/DatePicker/Upload
impact: HIGH
impactDescription: Replace complex Ant form controls with shadcn primitives
tags: forms, select, date, upload, combobox
---

## Migrate Ant Select/DatePicker/Upload

Replace Ant `Select`/`TreeSelect`/`Cascader`, `DatePicker`/`TimePicker`, and `Upload` with shadcn primitives (combobox, calendar + popover, file input/dropzone) wired to react-hook-form.

### Select / Combobox

Use shadcn Combobox or Select for searchable lists; keep aria labels and keyboard support.

```tsx
// Combobox with RHF
<FormField
  control={form.control}
  name="country"
  render={({ field }) => (
    <FormItem>
      <FormLabel>Country</FormLabel>
      <FormControl>
        <Popover>
          <PopoverTrigger asChild>
            <Button variant="outline" role="combobox" className="justify-between">
              {field.value ?? 'Select country'}
            </Button>
          </PopoverTrigger>
          <PopoverContent className="p-0">
            <Command>
              <CommandInput placeholder="Search..." />
              <CommandList>
                {countries.map(c => (
                  <CommandItem
                    key={c.code}
                    onSelect={() => field.onChange(c.code)}
                  >
                    {c.name}
                  </CommandItem>
                ))}
              </CommandList>
            </Command>
          </PopoverContent>
        </Popover>
      </FormControl>
      <FormMessage />
    </FormItem>
  )}
/>
```

### Date/Time

Use shadcn `Calendar` inside a `Popover`; pair with time input if needed.

```tsx
<FormField
  control={form.control}
  name="date"
  render={({ field }) => (
    <FormItem className="flex flex-col">
      <FormLabel>Date</FormLabel>
      <Popover>
        <PopoverTrigger asChild>
          <Button variant="outline" className="justify-start">
            {field.value ? field.value.toLocaleDateString() : 'Pick a date'}
          </Button>
        </PopoverTrigger>
        <PopoverContent className="p-0">
          <Calendar
            mode="single"
            selected={field.value}
            onSelect={field.onChange}
            initialFocus
          />
        </PopoverContent>
      </Popover>
      <FormMessage />
    </FormItem>
  )}
/>
```

### Upload

Use file input + dropzone pattern; keep accessibility and size limits explicit.

```tsx
<FormField
  control={form.control}
  name="files"
  render={({ field }) => (
    <FormItem>
      <FormLabel>Attachments</FormLabel>
      <FormControl>
        <div className="border border-dashed rounded-lg p-4 text-sm text-muted-foreground">
          <input
            type="file"
            multiple
            onChange={(e) => field.onChange(e.target.files)}
            aria-label="Upload files"
          />
          <p className="mt-2 text-xs text-muted-foreground">Max 10MB each</p>
        </div>
      </FormControl>
      <FormMessage />
    </FormItem>
  )}
/>
```

### Why this matters

- Eliminates Ant-specific form controls while retaining search, date picking, and uploads.
- Keeps keyboard/a11y behavior through shadcn + Radix primitives.

### Gotchas

- Ensure Combobox/Select closes on selection and supports keyboard navigation.
- For dates, handle time zones consistently; store ISO strings.
- For uploads, enforce size/type validation in the form and server.
