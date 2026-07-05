# cadeom — smarter outcomes

The official cadeom landing page and lead-capture website.

## Overview

A single-page marketing site that introduces cadeom — a document review and quality-assurance platform for the AEC (architecture, engineering, construction) industry.

The site walks visitors through four key screens:

1. **Hero** — Provocative questions with a signature typewriter animation, revealing the positioning statement
2. **The Reality** — Frames the problem: why current review processes fail, with a hard cost stat (5–15% of project value lost to rework)
3. **What Changes** — Three core capabilities: Cycle Time, Resolution Rate, Institutional Knowledge
4. **Get in Touch** — Lead-capture contact form

## Features

- **Horizontally-paged deck** — Smooth screen transitions with 5 navigation methods (dots, arrows, wheel, keyboard, swipe)
- **Signature animation** — Hero typewriter effect with variable character delays
- **Responsive design** — Adapts from 360px mobile to 4K+ desktop
- **Accessibility** — Respects `prefers-reduced-motion`, semantic HTML, ARIA labels, ≥44px hit targets
- **No dependencies** — Pure HTML/CSS/JavaScript, zero build step required

## Getting Started

### Local Development

Simply open `index.html` in a browser — no build or server required.

### Deploy to Your Domain

This site is optimized for **GitHub Pages** deployment:

1. Ensure the repo is public
2. Go to **Settings** → **Pages**
3. Set **Source** to "Deploy from a branch" → **main**
4. Wait ~1 minute for GitHub to build
5. Your site will be live at `https://cadeom.github.io/HOLSTEIN-PLATFORM`

To use a custom domain (e.g., `www.cadeom.com`):

1. In **Settings** → **Pages**, under "Custom domain", enter your domain
2. Update your domain's DNS records to point to GitHub Pages (GitHub will show you the exact steps)
3. GitHub will handle the SSL certificate automatically

## Form Submissions

⚠️ **Currently, the contact form is front-end only** — it validates and shows a success message but does not send data anywhere.

To capture leads in production, wire the form to a backend service:

- **Zapier** (no-code) — Create a Zap triggered by webhook
- **Formspree** — Simple form-to-email
- **Your own API** — POST form data to your backend

Update the form submission handler in `index.html` (line ~808) to send data to your endpoint before showing the success state.

## File Structure

```
├── index.html              # Main site (HTML/CSS/JS all-in-one)
├── assets/
│   └── logo.png           # cadeom icon mark
├── README.md              # This file
├── CLAUDE.md              # Design system & brand guidelines
└── screenshots/           # Reference captures of each screen
```

## Design System

All styling uses the **cadeom colour palette** (defined in CLAUDE.md):

- **Cerulean** `#007598` — Primary accent
- **Red/Crimson** `#d80032` — Signature accent (eyebrows, CTAs)
- **Cayenne** `#ea580c` — Warm secondary
- **Platinum** `#efefef` — Light backgrounds
- **Ink Black** `#111827` — Dark backgrounds & text

Typography is system sans (no web fonts) with fluid sizing via `clamp()`.

See `CLAUDE.md` for the complete design system, color palette, and layout specifications.

## Browser Support

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Mobile Safari (iOS 14+)

## License

© 2025 cadeom. All rights reserved.

---

**Questions?** Contact Jerome at jeromej@cadeom.com
