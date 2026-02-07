# Architecture

**Analysis Date:** 2026-02-07

## Pattern Overview

**Overall:** Static HTML/CSS/JS website - server-side rendering pattern with no build system or framework

**Key Characteristics:**
- No build tools, frameworks, or transpilation
- Complete CSS embedded inline in each HTML file
- Vanilla JavaScript (ES5-compatible) embedded in HTML
- Self-contained pages: each HTML file is independently navigable
- Progressive enhancement: works without JavaScript
- Responsive design via CSS media queries
- Intersection Observer API for scroll animations

## Layers

**Presentation Layer:**
- Purpose: DOM structure and semantic HTML markup
- Location: `index.html`, `about.html`, `contact.html`, `consulting.html`, `accelerator.html`, `workshops.html`, `thank-you.html`, `privacy-policy.html`, `terms.html`, `glossary.html`, `workshops/*.html` (8 workshop subpages)
- Contains: HTML5 semantics, heading hierarchy, form markup, CTAs, section structure
- Depends on: Google Fonts API (Montserrat, Lora), Images directory
- Used by: All browser requests

**Styling Layer:**
- Purpose: Visual design via CSS media queries and layout system
- Location: Inline `<style>` blocks within each HTML file (26-28 lines of CSS per file)
- Contains: Reset/normalize, design tokens (CSS custom properties), utility classes, responsive breakpoints, animations (reveal on scroll, stat counters), component styles (header, nav, hero, forms, buttons, cards, footer)
- Depends on: Google Fonts preconnect
- Used by: Browser rendering engine

**Interaction Layer:**
- Purpose: Client-side JavaScript for dynamic behavior
- Location: Inline `<script>` tags at end of HTML files (typically after main content)
- Contains: Mobile navigation toggle, Intersection Observer scroll animations, FAQ accordion, testimonial carousel slider, stat counter animation, cost chart animation
- Depends on: Vanilla JavaScript (no dependencies)
- Used by: User interactions (clicks, scrolls, touches)

**Integration Layer:**
- Purpose: External service integration
- Location: `contact.html` and consulting-related pages
- Contains: HubSpot forms embed (Portal ID: 5440022, Form ID: b6339de7-46ce-4160-8186-09fa4e40883f)
- Depends on: `https://js.hsforms.net/forms/v2.js` API
- Used by: Form submission and lead capture

**Asset Layer:**
- Purpose: Images and static media
- Location: `images/` directory
- Contains: Hero images, logos, team photos, partner logos, workshop images, icons
- Used by: Presentation layer image tags

## Data Flow

**Page Request → Rendering → User Interaction:**

1. Browser requests HTML file (e.g., `index.html`)
2. Server returns complete HTML with embedded CSS and JS
3. Browser parses HTML, applies CSS, renders page
4. Inline JavaScript executes:
   - Intersection Observer attaches to reveal elements
   - Mobile nav toggle listeners register
   - FAQ accordion listeners register
   - Testimonial carousel initializes
   - Stat counter initializes (inactive until scroll)
   - Cost chart animation initializes (inactive until scroll)
5. User scrolls/interacts
6. JavaScript triggers animations or state changes
7. Optional: User submits HubSpot form on contact.html
8. Form submission redirects to `thank-you.html`

**State Management:**
- Minimal client-side state: menu open/closed flag, carousel current slide index, animation flags
- No persistent state (no localStorage, no cookies beyond defaults)
- State stored in JavaScript variables scoped to IIFE (Immediately Invoked Function Expression)
- Page navigation is stateless (no SPA state transfer)

## Key Abstractions

**Design Token System (CSS Variables):**
- Purpose: Centralized color, spacing, typography tokens
- Examples: `--navy`, `--gold`, `--cream`, `--header-h`, `--radius`
- Pattern: Defined in `:root` selector, used throughout inline CSS
- Used by: Color assignments, sizing, responsive adjustments

**Utility Classes:**
- Purpose: Reusable CSS class patterns
- Examples: `.container-wide`, `.section-padding`, `.btn`, `.btn-gold`, `.text-gold`, `.text-navy`, `.sr-only`
- Pattern: Class-based styling for common patterns
- Used by: Multiple elements across pages

**Reveal Animation System:**
- Purpose: Unified scroll-triggered reveal animations
- Examples: `.reveal`, `.reveal-subtle`, `.reveal-left`, `.reveal-right`
- Pattern: CSS animations + Intersection Observer JavaScript triggers
- Classes: `.visible` class added by observer to trigger CSS transition
- Used by: Hero text, service cards, team bios, stat cards, CTA sections

**Component Patterns:**
- `.hero` section with background image and overlay content
- `.stat-card` with animated number counter (data attributes drive animation)
- `.service-card` with icon + title + description
- `.faq-item` accordion with expand/collapse state
- `.testimonial-slide` carousel with prev/next navigation
- `.footer` with link sections and branding

## Entry Points

**Home Page:**
- Location: `index.html`
- Triggers: Direct visit to domain root or explicit link
- Responsibilities: Hero introduction, stats overview, solutions/services grid, accelerator program highlight, testimonials carousel, FAQ, CTA sections

**About Page:**
- Location: `about.html`
- Triggers: "About Us" navigation link
- Responsibilities: Mission/vision statement, team bios with photos, organizational values

**Consulting Services Page:**
- Location: `consulting.html`
- Triggers: "Consulting" navigation link
- Responsibilities: Consulting service descriptions, assessment details, strategy development process

**Workshops Landing:**
- Location: `workshops.html`
- Triggers: "Workshops" navigation link (desktop) or collapsible menu (mobile)
- Responsibilities: All workshop catalog overview, filter/browse capabilities (if implemented), individual workshop cards with CTA

**Workshop Subpages (8 total):**
- Locations: `workshops/cultural-competency.html`, `workshops/health-equity.html`, `workshops/sdoh.html`, `workshops/resilience.html`, `workshops/difficult-conversations.html`, `workshops/communication.html`, `workshops/inclusive-leadership.html`, `workshops/implicit-bias.html`
- Triggers: Individual workshop links from workshops.html or navigation dropdown
- Responsibilities: Workshop-specific details, learning objectives, target audience, registration CTA

**Accelerator Program Page:**
- Location: `accelerator.html`
- Triggers: "Accelerator" reference from home page or direct navigation
- Responsibilities: 4-week program details, curriculum outline, outcomes highlight, enrollment CTA

**Contact/Lead Capture:**
- Location: `contact.html`
- Triggers: "Contact Us" CTA buttons throughout site
- Responsibilities: Contact form (HubSpot-powered), onboarding steps explanation, image background

**Thank You Page:**
- Location: `thank-you.html`
- Triggers: Form submission redirect from contact.html
- Responsibilities: Confirmation message, next steps guidance, CTA back to home

**Utility Pages:**
- `privacy-policy.html`: Legal privacy disclosure
- `terms.html`: Legal terms of service
- `glossary.html`: Health equity terminology reference
- `404.html`: Error page for missing resources

## Error Handling

**Strategy:** Graceful degradation

**Patterns:**
- JavaScript animations gracefully degrade if Intersection Observer not supported (fallback: immediately add `.visible` class)
- Forms gracefully depend on HubSpot API; if unavailable, form markup still displays but won't submit
- Images fail gracefully with alt text; page layout doesn't break
- No try/catch blocks needed; vanilla JS is resilient at this scale
- Missing `#` hash links resolve to page scroll but don't break if element missing

## Cross-Cutting Concerns

**Logging:** None implemented - no error tracking or analytics

**Validation:** Client-side form validation handled by HubSpot form library (on `contact.html` only)

**Authentication:** Not applicable - public marketing site

**SEO/Meta:** Comprehensive:
- JSON-LD structured data (FAQPage, Organization, WebSite schemas) on `index.html`
- Open Graph and Twitter Card meta tags on all pages
- Semantic HTML5 elements (header, nav, main, section, footer)
- Canonical URLs on all pages
- Meta descriptions on all pages

**Accessibility:**
- Semantic HTML (buttons not divs, proper heading hierarchy)
- ARIA labels on interactive elements (hamburger button, carousel controls, accordion triggers)
- Alt text on all images
- Screen reader only utility class (`.sr-only`)
- Sufficient color contrast (navy + gold palette tested)
- Keyboard navigation via native HTML elements
- Focus states on interactive elements
