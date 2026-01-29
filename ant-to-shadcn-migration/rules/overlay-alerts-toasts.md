---
title: Replace Ant alerts, modals, notifications, and spinners
impact: HIGH
impactDescription: Align feedback/overlay patterns with shadcn/Radix and Sonner
tags: overlays, alerts, toasts, modals, drawers, spinners
---

## Replace Ant alerts, modals, notifications, and spinners

Map Ant feedback components to shadcn/Radix primitives with a11y defaults and predictable z-index layering.

### Patterns

- `Alert` → `Alert` component with variant classes; ensure role and icon.
- `Modal`, `Modal.confirm` → `Dialog` (or `AlertDialog` for confirms); include focus trap and return focus.
- `Drawer` → `Sheet`.
- `Popover`, `Tooltip` → Radix `Popover`, `Tooltip`.
- `notification`, `message` → `Sonner` (`toast`) with variants and durations.
- `Spin` → `Skeleton` or minimal spinner utility; avoid blocking overlays when possible.

### Example: Modal.confirm → AlertDialog

```tsx
// Before (Ant)
Modal.confirm({
  title: 'Delete user?',
  content: 'This cannot be undone.',
  onOk: () => deleteUser(id),
})
```

```tsx
// After (shadcn)
import { AlertDialog, AlertDialogTrigger, AlertDialogContent, AlertDialogHeader, AlertDialogTitle, AlertDialogDescription, AlertDialogFooter, AlertDialogCancel, AlertDialogAction } from '@/components/ui/alert-dialog'

export function DeleteUser({ onConfirm }: { onConfirm: () => Promise<void> }) {
  return (
    <AlertDialog>
      <AlertDialogTrigger className="text-destructive">Delete</AlertDialogTrigger>
      <AlertDialogContent>
        <AlertDialogHeader>
          <AlertDialogTitle>Delete user?</AlertDialogTitle>
          <AlertDialogDescription>This cannot be undone.</AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel>Cancel</AlertDialogCancel>
          <AlertDialogAction onClick={onConfirm}>Delete</AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogContent>
    </AlertDialog>
  )
}
```

### Example: notification → Sonner toast

```tsx
// Before (Ant)
import { notification } from 'antd'
notification.success({ message: 'Saved', description: 'Profile updated' })
```

```tsx
// After (Sonner)
import { toast } from 'sonner'
toast.success('Saved', { description: 'Profile updated' })
```

### Z-index guidance

- Define a z-index scale (e.g., tooltip 50, popover 55, toast 60, modal 70, drawer 70).
- Ensure portals render near `body`; avoid legacy `z-9999` globals.

### Why this matters

- Restores a11y defaults (focus trap, ESC, aria roles).
- Replaces imperative APIs (`Modal.confirm`, `notification`) with composable components or toast calls.
- Simplifies styling with Tailwind variants instead of Ant CSS.

### Gotchas

- Remove Ant CSS imports once overlays are migrated; avoid double-styling.
- Check dark mode for alerts/toasts; ensure contrast and icon colors match tokens.

### Before → After (Modal.confirm and notification)

```tsx
// Before (Ant)
Modal.confirm({
  title: 'Delete user?',
  content: 'This cannot be undone.',
  onOk: () => deleteUser(id),
})
notification.success({ message: 'Saved', description: 'Profile updated' })
```

```tsx
// After (shadcn + sonner)
import { AlertDialog, AlertDialogTrigger, AlertDialogContent, AlertDialogHeader, AlertDialogTitle, AlertDialogDescription, AlertDialogFooter, AlertDialogCancel, AlertDialogAction } from '@/components/ui/alert-dialog'
import { toast } from 'sonner'

export function DeleteUser({ onConfirm }: { onConfirm: () => Promise<void> }) {
  return (
    <AlertDialog>
      <AlertDialogTrigger className="text-destructive">Delete</AlertDialogTrigger>
      <AlertDialogContent>
        <AlertDialogHeader>
          <AlertDialogTitle>Delete user?</AlertDialogTitle>
          <AlertDialogDescription>This cannot be undone.</AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel>Cancel</AlertDialogCancel>
          <AlertDialogAction onClick={onConfirm}>Delete</AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogContent>
    </AlertDialog>
  )
}

toast.success('Saved', { description: 'Profile updated' })
```
