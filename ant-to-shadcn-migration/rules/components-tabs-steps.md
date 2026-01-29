---
title: Migrate Tabs and Steps
impact: MEDIUM
impactDescription: Replace Ant Tabs/Steps with shadcn primitives and keep a11y
tags: components, tabs, steps
---

## Migrate Tabs and Steps

Replace Ant `Tabs`/`Steps` with shadcn `Tabs` and simple flex-based Steps while preserving keyboard and aria behavior.

### Tabs

```tsx
import { Tabs, TabsList, TabsTrigger, TabsContent } from '@/components/ui/tabs'

export function SettingsTabs() {
  return (
    <Tabs defaultValue="profile">
      <TabsList>
        <TabsTrigger value="profile">Profile</TabsTrigger>
        <TabsTrigger value="billing">Billing</TabsTrigger>
      </TabsList>
      <TabsContent value="profile">...</TabsContent>
      <TabsContent value="billing">...</TabsContent>
    </Tabs>
  )
}
```

Notes:
- Use stable `value` keys; keep content mounted if needed (controlled state).
- Ensure focus styles are visible; avoid custom JS for navigation.

### Steps

Ant `Steps` → simple flex row/column with icons/numbers.

```tsx
const steps = [
  { label: 'Account', status: 'done' },
  { label: 'Profile', status: 'current' },
  { label: 'Confirm', status: 'next' },
]

export function StepsInline() {
  return (
    <ol className="flex flex-col gap-2 md:flex-row md:gap-4">
      {steps.map((step, idx) => (
        <li key={step.label} className="flex items-center gap-2">
          <span className="flex h-7 w-7 items-center justify-center rounded-full border text-sm font-medium">
            {idx + 1}
          </span>
          <span className="text-sm">{step.label}</span>
        </li>
      ))}
    </ol>
  )
}
```

### Why this matters

- Replaces Ant components with lightweight, accessible primitives.
- Avoids extra JS for tab/step state management.

### Gotchas

- Keep tab content focusable; ensure arrow key behavior if customizing.
- For vertical tabs/steps, adjust flex direction and add aria orientation. 
