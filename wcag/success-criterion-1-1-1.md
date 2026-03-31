# WCAG 2.1 Success Criterion 1.1.1 — Non-text Content

**Guideline 1.1:** Provide text alternatives for any non-text content so that it can be changed into other forms people need, such as large print, braille, speech, symbols or simpler language.

**Success Criterion 1.1.1 Non-text Content — Level A**

> All non-text content that is presented to the user has a text alternative that serves the equivalent purpose, except for the situations listed below.

---

## Plain-English Explanation

Every image, icon, chart, graph, illustration, photograph, button with only an image, audio clip, video, animation, CAPTCHA, or decorative ornament needs a text alternative — unless it is purely decorative and conveys no information.

The key phrase is **"serves the equivalent purpose."** This is not a requirement to describe what an image looks like. It is a requirement to convey what the image *does* on the page. A photograph of a CEO on an About page and a photograph of the same CEO used as a link to their bio page have different purposes — and therefore need different alt text.

This criterion is **Level A**, which means it is the minimum baseline. Failure here is not a borderline accessibility issue — it is a hard block for screen reader users.

---

## The 9 Situations (A through H, plus test-only)

WCAG 2.1 defines distinct situations that each require different treatment. Understanding these situations is essential for getting 1.1.1 right.

---

### Situation A: Short description can serve the same purpose and present the same information

**When to use:** Standard informative images — photographs, illustrations, diagrams where the full information can be expressed in a sentence or short phrase.

**Technique:** Use the `alt` attribute on `<img>`. Keep it concise. The text should convey the same information as the image in context.

**Pass examples:**

```html
<!-- Product image -->
<img src="red-running-shoes.jpg" alt="Bright red running shoes with white laces and mesh upper">

<!-- News article hero image -->
<img src="flooding-downtown.jpg" alt="Floodwaters cover a downtown street, reaching the wheel wells of parked cars">

<!-- Person headshot in author bio -->
<img src="sarah-kim.jpg" alt="Sarah Kim, smiling, in a blue blazer against a blurred office background">
```

**Fail examples:**

```html
<!-- Fails: no alt attribute -->
<img src="red-running-shoes.jpg">

<!-- Fails: empty alt on informative image -->
<img src="flooding-downtown.jpg" alt="">

<!-- Fails: filename used as alt text -->
<img src="red-running-shoes.jpg" alt="red-running-shoes.jpg">

<!-- Fails: generic, non-descriptive -->
<img src="flooding-downtown.jpg" alt="photo">

<!-- Fails: redundant prefix -->
<img src="sarah-kim.jpg" alt="Image of Sarah Kim">
```

**Testing procedure:**
1. View page source or use browser inspector to check every `<img>` element.
2. For each image with a non-empty alt, remove the image from the page mentally and substitute the alt text. Does the page lose meaning? If yes, the alt text is insufficient.
3. Use a screen reader to navigate to the image. Listen to what is announced. Is it useful?

---

### Situation B: Short description cannot serve the same purpose (complex images)

**When to use:** Images where the information cannot be conveyed in a brief alt attribute — charts, graphs, maps, diagrams, infographics, complex illustrations.

**Technique:** Use a brief alt that identifies what the image is (e.g., "Bar chart showing quarterly revenue 2023") plus a long description provided via one of:
- `aria-describedby` pointing to a visible element containing the full description
- `<figure>` and `<figcaption>` with a full data table or prose description
- `<details>/<summary>` adjacent to the image
- A linked page containing the full description (older technique, less preferred)

The `longdesc` attribute is deprecated in HTML5 and should not be used in new work.

**Pass examples:**

```html
<!-- Using aria-describedby -->
<img
  src="q4-revenue-chart.png"
  alt="Bar chart: Q4 2023 revenue by product line"
  aria-describedby="chart-description"
>
<div id="chart-description">
  <p>Q4 2023 revenue by product line. Software: $4.2M (up 18% YoY). Hardware: $1.8M (down 3% YoY). Services: $2.1M (up 31% YoY). Total: $8.1M, up 15% from Q4 2022.</p>
</div>

<!-- Using figure/figcaption with details -->
<figure>
  <img src="org-chart.png" alt="Company organizational chart">
  <figcaption>
    <details>
      <summary>Full organizational structure (text version)</summary>
      <p>CEO: Jane Smith. Reporting to CEO: CTO (Mark Lee), CFO (Ana Ruiz), CMO (David Chen). Reporting to CTO: Engineering (3 teams), DevOps (1 team)... [full hierarchy follows]</p>
    </details>
  </figcaption>
</figure>
```

**Fail examples:**

```html
<!-- Fails: alt text alone cannot convey a complex chart -->
<img src="q4-revenue-chart.png" alt="A bar chart with several colored bars showing different values">

<!-- Fails: no mechanism for long description -->
<img src="org-chart.png" alt="Organizational chart">
```

**Testing procedure:**
1. Identify all images that contain data, relationships, or information requiring more than one sentence to convey.
2. Verify a long description mechanism exists and is programmatically associated.
3. Test that the long description contains all data present in the image.
4. Confirm a screen reader can navigate to and read the long description.

---

### Situation C: Image is a control or input

**When to use:** Images used as buttons, links, form inputs, or interactive controls.

**Technique:** The alt text must describe the function or destination — not the image. A button with a floppy disk icon saves something. The alt text is "Save", not "floppy disk".

**Pass examples:**

```html
<!-- Image button: describe the action -->
<input type="image" src="search-icon.png" alt="Search">

<!-- Image used as link: describe the destination -->
<a href="/home">
  <img src="logo.png" alt="Acme Corp — Home">
</a>

<!-- Icon-only button with image: describe the function -->
<button>
  <img src="trash-icon.png" alt="Delete item">
</button>

<!-- Print button -->
<button onclick="window.print()">
  <img src="printer.png" alt="Print this page">
</button>
```

**Fail examples:**

```html
<!-- Fails: describes appearance, not function -->
<input type="image" src="search-icon.png" alt="magnifying glass">

<!-- Fails: empty alt on a functional image (renders the control unnamed) -->
<a href="/home">
  <img src="logo.png" alt="">
</a>

<!-- Fails: describes the icon, not the destination -->
<a href="/contact">
  <img src="envelope.png" alt="envelope icon">
</a>
```

**Testing procedure:**
1. Identify all interactive elements that contain only images (no visible text).
2. Verify each has an alt attribute that describes its function or destination.
3. Use a keyboard-only navigation test: Tab to each image button and verify the announced name is meaningful.
4. Check with VoiceOver or NVDA: does the announced name make sense without seeing the screen?

---

### Situation D: Image contains text

**When to use:** Any image where text is part of the image — logos with text, screenshots of text, stylized typography, promotional banners, button images with text labels.

**Technique:** If the text in the image is the only meaningful content, the alt text must be exactly that text. If the image contains a logo, the alt text should be the logo name/text. If the image is a screenshot of code, the alt text (or long description) must contain the code.

**Pass examples:**

```html
<!-- Logo with company name text in it -->
<img src="acme-logo.png" alt="Acme Corp">

<!-- Promotional banner with text headline -->
<img src="sale-banner.png" alt="50% off all software — Sale ends March 31">

<!-- Button image with text -->
<img src="submit-button.png" alt="Submit">

<!-- Screenshot of a terminal command -->
<img
  src="install-command.png"
  alt=""
  aria-describedby="install-cmd-text"
>
<code id="install-cmd-text">npm install --save-dev @acme/toolkit</code>
```

**Fail examples:**

```html
<!-- Fails: describes the image appearance instead of the text content -->
<img src="acme-logo.png" alt="Blue and orange logo">

<!-- Fails: doesn't include the text in the image -->
<img src="sale-banner.png" alt="Sale banner">

<!-- Fails: omits important text content -->
<img src="submit-button.png" alt="green button">
```

**Additional note:** WCAG 1.4.5 (Level AA) discourages images of text entirely, except for logos and essential stylized text. If you are using images to achieve a specific visual typography effect, consider whether CSS and real text can achieve the same result.

---

### Situation E: Image is a CAPTCHA

**When to use:** Any CAPTCHA mechanism that relies on visual recognition.

**Technique:** CAPTCHAs are specifically called out in WCAG because they create an intentional barrier to automation — which also creates a barrier to assistive technology. The requirement has two parts:
1. The alt text should identify the CAPTCHA as a CAPTCHA (so a screen reader user knows what they're dealing with).
2. An alternative form of CAPTCHA must be provided that does not rely on vision.

**Pass example:**

```html
<!-- Visual CAPTCHA with audio alternative -->
<img src="captcha-image.png" alt="Visual CAPTCHA — enter the characters shown">
<a href="#audio-captcha">Switch to audio CAPTCHA</a>

<!-- Or with accessible CAPTCHA service -->
<!-- reCAPTCHA v2 has a built-in audio challenge -->
<div class="g-recaptcha" data-sitekey="your_site_key"></div>
```

**Fail examples:**

```html
<!-- Fails: no alt text — screen reader announces filename -->
<img src="captcha.png">

<!-- Fails: alt describes the distorted text (which helps bots, not users) -->
<img src="captcha.png" alt="XKBP92">

<!-- Fails: alt says "CAPTCHA" but no audio alternative exists -->
<img src="captcha.png" alt="CAPTCHA">
```

**Testing procedure:**
1. Locate all CAPTCHA elements on the site.
2. Verify alt text identifies the element as a CAPTCHA without revealing the solution.
3. Verify an audio or logic-based alternative is available.
4. Test the alternative with a screen reader user or simulated scenario.

---

### Situation F: Image is decorative

**When to use:** Images that are purely aesthetic and convey no information — background textures, decorative dividers, stock photos used as visual filler that add no meaning to the page, icon embellishments alongside text that already describes the action.

**Technique:** Use `alt=""` (empty alt attribute). This tells assistive technology to ignore the image. Do not omit the `alt` attribute — a missing attribute causes screen readers to fall back to announcing the filename or src URL, which is almost always unhelpful noise.

Alternatively, use `role="presentation"` or deliver the image as a CSS `background-image`.

**Pass examples:**

```html
<!-- Decorative divider between sections -->
<img src="decorative-wave.svg" alt="">

<!-- Stock photo used purely for visual atmosphere -->
<img src="office-background.jpg" alt="" role="presentation">

<!-- Icon that duplicates adjacent text (the text is sufficient) -->
<button>
  <img src="search-icon.svg" alt="">
  Search
</button>

<!-- Pure CSS decorative background (best approach) -->
<style>
  .hero { background-image: url('hero-texture.jpg'); }
</style>
<div class="hero">...</div>
```

**Fail examples:**

```html
<!-- Fails: missing alt attribute entirely — screen reader announces filename -->
<img src="decorative-wave.svg">

<!-- Fails: alt text added to a decorative image creates unnecessary noise -->
<img src="decorative-divider.png" alt="decorative divider">
```

**The judgment call:** Determining whether an image is "decorative" requires context. A stock photo of a person on an article about workplace culture is not purely decorative — it sets a tone and affects meaning. When in doubt, provide alt text.

---

### Situation G: Image test or exercise

**When to use:** Images that are used as part of a test where providing a text alternative would invalidate the test. The classic example is a visual pattern recognition test, a color identification exercise, or a visual puzzle where describing the image would give away the answer.

**Technique:** The alt text should identify that this is a test image and describe the type of test, without revealing the answer. The image itself is the required input.

**Pass example:**

```html
<!-- Visual pattern test where describing the pattern would give the answer -->
<img
  src="ishihara-plate-12.png"
  alt="Ishihara color vision test plate — identify the number you see"
>

<!-- Spot-the-difference puzzle -->
<img src="puzzle-a.jpg" alt="Spot the difference — Image A of 2">
<img src="puzzle-b.jpg" alt="Spot the difference — Image B of 2">
```

**Note:** This exception is narrow. It applies when the visual nature of the test *is* the test. It does not apply to general content images or educational diagrams.

---

### Situation H: Image is sensory

**When to use:** Images where the visual experience is the content — a sunset photograph used to evoke a mood, abstract art, textures. The point of the image is the experience of seeing it, and that experience cannot be text-described in a way that serves the same purpose.

**Technique:** Provide alt text that identifies what the image is, even if it cannot fully replicate the experience. The text alternative should convey as much as words can.

**Pass examples:**

```html
<!-- Abstract artwork on a gallery page -->
<img
  src="rothko-no-14.jpg"
  alt="Mark Rothko, No. 14, 1960. Large-scale oil painting with soft-edged rectangular fields of deep crimson red over dark burgundy, with a narrow pale band at top"
>

<!-- Sunset photograph on a travel blog -->
<img
  src="bali-sunset.jpg"
  alt="Sun setting over Bali rice terraces, sky gradating from orange to deep purple, palm trees silhouetted in the foreground"
>
```

**Note:** These alt texts cannot replicate the visual experience. But they convey meaningful information about what is being shown, which satisfies 1.1.1. The goal is equivalent purpose, not identical experience.

---

### The "Pure Decoration, Not Presented to Users" exception

If an image is truly not presented to users — it's hidden via `display:none`, `visibility:hidden`, or `aria-hidden="true"` — WCAG does not require a text alternative because the content is not being "presented to the user."

```html
<!-- Hidden image — no alt text requirement -->
<img src="tracking-pixel.gif" style="display:none" alt="">

<!-- Explicitly hidden from AT -->
<img src="loading-spinner.gif" aria-hidden="true" alt="">
```

---

## Common Failures for 1.1.1

WCAG documents specific failure techniques. These are the documented ways to fail this criterion:

**F3** — Failure due to using CSS `background-image` to insert an image that conveys information, without a text alternative.

```css
/* Fails if this image conveys information */
.warning { background-image: url('warning-icon.png'); }
```

Fix: Add a visually-hidden `<span>` with the text content, or use an `<img>` with proper alt text.

**F13** — Failure due to having a text alternative that does not include information that is conveyed by color differences.

If an image uses color to convey information (red bars vs. green bars in a chart), the alt text must describe those color-based distinctions.

**F20** — Failure due to not updating text alternatives when changes to non-text content occur.

If an image is dynamically replaced (e.g., a live weather icon), the alt text must update to match.

**F30** — Failure due to using text alternatives that are not alternatives (e.g., filenames, placeholder text like "spacer").

**F38** — Failure for non-text content used as a label: providing text alternative that is not a label.

**F39** — Failure due to providing a text alternative that is not null (`alt=""`) for images that should be ignored by AT.

**F65** — Failure due to omitting the `alt` attribute or `text` attribute on `<img>`, `<area>`, or `<input type="image">` elements.

---

## Programmatic Testing

These automated rules test for aspects of 1.1.1:

| Tool | Rule ID | What It Checks |
|------|---------|----------------|
| axe-core | `image-alt` | img elements must have alternate text |
| axe-core | `input-image-alt` | Input buttons must have alternate text |
| axe-core | `role-img-alt` | Elements with role="img" must have alternate text |
| axe-core | `image-redundant-alt` | Redundant alt text in linked images |
| WAVE | `alt_missing` | Missing alt attribute |
| WAVE | `alt_empty` | Empty alt attribute (flags for manual review) |
| WAVE | `alt_suspicious` | Suspicious alt text (filenames, placeholder text) |
| Lighthouse | `image-alt` | Image elements do not have alt attributes |
| Pa11y | WCAG2AA.1_1_1 | Full 1.1.1 check |

Automated tools can catch missing or empty alt attributes reliably. They cannot determine whether alt text is *meaningful* — that requires human judgment.

---

## Full Manual Testing Procedure

1. **Automated scan first:** Run axe, WAVE, or Lighthouse to catch the obvious failures (missing alt, empty alt on functional images).

2. **Visual inventory:** Load the page and list every image. Categorize each as: informative, functional, decorative, complex, text-containing, CAPTCHA, or sensory.

3. **Alt text review:** For each informative/functional image, verify the alt text conveys the same information or function. Read alt text aloud — if it sounds wrong, it is wrong.

4. **Screen reader walkthrough:** Use NVDA + Firefox or VoiceOver + Safari and navigate the page using only the keyboard. Listen to every image announcement. Note anything that sounds like a filename, that is redundant, or that is missing.

5. **Context check:** Read each image's alt text in the context of surrounding text. If the surrounding text already fully describes the image, `alt=""` may be appropriate. If the alt text repeats what is already written in the adjacent paragraph, it is redundant.

6. **Complex image audit:** For any charts, graphs, or infographics, verify a long description exists and contains all data shown in the image.

7. **Dynamic content:** For any images that change (carousels, live data visualizations, dynamically generated charts), verify that alt text updates when the image content changes.

---

*Part of the [WCAG Image Accessibility Checklist](../README.md)*
