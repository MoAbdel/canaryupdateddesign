# Canary — C01 Capsule landing

Static marketing page for the Canary C01 keyboard capsule.

## Stack

- Static HTML (`index.html`, `canary-print.html`)
- React 18 loaded from CDN (UMD) — see `<script>` tags at the bottom of `index.html`
- JSX in `src/*.jsx` is compiled in-browser by `@babel/standalone` (dev convenience — not ideal for production; see audit Phase 2)
- One Vercel serverless function: `api/subscribe.js` (Phase 1 stub, logs only)
- Deployed on Vercel as a static site (`vercel.json` sets `framework: null`)

## Local development

```bash
npm run dev
# Serves the current directory on http://localhost:3002 with caching disabled.
```

The form's POST to `/api/subscribe` will 404 on `npx http-server` — it only exists when deployed to Vercel or run with `vercel dev`.

## File map

```
index.html              # main page; <head> contains all meta + JSON-LD
canary-print.html       # printable variant (noindex)
robots.txt              # crawl + AI engine rules
sitemap.xml             # single-page sitemap with image entries
llms.txt                # AI-engine-readable brand + product summary
vercel.json             # Vercel config + security headers + cache rules
api/
  subscribe.js          # POST endpoint for the notify form (stub)
src/
  app.jsx               # React entry — composes sections in order
  components.jsx        # Reveal, Placeholder, FilmGateway, SignupForm, etc.
  sections-shared.jsx   # Manifesto, System, Capture, Community, Footer
  direction-safe.jsx    # Calm/typographic hero variant
  direction-bold.jsx    # Drop-culture hero variant + ticker
  tweaks-host.js        # Tweaks panel state (localStorage)
assets/
  canary-bug.png        # bug logomark
  canary-wordmark.png   # full wordmark
  canary-logo.png       # legacy
  canary-video.mp4      # 34 MB hero film — re-encode planned
  keyboards/            # C01 product photography
```

## Direction toggle

Two hero variants ship in the same page. Toggle via the bottom-right Tweaks panel (opens via parent-window postMessage in the design host, or by setting `localStorage['canary.tweaks.v1']` manually).

```js
// in DevTools console:
localStorage.setItem('canary.tweaks.v1', JSON.stringify({ direction: 'bold' }));
location.reload();
```

## Deploy

`git push` → Vercel auto-deploys. `vercel.json` controls headers, caching, and clean URLs. The current CSP is **report-only**; promote to enforcing after a few days of clean reports.

## Known gaps (see audit dated 2026-05-20)

- `SignupForm` posts to `/api/subscribe`, which currently only logs. Wire to a real ESP (Klaviyo / ConvertKit / Resend) in Phase 2.
- JSX is still transpiled in-browser. Replace with a real build (esbuild / Vite / Astro) in Phase 2.
- No `/privacy` or `/terms` page yet.
- Cookie consent banner not yet built; GA4 ships with consent default-denied.
