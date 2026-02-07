# Codebase Structure

**Analysis Date:** 2026-02-07

## Directory Layout

```
elevatedhealthequity/
├── images/                    # All website images and assets
│   ├── [Hero & Page Images]   # Hero backgrounds, page-specific imagery
│   ├── partners/              # Partner organization logos (13+ partners)
│   └── team/                  # Team member headshots (4 team members)
├── workshops/                 # Workshop subpage directory (8 pages)
├── [Page HTML Files]          # 11 main page files at root
├── favicon.ico                # Browser tab icon
├── [favicon variants]         # favicon-32x32.png, favicon-16x16.png, apple-touch-icon.png
├── .git/                      # Git repository (not committed to output)
├── .claude/                   # Claude project configuration
├── .planning/                 # Planning documents (this file)
└── .gitignore                 # Git exclusions
```

## Directory Purposes

**Root Directory:**
- Purpose: Contains all main HTML pages and configuration files
- Contains: HTML pages, favicon files
- Key files: `index.html` (entry point), `contact.html` (lead capture), `workshops.html` (catalog)

**`images/`:**
- Purpose: Centralized image asset repository
- Contains: JPG, JPEG, PNG, WebP formats
- Key files:
  - `elevated-health-equity-logo.png` - Primary branding logo
  - `hero-homepage.webp` - Main page hero image
  - `framework-diagram.png` - Solution framework visual
  - `workshop-*.jpg` / `workshop-*-new.png` - Workshop category images
  - `partner-*.png` - Partner organization logos (separate branded + transparent variants)
  - `team/*.png` - Team member photos
  - Service card images: `service-*.png`

**`images/partners/`:**
- Purpose: Partner logo asset collection
- Contains: 13+ partner organization logos in both full color and transparent variants
- Examples: `abrazo-health.png`, `cdc.png`, `cvs-health.png`, `united-healthcare.png`
- Naming pattern: Company name in kebab-case, `*-tc.png` suffix for transparent/white variants

**`images/team/`:**
- Purpose: Team member profile images
- Contains: JPEG and PNG team headshots
- Members: `brenna-gauchat.png`, `doran-ricks.jpeg`, `pamela-abner.png`, `rhonda-moret.png`

**`workshops/`:**
- Purpose: Individual workshop subpage collection
- Contains: 8 standalone HTML pages
- Pages:
  1. `communication.html` - Clear Connections: Elevating Communication Skills
  2. `cultural-competency.html` - Cultural Competency & Humility
  3. `difficult-conversations.html` - Difficult Conversations
  4. `health-equity.html` - Health Equity Fundamentals
  5. `implicit-bias.html` - Overcoming Implicit Bias
  6. `inclusive-leadership.html` - Inclusive Leadership
  7. `resilience.html` - Building Resilience
  8. `sdoh.html` - Social Determinants of Health

## Key File Locations

**Entry Points:**
- `index.html`: Primary entry point, home page with hero, stats, solutions, accelerator, testimonials, FAQ
- `workshops.html`: Workshops catalog and browsing page
- `404.html`: Error page for missing routes

**Configuration:**
- `favicon.ico`, `favicon-32x32.png`, `favicon-16x16.png`, `apple-touch-icon.png`: Favicons for browsers and devices

**Core Logic (Pages):**
- `contact.html`: Lead capture page with HubSpot form integration
- `consulting.html`: Consulting services overview
- `about.html`: Team and organizational information
- `accelerator.html`: Accelerator program details

**Utility Pages:**
- `thank-you.html`: Form submission confirmation (redirect target from contact.html)
- `privacy-policy.html`: Privacy policy legal page
- `terms.html`: Terms of service legal page
- `glossary.html`: Health equity terminology reference

**Testing:**
- None - static site, no test files

## Naming Conventions

**Files:**
- HTML pages: `[page-name].html` (kebab-case)
  - Examples: `index.html`, `about.html`, `consulting.html`, `workshops.html`
- Workshop pages: `workshops/[workshop-name].html`
  - Examples: `workshops/health-equity.html`, `workshops/implicit-bias.html`
- Images: `[purpose]-[variant].extension`
  - Examples: `hero-homepage.webp`, `workshop-cultural.jpg`, `workshop-cultural-new.png`
- Partner images: `[company-name].png` or `[company-name]-tc.png` (transparent)
  - Examples: `cdc.png`, `cvs-health-tc.png`
- Team images: `[first-name]-[last-name].extension`
  - Examples: `brenna-gauchat.png`, `doran-ricks.jpeg`

**Directories:**
- Lowercase, kebab-case: `images/`, `workshops/`, `.planning/`
- Semantic subdirectories: `images/partners/`, `images/team/`

**CSS Classes (Inline Styles):**
- Container: `.container-wide`
- Sections: `.section-padding`
- Buttons: `.btn`, `.btn-gold`, `.btn-gold-lg`, `.btn-gold-outline`
- Typography: `.text-gold`, `.text-navy`, `.text-muted`, `.text-script`, `.text-gold-line`
- Animations: `.reveal`, `.reveal-subtle`, `.reveal-left`, `.reveal-right`
- Animation delays: `.delay-1`, `.delay-2`, `.delay-3`
- Layout: `.hero`, `.stat-card`, `.service-card`, `.faq-item`, `.footer`
- Accessibility: `.sr-only` (screen reader only)

**JavaScript Variables/Functions:**
- Camel case: `hamburger`, `mobileNav`, `menuOpen`, `handleClick()`
- Scope: All code wrapped in IIFE `(function(){ ... })()`
- Event handlers: Inline `addEventListener` callbacks
- Data attributes: `data-target`, `data-prefix`, `data-suffix`, `data-delay`, `data-count`, `data-special`

## Where to Add New Code

**New Page:**
1. Create new HTML file in root directory: `[page-name].html`
2. Copy structure from existing page (e.g., `about.html`)
3. Update:
   - `<title>` and meta tags (description, OG, Twitter)
   - `<h1>` and page content
   - Header navigation links (mirror existing page navigation)
   - All relative image paths remain same (from root)
4. Test favicon links (use relative paths from root)
5. Inline CSS within `<style>` block - copy base styles from another page
6. Inline JavaScript at end if needed within `<script>` block

**New Workshop Subpage:**
1. Create new file in `workshops/` directory: `workshops/[workshop-name].html`
2. Copy structure from `workshops/communication.html` template
3. Update:
   - `<title>` with workshop name
   - Meta descriptions and OG tags
   - `<h1>` title and hero section
   - Relative paths to parent: `../favicon.ico`, `../images/workshop-*.jpg`
   - Adjust navigation to point to parent pages: `../consulting.html`, `../workshops.html`
   - Learning objectives, target audience, workshop description
4. Inline CSS from parent pages (adjust media breakpoints if needed)
5. Inline JavaScript with any interactive elements specific to workshop

**New Component/Section:**
1. Use existing component patterns as templates:
   - Service cards: Copy `.service-card` HTML structure
   - Stat cards: Copy `.stat-card` structure with data attributes
   - FAQ accordion: Copy `.faq-item` structure
   - Testimonial carousel: Copy carousel HTML + initialization code
2. Add styles inline in the page's `<style>` block
3. Add JavaScript in the page's `<script>` block if interactive
4. Add reveal animation classes (`.reveal`, `.reveal-left`, etc.) to auto-animate on scroll

**New Images:**
1. Place in `images/` directory for general assets
2. Place in `images/partners/` for partner logos
3. Place in `images/team/` for team member photos
4. Reference from HTML with relative paths: `<img src="images/[filename]" alt="description">`
5. Optimize format before commit (WebP for photos, PNG for diagrams/logos)

**Utilities and Shared Code:**
- All utility functions and classes are defined inline within each HTML file
- No separate JavaScript files or utility directories
- Reuse patterns: Copy-paste code from existing pages into new pages
- Common patterns to reuse:
  - `.container-wide` for layout constraint
  - `.section-padding` for vertical spacing
  - `.reveal` + Intersection Observer for scroll animations
  - `.btn-gold` for primary CTA buttons
  - Design tokens in `:root` for colors and spacing

## Special Directories

**`.git/`:**
- Purpose: Version control repository
- Generated: Yes (git init)
- Committed: Yes (full git history)

**`.claude/`:**
- Purpose: Claude project configuration and code review notes
- Generated: Yes (Claude workspace metadata)
- Committed: Yes

**`.planning/`:**
- Purpose: Planning and codebase analysis documents
- Generated: Yes (by GSD orchestrator)
- Committed: Yes (planning documents are tracked)
- Contains: `ARCHITECTURE.md`, `STRUCTURE.md` (and other analysis docs when generated)

**`node_modules/`:**
- Not present - no build system or npm dependencies

## Page Navigation Map

**Root Level Pages (linked from header):**
```
index.html (Home)
├── Link: consulting.html
├── Link: workshops.html (dropdown to subpages)
├── Link: about.html
└── Link: contact.html#contact-form (CTA button)

about.html
├── Link: consulting.html
├── Link: workshops.html
└── Link: contact.html

consulting.html
├── Link: index.html
├── Link: workshops.html
├── Link: about.html
└── Link: contact.html

contact.html
├── Form submission → thank-you.html
└── Navigation to other pages

accelerator.html (linked from index.html)

workshops.html (catalog)
└── Links to workshops/[workshop-name].html (8 workshops)

Privacy/Utility Pages (footer links):
├── thank-you.html
├── privacy-policy.html
├── terms.html
├── glossary.html
└── 404.html
```

**Workshop Subpage Navigation:**
```
workshops/[workshop-name].html
├── Navigation links back to: workshops.html, index.html, about.html, consulting.html
└── Relative paths: ../workshops.html
```

## Favicon and Meta Asset Locations

All favicon files in root directory:
- `favicon.ico` - Default favicon
- `favicon-32x32.png` - 32x32 variant
- `favicon-16x16.png` - 16x16 variant
- `apple-touch-icon.png` - Apple/iOS home screen icon

Reference in HTML head:
```html
<link rel="icon" type="image/x-icon" href="favicon.ico">
<link rel="icon" type="image/png" sizes="32x32" href="favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="favicon-16x16.png">
<link rel="apple-touch-icon" sizes="180x180" href="apple-touch-icon.png">
```

For workshop pages, adjust to parent directory:
```html
<link rel="icon" type="image/x-icon" href="../favicon.ico">
```
