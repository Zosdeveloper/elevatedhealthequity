# Codebase Concerns

**Analysis Date:** 2026-02-07

## Critical Issues

### Form Submission Failure — All Data Lost
**Severity:** Critical
**Files:** `contact.html:467-501`, `about.html:622-660`, `accelerator.html:646-683`, `workshops.html:607-644`, `workshops/cultural-competency.html:741-778`, `workshops/health-equity.html:741-778`, `workshops/implicit-bias.html:741-778`, `workshops/difficult-conversations.html`, `workshops/inclusive-leadership.html`, `workshops/communication.html`, `workshops/resilience.html`, `workshops/sdoh.html`

**Problem:**
All 13 HTML pages contain forms that prevent default submission, validate input on the client side, and display a success message—but never transmit the form data anywhere. Forms call `e.preventDefault()` followed by `alert()` or show an in-page success div, with no `fetch()`, no `<form action="...">`, and no integration with a backend service. Every visitor who fills out a contact form, workshop inquiry, or accelerator request sees a success message while their data is silently discarded.

**Impact:**
- 100% data loss on all form submissions
- Business cannot contact leads or follow up on inquiries
- Lost opportunities and broken customer acquisition funnel
- Users believe their submission was processed when it was not

**Fix approach:**
Integrate a form submission backend: Formspree, Netlify Forms, AWS SES, or custom backend. Add a `fetch()` or `XMLHttpRequest` call in the form submission handler to POST the validated data to an endpoint before showing the success message. Include error handling for failed submissions.

---

### Navigation Links Broken on Non-Root Hosting
**Severity:** Critical
**Files:** All pages (200+ links site-wide)

**Problem:**
Every page uses absolute-root navigation paths (`href="/index.html"`, `href="/workshops.html"`, `href="/contact.html"`) throughout navigation, footers, and CTAs. These links will fail (404) if:
- Site is hosted in a subdirectory (e.g., `example.com/ehe/`)
- Pages are opened via `file://` protocol locally
- Site is deployed to any non-root path

**Impact:**
- Site becomes unusable on any hosting environment except domain root
- Local development and testing via file explorer returns 404s on every link click
- Subdirectory deployments immediately broken

**Fix approach:**
Replace all absolute-root paths with relative paths:
- From root pages: use `index.html`, `contact.html`, `workshops.html`
- From `workshops/` subdirectory: use `../index.html`, `../contact.html`
Alternatively, document that the site must always be deployed at the domain root and add build-time validation.

---

## High Priority

### Missing `select` in CSS Reset — 11 Pages
**Severity:** High
**Files:** `index.html:36`, `workshops.html:36`, `about.html:36`, `glossary.html:36`, `privacy-policy.html:36`, `terms.html:36`, `404.html:36`, plus all 8 workshop subpages line 36

**Problem:**
11 pages have a CSS reset that applies to `button, input, textarea` but omits `select` elements. Two pages (`contact.html:37` and `accelerator.html:37`) correctly include `select`. This creates inconsistent styling: select elements on 11 pages retain the browser default font, borders, and outlines instead of inheriting Montserrat font and custom styling.

**Current (broken):**
```css
button,input,textarea,select{font-family:inherit;font-size:inherit;border:none;outline:none}
```

**Impact:**
- Visual inconsistency across the site (some form pages styled correctly, others not)
- Select dropdowns on inconsistent pages don't match the design system
- Poor user experience and brand inconsistency

**Fix approach:**
Add `select` to the reset rule on all 11 affected pages.

---

### Form Validation Lacks Accessible Error Messages
**Severity:** High
**Files:** `contact.html` (form validation at end of file), `about.html` (form validation at end of file), `accelerator.html` (form validation at end of file), `workshops.html` (form validation at end of file), all 8 workshop subpages

**Problem:**
Form validation provides visual feedback via red borders but no accessible error messages. This violates WCAG 3.3.1 (Error Identification):

- `contact.html`, `workshops.html`, `about.html` (except for `.error-msg` HTML that isn't used): only `borderColor = 'red'`, no text error messages
- `accelerator.html` alone correctly shows `.error-msg` text and uses `.error` CSS class
- Screen readers announce no errors; sighted users only see red borders

**Impact:**
- Screen reader users cannot identify which fields are invalid or why
- WCAG accessibility failure
- Confusing experience for some users

**Fix approach:**
For each invalid field:
1. Add `aria-invalid="true"` attribute
2. Add visible text error message (use existing `.error-msg` spans where available)
3. Use `aria-describedby="fieldname-error"` to link field to error message
4. Standardize validation approach across all pages (follow `accelerator.html` pattern)

---

### Links Indistinguishable from Body Text
**Severity:** High
**Files:** All pages (line 19 in `<style>` tag): `a{color:inherit;text-decoration:none}`

**Problem:**
The global anchor style removes all visual distinction between links and body text. Links in body copy are invisible to sighted users unless they hover. This violates WCAG 1.4.1 (Use of Color).

**Impact:**
- Sighted users cannot identify which text is clickable
- Navigating content by focus is the only way to discover links
- WCAG accessibility failure

**Fix approach:**
Scope the reset to navigation and button contexts only:
```css
nav a, .btn a { color: inherit; text-decoration: none; }
```
For body links, apply: `a { color: var(--gold); text-decoration: underline; }` or similar distinction. Alternatively, add underline to all links and adjust colors as needed.

---

### No Support for `prefers-reduced-motion`
**Severity:** High
**Files:** `index.html:79-86` (scroll reveal animations), `index.html:177-178` (EKG scroll animation), `index.html:249` (partner carousel animation), all pages with `.reveal` classes

**Problem:**
The site uses automatic animations on scroll (`.reveal`, `.reveal-subtle`, `.reveal-left`, `.reveal-right`) and continuous animations (carousel scroll, EKG line). Users who prefer reduced motion (due to vestibular disorders, photosensitive epilepsy, or motion sickness) experience discomfort. Violates WCAG 2.3.3 (Animation from Interactions).

**Impact:**
- Users with motion sensitivity disabled experience discomfort/nausea
- Some users cannot use the site at all
- WCAG accessibility failure

**Fix approach:**
Add to all pages:
```css
@media (prefers-reduced-motion: reduce) {
  .reveal, .reveal-subtle, .reveal-left, .reveal-right {
    opacity: 1;
    transform: none;
    transition: none;
  }
  .partners-track { animation: none; }
  .stats-ekg { animation: none; }
}
```

---

### No Spam Protection on Forms
**Severity:** High
**Files:** All pages with forms

**Problem:**
Once a form backend is integrated (see critical issue #1), open forms without bot protection will immediately be targeted by spam bots filling out contact forms, workshop registrations, and accelerator inquiries with garbage data.

**Impact:**
- Spam fills up the inbox within hours of going live
- Legitimate leads are lost in spam noise
- Email deliverability degrades

**Fix approach:**
Implement one of:
1. Honeypot field: hidden form field that bots fill but humans don't (simple, works well)
2. CAPTCHA: Google reCAPTCHA v3 (silent, doesn't interrupt UX) or v2 (visible checkbox)
3. Rate limiting: IP-based throttling on the backend
4. Client-side validation + server-side verification of submission patterns

---

## Medium Priority

### ~400+ Lines of Duplicate CSS Across 13 Files
**Severity:** Medium (Maintainability)
**Files:** All 13 HTML pages

**Problem:**
Every page duplicates the entire CSS in a `<style>` block: reset rules, design tokens (`:root`), utility classes, header/footer styles, buttons, animations, and page-specific styles. The shared portion (reset, tokens, header, footer, buttons) is ~400 lines, duplicated 13 times. Any design change (color update, layout adjustment, animation tweak) requires editing 13 files.

**Impact:**
- Highest single maintainability risk
- Changes take 13x longer and are error-prone
- Risk of inconsistencies: some pages updated, others forgotten
- No shared version control of component styles

**Example duplications:**
- Reset: `*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}` (line 30 in all files)
- Header: `.site-header{position:fixed;...}` (line 60+ in all files)
- Buttons: `.btn{display:inline-flex;...}` (line 103+ in all files)
- Animations: `.reveal{opacity:0;...}` (line 79-86 in all files)

**Fix approach:**
Extract all shared CSS to `styles.css`:
1. Create `css/shared.css` with reset, design tokens, header, footer, buttons, animations
2. Link on all pages: `<link rel="stylesheet" href="css/shared.css">`
3. Keep only page-specific styles in `<style>` blocks
4. Update all 13 pages once, test, done

This is the single biggest improvement for long-term maintainability.

---

### Unused CSS Custom Properties
**Severity:** Medium (Code Cleanliness)
**Files:** All pages, lines 58-61 in `:root`

**Problem:**
The design token section defines CSS variables that are never used anywhere:
- `--teal-cta: #176a89` — defined but not referenced
- `--logo-blue: hsl(203 60% 45%)` — defined but not referenced
- `--radius: 0.5rem` — defined but not referenced
- `--cream-dark: hsl(203 40% 88%)` — defined but not referenced

**Impact:**
- Unused variables clutter the codebase
- Developers can't tell if they're intentionally unused or accidentally forgotten
- Adds confusion during maintenance

**Fix approach:**
Remove all unused custom properties from `:root` block on all 13 pages. If any are intended for future use, add a comment like `/* reserved for future use */`.

---

### Inconsistent Form Validation UX
**Severity:** Medium (UX Consistency)
**Files:** `contact.html` vs `accelerator.html` form validation logic (end of file)

**Problem:**
Two different approaches to form validation feedback:
- `contact.html`, `about.html`, `workshops.html`: set `borderColor = 'red'` on invalid fields (no text message)
- `accelerator.html`: adds `.error` class and shows visible `.error-msg` text

This creates inconsistent user experience: some forms show only red borders, one form shows error text.

**Impact:**
- Inconsistent and confusing UX across the site
- Users expect different behavior on different pages
- Harder to maintain (two patterns to remember)

**Fix approach:**
Standardize on the `accelerator.html` pattern (visible error text + `.error` class) across all pages. Apply the same validation logic to `contact.html`, `about.html`, and `workshops.html`.

---

### Missing Favicon and Open Graph Meta
**Severity:** Medium (Professional Presentation)
**Files:** All pages (currently attempted but likely not found)

**Problem:**
Pages reference favicon files that may not exist or may not be deployed correctly:
```html
<link rel="icon" type="image/x-icon" href="favicon.ico">
<link rel="icon" type="image/png" sizes="32x32" href="favicon-32x32.png">
```
Browsers request `/favicon.ico` and return 404 if not present. No Open Graph meta tags for social sharing previews (though recent inspection suggests these were added).

**Impact:**
- Browsers log 404 errors for missing favicon
- Social shares don't show a preview image/title
- Less professional appearance in browser tabs
- Search engines may not handle metadata correctly

**Fix approach:**
1. Ensure favicon files exist at root: `favicon.ico`, `favicon-32x32.png`, `favicon-16x16.png`, `apple-touch-icon.png`
2. Verify Open Graph meta tags are present on all pages (appears to be done already)
3. Verify canonical URLs are set (appears to be done already)

---

### Hero Min-Height Inconsistency
**Severity:** Medium (Visual Consistency)
**Files:** `workshops.html:121` vs `index.html:121`

**Problem:**
`.hero` min-height differs between pages:
- `index.html`: `.hero{...min-height:55vh;}`
- `workshops.html`: `.hero{...min-height:100vh;}`

On workshops page, the hero forces a full viewport scroll before reaching content, while on the home page content is reachable without scrolling.

**Impact:**
- Inconsistent experience across pages
- Workshops page may be harder to scan quickly on mobile
- Intentionality unclear (could be a bug or intentional design)

**Fix approach:**
Review design intent and standardize. If intentional, document. If not, set both to the same value.

---

### Incomplete Sentence in Content
**Severity:** Medium (Content Quality)
**Files:** `accelerator.html:339`

**Problem:**
"Understand how recent executive orders, including restrictions on DEI programs, gender-affirming care, and civil rights enforcement." — Missing verb clause after "enforcement." Sentence is grammatically incomplete.

**Impact:**
- Looks unprofessional
- Reader confusion about the intended meaning

**Fix approach:**
Complete the sentence with a verb clause, e.g., "...affect your organization." or "...impact your strategic planning."

---

### Inline Style Duplication on CTA Button
**Severity:** Medium (Code Cleanliness)
**Files:** `index.html:661`

**Problem:**
A button element has both a class and inline styles that duplicate the class properties:
```html
<button class="btn-gold-lg" style="height:3.5rem;padding:0 2.5rem;font-size:1rem">
```
The inline `style` duplicates what `btn-gold-lg` already defines.

**Impact:**
- Unnecessary code duplication
- Makes CSS less maintainable (change in two places instead of one)
- Increases page size slightly

**Fix approach:**
Remove inline styles and rely on the `btn-gold-lg` class alone.

---

### Select Value Inconsistency
**Severity:** Low (Consistency)
**Files:** `about.html:501`

**Problem:**
Form select options use both raw ampersand and HTML entity encoding inconsistently:
```html
value="Workshops & Training"        <!-- raw & -->
value="Training &amp; Consulting"  <!-- encoded -->
```

**Impact:**
- Inconsistent data submission (one submits `&`, other submits `&amp;`)
- May cause data handling issues in backend
- Functionally works but looks unprofessional

**Fix approach:**
Standardize to `&amp;` everywhere for consistency in data values.

---

### Horizontal Steps Alignment on Contact Form
**Severity:** Low (Visual Polish)
**Files:** `contact.html:147`

**Problem:**
The horizontal progress steps use `calc(16.666%)` for horizontal offset to position the connecting line. This calculation may not align perfectly at all tablet breakpoints, especially with padding/margin variations.

**Impact:**
- Minor visual misalignment on some viewport sizes
- Usually imperceptible but looks slightly off under inspection

**Fix approach:**
Test at all responsive breakpoints (mobile, tablet, desktop). If misaligned, adjust the calculation or use absolute positioning anchored to step elements rather than a percentage offset.

---

### Image Onerror Fallback Inconsistency
**Severity:** Low (Error Handling)
**Files:** `workshops/cultural-competency.html`, `workshops/implicit-bias.html` (use `onerror` fallback), vs `workshops/health-equity.html`, `workshops/inclusive-leadership.html`, `workshops/resilience.html`, etc. (no fallback)

**Problem:**
Some workshop pages include `onerror` handlers to fall back to an alternative image if the primary image fails to load:
```html
<img src="..." onerror="this.onerror=null;this.src='fallback.jpg'">
```
Other workshop pages lack this fallback. Inconsistent error handling.

**Impact:**
- Some pages gracefully degrade on broken images, others show broken image icons
- Inconsistent reliability across the site

**Fix approach:**
Add `onerror` fallback to all workshop pages for consistency, or remove from all if fallback images aren't critical.

---

### Header Height Mismatch
**Severity:** Low (Layout)
**Files:** All pages

**Problem:**
The header height is hardcoded as `--header-h: 56px` (mobile) and `72px` (tablet+), but the actual logo height varies responsively:
- Mobile: `2.5rem` (40px)
- Tablet: `3.5rem` (56px)
- Desktop: `4.5rem` (72px)

At certain breakpoints, content may be positioned behind the variable-height header, causing text to overlap or be cut off.

**Impact:**
- Content may be hidden behind the header on some viewport sizes
- Layout shifts as header height changes
- Padding calculations may be off

**Fix approach:**
Measure actual header height with all responsive logo sizes and update the `--header-h` calculation to match, or use `max-height` to prevent content overlap.

---

### Form Novalidate Disables Native Validation
**Severity:** Low (Resilience)
**Files:** All pages with `<form novalidate>`

**Problem:**
All forms have the `novalidate` attribute, which disables browser native validation. If JavaScript fails to load or execute, forms have zero validation and can be submitted with invalid data (empty required fields, invalid emails, etc.).

**Impact:**
- No fallback validation if JS fails
- Invalid data could be submitted to backend if submission backend is added without JS working
- Poor resilience

**Fix approach:**
Remove `novalidate` to enable native HTML5 validation as a fallback. JavaScript validation can still enhance UX with custom messages, but native validation provides a safety net.

---

## Fragile Areas

### Form Submission JavaScript
**Files:** All pages (form submission code at end of `<script>` tag)
**Why fragile:**
- Current form handlers don't transmit data anywhere (no backend integration)
- Adding a backend will require changes to all 13 files
- No centralized form handler logic
- Inconsistent validation across pages

**Safe modification:**
Before integrating a backend, refactor form handling into a shared approach. Consider extracting form logic to an external `js/forms.js` file and include it on all pages, then modify only that one file to connect to the backend.

---

### CSS Reset and Design Tokens
**Files:** All pages (lines 26-63 in `<style>` tag)
**Why fragile:**
- Duplicated across 13 files
- Design token changes (colors, fonts, spacing) must be updated 13 times
- Easy to miss a page and create inconsistency
- Risk of drift: one page updated, others forgotten

**Safe modification:**
Extract to external `css/shared.css` first (big effort, but eliminates fragility forever). Then make changes in one place.

---

### Navigation Links
**Files:** All pages (header, footer, CTA sections)
**Why fragile:**
- Absolute-root paths (`/index.html`) hard-coded in 200+ places
- Changing hosting location requires finding and updating all paths
- Error-prone manual process
- High risk of missing some links

**Safe modification:**
Use relative paths instead. Test with a local file:// open to ensure links work on non-web-server environments.

---

## Test Coverage Gaps

**Unit/Integration Testing:** Not detected
**Files:** No test files (`.test.js`, `.spec.js`) found in the codebase.

**What's not tested:**
- Form validation logic (every page)
- Form submission handling (all forms)
- Scroll reveal animations
- Mobile navigation open/close behavior
- Dropdown menu interactions
- Image fallback logic (partial implementation)
- Link navigation and routing

**Risk:**
- No regression tests when forms are integrated with a backend
- Changes to validation logic could break silently
- Mobile navigation could become non-functional without notice
- Animation changes could break on specific viewport sizes

**Priority:** Medium — Add test files for form validation and navigation behavior once form backend is integrated.

---

*Concerns audit: 2026-02-07*
