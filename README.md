# Mayven & Co. — Marketing Site

Landing page for **Mayven & Co.**, an automation studio that installs AI intake
bots, automated booking calendars, and high-speed sales engines for local
businesses.

**Live page:** `index.html` + `tailwind.css` + `styles.css` (three files, no
runtime build step for visitors)

## Stack

- Plain HTML + compiled Tailwind CSS + GSAP + Lucide icons
- Custom WebGL caustic-wave background, liquid view transitions
- No framework, no bundler — serve the three files from any static host

## Structure

Single-page site with four JS-switched views:

| View | Purpose |
|---|---|
| Home | Hero, value props, social-proof cards |
| Systems (`services`) | Pipeline stage selector + package offers |
| Proof & Methodology (`solutions`) | How-it-works steps + FAQ accordion |
| Free Audit (`contact`) | Lead-capture form |

## Lead capture

The contact form posts to the `FORM_ENDPOINT` constant at the top of the
inline script (paste any JSON endpoint, e.g. Formspree). **If no endpoint is
configured — or the request fails — the form does not pretend the lead was
received.** Instead it opens the visitor's email app with the request
pre-filled (`FALLBACK_EMAIL`), so no inquiry is silently lost.

```js
const FORM_ENDPOINT = "";               // e.g. "https://formspree.io/f/xxxxxx"
const FALLBACK_EMAIL = "ayesha@mayvenco.com";
```

Until a real endpoint is configured, submissions open the visitor's mail app —
they are **not** delivered directly to the inbox.

## Security hardening

- **No Tailwind Play CDN in production** — utilities are compiled ahead of
  time into `tailwind.css` (`build/` holds the local Tailwind CLI setup;
  rebuild with `node build/tailwindcss -i build/input.css -o tailwind.css --minify`
  after changing class usage)
- **Content Security Policy** (meta tag in `index.html`): `default-src 'self'`,
  scripts restricted to self + cdnjs + unpkg + the exact hash of the inline
  app script, styles to self + Google Fonts, `object-src 'none'`,
  `form-action 'self' mailto:`. A `<meta>` CSP can't set every protection —
  when the host allows it, also send `X-Content-Type-Options: nosniff`,
  `Referrer-Policy`, `Permissions-Policy`, and HSTS headers
- **Subresource Integrity** (`integrity` + `crossorigin="anonymous"`) on the
  GSAP and Lucide CDN scripts — a tampered CDN file is rejected by the browser
- **Zero inline event handlers** — all clicks/submits/errors are wired with
  `addEventListener` in the main script, so CSP blocks no functionality
- **Input handling** — form values are trimmed, validated, and
  `encodeURIComponent`-encoded into the `mailto:` fallback; never injected
  into the DOM as HTML (no `innerHTML`/`document.write`/`eval` anywhere)
- **No storage, no cookies, no query-string parsing** — nothing to leak or
  poison from the client side

> **CSP hash maintenance:** the inline app script's SHA-256 hash is embedded
> in the CSP meta tag. If you edit the script, recompute it over the exact
> bytes between `<script>` and `</script>` and update the meta tag, or the
> whole page's JavaScript will be blocked.

## Accessibility & performance notes

- Zoom is not disabled; `prefers-reduced-motion` pauses the WebGL loop,
  cursor follower, and view transitions
- Custom cursor only engages on fine-pointer desktops and never hides the
  native cursor via CSS alone
- Pipeline stages are real `<button>`s, the FAQ is a proper accordion, and
  legal modals trap focus and restore it on close
- Animation loops pause when the tab is hidden

## Deployment

Any static host works (GitHub Pages, Netlify, Vercel, Cloudflare Pages).
Serve `index.html`, `tailwind.css`, and `styles.css` from the repository root
so relative paths (e.g. `erasebg-transformed.png`, which doubles as the
favicon) resolve.
