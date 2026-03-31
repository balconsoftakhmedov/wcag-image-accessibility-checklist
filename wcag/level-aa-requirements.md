# WCAG 2.1 Level AA Requirements — Images and Visual Content

Level AA is the target conformance level for most accessibility laws and regulations, including ADA Title II (2026 deadline), Section 508, EN 301 549 (EU), and most organizational accessibility policies.

Level AA includes all Level A requirements plus the additional criteria below.

---

## Checklist

---

## 1.4.3 Contrast (Minimum)

**Level AA**

**Guideline:** The visual presentation of text and images of text has a contrast ratio of at least 4.5:1, except for large text (3:1), incidental text, and logotypes.

**Applicability to images:** This criterion specifically calls out "images of text" — meaning text rendered as part of an image file. If your design uses an image to display text (a banner, a callout, a badge), that text must meet contrast requirements just like real HTML text.

### Contrast ratio requirements

| Text type | Required ratio |
|-----------|---------------|
| Normal text (< 18pt / < 14pt bold) | 4.5:1 |
| Large text (≥ 18pt / ≥ 14pt bold) | 3:1 |
| Text in images (normal size) | 4.5:1 |
| Text in images (large size) | 3:1 |
| Incidental text (decorative, no functionality) | No requirement |
| Logotype text (in logos and brand marks) | No requirement |

### Checklist

- [ ] Text rendered inside images meets the same contrast minimums as HTML text.
- [ ] Promotional banners with text overlay meet contrast requirements against the background image.
- [ ] Screenshots used as content (not documentation examples) that contain readable text meet contrast.
- [ ] Watermarks or overlay text on images meet contrast if they convey information.
- [ ] Price tags, labels, badges rendered as images meet contrast requirements.
- [ ] Button images with text labels meet contrast requirements.

### What to look for

```html
<!-- Image with text overlay — contrast must be checked against the actual background -->
<div class="hero">
  <img src="hero-background.jpg" alt="">
  <h1>Transform Your Accessibility</h1>  <!-- This text's contrast vs hero-background.jpg must be checked -->
</div>

<!-- Image of text — contrast must meet 4.5:1 -->
<img src="sale-50-percent.png" alt="50% off all products — this weekend only">
```

**How to test:**

1. Extract the text and its local background color from the image.
2. Use a contrast checker (WebAIM Contrast Checker, Colour Contrast Analyser) to measure the ratio.
3. For images with gradient or complex backgrounds, check the worst-case area — the point where text-to-background contrast is lowest.

**Tools:** Colour Contrast Analyser (desktop app, free) can sample colors directly from your screen. Use the eyedropper to sample the text color and the most similar background color in the image.

### Common failures

- White text on a light photographic background (insufficient against sky, snow, bright areas)
- Yellow or light-colored text over white or near-white image regions
- Dark navy text over dark photographic backgrounds
- Low-contrast watermarks that contain required information (e.g., copyright with usage restrictions)

---

## 1.4.4 Resize Text

**Level AA**

**Guideline:** Text can be resized without assistive technology up to 200% without loss of content or functionality.

**Applicability to images:** Text inside images does not scale when the browser font size is increased. This is a key reason why images of text should be avoided.

### Checklist

- [ ] Text that must be readable at all zoom levels is implemented as real HTML text, not as images.
- [ ] Where images of text are used, the information is also available as real text (in alt text or in the surrounding content).
- [ ] At 200% browser zoom, content does not overlap or become unreadable due to fixed-size image elements.

### What fails

- Navigation menus implemented as images of text (impossible to scale)
- Headline text in banners where the font choice is "only achievable as an image" (now solvable with web fonts)
- Product labels rendered as fixed images where the text cannot be enlarged

---

## 1.4.5 Images of Text

**Level AA**

**Guideline:** If the technologies being used can achieve the visual presentation, text is used to convey information rather than images of text. Exceptions: customizable (user can set font/color/size) and essential (specific visual presentation is required for the information).

**This is a strong recommendation against using images to display text.** The exceptions are narrow:

- **Logos and logotypes:** The visual treatment of the brand name is part of the brand identity.
- **Essential presentation:** Where a specific visual rendering is informationally necessary (a typeface specimen showing what a font looks like, a stylized signature in a document).

### Checklist

- [ ] Navigation text is implemented as HTML text, not as image sprites or PNGs.
- [ ] Headline fonts are delivered via web fonts (Google Fonts, Typekit, self-hosted), not as images.
- [ ] Call-to-action buttons use real text with CSS styling, not button images with baked-in text.
- [ ] Promotional banners use real text positioned over background images with CSS, not single images with text embedded.
- [ ] Testimonials, pull quotes, and callouts use real text.
- [ ] Data tables and statistics are real HTML, not screenshots or images of spreadsheets.
- [ ] The only images of text used are logos, watermarks, and genuinely essential visual text.

### Before/after example

```html
<!-- FAIL: Image of text for navigation -->
<nav>
  <a href="/products"><img src="nav-products.png" alt="Products"></a>
  <a href="/about"><img src="nav-about.png" alt="About"></a>
</nav>

<!-- PASS: Real text with CSS styling -->
<nav>
  <a href="/products">Products</a>
  <a href="/about">About</a>
</nav>

<!-- FAIL: Hero banner as single image with embedded text -->
<img src="hero-with-headline-and-cta.jpg" alt="Transform Your Workflow — Start Free Trial">

<!-- PASS: Background image + real HTML text -->
<section class="hero" style="background-image: url('hero-bg.jpg')">
  <h1>Transform Your Workflow</h1>
  <a href="/trial" class="cta-button">Start Free Trial</a>
</section>
```

---

## 1.4.10 Reflow

**Level AA** (added in WCAG 2.1)

**Guideline:** Content can be presented without loss of information or functionality, and without requiring scrolling in two dimensions for: vertical scrolling content at a width equivalent to 320 CSS pixels; horizontal scrolling content at a height equivalent to 256 CSS pixels.

**Applicability to images:** Fixed-width images that cause horizontal scrolling at small viewport widths fail this criterion.

### Checklist

- [ ] Images use responsive sizing (`max-width: 100%` or equivalent).
- [ ] Images in layouts do not cause horizontal scroll at 320px viewport width.
- [ ] Complex images (data visualizations) that must maintain aspect ratio have a zoom/pan or scrollable container that does not require the *page* to scroll horizontally.
- [ ] Image carousels and galleries reflow correctly at small viewport widths.

### CSS minimum baseline

```css
/* Every image should have at minimum: */
img {
  max-width: 100%;
  height: auto;
}
```

---

## 1.4.11 Non-text Contrast

**Level AA** (added in WCAG 2.1)

**Guideline:** The visual presentation of user interface components and graphical objects has a contrast ratio of at least 3:1 against adjacent colors.

**Applicability to images:** This criterion covers graphical objects — the meaningful parts of informational images that are required to understand the content.

### Checklist

- [ ] Chart elements (bars, lines, pie slices, data points) have at least 3:1 contrast against their backgrounds.
- [ ] Diagram components (arrows, connectors, boxes, icons in flowcharts) meet 3:1 contrast.
- [ ] Icon images used as UI components (not decorative) meet 3:1 contrast.
- [ ] The meaningful parts of informational SVGs meet 3:1 contrast.
- [ ] Map features that convey information (roads, borders, markers) meet 3:1 contrast.
- [ ] Image borders and outlines that are necessary for understanding the image meet 3:1 contrast.

### What "graphical object" means

A graphical object is any part of an image that is required to understand the information conveyed. In a line chart:
- The lines themselves are graphical objects (must meet 3:1 against the chart background)
- The data point markers are graphical objects
- The axis lines are graphical objects if the scale is needed to read the chart
- The decorative chart background color is not a graphical object

In a flowchart:
- Box borders are graphical objects
- Arrow connectors are graphical objects
- Text inside boxes is text (covered by 1.4.3, not 1.4.11)

### What is excluded

- Inactive/disabled UI components (not required to meet contrast)
- Components where appearance is determined by user agent (browser defaults)
- Logos and brand marks
- Photographs (the content of the photo, not UI components within an image)

### Common failures in data visualizations

- Light gray gridlines that are necessary for reading data values
- Pastel color palettes in pie charts or bar charts where adjacent slices are low contrast
- Light blue links on white backgrounds in diagram boxes
- Yellow category markers on white chart backgrounds

---

## 2.4.6 Headings and Labels

**Level AA**

**Applicability to images:** Not directly an image requirement, but relevant when images replace text headings. Section headings implemented as images must still meet this criterion — the heading must be descriptive.

### Checklist

- [ ] Section headings implemented as decorative typography images have alt text that serves as the heading text.
- [ ] The alt text for image-based headings is descriptive of the section content, not just the visual style.

---

## Summary Table: Level AA Image Checklist

| Criterion | What to Check | Automated? |
|-----------|---------------|-----------|
| 1.4.3 Contrast | Text in images meets 4.5:1 (normal) or 3:1 (large) | Partial — manual color sampling often needed |
| 1.4.4 Resize Text | Images of text don't scale with zoom | Manual — test at 200% zoom |
| 1.4.5 Images of Text | Text displayed as images where CSS would work | Manual review |
| 1.4.10 Reflow | Images responsive at 320px width | Manual — test at 320px viewport |
| 1.4.11 Non-text Contrast | Graphical objects meet 3:1 contrast | Low automation — manual color checking |

---

## Testing Level AA Image Requirements

### 1.4.3 / 1.4.11 — Contrast testing workflow

1. Open the image in a graphics editor or use a screen color picker.
2. Sample the foreground color (text, or meaningful graphic element).
3. Sample the most similar background color at the point of lowest contrast.
4. Enter both colors into a contrast checker (WebAIM, Colour Contrast Analyser, or browser DevTools).
5. Verify the ratio meets the threshold (4.5:1 for normal text/images of text; 3:1 for large text and graphical objects).

### 1.4.5 — Images of text audit

1. Load the page and open DevTools.
2. Inspect every text element. Check whether it is real HTML text or an `<img>` element.
3. Search CSS for `background-image` on elements that appear to contain text.
4. Flag any `<img>` elements whose alt text is a full sentence of readable text (likely an image of text).

### 1.4.10 — Reflow testing

1. In Chrome DevTools, use responsive design mode to set viewport width to 320px.
2. Scroll down the page. No horizontal scrolling should be required.
3. Check that all images scale within the 320px width.
4. Check that image containers do not overflow their parent elements.

---

*Part of the [WCAG Image Accessibility Checklist](../README.md)*
