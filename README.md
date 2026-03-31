# WCAG Image Accessibility Checklist

A comprehensive reference for developers, designers, and QA engineers implementing WCAG 2.1 image accessibility requirements. This repository covers Success Criterion 1.1.1 Non-text Content in full depth, decision frameworks for writing alt text, common failure patterns, and testing tooling.

Built for teams who need to ship accessible products — not just pass audits.

---

## What This Repository Is

WCAG 1.1.1 (Non-text Content) is Level A — the baseline. Getting it wrong means screen reader users encounter silent images, repeated filename noise, or meaningless "image of image of image" announcements. Getting it right requires understanding the full taxonomy of image types and the distinct technique each demands.

This repo covers:

- The complete WCAG 2.1 specification for Non-text Content, with plain-English explanations for all 9 situations
- Every Level A and Level AA requirement that touches images and visual content
- A decision tree for determining what alt text to write (and when to write nothing)
- A comprehensive guide to writing alt text that is actually useful
- The 12 most common alt text mistakes — with before/after fixes
- Testing tools across every category: browser extensions, screen readers, CI pipelines
- ADA Title II 2026 compliance context for web teams working with US public entities
- How to handle complex images: charts, graphs, maps, infographics, diagrams

---

## Table of Contents

### WCAG Reference
- [Success Criterion 1.1.1 — Non-text Content (full reference)](./wcag/success-criterion-1-1-1.md)
- [Level A Requirements for Images](./wcag/level-a-requirements.md)
- [Level AA Requirements for Images](./wcag/level-aa-requirements.md)
- [Alt Text Decision Tree](./wcag/decision-tree.md)

### Practical Guides
- [How to Write Alt Text](./guides/how-to-write-alt-text.md)
- [12 Common Alt Text Mistakes](./guides/common-mistakes.md)
- [Testing Tools Reference](./guides/testing-tools.md)
- [ADA Compliance 2026 — What Teams Need to Know](./guides/ada-compliance-2026.md)

### Examples
- [Complex Images: Charts, Graphs, Maps, Diagrams](./examples/complex-images.md)

---

## Quick-Start Checklist

The 10 most important checks. Run these before every release.

- [ ] **1. Every `<img>` has an `alt` attribute.** No exceptions. A missing `alt` attribute is not the same as `alt=""`. A missing attribute causes screen readers to announce the filename.

- [ ] **2. Decorative images use `alt=""`** — and ideally `role="presentation"` or are CSS backgrounds. If the image conveys no information and is purely visual, it should be invisible to assistive technology.

- [ ] **3. Informative images have descriptive alt text** that conveys the same information as the image. If removing the image and replacing it with only the alt text would cause information loss, the alt text needs work.

- [ ] **4. Functional images (buttons, links) describe the destination or action**, not the image itself. A search button with a magnifying glass icon should read "Search", not "magnifying glass icon".

- [ ] **5. No alt text starts with "image of", "photo of", or "picture of".** Screen readers already announce "image" before reading alt text. The redundancy creates noise.

- [ ] **6. Complex images (charts, graphs, infographics) have a text alternative that conveys the same data.** A bar chart alt attribute cannot convey 12 data series. Use `aria-describedby`, `<figure>/<figcaption>`, or `<details>/<summary>` for long descriptions.

- [ ] **7. Text in images is reproduced in the alt text** (or the surrounding page text). If an image contains text and the alt text does not include that text, the content is inaccessible.

- [ ] **8. Images of text are avoided** where real text can achieve the same visual result. When unavoidable (logos, brand assets), the alt text must exactly match the text in the image.

- [ ] **9. Alt text reads naturally when spoken aloud.** Test by having a screen reader read the page. If any announcement sounds awkward, redundant, or unhelpful, fix it.

- [ ] **10. CAPTCHAs provide an audio alternative.** Any CAPTCHA that requires visual interpretation must offer a non-visual alternative — audio CAPTCHA, logic puzzle, or similar.

---

## How to Use This Repository

**For developers implementing from scratch:** Start with the [Decision Tree](./wcag/decision-tree.md), then read [How to Write Alt Text](./guides/how-to-write-alt-text.md).

**For QA teams auditing existing sites:** Use the [Quick-Start Checklist](#quick-start-checklist) above, then consult [Common Mistakes](./guides/common-mistakes.md) for failure patterns to hunt for.

**For teams handling complex data visualizations:** Go directly to [Complex Images](./examples/complex-images.md).

**For teams with ADA Title II obligations:** Read [ADA Compliance 2026](./guides/ada-compliance-2026.md) first.

**For setting up automated testing:** See [Testing Tools](./guides/testing-tools.md) for CI-compatible options.

---

## Contributing

Corrections, additional examples, and new failure patterns are welcome. Open an issue or PR. This is meant to be a living reference.

---

## License

CC0 1.0 Universal — use freely, no attribution required.

---

*Automate these checks with Alt Audit → [https://altaudit.com](https://altaudit.com)*
