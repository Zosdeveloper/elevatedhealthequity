# CLAUDE.md - Elevated Health Equity

## Project Overview
Static website for Elevated Health Equity — a health equity consulting and training organization. Pure HTML/CSS/JS, no frameworks, no build tools.

**DO NOT touch** `C:\Users\Administrator\elevatedhealthequity-lovable\` — that is a separate/old project.

## Tech Stack
- Static HTML files (no framework, no SSG)
- All CSS is inline (`<style>` tags in each HTML file) — no external stylesheets
- Vanilla JS (Intersection Observer, accordion, slider, counters)
- HubSpot forms integration (contact + strategy pages)
- Hosted on GitHub Pages (see `CNAME` file)

## Design System
- **Navy**: #172A37 (primary dark)
- **Gold**: #ECBC0A (accent)
- **Font**: Montserrat (Google Fonts, weights 300–900)
- Consistent header/footer across all pages (manually duplicated, no includes)

## Pages
```
Root:
├── index.html          # Homepage
├── about.html          # About / team
├── contact.html        # Contact form (HubSpot)
├── consulting.html     # Consulting services
├── accelerator.html    # Accelerator program
├── workshops.html      # Workshop listing
├── glossary.html       # Health equity glossary
├── thank-you.html      # Form submission confirmation
├── privacy-policy.html
├── terms.html
├── 404.html
├── robots.txt, sitemap.xml
└── workshops/
    ├── communication.html
    ├── cultural-competency.html
    ├── difficult-conversations.html
    ├── health-equity.html
    ├── implicit-bias.html
    ├── inclusive-leadership.html
    ├── resilience.html
    └── sdoh.html
```

## Key Patterns

### No Build Step
- Edit HTML files directly — changes are live on push
- Each page is self-contained with its own `<style>` block
- No shared CSS file — style changes may need to be applied across multiple files

### JS Patterns
- Intersection Observer for scroll animations (fade-in, slide-up)
- FAQ accordion with `max-height` toggle
- Testimonial slider (manual, no library)
- Stat counter animations on scroll

### HubSpot Forms
- Embedded via HubSpot JS SDK on contact and strategy pages
- Form IDs are in the HTML — don't change without checking HubSpot dashboard

### Images
- All in `images/` directory
- Partner logos in `images/partners/`
- Team photos in `images/team/`
- Mix of JPG, PNG, WEBP formats

## SEO
- `robots.txt` and `sitemap.xml` present at root
- Meta tags in each page `<head>`
- Favicon set (favicon.ico, favicon-16x16.png, favicon-32x32.png, apple-touch-icon.png)

## Deployment
- GitHub Pages with custom domain (see `CNAME`)
- Auto-deploys from `main` branch
- No build step — push and it's live

## Git
- Repo: `Zosdeveloper/elevatedhealthequity` (public)
- Always push after changes
