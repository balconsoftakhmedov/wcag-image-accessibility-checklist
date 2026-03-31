# WCAG 2.1 Level A Requirements — Images and Visual Content

Level A is the minimum conformance level. These are not best practices — they are the baseline. Failing any of these means some users cannot access your content at all.

This page covers all Level A success criteria with meaningful impact on images, icons, graphics, and visual content.

---

## Checklist Format

Each item includes: the criterion, plain-English explanation, what to check, and common failure patterns.

---

## 1.1.1 Non-text Content

**Guideline:** All non-text content has a text alternative that serves the equivalent purpose.

**Full reference:** [Success Criterion 1.1.1 — Deep Reference](./success-criterion-1-1-1.md)

### Checklist

- [ ] Every `<img>` element has an `alt` attribute (even decorative images — use `alt=""`).
- [ ] Informative images have descriptive alt text that conveys equivalent information.
- [ ] Functional images (buttons, links) have alt text that describes the function or destination.
- [ ] Decorative images have `alt=""` (not a missing alt attribute).
- [ ] Images containing text have alt text that includes that text.
- [ ] Complex images (charts, graphs) have a mechanism for a long description.
- [ ] CAPTCHAs have an audio or logic-based alternative.
- [ ] `<input type="image">` elements have an `alt` attribute.
- [ ] `<area>` elements in image maps have an `alt` attribute.
- [ ] SVG elements used as images have accessible names (via `<title>`, `aria-label`, or `aria-labelledby`).

### What to look for

```html
<!-- FAIL: Missing alt attribute -->
<img src="product.jpg">

<!-- FAIL: Filename as alt text -->
<img src="product-hero-v3-FINAL.jpg" alt="product-hero-v3-FINAL.jpg">

<!-- FAIL: Placeholder alt text -->
<img src="product.jpg" alt="image" >
<img src="product.jpg" alt="photo">
<img src="product.jpg" alt="here">

<!-- PASS: Descriptive alt text -->
<img src="product.jpg" alt="Leather messenger bag with brass buckles, shown in dark brown">

<!-- PASS: Decorative image correctly marked -->
<img src="separator.png" alt="">
```

### SVG-specific checks

```html
<!-- FAIL: SVG with no accessible name -->
<svg xmlns="..." viewBox="0 0 24 24">
  <path d="..."/>
</svg>

<!-- PASS: SVG with title and role -->
<svg role="img" aria-labelledby="svg-title">
  <title id="svg-title">Shopping cart with 3 items</title>
  <path d="..."/>
</svg>

<!-- PASS: Decorative SVG -->
<svg aria-hidden="true" focusable="false">
  <path d="..."/>
</svg>
```

---

## 1.3.1 Info and Relationships

**Guideline:** Information, structure, and relationships conveyed through presentation can be programmatically determined or are available in text.

**Impact on images:** If an image is used to convey structure (e.g., a bullet point is a custom image, a "required" marker is a red asterisk image, a "new" badge is a starburst image), that structural meaning must be programmatically available.

### Checklist

- [ ] Custom icon markers (bullets, checkmarks, status indicators) have text alternatives or are accompanied by text.
- [ ] Status icons (error, warning, success) are not the only indicator of status — text or ARIA roles are also present.
- [ ] Required field markers that are images have text alternatives.
- [ ] Infographic structures that imply hierarchy or relationships convey that structure in text.

### What to look for

```html
<!-- FAIL: Status conveyed only through icon color -->
<img src="red-dot.png" alt="">  <!-- Means "error" but nothing says so -->

<!-- PASS: Status conveyed through image AND text -->
<img src="error-icon.png" alt="Error: "> Required field missing

<!-- PASS: Using ARIA role for status -->
<span role="img" aria-label="Error">⚠</span> Please enter a valid email
```

---

## 1.3.3 Sensory Characteristics

**Guideline:** Instructions do not rely solely on sensory characteristics such as shape, color, size, visual location, or orientation.

**Impact on images:** If instructions reference images by their appearance only ("click the green button", "see the image on the left"), users who cannot see cannot follow those instructions.

### Checklist

- [ ] Instructions do not reference images by color alone ("click the red icon").
- [ ] Instructions do not reference images by shape alone ("click the circular button").
- [ ] Instructions do not reference images by position alone ("the image above shows...").
- [ ] Any reference to a visual element includes a text label or description that works without vision.

### What to look for

```html
<!-- FAIL: Instruction relies on visual position -->
<p>Refer to the chart on the left for details.</p>

<!-- PASS: Instruction includes identifying text -->
<p>Refer to Figure 1: Quarterly Revenue chart for details.</p>

<!-- FAIL: Instruction relies on color -->
<p>Items marked with red icons require immediate action.</p>

<!-- PASS: Instruction includes text description -->
<p>Items marked with a red "Urgent" icon require immediate action.</p>
```

---

## 2.1.1 Keyboard

**Guideline:** All functionality is operable through a keyboard interface.

**Impact on images:** Image maps, image-based navigation, interactive SVGs, canvas-based image interfaces, and custom image galleries must be fully keyboard operable.

### Checklist

- [ ] Image map `<area>` elements are reachable and activatable via keyboard.
- [ ] Interactive SVG elements (clickable regions, toggles) are keyboard accessible.
- [ ] Image carousels and galleries have keyboard controls for navigation.
- [ ] Custom image pickers (profile photo upload, product color selectors with image swatches) are keyboard accessible.
- [ ] Canvas-based image interfaces have keyboard-accessible equivalents.

### What to look for

```html
<!-- FAIL: Image map with no keyboard support on area elements -->
<map name="nav-map">
  <area shape="rect" coords="0,0,100,50" href="/home" alt="Home">
  <!-- area elements support keyboard by default via tabindex and href -->
</map>

<!-- PASS: Ensure area elements have href (makes them keyboard focusable) -->
<map name="nav-map">
  <area shape="rect" coords="0,0,100,50" href="/home" alt="Home">
  <area shape="rect" coords="100,0,200,50" href="/about" alt="About">
</map>
```

---

## 2.4.4 Link Purpose (In Context)

**Guideline:** The purpose of each link can be determined from the link text alone, or from the link text together with its programmatically determined link context.

**Impact on images:** Image links (where an image is the only content of a link element) must have alt text that describes the destination, not the image.

### Checklist

- [ ] Linked images have alt text that describes where the link goes, not what the image shows.
- [ ] Icon links have descriptive alt text or `aria-label`.
- [ ] Logo links clearly indicate they navigate to the home page.
- [ ] Thumbnail links to larger images or articles have descriptive alt text.
- [ ] Multiple links to the same destination have consistent alt text.
- [ ] Multiple links with different destinations do not have identical alt text.

### What to look for

```html
<!-- FAIL: Alt text describes image, not destination -->
<a href="/products/shoe-model-x">
  <img src="shoe-thumbnail.jpg" alt="Red shoe">
</a>

<!-- PASS: Alt text describes destination -->
<a href="/products/shoe-model-x">
  <img src="shoe-thumbnail.jpg" alt="View Model X Running Shoe — Red">
</a>

<!-- FAIL: Multiple "Read more" image links -->
<a href="/article-1"><img src="arrow.png" alt="Read more"></a>
<a href="/article-2"><img src="arrow.png" alt="Read more"></a>

<!-- PASS: Links differentiated by context -->
<a href="/article-1"><img src="arrow.png" alt="Read more about WCAG 2.2 changes"></a>
<a href="/article-2"><img src="arrow.png" alt="Read more about alt text best practices"></a>
```

---

## 4.1.2 Name, Role, Value

**Guideline:** For all user interface components, the name and role can be programmatically determined.

**Impact on images:** Image-based UI components — custom image toggle switches, image radio buttons, image checkboxes, image selects — must expose their role, name, and current state to accessibility APIs.

### Checklist

- [ ] Custom image-based controls have `role` attributes that identify their type.
- [ ] Custom image controls have accessible names via `alt`, `aria-label`, or `aria-labelledby`.
- [ ] Custom image controls expose their state (`aria-checked`, `aria-selected`, `aria-expanded`, `aria-pressed`).
- [ ] Image-based form controls are associated with their labels.

### What to look for

```html
<!-- FAIL: Custom toggle with image, no role or state -->
<div onclick="toggle(this)">
  <img src="toggle-off.png" alt="Notifications">
</div>

<!-- PASS: Custom toggle with proper ARIA -->
<div
  role="switch"
  aria-checked="false"
  aria-label="Enable notifications"
  tabindex="0"
  onclick="toggle(this)"
  onkeydown="handleKey(event, this)"
>
  <img src="toggle-off.png" alt="" aria-hidden="true">
</div>
```

---

## Image Map Requirements Summary

Image maps deserve their own checklist section because they have multiple moving parts:

- [ ] The `<img>` with a `usemap` attribute has alt text describing the overall map (or `alt=""` if the areas cover the entire purpose).
- [ ] Every `<area>` element has an `alt` attribute.
- [ ] `<area>` elements with `href` attributes have descriptive alt text for their destination.
- [ ] `<area>` elements without `href` (inactive/decorative areas) have `alt=""`.
- [ ] All `<area>` hotspots are reachable via keyboard.

```html
<!-- Full passing image map example -->
<img
  src="us-regions-map.png"
  alt="United States regional sales territories"
  usemap="#regions-map"
>
<map name="regions-map">
  <area shape="poly" coords="..." href="/regions/northeast" alt="Northeast region">
  <area shape="poly" coords="..." href="/regions/southeast" alt="Southeast region">
  <area shape="poly" coords="..." href="/regions/midwest"   alt="Midwest region">
  <area shape="poly" coords="..." href="/regions/southwest" alt="Southwest region">
  <area shape="poly" coords="..." href="/regions/west"      alt="West region">
</map>
```

---

## Automated Test Coverage for Level A Image Requirements

| Criterion | Automated Coverage | Manual Required |
|-----------|-------------------|-----------------|
| 1.1.1 — Missing alt | High (axe, WAVE, Lighthouse) | Alt text quality review |
| 1.1.1 — Empty alt on functional | High | Decorative vs. informative judgment |
| 1.1.1 — Complex images | Low | All long description review |
| 1.3.1 — Info in icons | Low | Context-specific review |
| 1.3.3 — Sensory instructions | Very low | Manual content review |
| 2.1.1 — Keyboard for image controls | Medium | Custom controls need manual test |
| 2.4.4 — Link purpose | Medium | Context-dependent judgment |
| 4.1.2 — Name/Role/Value | Medium | Custom component review |

---

*Part of the [WCAG Image Accessibility Checklist](../README.md)*
