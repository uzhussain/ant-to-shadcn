---
name: ant-to-shadcn-migration
description: "Migrate Ant Design UIs to shadcn/ui + Tailwind (React/CRA/Vite → Next.js App Router). Use for: component replacement (no wrappers), Tailwind-first styling, canonical component consolidation, global CSS cleanup, route/layout scaffolding, and migration gotchas."
metadata:
  version: "1.0.0"
  category: "migration"
---

# Ant Design to shadcn/ui Migration Guide

## When to Apply

- Migrating a CRA/Vite React app that uses Ant Design to Next.js App Router with shadcn/ui.
- Replacing Ant Design components with shadcn/ui (and Radix) while moving styling to Tailwind and design tokens.
- Cleaning up legacy global CSS/LESS, z-index stacks, and inline styles that conflict with Tailwind/shadcn.
- Generating fresh layout/page skeletons per route (optionally role-aware) to reduce legacy cruft.

## Version Policy

Use the latest stable versions of:
- `next` ≥ 16.x (App Router)
- `react` ≥ 19.x
- `tailwindcss` ≥ 3.4.x
- `shadcn/ui` latest
- `@radix-ui` packages as pulled by shadcn/ui
- `class-variance-authority`, `tailwind-merge`, `lucide-react`, `sonner`
- `react-hook-form` ≥ 7.x and `zod` ≥ 3.x for forms (recommended)

## Rule Categories (by priority)

Apply rules from `rules/` in this order: `setup-*` (critical) → `theme-*` (critical) → `layout-*` → `forms-*` → `data-*` → `overlay-*` → `content-*` → `css-*` → `a11y-*` → `test-*` → `gotchas-*`

## Component Equivalence (Quick Reference)

**Direct replacements** (1:1 shadcn component exists):
`Button`, `Breadcrumb`, `Tabs`, `Pagination`, `Checkbox`, `Select`, `Slider`, `Switch`, `Avatar`, `Badge`, `Card`, `Carousel`, `Calendar`, `Accordion` (Collapse), `Popover`, `Tooltip`, `Alert`, `Progress`, `Skeleton`, `Separator` (Divider), `Sheet` (Drawer), `Dialog`/`AlertDialog` (Modal/Popconfirm)

**Pattern-based replacements** (compose from shadcn + Tailwind):

| Ant Design             | shadcn/ui + Tailwind pattern                                |
| ---------------------- | ----------------------------------------------------------- |
| Icon                   | `lucide-react` (drop `@ant-design/icons`)                   |
| Layout/Space/Flex/Grid | Tailwind flex/grid utilities; `Resizable` for splitters     |
| Menu/Dropdown          | `NavigationMenu`, `DropdownMenu`, or `Sheet` nav            |
| Form/Form.Item         | `Form` (react-hook-form + zod) + shadcn field components    |
| DatePicker/TimePicker  | `Calendar` + `Popover` + time input                         |
| AutoComplete/Cascader  | `Combobox`/`Command` with grouped options                   |
| Table                  | TanStack Table + shadcn `Table`                             |
| Message/Notification   | `sonner` toasts                                             |
| Upload                 | File input + dropzone + progress bar                        |
| Steps/Timeline         | Custom flex/vertical list with Tailwind                     |
| ConfigProvider/App     | Not needed — use Tailwind theme tokens + CSS vars           |
| Pro* (ProTable, etc.)  | Compose: TanStack Table + filters + forms + cards           |

## Migration Examples (Before → After)

### Form.Item → shadcn Form + react-hook-form + zod

```tsx
// BEFORE (Ant Design)
<Form onFinish={onSubmit}>
  <Form.Item name="email" label="Email" rules={[{ required: true, type: 'email' }]}>
    <Input />
  </Form.Item>
  <Button type="primary" htmlType="submit">Submit</Button>
</Form>

// AFTER (shadcn/ui + react-hook-form + zod)
const schema = z.object({ email: z.string().email() });
const form = useForm<z.infer<typeof schema>>({ resolver: zodResolver(schema) });

<Form {...form}>
  <form onSubmit={form.handleSubmit(onSubmit)}>
    <FormField control={form.control} name="email" render={({ field }) => (
      <FormItem>
        <FormLabel>Email</FormLabel>
        <FormControl><Input {...field} /></FormControl>
        <FormMessage />
      </FormItem>
    )} />
    <Button type="submit">Submit</Button>
  </form>
</Form>
```

### Modal.confirm → AlertDialog

```tsx
// BEFORE (Ant Design)
Modal.confirm({
  title: 'Delete item?',
  content: 'This action cannot be undone.',
  onOk: () => handleDelete(id),
});

// AFTER (shadcn/ui)
<AlertDialog>
  <AlertDialogTrigger asChild><Button variant="destructive">Delete</Button></AlertDialogTrigger>
  <AlertDialogContent>
    <AlertDialogHeader>
      <AlertDialogTitle>Delete item?</AlertDialogTitle>
      <AlertDialogDescription>This action cannot be undone.</AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>Cancel</AlertDialogCancel>
      <AlertDialogAction onClick={() => handleDelete(id)}>Delete</AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogContent>
</AlertDialog>
```

### message.success → sonner toast

```tsx
// BEFORE (Ant Design)
import { message } from 'antd';
message.success('Record saved successfully');
message.error('Failed to save record');

// AFTER (sonner)
import { toast } from 'sonner';
toast.success('Record saved successfully');
toast.error('Failed to save record');
```

## Migration Order

0) Verify/upgrade Ant to latest 6.x before mapping (align with [Ant components](https://ant.design/components/overview/)).
1) Setup: Tailwind/shadcn install, deps, project structure, shadcn MCP server ready.
2) CSS Cleanup: Identify global/LESS/inlined styles; decide what to keep; add base resets and z-index tokens.
3) Theming: Map Ant tokens → CSS vars/Tailwind theme; dark mode guardrails; spacing/radius/typography scales.
4) Layout/Nav (Responsive): Rebuild layouts/nav/breadcrumbs with desktop/tablet/mobile variants; role-aware scaffolding; drawer-on-mobile.
5) Forms: Convert `Form.Item` patterns to shadcn + react-hook-form + zod; validation and submission states; success/failure/inline errors.
6) Data Display: Tables/lists/empty states; pagination/sorting/filtering patterns; loading/skeleton/error states.
7) Overlays/Feedback: Dialogs/sheets/popovers/tooltips/toasts/alerts/spinners with z-index scale.
8) Content/Icons: Replace Ant icons/typography with lucide and Tailwind text styles; see `content-icons-typography`.
9) Accessibility/Interop: Focus traps, keyboard nav, ARIA, responsive checks; safe coexistence during phased rollout; see `a11y-responsive-checks`, `a11y-motion-contrast`, `responsive-touch`, `rtl-l10n`.
10) Testing/Verification: Visual checks, a11y scans, bundle/size linting, API/error-path checks; prefer modern UX fallbacks over 1:1 Ant copies when overlays/patterns conflict (`gotchas-ux-fallbacks`).
11) Performance/Bundle: verify Ant CSS/icons absent; use lucide; virtualize large tables/lists to avoid jank (`perf-bundle-guard`).
12) Observability: keep analytics/error logging when refactoring flows; don’t drop events when swapping notifications with toasts (`analytics-observability`).
13) Scroll/State: guard useEffect/init to avoid state resets; use stable handlers and shadcn ScrollArea for section navigation (`gotchas-scroll-state`).

## Pre-Migration Scan

Run these commands to inventory Ant usage and route findings to the appropriate rule files in `rules/`:

```bash
# Core Ant usage → setup-*, theme-*, css-* rules
npm ls antd
rg "antd|@ant-design/icons|ConfigProvider|theme\.useToken" src app

# Imperative APIs → forms-*, overlay-*, gotchas-ux-fallbacks rules
rg "Modal\.confirm|notification\.|message\." src app

# Styles/assets → css-legacy-inventory, css-asset-hygiene, fonts-setup rules
rg --files -g"*.less" src app
rg "global\.css|font-family|\.dark" src app

# Routing/auth → layout-*, gotchas-coexistence rules
rg "RequireAuth|useAuth|Clerk|Supabase|<Router" src app
```

## Post-Migration Verification (UI-first)

- Shell/nav: responsive desktop/tablet/mobile; active states; breadcrumbs; role-guarded routes.
- Forms: validation + error/success toasts; disabled states; loading indicators; keyboard focus order; screen reader labels.
- Tables/lists: empty/loading/error/skeleton states; pagination/sort/filter; row focus/hover.
- Overlays/toasts: focus trap, ESC/close, return focus; z-index layering; no console errors on open/close.
- Theming: light/dark parity; token application (colors/spacing/radius); contrast and focus outlines.
- CSS sanity: no leaking global styles; no unexpected z-index overlaps; Ant CSS isolated or removed; legacy UI parked separately.
- Performance: bundle size reduced (no Ant CSS), tree-shaking verified.

## Rules Reference

All detailed migration rules are in `rules/` — each file is named by its prefix (e.g., `rules/setup-antd-upgrade.md`, `rules/forms-rhf-shadcn.md`). See the Rule Categories section above for application order.

## Key Principles

- **No wrappers**: Replace Ant components directly with shadcn equivalents; don't wrap Ant in compatibility layers.
- **shadcn-first sourcing**: Install via `npx shadcn@latest` or shadcn MCP; don't hand-roll if a registry component exists.
- **Side-by-side migration**: Keep Ant only where not yet migrated; scaffold fresh App Router layouts per route.
- **Reuse vs scaffold**: Reuse existing structure if routes/styles are centralized and scoped. Fresh scaffold if Ant CSS bleeds globally or routing is unclear. Run scans first to decide.
