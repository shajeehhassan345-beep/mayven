# Mayven & Co. — Marketing Site

Landing page for **Mayven & Co.**, an automation studio that installs AI intake
bots, automated booking calendars, and high-speed sales engines for local
businesses.

**Live page:** `index.html` (single file, no build step)

## Stack

- Plain HTML + Tailwind CSS (Play CDN) + GSAP + Lucide icons
- Custom WebGL caustic-wave background, liquid view transitions
- No framework, no bundler — open `index.html` in a browser and it runs

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
const FORM_ENDPOINT = "";          // e.g. "https://formspree.io/f/xxxxxx"
const FALLBACK_EMAIL = "hello@mayven.co";
```

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
Serve from the repository root so `erasebg-transformed.png` resolves
(it doubles as the favicon; a dedicated multi-size icon set is a future
improvement).
