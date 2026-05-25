# McNamara Law — Full Redesign Spec
**Date:** 2026-05-25
**Direction:** Quiet Forest — trusted family advisor feel
**Scope:** Full design pass (palette + typography + layout) + all audit P1/P2/P3 fixes

---

## 1. Colour Tokens

Replace all `--color-navy/gold/cream` values with the Quiet Forest palette. Token names in CSS change; all `var(--color-navy)` references in HTML and CSS are renamed to `var(--color-forest)`, etc.

```css
--color-forest:         oklch(27% 0.10 158);   /* primary — replaces --color-navy */
--color-forest-dark:    oklch(20% 0.08 158);   /* replaces --color-navy-dark */
--color-forest-light:   oklch(34% 0.11 158);   /* replaces --color-navy-light */
--color-honey:          oklch(62% 0.13 72);    /* accent — replaces --color-gold */
--color-honey-light:    oklch(72% 0.12 75);    /* replaces --color-gold-light */
--color-parchment:      oklch(97.5% 0.012 80); /* background — replaces --color-cream */
--color-parchment-dark: oklch(95% 0.018 78);   /* replaces --color-cream-dark */
--color-white:          oklch(99% 0.004 80);   /* warm near-white — replaces #ffffff */
--color-ink:            oklch(18% 0.025 200);
--color-text:           oklch(30% 0.020 195);
--color-muted:          oklch(48% 0.018 200);  /* darkened — passes WCAG AA 4.5:1 on parchment */
--color-line:           oklch(88% 0.014 78);
--color-line-strong:    oklch(80% 0.018 75);
```

All `rgba(11, 37, 69, ...)` shadow values updated to use `oklch`-equivalent forest-tinted alphas.

---

## 2. Typography

**Remove:** Cormorant Garamond + Inter (9 variants total).

**Add:** Lora + DM Sans (5 variants total).

```html
<link href="https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;1,400&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
```

```css
--font-serif: "Lora", Georgia, "Times New Roman", serif;
--font-sans:  "DM Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
```

- All heading elements (`h1`–`h3`, `.lede`, `blockquote`) use `--font-serif`
- `h4`, body, UI elements, labels use `--font-sans`
- Type scale (`clamp` values) unchanged
- `.lede` italic style works naturally with Lora italic

---

## 3. Services Layout (index.html)

Replace the identical 6-card `display: grid; grid-template-columns: repeat(3, 1fr)` with a two-tier layout.

**Primary tier — 3 cards (Wills, Powers of Attorney, Estate Planning)**
- Layout: `display: grid; grid-template-columns: 56px 1fr` internally (icon left, content right)
- Larger card, full border, background tint on hover (no `translateY` lift)
- `service-card__link` "Learn more" arrow retained

**Secondary tier — 3 cards (Probate, Testamentary Trusts, Notary)**
- No icon
- Leading numbered prefix (`04`, `05`, `06`) in `--color-honey`, `font-size: 1rem`
- Minimal styling: one-line description, border-bottom only
- No "Learn more" link

**Layout wrapper:**
```css
.services-primary { display: grid; grid-template-columns: repeat(3, 1fr); gap: 28px; margin-bottom: 40px; }
.services-secondary { display: grid; grid-template-columns: repeat(3, 1fr); gap: 0; border: 1px solid var(--color-line); }
```

Hover: background shifts to `var(--color-parchment)`, no lift. No `::before` top-border scaleX animation on primary cards.

---

## 4. Trust Strip → Quote Block (index.html)

Remove the `.trust` section entirely (`.trust`, `.trust__inner`, `.trust__item`).

Replace with a single full-width quote block immediately after the hero:

```html
<section class="founder-quote">
  <div class="container">
    <blockquote>
      "My job is to help families have the conversations they have been putting off — and to put those conversations into documents that work, calmly and without surprise."
    </blockquote>
    <cite>— Erin McNamara, B.A., LL.B.</cite>
  </div>
</section>
```

```css
.founder-quote {
  background: var(--color-forest);
  color: var(--color-parchment);
  padding: 56px 0;
  text-align: center;
}
.founder-quote blockquote {
  font-family: var(--font-serif);
  font-size: clamp(1.35rem, 2.2vw, 1.9rem);
  font-style: italic;
  font-weight: 400;
  max-width: 820px;
  margin: 0 auto 20px;
  line-height: 1.5;
}
.founder-quote cite {
  font-family: var(--font-sans);
  font-size: 13px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--color-honey-light);
  font-weight: 600;
  font-style: normal;
}
```

No `::before` decorative quote mark. No radial gradient orbs.

---

## 5. Credential Cards (about.html)

Remove `border-left: 2px solid var(--color-honey)` from `.credential-card`.

Replace with:
```css
.credential-card {
  background: var(--color-parchment);
  border: 1px solid var(--color-line);
  padding: 24px 28px;
  border-radius: var(--radius-sm);
  transition: border-color 0.3s var(--ease);
}
.credential-card:hover { border-color: var(--color-honey); }
.credential-card__num {
  font-family: var(--font-sans);
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.2em;
  color: var(--color-honey);
  text-transform: uppercase;
  margin-bottom: 10px;
  display: block;
}
```

Add `<span class="credential-card__num">01</span>` (through `06`) above each `h3` in the HTML.

---

## 6. Hero & Decorative Changes

**Hero background:** Remove `radial-gradient` gold glow. Replace with:
```css
background: linear-gradient(160deg, var(--color-parchment) 0%, var(--color-parchment-dark) 100%);
```

**Hero grid texture (`::before`):** Remove entirely.

**Page-header grid texture (`::before`):** Remove entirely (same pattern, same tell).

**Quote section `::before` decoration:** Remove `content: "\201C"` pseudo-element from `.quote-section::before`. Section retains dark background + centred Lora italic — no additional decoration needed.

**Process step numbers:** Change from `font-size: 3rem` serif to:
```css
.process__step strong {
  font-family: var(--font-sans);
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--color-honey);
  letter-spacing: 0.08em;
}
```

---

## 7. Inline Style Overrides

Remove all `style="background: var(...)"` and `style="color: var(...)"` attributes from HTML across all 4 pages.

Add utility classes to `styles.css`:
```css
.section--white    { background: var(--color-white); }
.section--parchment { background: var(--color-parchment); }
.section--parchment-dark { background: var(--color-parchment-dark); }
.eyebrow--light    { color: var(--color-honey-light); }
```

Update HTML to use these classes instead.

---

## 8. Form Validation (contact.html + js/main.js)

**HTML additions:**
- Add `<p class="form-status" aria-live="polite" id="form-status"></p>` above the submit button
- Each required input (`first`, `last`, `email`) gets a sibling `<span class="field-error" id="<field>-error"></span>` and `aria-describedby="<field>-error"` on the input

**JS validation logic:**
- On blur: validate required fields (non-empty) and email format (`/^[^\s@]+@[^\s@]+\.[^\s@]+$/`)
- On submit: validate all required fields; if any fail, inject error messages, focus first failing field, return early
- On success: populate `#form-status` with "Thank you — we will be in touch" and disable button

```css
.field-error {
  display: block;
  font-size: 12px;
  color: oklch(45% 0.18 25);  /* warm red */
  margin-top: 4px;
  min-height: 18px;
}
.form-group input[aria-invalid="true"],
.form-group select[aria-invalid="true"] {
  border-color: oklch(45% 0.18 25);
}
```

Set `aria-invalid="true"` on failing fields, remove on correction.

---

## 9. Mobile Nav CTA (css/styles.css)

Current: `@media (max-width: 768px) { .nav__cta .btn { display: none; } }`

Replace with: hide "Book Consultation" button, show a compact call link:

```css
@media (max-width: 768px) {
  .nav__cta .btn--primary { display: none; }
  .nav__cta .btn--call {
    display: inline-flex;
    font-size: 13px;
    padding: 10px 16px;
  }
}
```

Add `<a href="tel:+16136911020" class="btn btn--primary btn--call">Call Us</a>` to `.nav__cta` in all 4 HTML files. Hidden on desktop via `.btn--call { display: none; }` in base styles.

---

## 10. WebP Image Serving

Wrap all `<img src="images/erin-mcnamara.jpg">` instances in `<picture>` elements:

```html
<picture>
  <source type="image/webp" srcset="images/erin-mcnamara.webp">
  <img src="images/erin-mcnamara.jpg" alt="..." width="682" height="686" loading="...">
</picture>
```

**Manual step required:** Generate `images/erin-mcnamara.webp` from the source JPEG and commit to the repo. This cannot be done programmatically here — flag for the client or handle at deployment via CDN image optimisation.

---

## 11. P3 Polish

- Add `inputmode="tel"` to `<input type="tel" id="phone">`
- Remove `filter: contrast(1.02) saturate(0.95)` from `.hero__visual__frame img`
- Nav link padding: `padding: 10px 16px` → `padding: 12px 16px` (44px tap target)
- Add focus trap to mobile menu JS: intercept Tab/Shift+Tab at menu boundary, wrap focus
- Replace nth-child border logic in process section with gap-based approach:
  ```css
  .process { gap: 0; }
  .process__step + .process__step { border-left: 1px solid var(--color-line); }
  @media (max-width: 1024px) {
    .process__step + .process__step { border-left: none; border-top: 1px solid var(--color-line); }
  }
  ```

---

## Content Additions (from live site)

Add to the Estate Planning service bullet list in `services.html`:
- "Pet provisions"
- "ODSP beneficiary gifts"

These appear in the live site's service descriptions but are missing from the repo.

---

## Files Changed

| File | Changes |
|------|---------|
| `css/styles.css` | Token rename + values, font swap, services layout, founder-quote, credential cards, hero/decoration, utility classes, form styles, mobile nav, P3 polish |
| `js/main.js` | Form validation, focus trap |
| `index.html` | Trust strip → founder-quote, service grid restructure, inline style → classes, picture elements, mobile CTA button |
| `about.html` | Credential card numbers, inline style → classes, picture elements, mobile CTA button |
| `services.html` | Content additions (pet provisions, ODSP), inline style → classes, mobile CTA button |
| `contact.html` | Form aria additions, form-status region, inline style → classes, mobile CTA button, inputmode |
| `images/erin-mcnamara.webp` | **Manual step** — generate and commit separately |

---

## Out of Scope

- Copy rewrites beyond the two content additions noted above
- New pages or sections
- Backend / form submission endpoint (form currently client-side only)
- Dark mode
