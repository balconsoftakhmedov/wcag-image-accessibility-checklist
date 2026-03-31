# Alt Text Decision Tree

Use this tree every time you encounter an image and are not sure what alt text (if any) to write. Work through it top to bottom. The first matching condition wins.

---

## ASCII Decision Tree

```
START: You have an image element on a page.
│
├─► Is the image hidden from all users?
│   (display:none, visibility:hidden, aria-hidden="true")
│   │
│   YES ──► No alt text requirement. If using <img>, include alt="" as a
│           precaution in case the element becomes visible.
│   │
│   NO ──► Continue ▼
│
├─► Does the image contain text?
│   (logos with wordmarks, banners with headlines, badges, buttons with text labels)
│   │
│   YES ──► Is it a logo or brand mark where the visual style is essential?
│           │
│           YES ──► alt="[exact text in the image]"
│                   Example: alt="Acme Corp"
│           │
│           NO ──► Can this be real HTML text instead?
│                  │
│                  YES ──► Replace the image with HTML text + CSS styling.
│                          (This satisfies WCAG 1.4.5 Level AA)
│                  │
│                  NO ──► alt="[exact text in the image]"
│                          Example: alt="50% off — Sale ends Sunday"
│   │
│   NO ──► Continue ▼
│
├─► Is the image the only content inside a link or button?
│   (no visible text alongside it)
│   │
│   YES ──► Is it a link?
│           │
│           YES ──► alt="[describe the link destination]"
│                   Example: <a href="/cart"><img src="cart.svg" alt="View cart"></a>
│                   Not: alt="shopping cart icon"
│           │
│           NO (it's a button) ──► alt="[describe the action]"
│                   Example: <button><img src="search.svg" alt="Search"></button>
│                   Not: alt="magnifying glass"
│   │
│   NO ──► Continue ▼
│
├─► Is the image purely decorative?
│   Ask: If I removed this image and replaced it with nothing,
│   would the page lose any meaning, information, or functionality?
│   │
│   YES, it would lose meaning ──► NOT decorative. Continue ▼
│   │
│   NO, no meaning is lost ──► Decorative. Use alt=""
│                              Also consider: role="presentation"
│                              or use CSS background-image instead.
│   │
│   UNSURE ──► When in doubt, provide alt text. It is better to over-describe
│              than to leave a sighted user's experience untranslatable.
│   │
│   Continue ▼
│
├─► Is it a CAPTCHA?
│   │
│   YES ──► alt="[Identify as CAPTCHA, do not reveal the answer]"
│            Example: alt="Visual CAPTCHA — enter the characters shown"
│            Also: Ensure an audio or logic-based alternative exists (required by WCAG).
│   │
│   NO ──► Continue ▼
│
├─► Is it a test or exercise where describing the image would give away the answer?
│   │
│   YES ──► alt="[Describe the type of test without revealing the answer]"
│            Example: alt="Color vision test — identify the number you see"
│   │
│   NO ──► Continue ▼
│
├─► Is it a complex image?
│   (chart, graph, infographic, map, diagram, flowchart, data visualization)
│   │
│   YES ──► STEP 1: Write a brief alt that names what the image IS.
│            Example: alt="Line chart: website traffic by channel, Jan–Dec 2024"
│           │
│            STEP 2: Provide a long description with all data.
│            Choose a mechanism:
│            - aria-describedby pointing to a visible description
│            - <figure> + <figcaption> with full data table or prose
│            - <details>/<summary> adjacent to the image
│            │
│            See: [Complex Images examples](../examples/complex-images.md)
│   │
│   NO ──► Continue ▼
│
└─► Informative image (standard case).
    Write alt text that conveys the same information as the image in context.
    │
    ├─► Consider context: Does surrounding text already describe the image?
    │   YES ──► Use alt="" (to avoid redundancy) OR write a brief non-redundant alt.
    │
    ├─► Consider purpose: What information does this image add that text does not?
    │   Write alt text that conveys exactly that.
    │
    └─► Consider length: Can the information be conveyed in ~150 characters or fewer?
        YES ──► Use alt attribute.
        NO  ──► This is a complex image (see above). Provide long description.
```

---

## Step-by-Step Text Version

If the ASCII tree is hard to follow, use this linear question-and-answer format.

### Question 1: Is the image hidden from all users?

**Yes** → No alt text requirement. Add `alt=""` to be safe.

**No** → Continue to Question 2.

---

### Question 2: Does the image contain text?

**Yes** → The alt text must include that text. If it is a logo, use the logo name exactly. If it is a promotional banner, use the full headline text. If possible, replace the image of text with real HTML text (satisfies 1.4.5).

**No** → Continue to Question 3.

---

### Question 3: Is the image the only content of a link or button?

**Yes, it is a link** → Write alt text that describes where the link goes. Think: if the image disappeared and only the alt text remained, would a user know what clicking it does and where they land?

**Yes, it is a button** → Write alt text that describes what the button does. The action, not the visual appearance.

**No** → Continue to Question 4.

---

### Question 4: Is the image decorative?

Test: Replace the image with blank space. Does the page lose information, meaning, or functionality?

**No loss** → Use `alt=""`. The image is decorative.

**Yes, there is loss** → Continue to Question 5.

**Uncertain** → Provide alt text. Always err toward accessibility.

---

### Question 5: Is it a CAPTCHA?

**Yes** → Identify it as a CAPTCHA in the alt text without revealing the answer. Ensure an audio or alternative CAPTCHA is available.

**No** → Continue to Question 6.

---

### Question 6: Is it a test where describing it would invalidate the test?

**Yes** → Describe the type of test without the answer.

**No** → Continue to Question 7.

---

### Question 7: Is it a complex image (chart, graph, infographic, map, diagram)?

**Yes** → Write a brief identifying alt text AND provide a long description mechanism. See [Complex Images](../examples/complex-images.md).

**No** → Continue to Question 8.

---

### Question 8: Standard informative image.

Write alt text that:
1. Conveys the same information as the image in its page context.
2. Does not start with "image of", "photo of", or "picture of".
3. Is approximately 150 characters or fewer.
4. Does not repeat information already stated in surrounding text.
5. Reads naturally when spoken aloud.

---

## Quick Reference by Image Type

| Image type | Alt text approach |
|-----------|-------------------|
| Product photo | Describe key visual attributes relevant to purchase decision |
| Person/headshot | Name + role (if known) or brief visual description if identity is the content |
| Decorative/background | `alt=""` |
| Logo | Company/brand name exactly |
| Icon-only button | Describe the action: "Save", "Delete", "Close dialog" |
| Icon-only link | Describe the destination: "Twitter profile", "Download PDF" |
| Icon alongside text | `alt=""` (text already describes it) |
| Banner with text | Reproduce the text content of the banner |
| Chart/graph | Brief label + long description with all data |
| Infographic | Brief label + long description with all information |
| Map | Brief label + text description of routes/regions/data shown |
| Screenshot | Describe what is shown, or reproduce important text content |
| CAPTCHA | "Visual CAPTCHA" — do not reveal the answer |
| Image of text | Reproduce the exact text |
| Abstract art | Describe what is depicted as best as words can convey |
| Sensory/mood photo | Describe the scene, mood, and visual elements that are the point |

---

## The Three Questions to Ask for Any Image

If you remember nothing else from this decision tree, ask these three questions:

1. **What information does this image provide that is not already in the surrounding text?** → Your alt text should convey exactly that information, and nothing more.

2. **If I read this alt text aloud to someone who cannot see the page, would they have what they need?** → If not, revise.

3. **If the image disappeared and only the alt text remained, would the page still make sense?** → If not, revise.

---

*Part of the [WCAG Image Accessibility Checklist](../README.md)*
