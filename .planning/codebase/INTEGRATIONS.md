# External Integrations

**Analysis Date:** 2026-02-07

## APIs & External Services

**Form Management:**
- HubSpot Forms - Lead capture and CRM integration
  - SDK: HubSpot Forms v2 JavaScript library
  - Auth: Portal ID and Form ID (hardcoded in HTML)
  - Endpoints:
    - `https://js.hsforms.net/forms/v2.js` - Form SDK loader
    - Portal ID: `5440022`
    - Form IDs used: `b6339de7-46ce-4160-8186-09fa4e40883f`
  - Form redirect: Post-submission redirects to `thank-you.html`

**CDN & Font Delivery:**
- Google Fonts API
  - Fonts loaded: Montserrat (weights: 300, 400, 500, 600, 700) and Lora (weights: 400, 500, 600, 700)
  - Domain: `https://fonts.googleapis.com`
  - Glyph server: `https://fonts.gstatic.com`
  - Optimization: Preconnect hints in `<head>` for performance

## Data Storage

**Databases:**
- None - Fully static site with no database

**File Storage:**
- Local filesystem only - All images stored in `C:\Users\Administrator\elevatedhealthequity\images\` directory
- No cloud storage or CDN for images (served from same origin)
- Image formats: PNG, JPG, WebP

**Caching:**
- None configured in codebase
- Relies on browser cache headers from hosting platform

## Authentication & Identity

**Auth Provider:**
- None - No user authentication required

**Contact Form Security:**
- HubSpot handles form submission security
- No session management or user accounts

## Monitoring & Observability

**Error Tracking:**
- None detected - No Sentry, LogRocket, Rollbar, or similar

**Logs:**
- None - No logging service integrated
- Browser console errors only (standard browser DevTools)

**Analytics:**
- None detected - No Google Analytics, Mixpanel, Segment, or similar tracking

## CI/CD & Deployment

**Hosting:**
- Not specified in codebase (could be any static host: GitHub Pages, Netlify, Vercel, AWS S3, traditional web server)
- Site designed to work on any static file server

**CI Pipeline:**
- None detected - No GitHub Actions, GitLab CI, Jenkins, or similar

**Build Process:**
- None - Direct file serving, no build step required

## Environment Configuration

**Required env vars:**
- None - All configuration is hardcoded

**Secrets location:**
- No secrets files (.env, credentials.json, etc.) detected
- HubSpot Portal ID and Form ID are publicly visible in HTML (not sensitive)

## Webhooks & Callbacks

**Incoming:**
- HubSpot Form submissions - Forms submitted from:
  - `C:\Users\Administrator\elevatedhealthequity\contact.html` - Main contact form
  - `C:\Users\Administrator\elevatedhealthequity\accelerator.html` - Accelerator program inquiry form
  - `C:\Users\Administrator\elevatedhealthequity\workshops.html` - Workshop registration form
  - Workshop subpages in `C:\Users\Administrator\elevatedhealthequity\workshops\` directory

**Outgoing:**
- Redirect to `thank-you.html` on form submission success
- No outbound API calls or webhooks to external services

## Form Pages & Locations

**HubSpot Integration Points:**
1. `C:\Users\Administrator\elevatedhealthequity\contact.html`
   - Form ID: `b6339de7-46ce-4160-8186-09fa4e40883f`
   - Form target element: `#hubspotForm`
   - Submit button text: "Start the Conversation"
   - Post-submit redirect: `thank-you.html`

2. `C:\Users\Administrator\elevatedhealthequity\accelerator.html`
   - Form ID: `b6339de7-46ce-4160-8186-09fa4e40883f` (same form)
   - Form target element: `#hubspotForm`
   - Post-submit redirect: `thank-you.html`

3. `C:\Users\Administrator\elevatedhealthequity\workshops.html`
   - Form ID: `b6339de7-46ce-4160-8186-09fa4e40883f` (same form)
   - Post-submit redirect: `thank-you.html`

## Meta & SEO Configuration

**Open Graph Tags:**
- Implemented on all pages for social sharing
- og:type, og:title, og:description, og:url, og:image, og:site_name

**Twitter Card Tags:**
- twitter:card (summary_large_image)
- twitter:title, twitter:description, twitter:image on all pages

**Canonical URLs:**
- Set on all pages (e.g., `https://elevatedhealthequity.com/index.html`)
- Proper multi-language and CDN handling

**Structured Data:**
- JSON-LD schema implementation detected on some pages
- Used for search engine crawlers and rich snippets

## Static Assets Configuration

**Favicons:**
- `favicon.ico` - Traditional favicon
- `favicon-32x32.png` - 32x32 PNG
- `favicon-16x16.png` - 16x16 PNG
- `apple-touch-icon.png` - 180x180 for Apple devices
- All served from root directory

**Sitemap:**
- `C:\Users\Administrator\elevatedhealthequity\sitemap.xml` - XML sitemap for search engines

**Robots.txt:**
- `C:\Users\Administrator\elevatedhealthequity\robots.txt` - Crawler directives

---

*Integration audit: 2026-02-07*
