---
title: Build, deploy, and env notes
impact: LOW
impactDescription: Keep Next config/env/CDN settings aligned post-migration
tags: build, deploy, env, next-config
---

## Build, deploy, and env notes

Align Next.js config and environment handling after migrating from Ant/CRA.

### Next config
- Ensure `next.config.js` uses App Router defaults; add `basePath`/`assetPrefix` if your deployment requires CDN paths.
- If serving assets from CDN, update image domains/remote patterns accordingly.

### Env handling
- Convert `REACT_APP_` vars to `NEXT_PUBLIC_` for client use; keep server-only vars unprefixed.
- Document required env vars; validate at startup where possible.

### Scripts
- Use standard scripts: `dev`, `build`, `start`, `lint`. Remove obsolete CRA scripts.

### Deploy
- Verify `npm run build` and `npm start` succeed without Ant CSS in the new shell.
- If legacy UI remains, ensure its assets are not bundled with the new shell; consider separate route/entry for legacy.

### Why this matters

- Prevents broken asset paths and env drift after restructuring.

### Gotchas

- When using `basePath`, update nav links, image paths, and API calls to respect it.
- Keep `.env.local` out of version control; ensure CI/CD provides required vars. 
