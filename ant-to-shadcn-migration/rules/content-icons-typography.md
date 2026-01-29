---
title: Replace Ant icons and typography with shadcn patterns
impact: MEDIUM
impactDescription: Remove Ant icon bundle and align text styles
tags: content, icons, typography
---

## Replace Ant icons and typography with shadcn patterns

Drop `@ant-design/icons` and Ant Typography components; use `lucide-react` icons and Tailwind text utilities aligned with shadcn defaults.

### Steps

1) Install lucide icons:
```bash
npm install lucide-react
```

2) Replace icons:
- Find `@ant-design/icons` imports.
- Map to lucide equivalents (e.g., `CheckCircleOutlined` → `Check`, `WarningOutlined` → `TriangleAlert`, `InfoCircleOutlined` → `Info`).
- Wrap icons in consistent size classes: `className="h-4 w-4"`.

3) Typography:
- Replace `Typography.Title`, `Typography.Text`, `Typography.Paragraph` with semantic tags and Tailwind/shadcn classes (`text-2xl font-semibold tracking-tight`, `text-muted-foreground`).
- Use shadcn `Heading` patterns or utility classes; keep max width for readability.

4) Icon-only buttons:
- Ensure `aria-label` is present.
- Use `variant="ghost"`/`"outline"` and size classes for consistency.

### Why this matters

- Removes a large icon bundle and aligns visuals with shadcn/Vercel defaults.
- Simplifies typography to semantic HTML + utilities.

### Gotchas

- Verify dark mode contrast for icons and text.
- Do not mix Ant icon font with lucide to avoid duplicate assets.
