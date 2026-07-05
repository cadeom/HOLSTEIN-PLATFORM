# Changes Log — cadeom Landing Page

All changes are tracked with point revisions and timestamped archives for easy rollback.

---

## Current Version: 1.1

| Version | Date | Time | Description | Archived File | Rollback |
|---------|------|------|-------------|---------------|----------|
| 1.1 | 2026-07-05 | 18:52 | Copy fix (whom by); Formspree lead capture; fixed screen 3 scroll and screen 2 header clipping on mobile/tablet | `260705-1852_pre-scroll-fix.html` | Copy archived file over index.html |
| 1.0 | 2026-07-05 | 14:30 | Initial release: standalone landing page with typewriter hero, 4-screen deck, responsive design | — | N/A (initial) |

---

## Version History

### 1.1 — Lead Capture + Mobile Scroll Fixes
**Date:** 2026-07-05 18:52
**Description:**
- Changed Q3 copy: "who by" to "whom by"
- Wired contact form to Formspree (submissions go to info@cadeom.com)
- Fixed screen 3 (pillars) not scrolling on any device: a `position: relative` on `.s3` was overriding the `position: absolute` every slide needs to act as a scroll container
- Fixed screen 2 header being clipped by the nav bar on iPad/landscape: replaced `align-items: center` vertical centering (which pushes overflowing content above the reachable scroll area) with overflow-safe `margin: auto` centering on `.impact-inner` and `.pil-inner`

**Files Changed:** `index.html`
**Archived:** `archived/260705-1852_pre-scroll-fix.html`

### 1.0 — Initial Release
**Date:** 2025-07-05 14:30  
**Description:** 
- Created standalone `index.html` (no runtime dependencies)
- Hero screen with typewriter animation of 4 provocative questions
- Reality screen with cost stat (5–15% project value lost)
- What Changes screen with 3 capability pillars
- Contact form (front-end only)
- Responsive design (mobile, tablet, desktop)
- HTTPS enabled on custom domain (www.cadeom.com)
- GitHub Pages deployment

**Files Changed:** `index.html`, `assets/logo.png`, `README.md`, `CLAUDE.md`  
**Deployed to:** https://www.cadeom.com  
**Archived:** N/A (initial release)

---

## How to Use This Log

1. **Before making changes:** Note the current version number (e.g., 1.0)
2. **Make your changes** to the website
3. **Archive the previous version:**
   - Copy `index.html` → `/archived/yymmdd-hhmm_description.html`
   - Example: `archived/250705-1430_hero-animation-update.html`
4. **Deploy the new version** to www.cadeom.com
5. **Update this log:**
   - Add row to the table with new version, date, time, description
   - Add detailed section below with what changed, files affected, etc.
6. **Commit to git:**
   ```bash
   git add changes.md index.html archived/
   git commit -m "Update to v1.x: [description]"
   git push
   ```

## Rolling Back

To revert to a previous version:

1. Check `changes.md` for the archived file you want to restore
2. Copy the archived file: `archived/yymmdd-hhmm_description.html` → `index.html`
3. Test locally (open in browser)
4. Deploy: `git add index.html && git commit -m "Rollback to v1.x" && git push`
5. Update `changes.md` to document the rollback

---

## Notes

- Keep descriptions **brief but specific** — future-you will thank you
- Archive BEFORE deploying so you always have the previous working version
- Use 24-hour time (hhmm) in filenames for clarity
- Version numbers use point revisions: 1.0, 1.1, 1.2, etc.
- Major feature additions = new major version (1.0 → 2.0)
- Bug fixes / tweaks = point revision (1.0 → 1.1)
