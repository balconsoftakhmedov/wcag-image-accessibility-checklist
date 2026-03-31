# 12 Common Alt Text Mistakes (With Before/After Fixes)

These are the most frequently found alt text failures in real-world accessibility audits. Each failure is shown with what the screen reader announces, why it fails, and how to fix it.

---

## Mistake 1: Starting with "image of", "photo of", or "picture of"

### The problem

Screen readers already announce "image" before reading the alt attribute. So `alt="image of a cat"` gets announced as:

> *"Image, image of a cat"*

The redundancy is minor but it adds noise across an entire page. Multiply it by 30 images on a product page and it becomes genuinely annoying for screen reader users.

### Before

```html
<img src="team-retreat.jpg" alt="Photo of the team at the company retreat">
<img src="product.jpg" alt="Image of the red running shoes">
<img src="ceo.jpg" alt="Picture of our CEO, James">
```

**What the screen reader says:** "Image, Photo of the team at the company retreat"

### After

```html
<img src="team-retreat.jpg" alt="The Alt Audit team at the 2024 company retreat in Lisbon">
<img src="product.jpg" alt="Red running shoes with white mesh upper and rubber sole">
<img src="ceo.jpg" alt="James Kim, CEO of Alt Audit">
```

### The rule

Never start alt text with "image", "photo", "picture", "icon", "graphic", "logo" (unless the word "logo" is part of the actual name, e.g., "Google Doodle logo"), or "screenshot". Jump straight into the content.

---

## Mistake 2: Keyword Stuffing

### The problem

Alt text is indexed by search engines, which has led to the terrible practice of loading alt attributes with target keywords. This produces alt text that is unusable for screen reader users and that modern search engines actually penalize.

### Before

```html
<img src="shoes.jpg" alt="running shoes buy running shoes online best running shoes 2024 cheap running shoes Nike Adidas shoes">

<img src="dentist.jpg" alt="dentist dentistry teeth whitening dental implants dental clinic San Francisco best dentist near me">
```

**What the screen reader says:** A full second of gibberish keyword soup that tells the user nothing about the image or its purpose.

### After

```html
<img src="shoes.jpg" alt="Brooks Ghost 16 running shoes in electric blue, shown from the side">

<img src="dentist.jpg" alt="Dr. Chen examining a patient's X-rays in the clinic consultation room">
```

### The rule

Alt text serves users, not crawlers. Write for a person who cannot see the image and needs to understand what it shows and what it does. If your SEO strategy depends on alt text stuffing, the strategy is wrong.

---

## Mistake 3: Using the Filename as Alt Text

### The problem

This is what happens when alt text is added automatically by a CMS without proper configuration, or when a developer uses the filename as a placeholder and forgets to update it.

### Before

```html
<img src="IMG_4851.jpg" alt="IMG_4851.jpg">
<img src="hero-image-v3-FINAL-web.jpg" alt="hero-image-v3-FINAL-web.jpg">
<img src="shutterstock_1234567890.jpg" alt="shutterstock_1234567890.jpg">
```

**What the screen reader says:** "Image, I-M-G-underscore-4-8-5-1-dot-J-P-G" — an uninterrupted stream of file metadata.

### After

```html
<img src="IMG_4851.jpg" alt="Product launch event, main stage with audience of approximately 300 people">
<img src="hero-image-v3-FINAL-web.jpg" alt="Person using laptop at a bright modern workspace">
<img src="shutterstock_1234567890.jpg" alt="">  <!-- If decorative -->
```

### How to catch this

Search your page source for `alt="` followed by content that matches file naming patterns: `IMG_`, `.jpg`, `.png`, `shutterstock`, `getty`, `adobe`, numbers-only.

---

## Mistake 4: Same Alt Text for Every Image

### The problem

Repeating the same alt text for multiple different images destroys the value of alt text entirely. This typically happens when a CMS sets a default alt text, or when a developer copies a template without updating individual values.

### Before

```html
<!-- All four product variant images on a product page -->
<img src="shoe-black.jpg" alt="Running shoe">
<img src="shoe-white.jpg" alt="Running shoe">
<img src="shoe-red.jpg"   alt="Running shoe">
<img src="shoe-blue.jpg"  alt="Running shoe">
```

**The impact:** A screen reader user navigating through these images hears "Image, Running shoe" four times with no way to distinguish the options.

### After

```html
<img src="shoe-black.jpg" alt="Ghost 16 in midnight black">
<img src="shoe-white.jpg" alt="Ghost 16 in arctic white">
<img src="shoe-red.jpg"   alt="Ghost 16 in crimson red">
<img src="shoe-blue.jpg"  alt="Ghost 16 in electric blue">
```

### The rule

If multiple images have the same alt text, ask: are these actually the same image? If not, each needs unique alt text that identifies what is different about it.

---

## Mistake 5: Missing the Alt Attribute Entirely

### The problem

The most common failure. A missing `alt` attribute is not the same as `alt=""`. When the attribute is absent, screen readers fall back to announcing the image source URL — often a CDN path with hash strings.

### Before

```html
<img src="hero-photo.jpg">
<img src="products/red-shoe.jpg">
<img src="https://cdn.example.com/assets/v2/img/1a2b3c4d/hero-xl.webp">
```

**What the screen reader says:** "Image, hero-photo dot J-P-G" — or worse, "Image, h-t-t-p-s-colon-slash-slash-C-D-N-dot-example-dot-com-slash-assets-slash-v-2-slash..."

### After

```html
<!-- For informative images: descriptive alt text -->
<img src="hero-photo.jpg" alt="Software engineer reviewing code on dual monitors in a modern office">

<!-- For decorative images: empty alt (still needs the attribute!) -->
<img src="decorative-curve.svg" alt="">
```

### Finding missing alt attributes

```bash
# Command-line check with grep
grep -rn '<img' ./public --include="*.html" | grep -v 'alt='

# Or use axe-core in CI to flag image-alt violations
npx axe https://yoursite.com --tags wcag2a
```

---

## Mistake 6: Generic Placeholder Text

### The problem

Someone added an alt attribute to comply with a lint warning, but added a useless value. This passes automated alt-detection checks while failing actual users.

### Before

```html
<img src="team-photo.jpg" alt="image">
<img src="chart.png"      alt="chart">
<img src="download.svg"   alt="icon">
<img src="hero.jpg"       alt="photo">
<img src="product.jpg"    alt="pic">
<img src="logo.png"       alt="logo">
```

**What these convey:** Nothing. "Image, image." "Image, chart." A user still has no idea what these show.

### After

```html
<img src="team-photo.jpg" alt="The 8-person engineering team at the San Francisco office">
<img src="chart.png"      alt="Bar chart: support ticket resolution time, Jan–Jun 2024">
<img src="download.svg"   alt="Download">  <!-- If it's a button/link -->
<img src="download.svg"   alt="">           <!-- If text "Download PDF" is adjacent -->
<img src="hero.jpg"       alt="Developer working at standing desk in home office setup">
<img src="product.jpg"    alt="Wireless noise-canceling headphones in matte black">
<img src="logo.png"       alt="Acme Corp">
```

### Automated detection

WAVE flags `alt="image"`, `alt="photo"`, `alt="pic"`, and similar values as "suspicious alt text" (WAVE_IMG_SUS). Include this check in your testing pipeline.

---

## Mistake 7: Alt Text Redundant with Surrounding Text

### The problem

When alt text repeats information that is already fully stated in the adjacent content, screen reader users hear everything twice. This is noise that degrades the listening experience.

### Before

```html
<h2>Our CEO: James Kim</h2>
<img src="james-kim.jpg" alt="James Kim, CEO">
<p>James Kim has led the company since 2019...</p>

<!-- Or: a figure with a descriptive caption -->
<figure>
  <img src="flooding-2024.jpg" alt="Severe flooding in downtown Chicago on August 14, 2024">
  <figcaption>Severe flooding in downtown Chicago on August 14, 2024</figcaption>
</figure>
```

**What the screen reader says:** "Heading level 2, Our CEO James Kim. Image, James Kim CEO. James Kim has led the company since 2019..."

### After

```html
<h2>Our CEO: James Kim</h2>
<img src="james-kim.jpg" alt="">  <!-- Name and role already in heading -->
<p>James Kim has led the company since 2019...</p>

<!-- Figure with caption: caption is the description -->
<figure>
  <img src="flooding-2024.jpg" alt="">
  <figcaption>Severe flooding in downtown Chicago on August 14, 2024</figcaption>
</figure>
```

### The rule

If the alt text would cause the same information to be announced twice in immediate succession, use `alt=""`. The information is accessible via the text — the image does not need to repeat it.

---

## Mistake 8: Alt Text That Is Too Long

### The problem

Writing alt text as a full paragraph or detailed visual description of everything in the image. This is usually an overcorrection — trying to be thorough — but it creates a bad listening experience and buries the key information in detail.

### Before

```html
<img src="homepage-screenshot.png"
  alt="A screenshot of the website homepage featuring a navigation bar at the top with links to Home, About, Products, Blog, and Contact, a large hero section with a blue gradient background and white text reading 'Transform Your Accessibility' with a yellow call-to-action button below it, followed by a row of three feature cards with icons, then a testimonials section with three customer quotes, and a footer with social media links and a newsletter signup form">
```

**Time to read:** About 20 seconds. By the time it ends, the user has lost context for why the image exists.

### After

```html
<img src="homepage-screenshot.png"
  alt="Homepage screenshot: hero section with 'Transform Your Accessibility' headline, three feature cards, and testimonials section"
  aria-describedby="homepage-detail">
<div id="homepage-detail" class="visually-hidden">
  <!-- Full description available if needed -->
</div>
```

Or, if the screenshot is in documentation where the full detail matters:

```html
<figure>
  <img src="homepage-screenshot.png" alt="Homepage layout overview">
  <figcaption>
    <details>
      <summary>Full interface description</summary>
      <p>[Full descriptive paragraph]</p>
    </details>
  </figcaption>
</figure>
```

### The rule

Alt text for most images should be a sentence or short phrase — enough to identify what the image shows and what purpose it serves. If you genuinely need more, use a long description mechanism.

---

## Mistake 9: Too Literal for Complex Images

### The problem

Describing the visual appearance of a complex image instead of conveying its information content. For data visualizations and diagrams, the data IS the alt text.

### Before

```html
<img src="q3-revenue.png"
  alt="A bar chart with blue bars of different heights on a white background with a light gray grid">
```

**What this conveys:** The visual style. Nothing about the data.

### After

```html
<img src="q3-revenue.png"
  alt="Bar chart: Q3 2024 revenue by region"
  aria-describedby="q3-revenue-desc">
<div id="q3-revenue-desc">
  <p>Q3 2024 revenue by region: North America $3.2M (highest), Europe $1.9M, APAC $1.1M, Latin America $0.4M. Total: $6.6M, up 22% from Q3 2023.</p>
</div>
```

### The rule

For charts and graphs, the alt attribute names the image. The data table, prose description, or `aria-describedby` target provides the actual content. Together they serve the equivalent purpose of a sighted user reading the chart.

---

## Mistake 10: Missing Context That Changes Meaning

### The problem

Writing alt text that is technically accurate in isolation but misleading or incomplete given the page context.

### Before

```html
<!-- On a page about misleading statistics in advertising -->
<img src="graph-showing-growth.png" alt="Graph showing dramatic upward growth trend">
```

This is technically accurate, but the entire point of the image in context is that the Y-axis is manipulated to exaggerate the growth. A screen reader user is missing the key information.

### After

```html
<img src="graph-showing-growth.png"
  alt="Manipulated bar chart with Y-axis starting at 98% rather than 0%, making a 1% increase appear as dramatic growth"
  aria-describedby="chart-manipulation-desc">
```

### The rule

Alt text must convey what the image communicates in its page context, including the point the image is making — not just what the image shows in isolation.

---

## Mistake 11: Wrong Language

### The problem

Alt text written in a different language than the page. This causes screen readers using language-detection to read the alt text in the wrong language, which can sound like gibberish.

### Before

```html
<!-- French page with English alt text -->
<html lang="fr">
...
<img src="boulangerie.jpg" alt="A French bakery with fresh bread and pastries in the window">
```

A French screen reader voice reading English phonetically is not useful.

### After

```html
<html lang="fr">
...
<img src="boulangerie.jpg" alt="Boulangerie avec du pain frais et des pâtisseries en vitrine">
```

If individual images are in a different language than the page, use the `lang` attribute on a container:

```html
<figure lang="de">
  <img src="german-sign.jpg" alt="Willkommen! Eintritt frei — Herzlich willkommen in unserem Geschäft">
  <figcaption>German shop sign: "Welcome! Free entry — Welcome to our shop"</figcaption>
</figure>
```

### The rule

Alt text must be in the language of the page, or — if the image explicitly contains content in a specific language — in that language with a `lang` attribute on a container element.

---

## Mistake 12: Inconsistent Style Across the Same Page

### The problem

When different people write alt text for different images on the same page or site, the result is jarring inconsistency: some images have long descriptive paragraphs, others have two-word labels, some are formal, some are casual. This creates an inconsistent and unpredictable experience.

### Before

```html
<img src="product-a.jpg" alt="The Apex Pro keyboard, a mechanical keyboard designed for professional typists, available in tactile or clicky switch options, USB-C connectivity, full NKRO, and a compact tenkeyless layout in space gray">

<img src="product-b.jpg" alt="mouse">

<img src="product-c.jpg" alt="Image showing the Vertex Monitor, 27-inch 4K panel">

<img src="product-d.jpg" alt="Premium Audio Headset">
```

Four images, four completely different conventions. No consistency in length, style, or what information is included.

### After

```html
<!-- Establish a consistent pattern: Product name + key specs in sentence form -->
<img src="product-a.jpg" alt="Apex Pro Keyboard: tenkeyless mechanical keyboard in space gray with USB-C">

<img src="product-b.jpg" alt="Apex Mouse: wireless optical mouse with adjustable DPI and side buttons">

<img src="product-c.jpg" alt="Vertex Monitor: 27-inch 4K panel with 144Hz refresh rate and USB-C hub">

<img src="product-d.jpg" alt="Vertex Headset: over-ear audio headset with noise cancellation and detachable mic">
```

### The rule

Create an alt text style guide for your project. Define:
- Whether to include product names (usually yes, unless in heading)
- What attributes to include for each content type
- Target length for each content type
- Whether to use periods
- What NOT to include

Document it and share it with your team, CMS editors, and content contributors.

---

## Quick Failure Reference

| Mistake | What screen reader announces | Criterion failed |
|---------|------------------------------|-----------------|
| Missing alt attribute | Filename or URL | 1.1.1 (F65) |
| `alt="image"` | "Image, image" | 1.1.1 (F30) |
| `alt="photo of"` | "Image, photo of [description]" | 1.1.1 |
| Keyword stuffing | Long keyword list | 1.1.1 |
| Filename as alt | Filename phonetically | 1.1.1 (F30) |
| Identical alt for all | Same text regardless of content | 1.1.1, 2.4.4 |
| Redundant alt | Information twice in a row | Not a hard fail, but bad UX |
| Wrong language | Foreign language in wrong voice | 3.1.2 (Level AA) |

---

*Part of the [WCAG Image Accessibility Checklist](../README.md)*
