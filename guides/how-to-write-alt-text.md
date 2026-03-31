# How to Write Alt Text

Alt text is not a description of what an image looks like. It is a text replacement for the information and function the image serves. That distinction changes everything about how you write it.

This guide covers principles, rules, length guidelines, what to include and exclude, context sensitivity, and 30+ real examples across every major image type.

---

## General Principles

### 1. Purpose, not appearance

Ask: **What is this image doing on this page?** The answer is your alt text. A photograph of a smiling customer on a testimonials page is doing something different from the same photograph used as a link to that customer's case study. The alt text for each should be different.

Bad: `alt="Woman smiling at camera"`
Good (testimonial): `alt="Maria Chen, founder of Bright Digital"`
Good (case study link): `alt="Read Maria Chen's case study"`

### 2. Serve the same purpose

The WCAG requirement is text that "serves the equivalent purpose." Equivalent purpose means a screen reader user should be able to accomplish the same task or receive the same information as a sighted user. If a sighted user sees a chart and learns that Q3 revenue was the highest in company history, a screen reader user should learn the same thing.

### 3. Context matters more than content

Two identical images can require different alt text on different pages, or even in different locations on the same page. A photograph of a sunset used to illustrate a travel article about Bali needs different alt text than the same photograph used as a background decorative element on a homepage hero. The image content is the same. The page context changes the alt text.

### 4. Alt text is read aloud

Screen readers announce images differently, but a typical NVDA announcement sounds like: *"Image, [alt text]"*. This means your alt text must work as spoken language. Read it aloud. Does it sound natural? Does it communicate something useful?

### 5. Be concise but complete

Alt text is not a caption. It is not an essay. But it also cannot be abbreviated to the point of uselessness. The goal is the shortest text that conveys the equivalent information. No padding, no filler words.

---

## Length Guidelines

There is no hard WCAG character limit for alt text. The oft-cited "125 characters" rule comes from older screen reader limitations, most of which no longer apply. However, practical guidelines apply:

**Target: 150 characters or fewer for standard images.**

This is not a rule — it is a signal. If your alt text exceeds 150 characters, ask whether you have a complex image that needs a long description mechanism instead.

**Absolute maximum for alt attribute: ~1000 characters.** Beyond this, screen readers may truncate. If you need more than ~1000 characters, use `aria-describedby` pointing to a separate element, or a `<details>/<summary>` long description block.

**For decorative images: exactly 0 characters.** That means `alt=""`. Not `alt=" "` (space). Not `alt="."`. Those both cause screen readers to announce something.

### Character counts in practice

| Image type | Typical alt text length |
|-----------|------------------------|
| Product photo | 80–140 characters |
| Person headshot | 30–60 characters |
| Decorative image | 0 (empty alt) |
| Logo | 5–30 characters |
| Icon button/link | 5–25 characters |
| Screenshot | 60–150 characters |
| Chart (brief alt + long desc) | 50–100 characters (brief alt only) |
| Infographic | 50–100 characters (brief alt) + full long desc |

---

## What to Include

### For informative photographs

- The main subject of the image
- Attributes of the subject that are relevant to the page context
- Setting or location, if relevant
- Action or state, if relevant
- The name of a person, if known and relevant
- Text visible in the image

**Do not include:** Every visual detail. The color of the sky. Irrelevant background elements. Your interpretation of the mood.

### For product images

- Product name (if not already in the heading)
- Key visual attributes a buyer would care about: color, material, style, key features visible
- Variant-specific details if this is a variant image (e.g., "shown in midnight blue")

**Do not include:** Marketing language. "Stylish", "beautiful", "premium" are subjective and add nothing.

### For diagrams and technical images

- What the diagram shows (the subject)
- What information the viewer is supposed to take away from it
- Key relationships, flows, or structures depicted

### For screenshots

- What application or interface is shown
- What state or view is depicted
- Any important text or data shown that is not available elsewhere on the page

---

## What to Exclude

### "Image of", "photo of", "picture of"

Screen readers already announce "image" before reading alt text. Starting your alt text with "Image of a cat" causes screen readers to announce "Image, Image of a cat" — a double announcement.

### Subjective descriptors without informational value

"Beautiful sunset" — beautiful is an opinion, not a fact. "Striking infographic" — striking describes your reaction, not the content.

Exception: When mood or aesthetic quality is the actual information being conveyed (sensory images, artwork, design showcases), subjective descriptors can be appropriate because the feeling *is* the content.

### File information

Never use `alt="IMG_4891.jpg"` or `alt="hero-image-v2-FINAL-web.png"`. These are failures, not placeholder text.

### Technical attributes

"JPEG image", "PNG icon", "SVG graphic" — format information is irrelevant to users.

### Redundant information

If the paragraph before the image says "Our CEO, James Kim, joined the company in 2015," the adjacent headshot does not need `alt="James Kim"` unless his identity is specifically the point of the image in that location.

---

## Context Sensitivity

### Same image, different alt text

```html
<!-- On an About page, in the team section -->
<img src="james-kim.jpg" alt="James Kim, CEO">

<!-- On a blog post byline -->
<img src="james-kim.jpg" alt="James Kim">

<!-- When used as a decorative accent alongside his name in a heading -->
<h2>
  <img src="james-kim.jpg" alt="">
  James Kim, CEO
</h2>

<!-- As a link to his LinkedIn profile -->
<a href="https://linkedin.com/in/jameskim">
  <img src="james-kim.jpg" alt="James Kim's LinkedIn profile">
</a>
```

### Redundant text: when `alt=""` is appropriate for informative-looking images

If the image content is fully described by adjacent text and removing the image (replacing with empty space) would lose nothing, `alt=""` is correct:

```html
<!-- The caption fully describes the photograph -->
<figure>
  <img src="flooding.jpg" alt="">
  <figcaption>
    Floodwaters inundated downtown Chicago on Tuesday, reaching depths of 18 inches
    in the Loop district and forcing closure of the CTA's Blue Line.
  </figcaption>
</figure>
```

---

## Punctuation Rules

**End with a period** if your alt text is a complete sentence. Screen readers pause at periods. This prevents the alt text from running into the next announced element.

```
alt="The home page of the redesigned dashboard, showing the revenue overview panel."
```

**No period needed** for short phrases and noun-based descriptions.

```
alt="Red canvas sneakers with white rubber sole"
alt="Sarah Chen, VP of Engineering"
```

**Commas are fine** for lists of attributes.

```
alt="Storefront with neon 'Open' sign, green awning, and window display of leather goods"
```

**No trailing comma** as the last character. `alt="Red shoe,"` — the comma at the end reads awkwardly.

**Avoid ellipsis** (`...`). It signals incompleteness and reads poorly.

---

## Language and Tone

- Write in **the language of the page**. If the page is in Spanish, write alt text in Spanish.
- Match the **register and voice** of the surrounding content. A technical documentation page can use clinical, precise alt text. A consumer retail site can be slightly warmer.
- **Active voice** where the image shows action: "Engineer reviewing circuit board diagrams" rather than "Circuit board diagrams being reviewed by an engineer."
- **Present tense** for descriptions: "CEO addressing a conference" rather than "CEO addressed a conference."
- **No internal assumptions.** If you cannot determine a person's identity with certainty, describe the role or action rather than guessing a name.

---

## 30+ Real Examples by Image Type

### Product Photography

```html
<!-- E-commerce product photo, main image -->
<img src="boot-main.jpg"
     alt="Chelsea boots in tan suede with elastic side panels and stacked heel, shown from 3/4 angle">

<!-- Product photo showing detail -->
<img src="boot-sole.jpg"
     alt="Close-up of boot sole showing lug tread pattern in dark brown rubber">

<!-- Product photo on a sale page -->
<img src="jacket-blue.jpg"
     alt="Quilted puffer jacket in navy blue, shown front view, unzipped">

<!-- Same product in a different color variant -->
<img src="jacket-red.jpg"
     alt="Quilted puffer jacket — shown in scarlet red">
```

### People and Portraits

```html
<!-- Author bio headshot -->
<img src="dr-amara-patel.jpg" alt="Dr. Amara Patel">

<!-- Team page photo with role -->
<img src="team-marcus.jpg" alt="Marcus Johnson, Lead Designer">

<!-- Conference speaker photo -->
<img src="keynote-speaker.jpg" alt="Keynote speaker at podium addressing a full conference hall">

<!-- Unidentified person used for illustration -->
<img src="customer-service.jpg" alt="Customer service representative speaking with a customer at a support desk">

<!-- Group photo -->
<img src="team-2024.jpg" alt="The 12-person Alt Audit team gathered for the 2024 annual retreat in Lisbon">
```

### Data Visualizations (brief alt only — always pair with long description)

```html
<!-- Bar chart -->
<img src="revenue-chart.png"
     alt="Bar chart: annual revenue 2020–2024, showing consistent year-over-year growth from $1.2M to $8.4M"
     aria-describedby="revenue-chart-desc">

<!-- Pie/donut chart -->
<img src="market-share.png"
     alt="Donut chart: market share by vendor in the accessibility software category, Q1 2025"
     aria-describedby="market-share-desc">

<!-- Line chart -->
<img src="user-growth.png"
     alt="Line chart: monthly active users January through December 2024, with annotations for product launch dates"
     aria-describedby="user-growth-desc">
```

### Icons and UI

```html
<!-- Decorative icon alongside text -->
<button>
  <img src="save-icon.svg" alt="">
  Save changes
</button>

<!-- Icon-only button -->
<button aria-label="Save changes">
  <img src="save-icon.svg" alt="">
</button>

<!-- Or: icon with meaningful alt in button context -->
<button>
  <img src="save-icon.svg" alt="Save changes">
</button>

<!-- Status icon -->
<img src="check-green.svg" alt="Success: ">
<span>Your changes have been saved.</span>

<!-- Warning icon -->
<img src="warning-yellow.svg" alt="Warning: ">
<span>This action cannot be undone.</span>
```

### Logos

```html
<!-- Company logo, standalone (e.g., header) -->
<img src="acme-logo.svg" alt="Acme Corp">

<!-- Company logo as a link home -->
<a href="/">
  <img src="acme-logo.svg" alt="Acme Corp — Home">
</a>

<!-- Partner/client logo in a partner section -->
<img src="nasa-logo.png" alt="NASA">

<!-- Award badge -->
<img src="inc5000-badge.png" alt="Inc. 5000 — Fastest Growing Companies 2024">
```

### Screenshots and Screen Captures

```html
<!-- Dashboard screenshot in documentation -->
<img src="dashboard-overview.png"
     alt="Dashboard overview showing revenue summary, active user count (14,382), and recent activity feed">

<!-- Error message screenshot -->
<img src="error-404.png"
     alt="404 error page with message: 'The page you're looking for can't be found. Return to Home.'">

<!-- Code screenshot (prefer real code blocks, but when necessary) -->
<img src="code-snippet.png"
     alt="JavaScript code: const user = await fetch('/api/user').then(r => r.json())"
     aria-describedby="code-full">
<pre id="code-full"><code>const user = await fetch('/api/user').then(r => r.json());</code></pre>
```

### Infographics and Diagrams

```html
<!-- Process diagram -->
<img src="onboarding-flow.png"
     alt="Onboarding process flow with 5 steps: Sign up, Verify email, Connect integration, Configure settings, Launch"
     aria-describedby="onboarding-flow-desc">

<!-- Comparison infographic -->
<img src="plan-comparison.png"
     alt="Side-by-side comparison of Starter, Pro, and Enterprise plans. See table below for full details."
     aria-describedby="comparison-table">

<!-- Technical architecture diagram -->
<img src="system-architecture.png"
     alt="System architecture diagram showing API gateway, three microservices, shared database, and CDN layer"
     aria-describedby="architecture-desc">
```

### Maps

```html
<!-- Location map -->
<img src="office-map.png"
     alt="Map showing Alt Audit office location at 742 Market Street, San Francisco, near Montgomery BART station"
     aria-describedby="office-directions">

<!-- Data map -->
<img src="us-sales-map.png"
     alt="U.S. choropleth map showing sales density by state, 2024. Highest density in California, Texas, and New York."
     aria-describedby="us-sales-data">
```

### Photography (editorial / illustrative)

```html
<!-- Article hero image -->
<img src="remote-work.jpg"
     alt="Person working on laptop at a kitchen table with coffee and notebooks, sunlight coming through window behind them">

<!-- Abstract or mood image used editorially (sensory content) -->
<img src="foggy-morning.jpg"
     alt="Early morning fog over a mountain valley, individual trees emerging from the mist in the foreground">

<!-- Before/after pair -->
<img src="website-before.jpg" alt="Website before redesign: cluttered layout with small text and low-contrast navigation">
<img src="website-after.jpg"  alt="Website after redesign: clean layout with large headlines, clear sections, and high-contrast navigation">
```

### Promotional and Marketing

```html
<!-- Banner with text and offer -->
<img src="spring-sale.jpg" alt="Spring Sale — 30% off all plans through April 30. Use code SPRING30 at checkout.">

<!-- Event announcement image -->
<img src="webinar-promo.jpg" alt="Free Webinar: WCAG 2.2 What's New — Thursday April 10, 2PM ET. Register at the link below.">

<!-- Social proof badge -->
<img src="g2-leader.png" alt="G2 Leader, Spring 2025, Accessibility Software category">
```

---

## Checklist Before Publishing

- [ ] Alt text does not start with "image of", "photo of", "picture of"
- [ ] Decorative images have `alt=""` (not a missing alt attribute)
- [ ] Alt text reads naturally when spoken aloud
- [ ] Alt text is under ~150 characters (or a long description is provided)
- [ ] Alt text conveys the same information the image provides in context
- [ ] Functional images (buttons, links) describe function/destination, not appearance
- [ ] Images of text reproduce the text exactly
- [ ] Complex images have a long description mechanism

---

*Part of the [WCAG Image Accessibility Checklist](../README.md)*
