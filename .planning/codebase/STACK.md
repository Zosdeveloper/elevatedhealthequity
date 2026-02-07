# Technology Stack

**Analysis Date:** 2026-02-07

## Languages

**Primary:**
- HTML5 - All page structure (`*.html` files)
- CSS3 - Inline styling within `<style>` tags in each HTML file
- JavaScript (ES5) - Vanilla JavaScript, no frameworks or bundlers

**No secondary languages detected** - This is a pure static site with no backend runtime.

## Runtime

**Environment:**
- Static HTML hosting (no application server required)
- Runs directly in web browsers (all modern browsers with ES5 support)
- Compatible with any static file server or CDN

**Package Manager:**
- None - No npm, yarn, pip, or other package managers in use

## Frameworks

**Core:**
- None - No frameworks used
- Pure static HTML/CSS/JavaScript site

**Build/Dev:**
- None - No build tooling detected
- No bundlers, transpilers, or preprocessors

## Key Dependencies

**Font Delivery:**
- Google Fonts API - Montserrat and Lora font families
  - Loaded via `https://fonts.googleapis.com/css2`
  - Preconnect optimization to `https://fonts.googleapis.com` and `https://fonts.gstatic.com`

**Form Integration:**
- HubSpot Forms v2 JavaScript SDK
  - Loaded from `https://js.hsforms.net/forms/v2.js`
  - Used on contact pages for lead capture

**No other external dependencies** - No analytics, tracking libraries, or third-party JavaScript frameworks detected.

## Configuration

**Environment:**
- No configuration files needed
- All settings hardcoded in HTML/CSS

**Build:**
- No build configuration (no package.json, tsconfig.json, babel.config.js, etc.)
- Static assets served as-is

**Static Assets:**
- Images directory: `C:\Users\Administrator\elevatedhealthequity\images\` (PNG, JPG, WebP formats)
- Favicons: ico and png formats at root
- Robots.txt: `C:\Users\Administrator\elevatedhealthequity\robots.txt`
- Sitemap.xml: `C:\Users\Administrator\elevatedhealthequity\sitemap.xml`

## Platform Requirements

**Development:**
- Any text editor (VS Code, Sublime, etc.)
- No build server needed
- No package installation required

**Production:**
- Static file hosting (GitHub Pages, Netlify, Vercel, AWS S3, traditional web server)
- HTTP/HTTPS support
- Serves HTML, CSS (inline), JavaScript, and image assets
- No database or backend server required

## Code Organization

**All CSS:** Inline within `<style>` tags in HTML head sections - no external stylesheets

**All JavaScript:** Inline within `<script>` tags at end of HTML body - no external JS files

**No CSS preprocessors** (SASS, LESS, PostCSS) in use

**No JavaScript frameworks** (React, Vue, Angular, Svelte) - pure vanilla JavaScript using:
- Intersection Observer API for scroll animations
- Event listeners for interactions
- DOM manipulation for dynamic effects

---

*Stack analysis: 2026-02-07*
