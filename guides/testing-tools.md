# Testing Tools for Image Accessibility

No single tool catches everything. Automated tools can find structural failures (missing alt attributes, empty alt on functional images) but cannot judge whether alt text is meaningful, contextually appropriate, or accurate. A complete testing strategy layers automated scanning, manual review, and real screen reader testing.

This reference covers tools in each category with honest assessments of what they do and don't catch.

---

## Table of Contents

- [Browser Extensions](#browser-extensions)
- [Command-Line and Programmatic Tools](#command-line-and-programmatic-tools)
- [Online Testing Tools](#online-testing-tools)
- [Screen Readers](#screen-readers)
- [Contrast Checkers](#contrast-checkers)
- [CI/CD Integration](#cicd-integration)
- [Coverage Matrix](#coverage-matrix)

---

## Browser Extensions

### axe DevTools

- **URL:** https://www.deque.com/axe/devtools/
- **Free/Paid:** Free browser extension; paid professional version available
- **Browser:** Chrome, Firefox, Edge
- **What it checks for images:**
  - Missing alt attributes (`image-alt` rule)
  - `<input type="image">` without alt (`input-image-alt`)
  - Elements with `role="img"` without accessible names (`role-img-alt`)
  - Image links with redundant alt text (`image-redundant-alt`)
  - Objects without alternatives (`object-alt`)
  - Area elements without alt text (`area-alt`)
- **Pros:**
  - Widely regarded as the most accurate automated accessibility tool
  - Zero false positives is a core design goal — if axe flags it, it is genuinely broken
  - Integrates with DevTools panel for in-context diagnosis
  - Rules are mapped to WCAG criteria
- **Cons:**
  - Cannot judge alt text quality — it only detects missing or empty attributes
  - Does not flag poor quality alt text ("photo", "image") in the free version
  - The "Best Practices" category in the paid version catches some quality issues
- **How to use:** Install extension → open DevTools → Axe DevTools tab → Analyze. Or right-click any element → Inspect with axe.

---

### WAVE (Web Accessibility Evaluation Tool)

- **URL:** https://wave.webaim.org/extension/
- **Free/Paid:** Free
- **Browser:** Chrome, Firefox
- **What it checks for images:**
  - Missing alt (`ERROR: Missing alternative text`)
  - Suspicious alt text — filenames, "image", "photo" (`ALERT: Suspicious alternative text`)
  - Empty alt on functional image (`ERROR: Linked image missing alternative text`)
  - Redundant alt on linked images (`ALERT: Redundant alternative text`)
  - Image map areas without alt (`ERROR: Missing alternative text for image map area`)
  - Decorative images with alt text (flagged for manual review)
- **Pros:**
  - Visual overlay makes it easy to see which images have which issues
  - Alerts category catches quality issues that axe misses (suspicious alt text)
  - Shows actual alt text values in the UI — fast for reviewing quality
  - Free with no usage limits
- **Cons:**
  - Higher false positive rate than axe
  - Suspicious alt text detection has both false positives and false negatives
  - Can be slow on pages with many elements
- **How to use:** Install extension → click WAVE icon on any page → review the overlay. The "Details" tab lists all flagged items categorized by severity.

---

### Lighthouse (Chrome DevTools)

- **URL:** Built into Chrome DevTools (no installation needed)
- **Free/Paid:** Free
- **Browser:** Chrome, Edge
- **What it checks for images:**
  - Missing alt attributes (`image-alt` audit — part of Accessibility category)
  - `<input type="image">` without alt
  - Links with only images and no alt text
- **Pros:**
  - Built into every Chrome browser — no setup required
  - Produces a scored report good for tracking improvement over time
  - Integrates into CI via `lighthouse-ci`
  - Good general performance + accessibility combined report
- **Cons:**
  - Smallest ruleset of the three major extensions
  - Does not catch suspicious/poor quality alt text
  - Uses axe-core under the hood for accessibility checks — same coverage as axe
- **How to use:** DevTools → Lighthouse tab → Check "Accessibility" → Analyze page. Or via CLI: `npx lighthouse https://example.com --only-categories=accessibility`

---

### IBM Equal Access Checker

- **URL:** https://www.ibm.com/able/toolkit/tools/
- **Free/Paid:** Free
- **Browser:** Chrome, Firefox
- **What it checks for images:**
  - Non-text content violations (WCAG 1.1.1)
  - Image-of-text detection
  - Several alt text quality heuristics
- **Pros:**
  - More aggressive quality checking than axe or WAVE
  - Flags some alt text quality issues others miss
  - Good for organizations with strong IBM accessibility tooling investments
- **Cons:**
  - Higher false positive rate
  - Less widely used, so smaller community knowledge base
  - UI is less intuitive than axe or WAVE

---

### ARC Toolkit

- **URL:** https://www.paciellogroup.com/toolkit/
- **Free/Paid:** Free (requires registration)
- **Browser:** Chrome
- **What it checks for images:**
  - All 1.1.1 image requirements
  - Highlights all images on page with their alt text visible
  - Color contrast for images of text
- **Pros:**
  - The "Images" tab shows every image and its alt text in a scannable list — excellent for alt text audits
  - Good for manual review workflows
- **Cons:**
  - Requires account registration
  - Less commonly integrated into CI pipelines

---

## Command-Line and Programmatic Tools

### axe-core (npm)

- **URL:** https://github.com/dequelabs/axe-core
- **Free/Paid:** Free, open source (Apache 2.0)
- **What it checks:** Full axe ruleset (same as browser extension)
- **Pros:**
  - Can be run programmatically in any Node.js environment
  - Integrates with Playwright, Puppeteer, Cypress, Jest
  - Returns structured JSON — easy to parse for CI pipelines
  - Used by both WAVE and Lighthouse under the hood
- **Cons:**
  - Requires a running page (headless browser or live URL)
  - Same coverage limitations as the browser extension

```bash
# Quick CLI scan
npx axe https://yoursite.com

# Focused on Level A only
npx axe https://yoursite.com --tags wcag2a

# Output to JSON
npx axe https://yoursite.com --save results.json
```

```javascript
// Playwright integration
const { chromium } = require('playwright');
const AxeBuilder = require('@axe-core/playwright').default;

const browser = await chromium.launch();
const page = await browser.newPage();
await page.goto('https://yoursite.com');

const results = await new AxeBuilder({ page })
  .withTags(['wcag2a', 'wcag2aa'])
  .analyze();

console.log(results.violations);
```

---

### Pa11y

- **URL:** https://pa11y.org/
- **Free/Paid:** Free, open source
- **What it checks:** WCAG 2.1 Level A and AA via HTML_CodeSniffer
- **Pros:**
  - Straightforward CLI — `pa11y https://example.com`
  - Good for quick spot checks and small site audits
  - Can crawl multiple URLs from a config file
  - JSON output for CI integration
- **Cons:**
  - Based on HTML_CodeSniffer, which has slightly different coverage than axe-core
  - Less actively maintained than axe
- **Installation and use:**

```bash
npm install -g pa11y

# Basic scan
pa11y https://yoursite.com

# WCAG 2.1 AA standard
pa11y --standard WCAG2AA https://yoursite.com

# JSON output
pa11y --reporter json https://yoursite.com > results.json
```

---

### Playwright + axe-core (recommended CI stack)

- **URL:** https://playwright.dev/ + https://github.com/dequelabs/axe-core
- **Free/Paid:** Free, open source
- **Why this combination:**
  - Playwright handles browser automation with excellent cross-browser support
  - axe-core provides the accessibility scanning ruleset
  - Together they test real rendered pages, including JavaScript-heavy SPAs
  - Tests can verify specific image accessibility patterns programmatically

```javascript
// Test that all images on a product page have alt text
test('product images have alt text', async ({ page }) => {
  await page.goto('/products/widget');

  const images = await page.locator('img').all();
  for (const img of images) {
    const alt = await img.getAttribute('alt');
    expect(alt).not.toBeNull(); // alt attribute must exist
    // Note: empty string is valid for decorative images
  }
});

// Test with axe-core for full accessibility scan
test('product page has no image accessibility violations', async ({ page }) => {
  await page.goto('/products/widget');
  const results = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa'])
    .analyze();
  expect(results.violations).toHaveLength(0);
});
```

---

### Tenon.io API

- **URL:** https://tenon.io/
- **Free/Paid:** Paid (free trial available)
- **What it checks:** Comprehensive WCAG and Section 508 checks via API
- **Pros:**
  - API-first design — integrates into any CI system
  - Detailed issue reporting with remediation guidance
  - Good for teams that need audit trails and issue tracking
- **Cons:**
  - Paid service
  - Overkill for smaller teams

---

## Online Testing Tools

### WebAIM WAVE (web version)

- **URL:** https://wave.webaim.org/
- **Free/Paid:** Free
- **How to use:** Enter any publicly accessible URL.
- **What it checks:** Same as the WAVE browser extension (see above).
- **Limitation:** Cannot test pages behind authentication or on localhost. Use the browser extension for those.

---

### Deque University axe Monitor

- **URL:** https://www.deque.com/axe/monitor/
- **Free/Paid:** Paid
- **What it does:** Scheduled automated scanning of entire sites, with issue tracking and reporting over time.
- **Use case:** Enterprise teams that need ongoing monitoring of large site inventories.

---

### AATT (Automated Accessibility Testing Tool)

- **URL:** https://github.com/paypal/AATT
- **Free/Paid:** Free, open source (PayPal)
- **What it does:** Server-side accessibility testing via API, supports multiple engines.

---

### AChecker

- **URL:** https://achecker.achecks.ca/
- **Free/Paid:** Free
- **What it checks:** WCAG 2.0 Level A, AA, AAA via URL or HTML paste.
- **Note:** Older tool, but still useful for quick spot checks.

---

## Screen Readers

Automated tools cannot replace screen reader testing. Use them to understand what real users experience.

### NVDA (Windows)

- **URL:** https://www.nvaccess.org/
- **Free/Paid:** Free, open source
- **Best used with:** Firefox (most common combination in accessibility testing)
- **For image testing:**
  - Press `G` in Browse Mode to navigate between images
  - Press `Insert+F7` to open the Elements List and filter for graphics
  - Listen to how each image is announced: "Image, [alt text]"
  - Missing alt: NVDA announces the filename or URL
  - Empty alt: NVDA skips the image silently (correct behavior for decorative)
- **Essential shortcuts:**
  - `G` — next graphic
  - `Shift+G` — previous graphic
  - `Insert+F7` — elements list (shows all images and their alt text)

### JAWS (Windows)

- **URL:** https://www.freedomscientific.com/products/software/jaws/
- **Free/Paid:** Paid ($90/year for home use; enterprise pricing varies)
- **Note:** Most used screen reader in enterprise and government settings. Test here if your users are likely to be in those environments.
- **For image testing:**
  - Press `G` to navigate between graphics
  - `Insert+F7` for graphics list
  - JAWS has some differences in announcing complex ARIA patterns

### VoiceOver (macOS and iOS)

- **URL:** Built into all Apple devices (no installation)
- **Enable on macOS:** System Preferences → Accessibility → VoiceOver → Enable, or `Command+F5`
- **Best used with:** Safari (Apple's own screen reader works best with Apple's own browser)
- **For image testing:**
  - Use Rotor (`Control+Option+U`) → select "Images" to list all images
  - Navigate with `Control+Option+Right Arrow`
  - VoiceOver announces: "image, [alt text]" or "image" followed by filename
- **Why to test here:** VoiceOver/Safari is the dominant AT combination on mobile (iOS). Mobile testing requires physical or simulated iOS devices.

### TalkBack (Android)

- **URL:** Built into Android devices
- **Enable:** Settings → Accessibility → TalkBack
- **For image testing:**
  - Swipe through elements — images should be announced with their alt text
  - Test in Chrome on Android (most common mobile browser)
- **Why to test here:** Second most common mobile AT combination after VoiceOver/iOS.

### ORCA (Linux)

- **URL:** https://wiki.gnome.org/Projects/Orca
- **Free/Paid:** Free, open source
- **Use case:** Testing for Linux desktop users; less common but important for government/enterprise Linux environments.

---

## Contrast Checkers

### Colour Contrast Analyser (desktop application)

- **URL:** https://www.tpgi.com/color-contrast-checker/
- **Free/Paid:** Free
- **Platform:** Windows, macOS
- **Why this for images:** Has an eyedropper tool to sample colors directly from anywhere on your screen — including from images. Essential for checking text-in-images contrast and graphical object contrast (1.4.11).
- **How to use:**
  1. Open the application
  2. Use the eyedropper to sample text color from the image
  3. Use the eyedropper to sample the local background color
  4. The app instantly shows the contrast ratio and pass/fail for 1.4.3 and 1.4.11

### WebAIM Contrast Checker

- **URL:** https://webaim.org/resources/contrastchecker/
- **Free/Paid:** Free
- **How to use:** Enter hex color codes manually. Useful when you can extract colors from design files.

### Who Can Use

- **URL:** https://www.whocanuse.com/
- **Free/Paid:** Free
- **What it adds:** Shows how many people with specific vision conditions can read a given color combination. Useful for making the case for improving contrast.

---

## CI/CD Integration

### lighthouse-ci

- **URL:** https://github.com/GoogleChrome/lighthouse-ci
- **What it does:** Runs Lighthouse audits (including accessibility) in CI, with configurable thresholds and assertions.

```yaml
# GitHub Actions example
- name: Lighthouse CI
  uses: treosh/lighthouse-ci-action@v9
  with:
    urls: |
      https://staging.example.com/
      https://staging.example.com/products/
    budgetPath: ./budget.json
    uploadArtifacts: true
```

```json
// lighthouserc.json — assert no accessibility regressions
{
  "ci": {
    "assert": {
      "assertions": {
        "categories:accessibility": ["error", {"minScore": 0.9}],
        "image-alt": ["error", {"maxLength": 0}]
      }
    }
  }
}
```

### axe-core in Jest

```javascript
// jest.config.js
// Install: npm install --save-dev jest-axe

import { axe, toHaveNoViolations } from 'jest-axe';
expect.extend(toHaveNoViolations);

test('ProductImage component has no accessibility violations', async () => {
  const { container } = render(<ProductImage src="/shoes.jpg" alt="Red running shoes" />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### Cypress axe

```javascript
// Install: npm install --save-dev cypress-axe
// cypress/support/commands.js
import 'cypress-axe';

// In test:
it('product page images are accessible', () => {
  cy.visit('/products');
  cy.injectAxe();
  cy.checkA11y('img', {
    runOnly: ['image-alt', 'input-image-alt', 'role-img-alt']
  });
});
```

---

## Coverage Matrix

| Tool | Missing alt | Empty alt quality | Suspicious alt | Complex image quality | Contrast in images | Screen reader experience |
|------|------------|------------------|----------------|----------------------|-------------------|--------------------------|
| axe DevTools | High | Medium | Low | No | No | No |
| WAVE extension | High | Medium | Medium | No | No | No |
| Lighthouse | High | Low | No | No | No | No |
| IBM Equal Access | High | Medium | Medium | Low | Low | No |
| ARC Toolkit | High | Medium | Medium | No | No | No |
| Pa11y CLI | High | Low | No | No | No | No |
| Colour Contrast Analyser | No | No | No | No | High | No |
| NVDA screen reader | N/A | N/A | N/A | Manual | No | Full |
| VoiceOver | N/A | N/A | N/A | Manual | No | Full |

**Key:** High = reliably detects; Medium = detects some cases; Low = minimal detection; No = not checked; Manual = requires human judgment with tool assistance.

---

## Recommended Testing Stack

**For individual developers:**
1. WAVE browser extension — daily use during development
2. Lighthouse in DevTools — before each PR
3. NVDA + Firefox (Windows) or VoiceOver + Safari (macOS) — weekly spot checks

**For teams with CI/CD:**
1. axe-core via Playwright — automated on every PR
2. lighthouse-ci — accessibility score gate on every deploy
3. NVDA or VoiceOver — manual testing on each major feature release

**For compliance audits:**
1. Pa11y or axe CLI — full site crawl baseline
2. WAVE — detailed per-page review
3. NVDA + Firefox AND VoiceOver + Safari — full screen reader walkthrough of key user flows
4. Colour Contrast Analyser — manual contrast checking for all images of text and data visualizations

---

*Part of the [WCAG Image Accessibility Checklist](../README.md)*
