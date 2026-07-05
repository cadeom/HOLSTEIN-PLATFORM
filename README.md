# Handoff: cadeom Landing Page

## Overview
A single-page marketing / lead-capture site for **cadeom** — a document review and quality-assurance platform for the AEC (architecture, engineering, construction) industry. The page is a full-screen, horizontally-paged "deck" of four screens that walk a prospect from problem → cost → solution → contact:

1. **Hero** — a typewriter sequence of four provocative questions, which then lifts away to reveal a positioning statement.
2. **The Reality** — a narrative about why current review processes fall short, paired with a hard cost stat (5–15% of project value lost to rework).
3. **What changes with cadeom** — three capability pillars (Cycle Time, Resolution Rate, Institutional Knowledge).
4. **Get in touch** — a contact/enquiry form (name, email, company, role, team size).

Primary goal: capture qualified enquiries via the form. Secondary goal: communicate what cadeom is and why it matters.

## About the Design Files
The files in this bundle are **design references created in HTML** — a working prototype showing the intended look, motion, and behaviour. They are **not production code to copy directly.**

- `Cadeom Landing v5.dc.html` is authored in a lightweight in-house component format ("DC"). It loads a runtime (`support.js`) that provides the `<x-dc>` / `<helmet>` custom elements and the `DCLogic` base class. **You do not need this runtime in production** — it is only what makes the prototype open standalone in a browser. The meaningful, portable parts are: the CSS in the `<style>` block, the HTML markup inside `<x-dc>`, and the plain-JavaScript behaviour inside the `class Component ... componentDidMount()` block.
- Your task is to **recreate this design in the target codebase's environment** (React, Vue, Svelte, plain HTML/CSS/JS, etc.) using its established patterns, component library, and conventions. If no front-end environment exists yet, choose the most appropriate framework for the project and implement there.
- All copy in this prototype is final and reviewed — reproduce it exactly (see the "Copy" callouts per screen).

## Fidelity
**High-fidelity (hifi).** Colours, typography, spacing, motion timing, and interactions are all final and intentional. Recreate the UI faithfully using the codebase's libraries and patterns. Exact values are given below in **Design Tokens**.

## Global Layout & Behaviour

### The paged-deck model
- The four screens are stacked as absolutely-positioned full-viewport layers inside a fixed `.slides` container (`position: fixed; inset: 0`).
- Only one screen is "active" at a time. Screens transform horizontally: active = `translateX(0)`, screens before the active one = `translateX(-100%)`, screens after = `translateX(100%)`.
- Transition: `transform 0.82s cubic-bezier(0.76, 0, 0.24, 1)`.
- `<html>, <body>` have `overflow: hidden` and `height: 100%` — the page itself never scrolls; each screen scrolls internally if its content is taller than the viewport (`.slide { overflow-y: auto; scrollbar-gutter: stable; }`). **`scrollbar-gutter: stable` is important** — it reserves scrollbar width so a background watermark anchored to the right/left edge doesn't jump when a scrollbar appears/disappears.

### Navigation (five ways to move between screens)
1. **Dots** — a fixed centered row of 4 dots at bottom (`.dots`), clickable, active dot enlarges and turns cerulean.
2. **Edge arrows** — circular ‹ / › buttons pinned to the viewport's vertical center at the left/right edges. `‹` hides on the first screen, `›` hides on the last. **Only shown at viewport ≥ 1200px** (below that, the 1080px content container has no side gutter, so the arrows would overlap card content — see the `@media (max-width: 1199px) { .navarrow { display: none } }` rule). They are also hidden entirely below 760px.
3. **Mouse wheel / trackpad** — throttled to one screen change per 820ms. If the current screen has internal scroll room, the wheel scrolls the content first and only pages once you hit the top/bottom edge.
4. **Keyboard** — ArrowRight/PageDown/ArrowDown → next; ArrowLeft/PageUp/ArrowUp → previous.
5. **Touch swipe** — horizontal swipe of >55px changes screen.

The central navigation function is `go(index)` (exposed as `window.go`), which guards against re-entry while a transition is in flight (`busy` flag, released after 780ms), updates every screen's transform, and calls `updateUI()`.

### Light / dark chrome switching
Screens 1 (hero) and 4 (form) have a **dark** navy background; screens 2 and 3 are **light** (platinum). The nav bar and dots are always light-glass in this build, but the code tracks a `DARK = [0, 3]` array and toggles a `.light` class on the nav and dots per screen — wire this up so chrome contrast can adapt per screen if needed. The "Get in touch" nav CTA fades in (`.show`) and stays visible on all screens.

## Screens / Views

### Screen 1 — Hero (`.s1`, dark)
- **Purpose:** hook the visitor with pointed questions, then deliver the positioning statement.
- **Layout:** flex-centered; inner content max-width 1080px, padding `86px var(--pad) 68px`. A faint logo watermark (`.hero-mark`, opacity 0.13) is anchored to the right, vertically centered, `height: min(52vh, 370px)`.
- **Background:** navy `#111827` with three subtle radial gradients (cerulean top-right, red bottom-left, frozen-blue center) — see token list.
- **Components:**
  - **Eyebrow** (`.hero-ey`): "Now in development" — 11px, weight 600, uppercase, letter-spacing 0.14em, color red `#d80032`, with an 18px×1px red rule before it (flex gap 10px).
  - **Question stack** (`.q-stack`): four lines, each `clamp(18px, 2.3vw, 31px)`, weight 300, line-height 1.3, color white, letter-spacing -0.015em. Lines start `visibility: hidden` and are revealed one character at a time by the typewriter (see Interactions).
  - **Positioning statement** (`.hero-statement`, initially hidden, absolutely positioned over the screen): revealed after the questions lift away. The paragraph (`.hero-def`) is `clamp(19px, 2.3vw, 30px)`, line-height 1.4, color **cayenne `#ea580c`**, max-width 900px, `text-wrap: balance`. Within it:
    - `.hs-brand` "cadeom" — white, weight 700, 1.12em, letter-spacing -0.03em.
    - `.hs-tag` "smarter outcomes." — block, white, weight 700, margin-top 0.5em.
    - `.hs-note` "Get in touch to find out more." — block, muted white, 13.5px, margin-top 34px.
- **Copy (exact):**
  - Q1: `How much does each review cycle cost?`
  - Q2: `What percentage of project margin is lost to rework?`
  - Q3: `Could you easily prove what was reviewed, who by and when?`
  - Q4: `Are you aware if you are repeating the same design issues across projects?`
  - Statement: `cadeom streamlines end-to-end document reviews, tailored to your workflow, and integrates with your existing tools. Delivering faster review cycles, clearer communication, and a complete audit trail. AI assists reviewers, improves your process, and generates insights for future reviews. smarter outcomes.`

### Screen 2 — The Reality (`.s2`, light)
- **Purpose:** frame the problem and quantify the cost.
- **Layout:** two-column grid (`grid-template-columns: 1fr 1.05fr; gap: 52px`), max-width 900px, vertically centered. Collapses to one column below 760px.
- **Left column:**
  - **Opener** (`.impact-opener`): three lines — "The tools exist." / "The process runs." / "Projects get delivered." (last line dimmed via `.dim`). `clamp(22px, 3vw, 38px)`, weight 300, color ink.
  - Red divider (`.impact-divider`): 44px × 2px, red.
  - **Body** (`.impact-body`): two paragraphs, 15px, line-height 1.8, color `--l-mid`.
  - **Close** (`.impact-close`): "The difference is not a technology gap. / It is a process design gap." — 15px, weight 600, cerulean.
- **Right column** (`.impact-right`): a dark ink card (`background: #111827; border-radius: 14px; padding: 46px 40px`):
  - **Stat number** (`.stat-num`): "5–15%" — `clamp(72px, 9vw, 116px)`, weight 700, red, line-height 0.9.
  - **Stat label** (`.stat-label`): "total project value" — white, weight 600.
  - **Stat sub** (`.stat-sub`): "is lost to rework that should have been caught in review. *Source: Construction Industry Institute*" (source in `<em>`). 13px, muted white, with a 2px frozen-blue left border.
  - **Gap callout** (`.stat-gap`): a red-tinted box (`background: rgba(216,0,50,0.10)`, 3px red left border): "A meaningful proportion of that is recoverable through a better review process. The question is not whether a better process is possible, **but how much you stand to recover.**" (last clause in `<strong>`, white).
- **Copy (exact) — body paragraphs:**
  - P1: `But take a moment to look at how your reviews actually work, not how they are described in a procedure, but how they happen day to day.`
  - P2: `Most organisations have built their review process on tools where review was an afterthought, not the core offering. That process works until it doesn't. Until a missed issue becomes a defect on site. Until your organisation is required to provide evidence of a complete comment trail, and the answer is a scramble of spreadsheets and marked-up PDFs spread across multiple folders.`

### Screen 3 — What changes with cadeom (`.s3`, light)
- **Purpose:** present the three core capabilities.
- **Layout:** max-width 1080px, vertically centered. Background is platinum with two radial gradients (cerulean top-right, frozen-blue bottom-left). A faint logo watermark (`.s3-mark`, opacity 0.055) is anchored bottom-left. Hidden below 760px.
- **Head:** eyebrow (`.sec-ey`) "What changes with cadeom" (red, uppercase, with rule) + title (`.pil-title`) "Software that puts the **review** first." — `clamp(24px, 3vw, 36px)`, weight 300, ink, with "review" in cerulean via `.accent`.
- **Grid** (`.pil-grid`): three cards, `repeat(3,1fr)`, gap 18px. One column below 760px. Each card (`.pil`): white, 1px border, 12px radius, subtle shadow, padding `28px 26px 30px`, position relative.
  - Each card has: a large faint index numeral top-right (`.pil-idx`, 42px weight 700, colored at 16% alpha of the card's accent), a 40×4px rounded accent bar (`.pil-top`), an uppercase tag (`.pil-tag`), a heading (`.pil-h`, `clamp(16px,1.6vw,20px)` weight 500), a body paragraph (`.pil-p`, 14px line-height 1.76), and an italic pull-quote (`.pil-pull`) with a colored 2px left border.
  - **Card 1 (`.c1`) accent = cerulean `#007598`.** Tag "Cycle Time". Heading "Every stage of a review, tracked in real time." Body: `Know the status of any review, who or what is blocking it, and how cycle times trend across your teams and project types. Insights available from every stage.` Pull: `"Which reviewers consistently catch critical issues? Which document types generate the most comments?"`
  - **Card 2 (`.c2`) accent = cayenne `#ea580c`.** Tag "Resolution Rate". Heading "Structured comments that carry forward across revisions." Body: `Comments are categorised and templated the moment they are raised, and traceable from creation through response to closeout. Document originators respond in context, and unresolved comments transfer to the next revision automatically, so that resolution happens inside the process.` Pull: `"Traceable from creation through response through closeout."`
  - **Card 3 (`.c3`) accent = red `#d80032`.** Tag "Institutional Knowledge". Heading "Review results captured as data, not PDFs in email." Body: `Every closed review feeds the next. Over time cadeom builds a picture of your review quality, where issues cluster and where reviewers excel, and AI surfaces relevant patterns and flags likely issues, to make professional judgement sharper.` Pull: `"That knowledge exists, it just never gets captured in a form anyone can act on."`

### Screen 4 — Get in touch (`.s4`, dark)
- **Purpose:** capture an enquiry.
- **Layout:** single centered column, max-width 540px, top-aligned with padding `104px var(--pad) 90px` (top padding clears the fixed nav). Background navy with two radial gradients.
- **Components:**
  - Eyebrow (`.form-ey`) "Get in touch" (red, uppercase, rule).
  - Heading (`.form-h`) "Interested? Get in touch." — `clamp(22px,2.6vw,32px)`, weight 300, white.
  - Sub (`.form-sub`) "If you'd like to find out more about cadeom, leave your details and we'll be in touch." — 14.5px, muted white.
  - **Form** (`.reg-form`, `#regForm`): vertical stack, gap 14px.
    - Row of two: **Full name** (text, required, placeholder "Jane Smith") + **Work email** (email, required, placeholder "jane@firm.com").
    - **Company** (text, required, placeholder "Your company").
    - Row of two: **Role** (select, required — options: Project Manager, Lead Engineer, Architect, Document Controller, BIM Manager, Principal / Director, Other) + **Team size** (select, required — options: Under 10 people, 10 to 50 people, 50 to 200 people, 200+ people).
    - Footer (`.form-foot`): submit button "Get in touch" (`.btn-sub`, red) + note "No spam. Only updates about cadeom."
    - Inputs: 44px tall, dark translucent fill, 1px border, 4px radius, white text. Focus state: cerulean border + `box-shadow: 0 0 0 3px rgba(0,117,152,0.18)` + cerulean-tinted fill. Selects use a custom CSS triangle caret (`.sel-w::after`); native option list is dark.
  - **Success state** (`.form-ok`, hidden until submit): a cerulean check-ring, heading "Thanks — we'll be in touch.", sub "We'll get back to you shortly with more about cadeom."

## Interactions & Behavior

### Hero typewriter (the signature animation)
- On mount, `runTypewriter()` runs. It types each question into its line character-by-character.
- **Per-character timing** (`charDelay`): base `46ms ± 9ms`; **+90ms** after `, ;`; **+90ms** after `. ? !`; +42ms after em-dash; −10ms for a space; occasional +68ms "thinking" pause (13% chance) after a space. A blinking block cursor (`.q-cursor`, `▌`, red, `blink 0.75s steps(1) infinite`) trails the text; it is removed when a line finishes unless it's the last line.
- **Sequence:** wait 500ms → type Q1 → wait ~1100–1360ms → Q2 → wait ~1150–1410ms → Q3 → wait ~1150–1410ms → Q4 (keep cursor) → wait 1200ms → add `.lift` to `.hero-inner` (questions slide up 72px and fade out over 0.7s) → wait 520ms → add `.show` to `.hero-statement` (statement rises 36px into place and fades in over ~0.9s) → reveal nav CTA.
- **Reduced motion** (`prefers-reduced-motion: reduce`): all four questions appear instantly, the lift + statement are shown immediately, no typing. Honour this.

### Form submit
- `#regForm` submit handler: `preventDefault()`, run native `checkValidity()` (and `reportValidity()` if invalid), then hide the form and show the success state.
- **IMPORTANT — the prototype does NOT send the data anywhere.** There is no backend, email, or network call; the submission is discarded. **In production you must wire the form to a real destination** (form-to-email service, CRM/marketing endpoint, or your own API), with proper loading and error states so a failed send does not silently show the success message. Validate name (required), work email (required, valid email), company (required), role (required), team size (required).

### Screen transitions
- `go(index)` — see Global Behaviour. Guarded by `busy` for 780ms.
- Inner content of screens 2–4 has an entrance transition: when a screen is not active, its inner container sits at `opacity: 0; translateX(46px)`; when active, it eases to `opacity: 1; translateX(0)` over ~0.78s with a 0.14s delay — so content settles in just after the screen slides in.

### Responsive behavior
- **≤ 760px:** nav shrinks to 56px and drops the "smarter outcomes" tagline; edge arrows hidden; screens 2–4 top-align; impact grid, pillar grid, and form rows all collapse to one column; watermarks hidden; dots enlarge; hero statement type scales down to `clamp(18px,4.6vw,24px)`.
- **761px–1199px:** edge arrows hidden (no gutter); everything else as desktop.
- **≥ 1200px:** full layout with edge arrows.

## State Management
Minimal, all local:
- `cur` — current screen index (0–3).
- `busy` — transition lock (boolean), released 780ms after a `go()`.
- `lastW` — timestamp of last wheel-triggered page change (wheel throttle).
- `tx` — touch start X (swipe detection).
- Typewriter progress is local to `runTypewriter()` / `typeInto()`.
- Form: submitted vs not (drives form ↔ success-state visibility). In production add: submitting, submit-error.

In a component framework, model `cur` as state driving the active screen; keep the typewriter as an effect that runs once on mount and respects reduced-motion; model form state (idle / submitting / success / error) explicitly.

## Design Tokens

### Colors
| Token | Hex | Use |
|---|---|---|
| Ink / dark bg | `#111827` | dark screen backgrounds, ink text, stat card |
| White | `#ffffff` | dark-screen text |
| Muted white | `rgba(255,255,255,0.56)` | secondary text on dark |
| Faint white | `rgba(255,255,255,0.26)` | tertiary/placeholder on dark |
| Border white | `rgba(255,255,255,0.10)` | input borders on dark |
| Platinum / light bg | `#efefef` | light screen backgrounds |
| Light ink | `#111827` | headings on light |
| Light mid | `rgba(17,24,39,0.66)` | body text on light |
| Light faint | `rgba(17,24,39,0.42)` | dimmed text on light |
| Light border | `rgba(17,24,39,0.12)` | card borders on light |
| Cerulean | `#007598` | primary accent, links, focus, card 1, "review" |
| Cerulean hover | `#005f7a` | primary button hover |
| Cayenne | `#ea580c` | positioning statement, card 2 accent |
| Frozen (light blue) | `#7dd3fc` | subtle accents, stat-sub border, gradients |
| Red / crimson | `#d80032` | eyebrows, stat number, CTA buttons, card 3, dividers |
| Red hover | `#b0002a` | red button hover |

Hero gradient (navy): `radial-gradient(ellipse at 82% 20%, rgba(0,117,152,0.16), transparent 50%)`, `radial-gradient(ellipse at 10% 82%, rgba(216,0,50,0.09), transparent 44%)`, `radial-gradient(ellipse at 46% 50%, rgba(125,211,252,0.05), transparent 54%)`.
Pillars gradient (platinum): `radial-gradient(ellipse at 84% 16%, rgba(0,117,152,0.11), transparent 52%)`, `radial-gradient(ellipse at 6% 92%, rgba(125,211,252,0.16), transparent 48%)`.
Form gradient (navy): `radial-gradient(ellipse at 20% 56%, rgba(0,117,152,0.12), transparent 50%)`, `radial-gradient(ellipse at 88% 30%, rgba(125,211,252,0.05), transparent 46%)`.

### Typography
- Family: system sans stack — `-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', Arial, sans-serif`. (No web font is loaded; substitute the codebase's default sans if it has one.)
- Weights used: 300 (large display / questions / headings), 500 (buttons, card headings), 600 (nav name, labels, eyebrows), 700 (brand word, stat number, index numerals, section eyebrows).
- Display sizes are fluid via `clamp()` — see each component above.
- Uppercase micro-labels: 10–11px, letter-spacing 0.12–0.14em.

### Spacing / layout
- Content max-width: 1080px (hero, pillars), 900px (impact), 540px (form).
- Page gutter `--pad`: `clamp(24px, 5vw, 64px)`.
- Nav height: 60px (desktop) / 56px (mobile).
- Card grid gap: 18px; impact column gap: 52px.

### Radius
- Buttons / inputs: 4px. Cards (pillars): 12px. Stat card: 14px. Callout box: 6px. Dots/rings: 50%.

### Shadow
- Nav: `0 6px 22px rgba(18,24,38,0.10)`.
- Pillar card: `0 2px 12px rgba(18,24,38,0.05)`.
- Edge arrow: `0 4px 16px rgba(18,24,38,0.14)` (hover `0 6px 22px rgba(18,24,38,0.2)`).

### Motion
- Screen slide: `transform 0.82s cubic-bezier(0.76, 0, 0.24, 1)`.
- Inner content entrance: `opacity 0.7s ease 0.14s, transform 0.78s cubic-bezier(0.22,0.61,0.36,1) 0.14s`.
- Hero lift: `transform 0.7s cubic-bezier(0.22,0.61,0.36,1), opacity 0.6s`.
- Statement reveal: `opacity 0.85s ease, transform 0.95s cubic-bezier(0.22,0.61,0.36,1)`.
- Typewriter: see Interactions.

## Assets
- `assets/logo.png` — the cadeom icon mark (used at 32px in the nav, and as large faint watermarks on the hero and pillars screens). **This is the icon mark only; there is no wordmark lockup file** — the "cadeom" wordmark in the nav is set in live text (system sans, weight 600, letter-spacing -0.03em), not an image. If the real brand wordmark uses a specific typeface, substitute it in production.
- No other images. The success checkmark and select caret are inline SVG / CSS, not asset files.

### Brand palette (authoritative)
Cerulean `#007598`, Ink Black `#111827`, Cayenne `#ea580c`, Frozen Lake `#7dd3fc`, Classic Crimson `#d80032`, Platinum `#efefef`, White `#ffffff`. Stay within this palette.

## Files
- `Cadeom Landing v5.dc.html` — the full design: CSS (`<style>` in `<helmet>`), markup (inside `<x-dc>`), and behaviour (inside `class Component`'s `componentDidMount`). This is the source of truth.
- `support.js` — the prototype runtime only. Reference for how the prototype boots; **not needed in production.**
- `assets/logo.png` — the logo mark.
- `screenshots/` — reference captures of each screen: `screen-1-hero.png` (questions), `screen-1b-statement.png` (post-reveal statement), `screen-2-reality.png`, `screen-3-pillars.png`, `screen-4-form.png`. Desktop width; tall screens may be vertically cropped to the capture viewport — the HTML is authoritative for full layout.

## Notes for implementation
- Recreate the copy **verbatim** — it has been through review.
- Respect `prefers-reduced-motion` for the typewriter and slide transitions.
- Keep hit targets ≥ 44px (inputs already are).
- The single biggest production gap is **wiring the form to a real backend** — do not ship the front-end-only success state.
- Consider whether the horizontally-paged deck is the right pattern in your target context; on mobile especially, a vertical scroll of the same four sections may be more idiomatic. The content and styling all transfer either way.
