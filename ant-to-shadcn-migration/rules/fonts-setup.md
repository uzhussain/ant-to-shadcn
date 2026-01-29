---
title: Fonts and typography setup
impact: MEDIUM
impactDescription: Align typography and preload strategy; drop Ant font assumptions
tags: fonts, typography, performance
---

## Fonts and typography setup

Replace Ant font assumptions with explicit font loading (local or hosted), set typography scales, and ensure preload where needed.

### Steps

1) Choose font source:
- Use `next/font` for Google or local fonts; prefer variable fonts when available.

2) Configure in `app/layout.tsx`:
```tsx
import { Inter } from 'next/font/google'
const inter = Inter({ subsets: ['latin'], display: 'swap' })

export const metadata = { title: 'App' }

export default function RootLayout({ children }) {
  return (
    <html lang=\"en\" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

3) Tailwind theme:
- Set `fontFamily.sans` to the chosen font.
- Define heading/body scales via utilities (e.g., `text-2xl font-semibold tracking-tight`).

4) Preload/FOIT:
- `next/font` handles preload automatically; keep `display: 'swap'`.

5) Remove Ant font expectations:
- Eliminate references to Ant-specific font stacks or CSS.

### Why this matters

- Ensures consistent typography and prevents layout shift from late-loading fonts.

### Gotchas

- If using multiple scripts, include subsets.
- Keep font choice centralized; avoid ad-hoc `font-family` overrides in components.
