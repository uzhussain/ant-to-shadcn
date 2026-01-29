---
title: Install shadcn/ui and Tailwind
impact: CRITICAL
impactDescription: Baseline dependencies and project wiring for migration
tags: setup, tailwind, shadcn, radix
---

## Install shadcn/ui and Tailwind

Set the baseline stack before replacing Ant components.

### Steps

1) Install deps:
```bash
npm install next@latest react@latest react-dom@latest
npm install tailwindcss@latest postcss@latest autoprefixer@latest
npm install @radix-ui/react-popover @radix-ui/react-tooltip @radix-ui/react-dialog @radix-ui/react-dropdown-menu
npm install class-variance-authority tailwind-merge lucide-react sonner
```

2) Init Tailwind:
```bash
npx tailwindcss init -p
```

3) Update `tailwind.config.js` content paths for App Router:
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './app/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: { extend: {} },
  plugins: [],
}
```

4) Add base styles `app/globals.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

5) Install shadcn/ui CLI and init:
```bash
npx shadcn-ui@latest init
```

6) Add core components (buttons, inputs, dialogs, toasts, nav primitives) as needed:
```bash
npx shadcn-ui@latest add button input textarea select checkbox radio-group switch dialog sheet popover tooltip dropdown-menu toast breadcrumb navigation-menu table badge alert skeleton card separator sonner
```

### Why this matters

- Ensures Tailwind scans App Router paths.
- Gets Radix primitives aligned with shadcn components for a11y.
- Sonner provides Ant-like notification ergonomics with modern styling.

### Gotchas

- If CSS modules or legacy global styles exist, ensure `globals.css` loads after resets but before component CSS to control specificity.
- Avoid mixing Ant and shadcn styles in the same component; isolate during transition. 
