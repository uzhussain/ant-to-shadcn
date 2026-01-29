---
title: Handle API loading, success, and failure states
impact: HIGH
impactDescription: Prevent console errors and surface clear UX on submissions
tags: forms, api, toasts, errors, loading
---

## Handle API loading, success, and failure states

Ensure form submissions and mutations present clear loading, success, and error states without console noise. Replace Ant `message`/`notification` usage with Sonner toasts and inline feedback.

### Steps

1) Wrap API calls with try/catch:
```tsx
const onSubmit = form.handleSubmit(async (values) => {
  try {
    await api.save(values)
    toast.success('Saved', { description: 'Changes updated' })
  } catch (err) {
    toast.error('Save failed', { description: 'Try again' })
    form.setError('root', { message: 'Could not save. Check your data.' })
  }
})
```

2) Loading/disabled states:
- Disable submit while loading: `disabled={form.formState.isSubmitting}`.
- Show inline loading text or spinner near the submit button.

3) Inline and global errors:
- Use `FormMessage` for field errors and `form.setError('root', ...)` for form-level issues.
- Keep toast copies concise; avoid duplicate error text.

4) Empty/error/loading states for data fetch:
- Use skeletons for initial load.
- Show actionable empty states (CTA to create data).
- Provide retry buttons for errors; avoid silent failures.

5) Console hygiene:
- Avoid unhandled promise rejections; always `catch`.
- Log detailed errors to monitoring, not console (or gate behind `process.env.NODE_ENV === 'development'`).

### Why this matters

- Matches modern UX expectations (clear status, retryable errors).
- Prevents noisy console errors during submissions.
- Reduces regressions when replacing Ant notifications with toasts.

### Gotchas

- Keep toast durations reasonable; avoid stacking during rapid submissions.
- For critical flows, also show inline error text near the affected fields.
