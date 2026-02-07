# Testing Patterns

**Analysis Date:** 2026-02-07

## Test Framework Status

**No Testing Framework Detected**

This is a static HTML/CSS/JavaScript website with **no automated testing infrastructure**:

- **No test runner** (Jest, Vitest, Mocha, etc.)
- **No assertion library** (Chai, expect.js, etc.)
- **No test configuration files** (.test.js, .spec.js, jest.config.js, vitest.config.js, etc.)
- **No test directories** (tests/, __tests__/, test/, spec/)
- **No test scripts** in package.json (no package.json exists at all)
- **No CI/CD pipelines** configured for automated testing

## Manual Testing Approach

Since this is a vanilla static site with no build process, quality assurance relies on manual testing:

**Implicit Testing Patterns in Code:**

The codebase includes defensive patterns that suggest manual QA process:

1. **Progressive Enhancement (Intersection Observer fallback):**
   ```javascript
   if('IntersectionObserver' in window){
     var obs = new IntersectionObserver(function(entries){...});
   } else {
     // Fallback for older browsers
     reveals.forEach(function(el){ el.classList.add('visible'); });
   }
   ```
   Pattern used at: `index.html` lines 1361-1373 (scroll reveals), 1494-1509 (stat counters), 1587-1599 (cost chart)

   This indicates testing across different browser capabilities is expected.

2. **Null-checking for optional elements:**
   ```javascript
   if(tcTrack){...}
   if(tcPrev) tcPrev.addEventListener('click', function(){...});
   if(costSection) costObs.observe(costSection);
   ```
   Pattern used throughout `index.html`. Suggests manual testing verifies element presence before binding events.

3. **State tracking to prevent duplicate animations:**
   ```javascript
   var costAnimated = false;
   function animateCostChart(){
     if(costAnimated) return;
     costAnimated = true;
     // animation code...
   }
   function resetCostChart(){
     costAnimated = false;
     // reset code...
   }
   ```
   Pattern at `index.html` lines 1514-1599. Indicates manual testing for repeated scroll animations.

4. **Touch event handling with fallback:**
   ```javascript
   tcTrack.addEventListener('touchstart', function(e){ startX = e.touches[0].clientX; }, {passive:true});
   // Later: diff detection for swipe threshold
   if(Math.abs(diff) > 50){ goTo(...); }
   ```
   Pattern at `index.html` lines 1428-1437. Suggests manual testing on mobile devices for swipe gesture detection.

## Key Interactive Features (Manual Test Checklist)

These features should be manually tested:

**Mobile Navigation:**
- Hamburger toggle opens/closes mobile menu
- Menu closes when link is clicked
- Icon changes from hamburger to X when open
- Location: `index.html` lines 1324-1355

**FAQ Accordion:**
- Only one FAQ item open at a time
- Clicking open item closes it
- Clicking closed item opens it while closing others
- `aria-expanded` attribute toggles correctly
- Answer height animates smoothly (max-height transition)
- Location: `index.html` lines 1376-1395

**Testimonial Slider:**
- Manual navigation (prev/next buttons) advances slides correctly
- Counter shows "X / Total"
- Progress bar updates
- Auto-rotate continues after 6 seconds (resets on manual click)
- Touch/swipe gestures work on mobile (50px threshold)
- Location: `index.html` lines 1400-1440

**Stat Cards Number Counter:**
- Numbers animate from 0 to target value when section scrolls into view
- Uses easeOutQuart easing
- Duration: 1600ms
- Animation resets when scrolling out of view
- Prefix ($) and suffix (%, etc.) display correctly
- Location: `index.html` lines 1445-1509

**Cost of Inaction Chart:**
- SVG line draws with animation
- Dots appear with staggered delays
- End dot pulses with glow effect
- Labels fade in with number counting
- Animation resets when scrolling out and in again
- Special values (different duration) animate correctly
- Location: `index.html` lines 1514-1599

**Scroll Reveal Animations:**
- Elements fade in + slide when scrolling into view (12% threshold)
- Four animation types: `.reveal`, `.reveal-subtle`, `.reveal-left`, `.reveal-right`
- Stagger delays work correctly (delay-1, delay-2, delay-3)
- Each element observes only once (unobserve after trigger)
- Fallback to immediate display if IntersectionObserver not available
- Location: `index.html` lines 1358-1373

**Responsive Design:**
- Test at breakpoints: 480px, 768px, 1024px
- Header height changes: 56px mobile, 72px desktop
- Navigation switches from mobile hamburger to desktop menu at 1024px
- Footer grid layout: 1 column mobile, 2 columns tablet, 4 columns desktop
- Service cards, stat cards, buttons respond appropriately
- No horizontal scrolling at any viewport size
- Location: All files use `@media` queries extensively

## Testing Recommendations

**Given no formal testing framework exists:**

1. **Consider Manual Testing Tools:**
   - Cross-browser testing: BrowserStack, LambdaTest
   - Accessibility: WAVE, Axe DevTools, Lighthouse
   - Performance: PageSpeed Insights, GTmetrix
   - Responsive: Chrome DevTools device emulation, Responsively App

2. **Browser Compatibility to Test:**
   - Chrome/Edge (latest 2 versions)
   - Firefox (latest 2 versions)
   - Safari (latest 2 versions, both macOS and iOS)
   - Mobile browsers (iOS Safari, Chrome Android)
   - Older browsers should fallback gracefully (IntersectionObserver fallback proves this is a concern)

3. **Manual Regression Testing Checklist:**
   - All 7 core pages load without JavaScript errors (check console)
   - Mobile nav works on all devices
   - All forms submit (contact, workshop strategy)
   - All slider/carousel interactions work
   - All animations trigger at correct scroll points
   - Touch/swipe works on tablets and phones
   - Keyboard navigation works (Tab, Enter for focus management)
   - All interactive elements have proper focus states

4. **Performance Testing:**
   - Page loads quickly (Largest Contentful Paint < 2.5s)
   - Scroll animations don't cause jank (60fps)
   - Touch interactions are responsive (< 100ms)
   - No memory leaks on repeated slider interactions

5. **Accessibility Testing:**
   - Screen reader compatibility (NVDA, JAWS, VoiceOver)
   - Keyboard-only navigation works
   - Color contrast meets WCAG AA (navy/gold palette tested)
   - Form labels properly associated
   - ARIA attributes correct (aria-expanded on FAQ, role attributes if needed)

## Test Data / Fixtures

No test data fixtures exist. Manual testing uses real page content.

**Sample Elements for Manual Testing:**

FAQ items live in HTML with this structure:
```html
<div class="faq-item">
  <button class="faq-trigger" aria-expanded="false">Question Text</button>
  <div class="faq-answer">
    <div class="faq-answer-inner">Answer content</div>
  </div>
</div>
```

Stat cards with data attributes:
```html
<div class="stat-card reveal" style="animation-delay:...">
  <div class="stat-number" data-target="85" data-suffix="%"></div>
  <div class="stat-label">Label</div>
</div>
```

Testimonial slides for slider testing:
```html
<div id="testimonialsTrack" class="testimonials-track">
  <div class="testimonial-slide">Slide content...</div>
  <div class="testimonial-slide">Slide content...</div>
</div>
```

## Known Testing Gaps

1. **No unit tests** for JavaScript functions (animateCounter, goTo, easeOutQuart)
2. **No integration tests** for complex interactions (form submission with animations)
3. **No visual regression tests** (UI appearance across browsers)
4. **No performance tests** (animation frame rates, memory usage)
5. **No accessibility automated tests** (ARIA compliance, semantic HTML)
6. **No end-to-end tests** (full user workflows: navigate to contact > scroll through page > fill form)

## Error Handling & Resilience

The code uses defensive programming practices that reduce bug likelihood:

- **Null checks before DOM operations:** All event listener attachments check element existence
- **Feature detection:** IntersectionObserver and other APIs checked before use
- **Timeout/interval cleanup:** Auto-timer for slider properly clears with `clearInterval`
- **CSS fallbacks:** Every animation has a `:hover` or static state if JavaScript fails
- **HTML validation:** All pages are valid HTML (DOCTYPE, proper nesting, form elements)

**Implicit assumption:** Code is manually tested in development before deployment.

---

*Testing analysis: 2026-02-07*

**Recommendation:** Consider implementing automated testing or at minimum a documented manual QA checklist for release processes. Current approach relies entirely on developer care and browser testing discipline.
