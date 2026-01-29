---
title: Map Ant Design tokens to Tailwind & CSS vars
impact: CRITICAL
impactDescription: Preserve visual parity while removing Ant token dependency
tags: theme, tokens, tailwind, css-vars, dark-mode
---

## Map Ant Design tokens to Tailwind & CSS vars

Translate Ant Design theming (`ConfigProvider`, `theme.useToken`) to Tailwind theme extensions and CSS variables to keep parity and enable dark mode.

### Steps

1) Inventory Ant tokens:
- Search for `ConfigProvider`, `theme.useToken`, `token` props, and inline styles using Ant variables.
- Note primary/secondary colors, border radius, spacing, typography scale, and component-specific overrides.

2) Define CSS variables in `app/globals.css` (root + dark):
```css
:root {
  --color-primary: 0 112 243;
  --color-success: 34 197 94;
  --color-warning: 234 179 8;
  --color-danger: 239 68 68;
  --radius: 0.5rem;
  --shadow-md: 0 10px 15px -3px rgb(0 0 0 / 0.1);
}
.dark {
  --color-primary: 96 165 250;
  --color-success: 74 222 128;
  --color-warning: 250 204 21;
  --color-danger: 248 113 113;
  --radius: 0.5rem;
  --shadow-md: 0 10px 15px -3px rgb(0 0 0 / 0.35);
}
```

3) Extend Tailwind theme to use vars:
```js
// tailwind.config.js
const { fontFamily } = require('tailwindcss/defaultTheme')

module.exports = {
  content: ['./app/**/*.{js,ts,jsx,tsx,mdx}', './components/**/*.{js,ts,jsx,tsx,mdx}'],
  theme: {
    extend: {
      colors: {
        primary: {
          DEFAULT: 'rgb(var(--color-primary) / <alpha-value>)',
        },
        success: 'rgb(var(--color-success) / <alpha-value>)',
        warning: 'rgb(var(--color-warning) / <alpha-value>)',
        danger: 'rgb(var(--color-danger) / <alpha-value>)',
      },
      borderRadius: {
        lg: 'var(--radius)',
        md: 'calc(var(--radius) - 2px)',
        sm: 'calc(var(--radius) - 4px)',
      },
      boxShadow: {
        md: 'var(--shadow-md)',
      },
      fontFamily: {
        sans: ['Inter', ...fontFamily.sans],
      },
    },
  },
  plugins: [],
}
```

4) Replace Ant token usage:
- `ConfigProvider` theme tokens → remove and rely on CSS vars/Tailwind classes.
- Inline styles with Ant tokens → map to Tailwind classes using new color/radius variables.
- Component-specific overrides → prefer utility classes or minimal CSS modules using the vars.

5) Dark mode:
- Use `class` strategy (`.dark`) and ensure toggling adds/removes the class at `<html>` level.
- Validate contrast for primary, background, borders, and focus outlines.

### Before → After example (button)

```tsx
// Before (Ant)
<ConfigProvider theme={{ token: { colorPrimary: '#1677ff', borderRadius: 6 } }}>
  <Button type="primary">Save</Button>
</ConfigProvider>
```

```tsx
// After (shadcn)
import { Button } from '@/components/ui/button'

export function SaveButton() {
  return <Button className="bg-primary text-white rounded-lg">Save</Button>
}
```

### Why this matters

- Eliminates Ant token dependency, keeps visual parity via vars.
- Centralizes light/dark and spacing/radius in one place.
- Tailwind classes stay expressive while still honoring design tokens.

### Gotchas

- Ensure global CSS vars load before any component CSS; place in `globals.css`.
- Watch z-index stacks: define z-index scale (modals, popovers, toasts) to prevent conflicts with legacy styles.
- If Ant and shadcn coexist temporarily, scope legacy overrides to `.ant-` classes to avoid leaking onto shadcn components.
