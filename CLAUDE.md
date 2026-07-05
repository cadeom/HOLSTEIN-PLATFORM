# CLAUDE.md — cadeom brand & design system

Persistent project context. cadeom is a **document review and quality-assurance platform for the AEC (architecture, engineering, construction) industry** — built end to end around the review, not adapted from a document manager. Positioning line: **"cadeom — smarter outcomes."**

Apply the following to any cadeom design work unless the user says otherwise.

## Logo & assets
- **Logo mark:** `assets/logo.png` — the cadeom icon mark (used at ~32px in nav; also as large, very faint watermarks behind hero/section screens).
- **There is no wordmark lockup file.** The "cadeom" wordmark is set in live text (system sans, weight 600, letter-spacing -0.03em), always **lowercase**. If a real brand typeface for the wordmark is provided, use it; otherwise live text is fine.
- Copy assets into the working folder — don't hot-link across projects.

## Colour palette (authoritative — stay within it)
| Name | Hex | Role |
|---|---|---|
| Cerulean | `#007598` | Primary accent — links, focus rings, primary UI, card-1 accent |
| Cerulean (hover) | `#005f7a` | Primary hover |
| Ink Black | `#111827` | Dark backgrounds, primary text, feature cards |
| Cayenne | `#ea580c` | Warm accent — secondary highlight |
| Frozen Lake | `#7dd3fc` | Light-blue accent, subtle borders, background washes |
| Classic Crimson | `#d80032` | Signature accent — eyebrows, key stats, CTA buttons, dividers |
| Crimson (hover) | `#b0002a` | Crimson button hover |
| Platinum | `#efefef` | Light backgrounds |
| White | `#ffffff` | Text on dark, cards |

Neutral greys are alpha tints of Ink Black (`rgba(17,24,39,…)` on light, `rgba(255,255,255,…)` on dark) — **not** off-palette blue-greys. Don't introduce new hues; if a harmonious in-between is needed, derive it from these via oklch.

**Accent usage:** Crimson is the signature — use it for eyebrows, the one key statistic, CTAs, and dividers. Cerulean is the primary/interactive colour. Cayenne is a warm secondary. Frozen Lake is for soft background gradients and subtle borders. Don't let Crimson and Cayenne fight for the same emphasis on one screen.

## Typography
- **Family:** system sans stack — `-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', Arial, sans-serif`. No heavy web fonts. (Avoid overused AI-slop fonts — Inter, Roboto, Fraunces, etc.)
- **Weights:** 300 for large display/headings (light, airy), 500 for buttons & card headings, 600 for nav/labels/eyebrows, 700 for the brand word and hero statistics.
- **Fluid sizes** via `clamp()`. Micro-labels/eyebrows: 10–11px uppercase, letter-spacing 0.12–0.14em.
- Display headings: weight 300, negative letter-spacing (~-0.02em), `text-wrap: balance`.

## Design principles
- **Problem-first, evidence-led.** Lead with the customer's pain and quantified cost, then the capability, then the outcome. Speak to the budget holder first (money, margin, risk, proof); keep operational jargon (audit trail, resolution rate) for deeper pages.
- **Restraint / minimalism.** One thousand no's for every yes. No filler content, no data-slop stats or decorative icons. Every element earns its place. Ask before adding new sections/copy.
- **Consistent language across screens.** Shared motifs (faint logo watermark, red eyebrow with a short rule, left-border quote/callout, background radial-gradient washes) tie screens together. Don't let one screen drift into a different visual language.
- **Rhythm via background.** Alternate dark (Ink) and light (Platinum) backgrounds for pacing; both carry subtle cerulean/frozen radial gradients.
- **Purposeful motion.** Smooth, eased transitions (cubic-bezier, ~0.7–0.85s). Signature hero move: questions type out, then lift/fade as a positioning statement rises in. Always honour `prefers-reduced-motion`.
- **British English** in copy (organisation, categorised, closeout). **No hyphens/em-dashes as pause devices** — rewrite as separate sentences or with commas. cadeom always lowercase.

## Voice & copy
- Direct, confident, concrete. Not preachy or over-conversational.
- Frame around cost, risk, proof, and integration ("integrates with your existing tools", "doesn't compromise the review process").
- Reproduce reviewed copy verbatim.

## Layout / craft defaults
- Content max-widths ~540–1080px, centered; page gutter `clamp(24px, 5vw, 64px)`.
- Radius: 4px buttons/inputs, 10–14px cards. Soft shadows (`rgba(18,24,38,0.05–0.14)`).
- Use flex/grid with `gap` for spacing (not inline flow or per-element margins).
- Inline styles / self-contained CSS; hit targets ≥44px; reserve scrollbar space (`scrollbar-gutter: stable`) where a fixed edge element could jump.
- Responsive: collapse multi-column grids to one column and simplify chrome on small screens.

## Notes
- Marketing site form is front-end only in prototypes — real deployments must wire submissions to a backend/CRM.
- The developer one-pager (tech stack, AWUs, hiring) is internal/recruiting — keep it OFF client-facing marketing material.
