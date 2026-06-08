# Agent notes for the Canary landing page

This repository is a **static site**, not a Next.js project. There is no `app/`, no `pages/`, no `node_modules/next/`.

- Entry: `index.html`
- React + JSX served from CDN, transpiled in-browser via `@babel/standalone`
- One Vercel function: `api/subscribe.js`
- Tweaks panel state lives in `localStorage['canary.tweaks.v1']`

When adding routes:
- API routes are files under `api/` (Vercel serverless function convention)
- `middleware.js` (root, Vercel Edge Middleware) gates the whole site behind a
  password — see `docs/superpowers/specs/2026-06-08-site-password-gate-design.md`.
  It fails closed: the `SITE_PASSWORD` and `GATE_TOKEN` env vars MUST be set in
  Vercel or the site locks out everyone. Public paths are allowlisted in the file.

See `README.md` for the full file map.
