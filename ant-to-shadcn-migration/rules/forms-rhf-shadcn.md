---
title: Migrate Ant Form/Form.Item to shadcn + react-hook-form
impact: HIGH
impactDescription: Modern forms with validation, success/failure states, and a11y
tags: forms, validation, rhf, zod
---

## Migrate Ant Form/Form.Item to shadcn + react-hook-form

Replace Ant’s `Form`/`Form.Item` with shadcn form components powered by react-hook-form and zod. Cover loading, success/failure toasts, inline errors, and keyboard accessibility.

### Steps

1) Install recommended deps:
```bash
npm install react-hook-form zod @hookform/resolvers
```

2) Define schema and form:
```tsx
import { z } from 'zod'
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { Form, FormField, FormItem, FormLabel, FormControl, FormMessage } from '@/components/ui/form'
import { Input } from '@/components/ui/input'
import { Button } from '@/components/ui/button'
import { toast } from 'sonner'

const schema = z.object({
  email: z.string().email(),
  name: z.string().min(1),
})

export function ProfileForm() {
  const form = useForm({
    resolver: zodResolver(schema),
    defaultValues: { email: '', name: '' },
  })

  const onSubmit = async (values: z.infer<typeof schema>) => {
    try {
      // await api.updateProfile(values)
      toast.success('Profile updated')
    } catch (err) {
      toast.error('Update failed', { description: 'Try again' })
    }
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Email</FormLabel>
              <FormControl><Input type="email" {...field} /></FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="name"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Name</FormLabel>
              <FormControl><Input {...field} /></FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <div className="flex items-center gap-2">
          <Button type="submit" disabled={form.formState.isSubmitting}>Save</Button>
          {form.formState.isSubmitting && <span className="text-sm text-muted-foreground">Saving…</span>}
        </div>
      </form>
    </Form>
  )
}
```

3) Error/success handling:
- Use Sonner toasts for success/failure; keep inline `FormMessage` for field errors.
- Disable submit while loading; show a lightweight inline spinner or text.
- Handle API errors and unexpected failures; log to console only for debugging.

4) Required/validation indicators:
- Use `FormMessage` instead of manual text.
- Include `aria-invalid` via shadcn inputs automatically; ensure labels are present.

5) Replace Ant-specific patterns:
- `Form.Item help`, `validateStatus` → `FormMessage` + toasts for global errors.
- `onFinish`/`onFinishFailed` → `handleSubmit` with try/catch.

### Why this matters

- Removes Ant form reliance; provides modern validation and better a11y defaults.
- Standardizes success/failure UX with toasts plus inline field errors.

### Gotchas

- Keep schema close to the form; avoid drifting validation rules.
- For file uploads, date pickers, selects/combobox, add specialized shadcn components but keep the same pattern (`FormField` + controlled component).
