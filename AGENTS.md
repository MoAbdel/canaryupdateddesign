# Agent notes for the Canary landing page

This repository is a **static site**, not a Next.js project. There is no `app/`, no `pages/`, no `node_modules/next/`.

- Entry: `index.html`
- React + JSX served from CDN, transpiled in-browser via `@babel/standalone`
- One Vercel function: `api/subscribe.js`
- Tweaks panel state lives in `localStorage['canary.tweaks.v1']`

When adding routes:
- API routes are files under `api/` (Vercel serverless function convention)
- There is no middleware layer — the site is essentially static

See `README.md` for the full file map.
