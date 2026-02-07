# Coding Conventions

**Analysis Date:** 2026-02-07

## HTML Structure

**DOCTYPE & Meta:**
- All files start with `<!DOCTYPE html>` and `<html lang="en">`
- Standard meta tags included on every page: charset, viewport, favicon refs, title, description, canonical, OG tags, Twitter tags
- Example head structure in all files: `index.html`, `about.html`, `contact.html`, `consulting.html`, `accelerator.html`, `workshops.html`

**File Organization:**
```html
<head>
  <!-- favicon/icons -->
  <title>Page Title | Elevated Health Equity</title>
  <meta name="description" content="...">
  <!-- canonical & OG/Twitter meta -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <style>/* inline CSS only */</style>
</head>
<body>
  <!-- header, main content, footer -->
  <script>
    /* inline JavaScript only */
  </script>
</body>
</html>
```

**All CSS is inline** - no external stylesheets. All CSS lives in `<style>` tags within `<head>`. Each page contains its own complete CSS (repeated across files).

**All JavaScript is inline** - no external JS files. All JavaScript lives in `<script>` tags at end of `<body>`. Wrapped in IIFE (Immediately Invoked Function Expression) with `'use strict'`.

## CSS Conventions

**CSS Organization Within `<style>` Tags:**
CSS is organized in logical sections with comment headers:
```css
/* ============================================================
   RESET & BASE
   ============================================================ */
/* ============================================================
   DESIGN TOKENS (CSS custom properties)
   ============================================================ */
/* ============================================================
   UTILITY
   ============================================================ */
/* ============================================================
   SCROLL ANIMATIONS
   ============================================================ */
/* ============================================================
   TYPOGRAPHY HELPERS
   ============================================================ */
/* ============================================================
   BUTTONS
   ============================================================ */
/* ============================================================
   HEADER
   ============================================================ */
/* ============================================================
   FOOTER
   ============================================================ */
/* ============================================================
   MOBILE RESPONSIVE — max-width:767px
   ============================================================ */
```

**Design Tokens (CSS Variables):**
All colors and dimensions use CSS custom properties defined in `:root`:
```css
:root{
  --navy:hsl(210 50% 18%);           /* Primary dark color #172A37 */
  --navy-light:hsl(210 45% 25%);     /* Lighter navy */
  --navy-dark:hsl(210 55% 12%);      /* Darker navy */
  --gold:hsl(42 90% 55%);             /* Primary accent #ECBC0A */
  --gold-light:hsl(45 100% 70%);     /* Lighter gold */
  --gold-dark:hsl(38 85% 45%);       /* Darker gold */
  --cream:hsl(203 47% 93%);          /* Light background */
  --cream-dark:hsl(203 40% 88%);     /* Slightly darker cream */
  --bg:hsl(210 25% 97%);             /* Page background */
  --fg:hsl(210 40% 15%);             /* Primary text color */
  --muted-fg:hsl(210 15% 45%);       /* Muted/secondary text */
  --card:hsl(0 0% 100%);             /* Card backgrounds */
  --border:hsl(210 20% 88%);         /* Border color */
  --primary-fg:hsl(45 100% 96%);     /* Light text (on dark bg) */
  --teal-cta:#176a89;                /* Alternative CTA color */
  --logo-blue:hsl(203 60% 45%);      /* Logo accent */
  --radius:0.5rem;                   /* Border radius utility */
  --header-h:56px;                   /* Mobile header height */
}
@media(min-width:768px){:root{--header-h:72px}}  /* Desktop header height */
```

**Color Palette Usage:**
- **Navy (#172A37 / `--navy`):** Primary color for headers, footer, text, borders
- **Gold (#ECBC0A / `--gold`):** Accent color for CTAs, highlights, underlines, hover states
- **Cream (#EBF2F8 / `--cream`):** Light backgrounds, hover states
- **Background (#F8FAFB / `--bg`):** Page background
- Text uses `--fg` (dark) and `--muted-fg` (secondary)

**Typography:**
- Font family: `'Montserrat', system-ui, sans-serif` for all body text and headings
- Secondary font: `'Lora', Georgia, serif` for script/italic text (utility class `.text-script`)
- Imported from Google Fonts with weights: Montserrat (300, 400, 500, 600, 700, italic); Lora (400-700, italic)
- Example from `index.html` line 25: `<link href="https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500;1,600&family=Montserrat:ital,wght@0,300;0,400;0,500;0,600;0,700;1,500&display=swap">`

**Responsive Design Approach:**
- Mobile-first approach with breakpoints: 768px (tablet), 1024px (desktop), 480px (mobile max)
- Breakpoints used consistently: `@media(min-width:768px)`, `@media(min-width:1024px)`, `@media(max-width:767px)`, `@media(max-width:480px)`
- Example from `index.html` line 63: `@media(min-width:768px){:root{--header-h:72px}}`
- Example responsive section from `index.html` lines 439-499 shows stacked mobile, multi-column desktop layout

**CSS Utilities:**
- `.container-wide` - max-width container with responsive padding (1.5rem mobile, 2rem tablet, 3rem desktop)
- `.section-padding` - standard section vertical padding (3.5rem mobile, 7rem tablet, 8rem desktop)
- `.sr-only` - screen reader only hidden content
- `.text-gold`, `.text-navy`, `.text-muted` - color utilities
- `.text-script` - italic Lora font styling
- `.text-gold-line` - gold underline with background-image gradient

**Scroll Animation Classes:**
- `.reveal` - fade in + slide down (30px), 0.6s ease-out
- `.reveal-subtle` - fade in + slide down (16px), 0.5s ease-out
- `.reveal-left` - fade in + slide in from left (-40px), 0.7s cubic-bezier
- `.reveal-right` - fade in + slide in from right (40px), 0.7s cubic-bezier
- `.delay-1`, `.delay-2`, `.delay-3` - staggered animation delays (0.15s, 0.3s, 0.45s)
- Implemented with CSS transitions: `opacity .6s ease-out, transform .6s ease-out`
- JavaScript adds `.visible` class to trigger animations

**Button Styling:**
- `.btn-gold` - gold background, navy text, uppercase, 3rem height, rounded (border-radius: 9999px)
- `.btn-gold-lg` - larger variant, 3.5rem height, 1rem font size
- `.btn-gold-outline` - transparent background, gold border, transforms to solid on hover
- All buttons use: `display:inline-flex`, `gap:.5rem`, `font-weight:600`, `transition:all .3s`
- SVG icons inside buttons animate with `transform:translateX(4px)` on hover

**Specificity & Naming:**
- Use simple class names: `.faq-item`, `.stat-card`, `.service-card`, `.outcome-card`, `.testimonial-slide`
- Nested selectors use camelCase or kebab-case consistently: `.faq-trigger`, `.faq-answer`, `.faq-icon`
- Child selectors use direct child combinator: `.nav-dropdown>a`, `.footer-col h4`
- Hover/pseudo-classes: `:hover`, `:active`, `.open` (JavaScript toggle class)
- State classes: `.visible` (scroll animation trigger), `.open` (accordion/dropdown), `.active` (nav)

**Media Query Pattern:**
```css
/* Default (mobile) styling */
.element { ... }

/* Tablet and up */
@media(min-width:768px){ .element { ... } }

/* Desktop and up */
@media(min-width:1024px){ .element { ... } }

/* Mobile only overrides (grouped at bottom) */
@media(max-width:767px){
  .element { ... }
  .other-element { ... }
}
```

## JavaScript Conventions

**Structure & Scope:**
- All JavaScript wrapped in IIFE: `(function(){ 'use strict'; ... })();`
- Uses `'use strict';` at top of function
- Variables declared with `var` (no `let`/`const`)
- Simple variable naming: `hamburger`, `mobileNav`, `menuOpen`, `current`, `total`

**DOM Selection Pattern:**
```javascript
var element = document.getElementById('elementId');
var elements = document.querySelectorAll('.class-selector');
var element = document.querySelector('.single-selector');
```

**Event Handling:**
```javascript
element.addEventListener('click', function(){
  // event handler code
});

// Query and loop pattern:
elements.forEach(function(el){
  el.addEventListener('click', function(){
    // handler
  });
});
```

**Intersection Observer Pattern (Used for Scroll Animations & Stat Counters):**
```javascript
if('IntersectionObserver' in window){
  var obs = new IntersectionObserver(function(entries){
    entries.forEach(function(e){
      if(e.isIntersecting){
        e.target.classList.add('visible');
        obs.unobserve(e.target);
      }
    });
  }, {threshold:0.12});

  reveals.forEach(function(el){ obs.observe(el); });
} else {
  // Fallback for browsers without IntersectionObserver
  reveals.forEach(function(el){ el.classList.add('visible'); });
}
```

**Common Patterns:**

**Mobile Navigation Toggle:**
```javascript
var hamburger = document.getElementById('hamburger');
var mobileNav = document.getElementById('mobileNav');
var menuOpen = false;

hamburger.addEventListener('click', function(){
  menuOpen = !menuOpen;
  mobileNav.classList.toggle('open', menuOpen);
  // Update hamburger icon (SVG toggle between menu/X)
});

// Close on link click
mobileNav.querySelectorAll('a').forEach(function(link){
  link.addEventListener('click', function(){
    menuOpen = false;
    mobileNav.classList.remove('open');
  });
});
```

**FAQ Accordion (Single Open):**
```javascript
var items = document.querySelectorAll('.faq-item');
items.forEach(function(item){
  var trigger = item.querySelector('.faq-trigger');
  var answer = item.querySelector('.faq-answer');
  trigger.addEventListener('click', function(){
    var isOpen = item.classList.contains('open');
    // Close all
    items.forEach(function(other){
      other.classList.remove('open');
      other.querySelector('.faq-trigger').setAttribute('aria-expanded','false');
      other.querySelector('.faq-answer').style.maxHeight = null;
    });
    // Open clicked if it was closed
    if(!isOpen){
      item.classList.add('open');
      trigger.setAttribute('aria-expanded','true');
      answer.style.maxHeight = answer.scrollHeight + 'px';
    }
  });
});
```

**Testimonial Slider (Auto-rotate with Navigation):**
```javascript
var tcTrack = document.getElementById('testimonialsTrack');
var current = 0;
var autoTimer = null;

function goTo(idx){
  current = ((idx % total) + total) % total;
  tcTrack.style.transform = 'translateX(-' + (current * 100) + '%)';
  tcCounter.textContent = (current + 1) + ' / ' + total;
  tcProgress.style.width = (((current + 1) / total) * 100) + '%';
  resetAuto();
}

// Touch/swipe support
tcTrack.addEventListener('touchstart', function(e){ startX = e.touches[0].clientX; }, {passive:true});
tcTrack.addEventListener('touchend', function(e){
  var diff = startX - e.changedTouches[0].clientX;
  if(Math.abs(diff) > 50){ goTo(diff > 0 ? current + 1 : current - 1); }
});
```

**Number Counter Animation:**
```javascript
function animateCounter(el){
  var target = parseInt(el.getAttribute('data-target'), 10);
  var prefix = el.getAttribute('data-prefix') || '';
  var suffix = el.getAttribute('data-suffix') || '';
  var duration = 1600;
  var frame = 0;

  function easeOutQuart(t){ return 1 - Math.pow(1 - t, 4); }

  var counter = setInterval(function(){
    frame++;
    var progress = easeOutQuart(frame / totalFrames);
    var current = Math.round(progress * target);
    el.textContent = prefix + current + suffix;
    if(frame === totalFrames) clearInterval(counter);
  }, frameDuration);
}
```

**Animation Frame Pattern (Cost Chart):**
```javascript
function animateCount(el, target, suffix, duration){
  var start = 0;
  var startTime = null;
  function step(ts){
    if(!startTime) startTime = ts;
    var progress = Math.min((ts - startTime) / duration, 1);
    var ease = 1 - Math.pow(1 - progress, 3);
    var current = Math.round(start + (target - start) * ease);
    el.textContent = '$' + current + suffix;
    if(progress < 1) requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}
```

**HTML Attribute Usage in JavaScript:**
- Data attributes for configuration: `data-target`, `data-prefix`, `data-suffix`, `data-delay`, `data-special`
- Example: `<span class="stat-number" data-target="85" data-suffix="%" data-prefix="$"></span>`
- Retrieved with: `el.getAttribute('data-target')`

**Accessibility Attributes:**
- FAQ items use `aria-expanded="true"|"false"` on triggers
- Set in JavaScript: `trigger.setAttribute('aria-expanded','true')`

**Comments Style:**
```javascript
/* ----------------------------------------------------------
   SECTION NAME (Uppercase)
   ---------------------------------------------------------- */
// Line comments for inline explanations
```

## CSS Minification Note

CSS and JavaScript are **not minified** - they are human-readable and included directly in HTML files. All formatting preserved for readability.

## File Naming

**HTML Files:**
- Lowercase, hyphenated: `index.html`, `about.html`, `contact.html`, `consulting.html`, `accelerator.html`, `workshops.html`, `thank-you.html`, `privacy-policy.html`, `terms.html`, `glossary.html`, `404.html`
- Workshop subpages: `workshops/health-equity.html`, `workshops/cultural-competency.html`, etc.

**Images:**
- Lowercase, hyphenated: `hero-homepage.webp`, `logo.svg`, `icon-*.svg`
- Stored in `images/` directory

**No External Dependencies:**
- No npm packages
- No build tools
- No framework libraries
- Google Fonts only external resource (preconnected for performance)
- HubSpot forms embedded via script tags on form pages

---

*Convention analysis: 2026-02-07*
