# Elevated Health Equity — Design System & Site Architecture Plan

---

## 1. DESIGN SYSTEM

### 1.1 Color Palette

| Role | Color | Hex / Value |
|------|-------|-------------|
| **Primary Navy** | Dark navy blue | `#172A37` |
| **Primary Deep Blue** | Deep institutional blue | `#002B5C` |
| **Accent Gold** | Warm gold (primary accent) | `#ECBC0A` |
| **Accent Gold Hover** | Darker gold (hover state) | `#EDA009` |
| **White** | Backgrounds, cards | `#FFFFFF` |
| **Light Gray** | Subtle section backgrounds | `#F5F5F5` |
| **Medium Gray** | Borders, muted elements | `#C7C9CB` |
| **Dark Text** | Body copy on light backgrounds | `#1F3A4B` |

**Gradients:**
- Hero overlay: `linear-gradient(to right, rgba(23,42,55,1), rgba(23,42,55,0.54), rgba(23,42,55,0))`
- CTA overlay: `linear-gradient(to right, rgba(0,43,92,0.95), rgba(0,43,92,0.7))`
- Navy blend: `linear-gradient(to right, rgba(23,42,55,1), rgba(0,43,92,1))`
- Subtle section wash: `linear-gradient(to bottom, rgba(192,193,195,0.2), rgba(192,193,195,0))`

### 1.2 Typography

| Usage | Font Family | Weights | Notes |
|-------|------------|---------|-------|
| **Headings** | **Oswald** (Google Fonts) | 400, 600, 700 | Uppercase or title-case, condensed display font |
| **Body / UI** | **Montserrat** (Google Fonts) | 400, 700 | Clean sans-serif, highly readable |

**Type Scale (Desktop):**
- Hero headline: ~48-56px, Oswald 700, uppercase, white
- Section headings: ~32-40px, Oswald 600-700
- Sub-headings: ~20-24px, Montserrat 700
- Body text: ~16px, Montserrat 400, line-height ~1.6
- Small/caption: ~14px, Montserrat 400

### 1.3 Button Styles

| Type | Style |
|------|-------|
| **Primary CTA** | Gold background (`#ECBC0A`), dark navy text (`#172A37`), bold weight, rounded corners (~4px), hover → `#EDA009` |
| **Secondary CTA** | Transparent/outlined, white border on dark backgrounds, gold border on light backgrounds |
| **Text Link** | Gold color with underline on hover |

### 1.4 Section Patterns

| Pattern | Description |
|---------|-------------|
| **Hero** | Full-width background image, dark gradient overlay (left-heavy), white text left-aligned, gold CTA button. Padding: 75-90px vertical |
| **Stats Bar** | Navy/dark background, 3-column layout with large numbers + descriptions |
| **Content + Image** | Two-column split (text left, image right or vice versa). Image floats right at ~50% width on desktop, stacks on mobile |
| **Card Grid** | White cards on light gray background, 2-3 column grid, subtle shadow or border |
| **Testimonial Carousel** | White cards with quote text, attribution name + company, possibly with headshot |
| **CTA Band** | Full-width dark blue overlay on background image, centered headline + gold button |
| **Process Steps** | 3-step numbered sequence with icons: "Share Your Story → Let's Connect → Your Custom Roadmap" |
| **Logo Slider** | Client/partner logos in horizontal scrolling strip, light gray background |

### 1.5 Spacing Conventions

| Context | Desktop | Tablet/Mobile |
|---------|---------|---------------|
| Section vertical padding | 80-100px | 50-60px |
| Hero vertical padding | 75-90px | 30-60px |
| Inner content max-width | ~1200px centered | Full-width with 20px side padding |
| Card internal padding | 30-40px | 20-30px |
| Element vertical gaps | 20-30px | 15-20px |

### 1.6 Additional Design Details

- **Image treatment**: Lazy-loaded backgrounds via Intersection Observer, `background-size: cover`, `background-position: center`
- **Animations**: Text rotator with "flipUp" effect (1500ms), subtle fade-ins on scroll
- **Icons**: Custom SVG icons for solution areas (flat style, likely gold or white on dark)
- **Forms**: HubSpot embedded forms with custom styling, semi-transparent white container (`rgba(255,255,255,0.8)`) on dark sections
- **Grid system**: Isotope.js for workshop card filtering with fitRows layout

---

## 2. SITE ARCHITECTURE

### 2.1 Navigation Structure

```
[Logo]                                              [Schedule a Strategy Session]

Home  |  Accelerator Program  |  Workshops ▾  |  About Us

                                   └── Navigating with Care: Cultural Competency
                                   └── Health Equity Fundamentals
                                   └── Social Determinants of Health (SDOH)
                                   └── Building Resilience in Healthcare Organizations
                                   └── Managing Difficult Conversations in Healthcare
                                   └── Workplace Communication Skills for Healthcare Teams
                                   └── Inclusive Leadership in Healthcare Organizations
                                   └── Overcoming Implicit Bias in Healthcare
```

- **Header**: Sticky/fixed, navy background, logo left, nav center, gold CTA button right
- **Mobile**: Hamburger menu with slide-out drawer

### 2.2 Page Hierarchy

```
elevatedhealthequity.com/
├── / (Home)
├── /accelerator-program
├── /workshops/
│   ├── /workshops/cultural-competency
│   ├── /workshops/health-equity-fundamentals
│   ├── /workshops/social-determinants-of-health
│   ├── /workshops/building-resilience
│   ├── /workshops/managing-difficult-conversations
│   ├── /workshops/workplace-communication-skills
│   ├── /workshops/inclusive-leadership
│   └── /workshops/overcoming-implicit-bias
├── /about
├── /contact (or modal/form)
└── /privacy-policy
```

### 2.3 Component Library Needed

These are the reusable Elementor sections/templates to build:

| Component | Used On |
|-----------|---------|
| **Global Header** | All pages |
| **Global Footer** | All pages |
| **Hero Section** (full-width image + overlay + CTA) | Home, Accelerator, Workshops, About |
| **Stats Bar** (3-column numbers) | Home |
| **Solutions Grid** (icon + title + description cards) | Home |
| **Program Overview Block** (features + outcomes) | Home, Accelerator |
| **Testimonial Carousel** | Home, Accelerator |
| **CTA Band** (dark background + centered text + button) | All pages |
| **3-Step Process** (numbered with icons) | Home, Accelerator |
| **Workshop Card Grid** (filterable) | Workshops index |
| **Workshop Detail Layout** (hero + content + sidebar) | Workshop subpages |
| **Team Grid** | About |
| **Contact/Strategy Session Form** | Home (embedded), possibly standalone |
| **Logo Slider** | Home, About |

---

## 3. HOME PAGE PLAN

### 3.1 Section-by-Section Layout (Top to Bottom)

1. **Header** — Sticky navy bar, logo left, nav links center, gold "Schedule a Strategy Session" button right

2. **Hero Section** — Full-width background image with dark gradient overlay. Headline: "Redirecting Trends & Elevating Outcomes for Improved [rotating text]". Subtext paragraph. Gold "Start the Conversation" CTA.

3. **Impact Stats Bar** — Dark navy background, 3 columns:
   - "$93B in excess medical care costs"
   - "$42B in untapped productivity"
   - "$612T+ projected annual cost by 2040"

4. **Solutions Section** — Light/white background. Heading: "Solutions to Drive Inclusion & Impact". Intro paragraph. 6-card grid (2×3) with SVG icons:
   - Assessment & Strategy Development
   - Data Analytics & AI
   - Capacity Building Workshops
   - Policy Analysis & Development
   - Stakeholder Engagement
   - Strategic Implementation and Impact

5. **Program Spotlight** — Two-column layout. Left: text about the Health Equity Resilience Accelerator (4-month program description). Right: image placeholder. Gold CTA: "Learn More"

6. **Outcomes Row** — 3 columns with icons/badges:
   - Confident & Empowered
   - Health Equity Leadership Readiness
   - Authentic & Courageous

7. **Testimonials Carousel** — Light gray background. Rotating testimonial cards with quotes, names, and organizations (CDC, Spark Therapeutics, CVS Pharmacy, etc.)

8. **Workshops Overview** — Heading: "Training & Workshops". Grid/list of workshop titles linking to subpages. Brief descriptions.

9. **Resource CTA** — "Navigating the New Climate: A Guide to Reframing Diversity & Inclusion" with "Get Your Copy" button

10. **Engagement Process** — 3-step visual:
    - Step 1: Share Your Story
    - Step 2: Let's Connect
    - Step 3: Your Custom Roadmap

11. **Strategy Session CTA Band** — Full-width dark blue background image + overlay. Centered heading + "Schedule a Strategy Session" gold button → triggers contact form or scrolls to form

12. **Contact Form Section** — Embedded form: Name, Email, Organization, Message. Connected to HubSpot (placeholder for now, will integrate later)

13. **Footer** — Navy background, 3-4 column layout:
    - Column 1: Logo + brief tagline
    - Column 2: Quick links (Home, Accelerator, Workshops, About)
    - Column 3: Contact info / office locations
    - Column 4: Social media icons
    - Bottom bar: © 2026 Elevated Health Equity. All rights reserved. | Privacy Policy

### 3.2 Design Refinements (≤10% Deviation)

The refreshed version will apply these subtle improvements while staying true to the existing visual identity:

- **Sharper typography hierarchy** — Slightly more contrast between heading sizes, tighter line-heights on headlines
- **Refined card styling** — Subtle box-shadow (`0 2px 12px rgba(0,0,0,0.08)`) instead of flat borders; 8px border-radius for a slightly softer feel
- **Smoother transitions** — CSS transitions on buttons and cards (0.3s ease) for hover states
- **Improved spacing rhythm** — Consistent 80px/100px section padding with an 8px base grid
- **Better mobile stacking** — Ensuring all two-column layouts break cleanly at 768px with proper content reordering
- **Subtle scroll animations** — Fade-up on section entry (via Elementor's built-in motion effects, no custom JS needed)
- **Accessibility improvements** — Proper color contrast ratios (WCAG AA), focus states on interactive elements, semantic heading hierarchy

### 3.3 Technical Approach

| Aspect | Approach |
|--------|----------|
| **Page Builder** | Elementor Pro (sections, columns, widgets) |
| **Theme** | Hello Elementor (minimal base theme) |
| **Fonts** | Enqueued via Elementor's Google Fonts integration (Oswald + Montserrat) |
| **Contact Form** | Elementor Form widget with HubSpot integration (via webhook or HubSpot plugin) |
| **Responsive** | Elementor's responsive mode for desktop/tablet/mobile breakpoints |
| **Images** | Placeholder positions marked — you will upload actual images/logo |
| **Custom CSS** | Minimal, only for refinements Elementor can't handle natively |
| **Deliverable** | Elementor-compatible HTML/CSS template with inline comments marking each section for easy recreation in the visual editor |

---

## 4. DELIVERABLE FORMAT

Since this is for WordPress + Elementor, I will build the home page as:

1. **A complete HTML/CSS file** that precisely matches the design system above — this serves as both a visual reference and a code reference for building in Elementor
2. **Section-by-section mapping notes** so each HTML section can be recreated using Elementor widgets (headings, text editors, image widgets, icon boxes, form widgets, etc.)
3. **Custom CSS snippet file** for any styles that need to be added to Elementor's Custom CSS or the theme's Additional CSS

---

## 5. QUESTIONS / DECISIONS NEEDED

Before I build, please confirm:

1. **Logo**: Will you upload the Elevated Health Equity logo, or should I use a text placeholder?
2. **Hero image**: Do you have a specific hero background image, or should I design around a placeholder?
3. **Testimonials**: Should I use the same testimonials from the current site, or will you provide updated ones?
4. **Contact form fields**: Name, Email, Organization, Message — are these the right fields for the "Schedule a Strategy Session" form?
5. **Color adjustments**: The current site uses gold (`#ECBC0A`) as the primary accent. Keep this exactly, or are you open to a very slight warmth adjustment?
6. **Blog section**: The current home page has blog posts at the bottom. Include this on the new site's home page, or omit for now?

---

*Awaiting your approval to proceed to Phase 2 (Home Page Build).*
