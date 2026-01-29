---
title: Result and Empty states
impact: MEDIUM
impactDescription: Replace Ant Result/Empty with consistent empty/success/error patterns
tags: components, empty, result, states
---

## Result and Empty states

Replace Ant `Result`/`Empty` with simple, consistent empty/success/error blocks using shadcn primitives.

### Empty
```tsx
export function EmptyState({ title, action }: { title: string; action?: React.ReactNode }) {
  return (
    <div className="flex flex-col items-center justify-center gap-2 py-10 text-center">
      <p className="text-sm text-muted-foreground">{title}</p>
      {action}
    </div>
  )
}
```

### Success / Error (Result analogue)
```tsx
import { Alert, AlertDescription, AlertTitle } from '@/components/ui/alert'
import { CheckCircle, TriangleAlert } from 'lucide-react'

export function SuccessBlock({ title, description }: { title: string; description?: string }) {
  return (
    <Alert>
      <CheckCircle className="h-4 w-4" />
      <AlertTitle>{title}</AlertTitle>
      {description && <AlertDescription>{description}</AlertDescription>}
    </Alert>
  )
}

export function ErrorBlock({ title, description, action }: { title: string; description?: string; action?: React.ReactNode }) {
  return (
    <Alert variant="destructive">
      <TriangleAlert className="h-4 w-4" />
      <AlertTitle>{title}</AlertTitle>
      {description && <AlertDescription>{description}</AlertDescription>}
      {action}
    </Alert>
  )
}
```

### Why this matters

- Replaces Ant-specific Result/Empty components with lightweight, consistent patterns.
- Encourages actionable empty/error states with CTA or retry.

### Gotchas

- Keep icons sized (`h-4 w-4`) and aligned.
- Provide clear CTA for empty/error when applicable. 
