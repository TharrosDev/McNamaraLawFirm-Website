# McNamara Law — Quiet Forest Full Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign McNamaraLawFirm-Website with the Quiet Forest palette (forest green + honey), Lora + DM Sans type pair, restructured service layout, and all audit P1–P3 fixes.

**Architecture:** Static HTML/CSS/JS site — no build system. All changes target `css/styles.css`, `js/main.js`, and four HTML pages. Implementation is via `mcp__github-tharros__*` GitHub MCP tools: read the current file, apply changes, push the updated file. Each task is a self-contained file write.

**Tech Stack:** Vanilla HTML5, CSS3 (custom properties, `oklch()`, `clamp()`), Vanilla JS ES5+ IIFE, GitHub MCP (`owner: TharrosDev`, `repo: McNamaraLawFirm-Website`, `branch: main`).

**Spec:** `docs/superpowers/specs/2026-05-25-mcnamara-full-redesign-design.md`

---

## File Map

| File | What changes |
|------|-------------|
| `css/styles.css` | Token rename + OKLCH values; font variables; services two-tier layout; founder-quote section; credential card styles; remove hero/page-header grid textures; remove quote-mark pseudo; process step numbers; utility classes; form error styles; mobile nav CTA; nav tap targets |
| `js/main.js` | Form validation (blur + submit + aria); mobile menu focus trap |
| `index.html` | Google Fonts URL; trust strip → founder-quote; service grid restructure; inline styles → utility classes; mobile CTA btn; `<picture>` elements |
| `about.html` | Google Fonts URL; credential card `<span>` numbers; inline styles → utility classes; mobile CTA btn; `<picture>` elements |
| `services.html` | Google Fonts URL; add pet provisions + ODSP bullets; inline styles → utility classes; mobile CTA btn |
| `contact.html` | Google Fonts URL; form `aria-describedby` + field-error spans + form-status; `inputmode="tel"`; inline styles → utility classes; mobile CTA btn |

---

## Task 1: CSS — `:root` token system + font variables

**Files:**
- Modify: `css/styles.css` (`:root` block only)

- [ ] **Step 1: Read current styles.css**

Fetch: `mcp__github-tharros__get_file_contents` → `css/styles.css`
Note the current SHA — required for the update call.

- [ ] **Step 2: Replace the `:root` block**

Find the existing `:root { ... }` block (lines 1–27 approx) and replace it entirely with:

```css
:root {
  --color-forest:         oklch(27% 0.10 158);
  --color-forest-dark:    oklch(20% 0.08 158);
  --color-forest-light:   oklch(34% 0.11 158);
  --color-honey:          oklch(62% 0.13 72);
  --color-honey-light:    oklch(72% 0.12 75);
  --color-parchment:      oklch(97.5% 0.012 80);
  --color-parchment-dark: oklch(95% 0.018 78);
  --color-white:          oklch(99% 0.004 80);
  --color-ink:            oklch(18% 0.025 200);
  --color-text:           oklch(30% 0.020 195);
  --color-muted:          oklch(48% 0.018 200);
  --color-line:           oklch(88% 0.014 78);
  --color-line-strong:    oklch(80% 0.018 75);

  --font-serif: "Lora", Georgia, "Times New Roman", serif;
  --font-sans:  "DM Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;

  --shadow-sm: 0 1px 2px oklch(27% 0.10 158 / 0.06), 0 1px 3px oklch(27% 0.10 158 / 0.05);
  --shadow-md: 0 4px 16px oklch(27% 0.10 158 / 0.08), 0 2px 6px oklch(27% 0.10 158 / 0.05);
  --shadow-lg: 0 24px 48px -16px oklch(27% 0.10 158 / 0.18), 0 8px 24px oklch(27% 0.10 158 / 0.08);

  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 14px;

  --container: 1240px;
  --container-narrow: 980px;

  --ease: cubic-bezier(0.22, 1, 0.36, 1);
  --dur: 0.5s;
}
```

- [ ] **Step 3: Find-replace all token references throughout the file**

Apply these substitutions in the order listed (order matters to avoid partial matches):

| Find | Replace |
|------|---------|
| `var(--color-navy-dark)` | `var(--color-forest-dark)` |
| `var(--color-navy-light)` | `var(--color-forest-light)` |
| `var(--color-navy)` | `var(--color-forest)` |
| `var(--color-gold-light)` | `var(--color-honey-light)` |
| `var(--color-gold)` | `var(--color-honey)` |
| `var(--color-cream-dark)` | `var(--color-parchment-dark)` |
| `var(--color-cream)` | `var(--color-parchment)` |

Also update these three raw `rgba` values that appear in the file body (not in `:root`):
- `rgba(250, 247, 241, 0.96)` → `oklch(97.5% 0.012 80 / 0.96)` (in `.header.scrolled`)
- `rgba(250,247,241,0.7)` → `oklch(97.5% 0.012 80 / 0.7)` (in `.footer a`)
- `rgba(250,247,241,0.5)` → `oklch(97.5% 0.012 80 / 0.5)` (in `.footer__bottom`)

- [ ] **Step 4: Push the updated CSS**

```
mcp__github-tharros__create_or_update_file
  owner: TharrosDev
  repo: McNamaraLawFirm-Website
  path: css/styles.css
  branch: main
  message: "style: swap Quiet Forest token palette and Lora/DM Sans fonts"
  sha: <sha from Step 1>
  content: <full updated file>
```

- [ ] **Step 5: Verify**

Re-fetch `css/styles.css` and confirm:
- `:root` block contains `--color-forest`, `--color-honey`, `--color-parchment`
- No remaining `--color-navy`, `--color-gold`, `--color-cream` references
- `--font-serif` references `"Lora"`, `--font-sans` references `"DM Sans"`

---

## Task 2: CSS — Layout and structural changes

**Files:**
- Modify: `css/styles.css` (multiple sections)

- [ ] **Step 1: Read current styles.css** (get fresh SHA after Task 1)

- [ ] **Step 2: Replace the `.services` section with two-tier layout**

Remove the existing `.services { ... }` block and `.service-card { ... }` and all `.service-card__*` blocks. Replace with:

```css
/* ------- Services — two-tier layout ------- */
.services-primary {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 28px;
  margin-bottom: 28px;
}
.services-secondary {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  border: 1px solid var(--color-line);
  background: var(--color-white);
}
.service-card {
  background: var(--color-white);
  border: 1px solid var(--color-line);
  position: relative;
  transition: background 0.3s var(--ease), border-color 0.3s var(--ease);
  overflow: hidden;
}
.service-card:hover {
  background: var(--color-parchment);
  border-color: var(--color-honey);
}
.service-card--primary {
  display: grid;
  grid-template-columns: 56px 1fr;
  gap: 20px;
  padding: 36px 32px;
  align-items: start;
}
.service-card--secondary {
  padding: 28px 32px;
  border: none;
  border-right: 1px solid var(--color-line);
}
.service-card--secondary:last-child { border-right: none; }
.service-card__icon {
  width: 56px; height: 56px;
  display: grid; place-items: center;
  background: var(--color-parchment);
  border: 1px solid var(--color-line);
  color: var(--color-honey);
  flex-shrink: 0;
  transition: all 0.3s var(--ease);
}
.service-card:hover .service-card__icon {
  background: var(--color-forest);
  color: var(--color-honey-light);
  border-color: var(--color-forest);
}
.service-card__body { min-width: 0; }
.service-card--primary h3 {
  font-size: 1.35rem;
  margin-bottom: 10px;
}
.service-card--primary p {
  color: var(--color-muted);
  font-size: 0.97rem;
  line-height: 1.7;
  margin-bottom: 18px;
}
.service-card__num {
  display: block;
  font-family: var(--font-sans);
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.2em;
  color: var(--color-honey);
  text-transform: uppercase;
  margin-bottom: 10px;
}
.service-card--secondary h3 {
  font-size: 1.2rem;
  margin-bottom: 8px;
}
.service-card--secondary p {
  color: var(--color-muted);
  font-size: 0.94rem;
  line-height: 1.65;
  margin: 0;
}
.service-card__link {
  font-size: 13px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.14em;
  color: var(--color-forest);
  display: inline-flex; align-items: center; gap: 6px;
  transition: color 0.25s var(--ease);
}
.service-card:hover .service-card__link { color: var(--color-honey); }
.service-card--centered { text-align: center; }
.service-card--centered .service-card__icon { margin: 0 auto 22px; }
.service-card--centered.service-card--primary {
  grid-template-columns: 1fr;
  text-align: center;
}
```

- [ ] **Step 3: Add `.founder-quote` section and remove `.trust` styles**

Remove the `.trust { ... }`, `.trust__inner { ... }`, and `.trust__item { ... }` blocks.

Add after the hero section styles:

```css
/* ------- Founder quote (replaces trust strip) ------- */
.founder-quote {
  background: var(--color-forest);
  color: var(--color-parchment);
  padding: clamp(48px, 6vw, 80px) 0;
  text-align: center;
}
.founder-quote blockquote {
  font-family: var(--font-serif);
  font-size: clamp(1.25rem, 2vw, 1.85rem);
  font-style: italic;
  font-weight: 400;
  max-width: 820px;
  margin: 0 auto 22px;
  line-height: 1.5;
  color: var(--color-parchment);
}
.founder-quote cite {
  font-family: var(--font-sans);
  font-size: 12px;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--color-honey-light);
  font-weight: 600;
  font-style: normal;
}
```

- [ ] **Step 4: Update credential card styles**

Replace the `.credential-card { ... }` and `.credential-card h3 { ... }` and `.credential-card p { ... }` blocks with:

```css
.credential-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 20px;
  margin-top: 32px;
}
.credential-card {
  background: var(--color-parchment);
  border: 1px solid var(--color-line);
  padding: 24px 28px;
  border-radius: var(--radius-sm);
  transition: border-color 0.3s var(--ease);
}
.credential-card:hover { border-color: var(--color-honey); }
.credential-card__num {
  display: block;
  font-family: var(--font-sans);
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.2em;
  color: var(--color-honey);
  text-transform: uppercase;
  margin-bottom: 10px;
}
.credential-card h3 {
  font-family: var(--font-serif);
  font-size: 1.2rem;
  font-weight: 500;
  color: var(--color-forest);
  margin-bottom: 8px;
  letter-spacing: 0;
}
.credential-card p {
  color: var(--color-muted);
  font-size: 0.95rem;
  margin: 0;
}
```

- [ ] **Step 5: Remove decorative elements and update hero + quote section**

**Hero** — find `.hero { ... }` and update the background:
```css
.hero {
  position: relative;
  padding: clamp(80px, 11vw, 160px) 0 clamp(80px, 9vw, 140px);
  overflow: hidden;
  background: linear-gradient(160deg, var(--color-parchment) 0%, var(--color-parchment-dark) 100%);
}
```

Remove the entire `.hero::before { ... }` block (the CSS grid-line texture).

**Page header** — remove the entire `.page-header::before { ... }` block (same grid-line texture).

**Quote section** — remove the entire `.quote-section::before { ... }` block (the `"\201C"` decoration).

**Process step numbers** — find `.process__step strong { ... }` and replace with:
```css
.process__step strong {
  font-family: var(--font-sans);
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--color-honey);
  display: block;
  margin-bottom: 16px;
  line-height: 1;
  letter-spacing: 0.08em;
}
```

- [ ] **Step 6: Add utility classes, form error styles, and mobile nav fix**

At the end of the stylesheet, before the responsive section, add:

```css
/* ------- Utility classes ------- */
.section--white           { background: var(--color-white); }
.section--parchment       { background: var(--color-parchment); }
.section--parchment-dark  { background: var(--color-parchment-dark); }
.eyebrow--light           { color: var(--color-honey-light); }

/* ------- Form errors ------- */
.field-error {
  display: block;
  font-size: 12px;
  color: oklch(45% 0.18 25);
  margin-top: 4px;
  min-height: 18px;
}
.form-group input[aria-invalid="true"],
.form-group select[aria-invalid="true"],
.form-group textarea[aria-invalid="true"] {
  border-color: oklch(45% 0.18 25);
  box-shadow: 0 0 0 3px oklch(45% 0.18 25 / 0.10);
}
.form-status {
  font-size: 14px;
  color: var(--color-forest);
  font-weight: 600;
  min-height: 22px;
  margin-bottom: 12px;
}

/* ------- Mobile CTA call button ------- */
.btn--call { display: none; }
```

Update the nav link tap target in the base nav styles:
```css
/* find: padding: 10px 16px; in .nav__menu a */
/* replace with: */
padding: 12px 16px;
```

Update the `@media (max-width: 768px)` block for the nav CTA:
```css
/* find: .nav__menu, .nav__cta .btn { display: none; } */
/* replace with: */
.nav__menu { display: none; }
.nav__cta .btn--primary { display: none; }
.btn--call { display: inline-flex !important; font-size: 13px; padding: 10px 16px; }
```

Update the `@media (max-width: 1024px)` services grid:
```css
/* find: .services { grid-template-columns: repeat(2, 1fr); } */
/* replace with: */
.services-primary { grid-template-columns: repeat(2, 1fr); }
.services-secondary { grid-template-columns: repeat(2, 1fr); }
.service-card--secondary:nth-child(2) { border-right: none; }
.service-card--secondary:nth-child(1),
.service-card--secondary:nth-child(2) { border-bottom: 1px solid var(--color-line); }
```

Update the `@media (max-width: 768px)` services grid:
```css
/* find: .services { grid-template-columns: 1fr; } */
/* replace with: */
.services-primary { grid-template-columns: 1fr; }
.services-secondary { grid-template-columns: 1fr; }
.service-card--secondary { border-right: none; border-bottom: 1px solid var(--color-line); }
.service-card--secondary:last-child { border-bottom: none; }
```

Remove the `.hero__visual::before, .hero__visual::after { display: none; }` line from `@media (max-width: 768px)` — the `::after` dot texture stays visible on mobile now.

Update the `@media (max-width: 1024px)` process border fix with gap-based approach:
```css
/* remove: .process__step:nth-child(2) { border-right: none; } */
/* remove: .process__step:nth-child(1), .process__step:nth-child(2) { border-bottom: ... } */
/* replace with: */
.process__step + .process__step { border-left: 1px solid var(--color-line); }
.process { grid-template-columns: repeat(2, 1fr); }
```

- [ ] **Step 7: Push the updated CSS**

```
mcp__github-tharros__create_or_update_file
  path: css/styles.css
  message: "style: services layout, founder-quote, credential cards, decoration cleanup"
  sha: <fresh sha>
  content: <full updated file>
```

---

## Task 3: index.html — Full restructure

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Read current index.html** (note SHA)

- [ ] **Step 2: Update Google Fonts `<link>` tag**

```html
<!-- Remove: -->
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,400;1,500&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">

<!-- Replace with: -->
<link href="https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;1,400&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
```

- [ ] **Step 3: Add mobile CTA call button to nav**

Find `.nav__cta` div and add the call button:
```html
<div class="nav__cta">
  <a href="contact.html" class="btn btn--primary">Book Consultation</a>
  <a href="tel:+16136911020" class="btn btn--primary btn--call">Call Us</a>
  <button class="hamburger" type="button" aria-label="Toggle navigation menu" aria-expanded="false"><span></span><span></span><span></span></button>
</div>
```

- [ ] **Step 4: Wrap hero image in `<picture>` element**

```html
<!-- Remove: -->
<img src="images/erin-mcnamara.jpg" alt="Erin McNamara, Barrister &amp; Solicitor" width="682" height="686" loading="eager" fetchpriority="high">

<!-- Replace with: -->
<picture>
  <source type="image/webp" srcset="images/erin-mcnamara.webp">
  <img src="images/erin-mcnamara.jpg" alt="Erin McNamara, Barrister &amp; Solicitor" width="682" height="686" loading="eager" fetchpriority="high">
</picture>
```

- [ ] **Step 5: Remove the `.trust` section and add `.founder-quote`**

Remove the entire `<section class="trust">...</section>` block (the 4-stat strip).

In its place, immediately after `</section>` (closing the hero section), insert:

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

- [ ] **Step 6: Restructure the services grid**

In the services `<section class="section">`, replace the `<div class="services">` and all 6 `<article class="service-card">` cards with two groups:

**Primary group** (Wills, Powers of Attorney, Estate Planning):
```html
<div class="services-primary">
  <article class="service-card service-card--primary reveal">
    <div class="service-card__icon" aria-hidden="true">
      <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M6 2h9l5 5v13a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2z"/><path d="M14 2v6h6"/><path d="M8 13h8M8 17h6"/></svg>
    </div>
    <div class="service-card__body">
      <h3>Wills &amp; Testaments</h3>
      <p>From straightforward last wills to multi-trust testamentary plans for complex family situations, foreign assets and business succession.</p>
      <a href="services.html#wills" class="service-card__link">Learn more →</a>
    </div>
  </article>

  <article class="service-card service-card--primary reveal">
    <div class="service-card__icon" aria-hidden="true">
      <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/><path d="M9 12l2 2 4-4"/></svg>
    </div>
    <div class="service-card__body">
      <h3>Powers of Attorney</h3>
      <p>Carefully drafted Continuing Powers of Attorney for Property and Personal Care, so the people you trust can act if you cannot.</p>
      <a href="services.html#poa" class="service-card__link">Learn more →</a>
    </div>
  </article>

  <article class="service-card service-card--primary reveal">
    <div class="service-card__icon" aria-hidden="true">
      <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 22c0-4 4-7 8-7s8 3 8 7"/></svg>
    </div>
    <div class="service-card__body">
      <h3>Estate Planning</h3>
      <p>Integrated plans for blended families, dependent loved ones, business owners, farm property and beneficiaries outside Canada.</p>
      <a href="services.html#planning" class="service-card__link">Learn more →</a>
    </div>
  </article>
</div>
```

**Secondary group** (Probate, Trusts, Notary):
```html
<div class="services-secondary">
  <article class="service-card service-card--secondary reveal">
    <span class="service-card__num" aria-hidden="true">04</span>
    <h3>Probate Applications</h3>
    <p>Certificates of Appointment of Estate Trustee — prepared and shepherded through the Ontario Superior Court of Justice.</p>
  </article>

  <article class="service-card service-card--secondary reveal">
    <span class="service-card__num" aria-hidden="true">05</span>
    <h3>Testamentary Trusts</h3>
    <p>Trusts that take effect under a will — for minor children, beneficiaries with disabilities, spendthrift heirs and tax-effective transfers.</p>
  </article>

  <article class="service-card service-card--secondary reveal">
    <span class="service-card__num" aria-hidden="true">06</span>
    <h3>Notary &amp; Commissioner</h3>
    <p>Notarial copies, oath administration, travel-consent letters, passport-guarantor services and witnessing of agreements.</p>
  </article>
</div>
```

- [ ] **Step 7: Wrap about-section image in `<picture>`**

```html
<!-- In the about section (second image), replace: -->
<img src="images/erin-mcnamara.jpg" alt="Erin McNamara, Barrister &amp; Solicitor — McNamara Law Services" width="682" height="686" loading="lazy">

<!-- With: -->
<picture>
  <source type="image/webp" srcset="images/erin-mcnamara.webp">
  <img src="images/erin-mcnamara.jpg" alt="Erin McNamara, Barrister &amp; Solicitor — McNamara Law Services" width="682" height="686" loading="lazy">
</picture>
```

- [ ] **Step 8: Replace all inline style overrides with utility classes**

| Find (inline style) | Replace with (class) |
|---------------------|----------------------|
| `style="color: var(--color-gold-light);"` on `<span class="eyebrow">` | remove inline style, add class `eyebrow--light` |
| `class="section about"` with no inline bg | no change needed (about section bg comes from CSS) |

Check all elements with `style=` attributes — remove each one and add the matching utility class from Task 2 Step 6.

- [ ] **Step 9: Push updated index.html**

```
mcp__github-tharros__create_or_update_file
  path: index.html
  message: "feat: Quiet Forest — founder quote, two-tier services, picture elements"
  sha: <sha from Step 1>
```

---

## Task 4: about.html — Credential numbers + inline styles

**Files:**
- Modify: `about.html`

- [ ] **Step 1: Read current about.html** (note SHA)

- [ ] **Step 2: Update Google Fonts link** (same swap as Task 3 Step 2)

- [ ] **Step 3: Add mobile CTA call button** (same as Task 3 Step 3)

- [ ] **Step 4: Add credential card numbers**

Each `.credential-card` in the `.credential-grid` gets a `<span class="credential-card__num">` as the first child. Numbers 01–06 in order:

```html
<!-- Example — apply to all 6 cards: -->
<div class="credential-card">
  <span class="credential-card__num">01</span>
  <h3>Law Society of Ontario</h3>
  <p>Licensed Barrister &amp; Solicitor in good standing.</p>
</div>
```

Numbers: `01` Law Society → `02` County of Carleton → `03` Ottawa Estate Planning Council → `04` University of Ottawa → `05` Queen's Faculty → `06` Reach Canada

- [ ] **Step 5: Wrap about image in `<picture>`**

```html
<picture>
  <source type="image/webp" srcset="images/erin-mcnamara.webp">
  <img src="images/erin-mcnamara.jpg" alt="Erin McNamara, Barrister &amp; Solicitor" width="682" height="686" loading="eager">
</picture>
```

- [ ] **Step 6: Replace inline style overrides with utility classes**

```html
<!-- Find: -->
<section class="section about" style="background: var(--color-cream);">
<!-- Replace with: -->
<section class="section about section--parchment">

<!-- Find: -->
<section class="section" style="background: var(--color-white);">
<!-- Replace with: -->
<section class="section section--white">

<!-- Find (in CTA eyebrow): -->
<span class="eyebrow" style="color: var(--color-gold-light);">
<!-- Replace with: -->
<span class="eyebrow eyebrow--light">
```

- [ ] **Step 7: Push updated about.html**

```
mcp__github-tharros__create_or_update_file
  path: about.html
  message: "feat: Quiet Forest — credential numbers, utility classes, picture element"
```

---

## Task 5: services.html — Content additions + inline styles

**Files:**
- Modify: `services.html`

- [ ] **Step 1: Read current services.html** (note SHA)

- [ ] **Step 2: Update Google Fonts link** (same swap as Task 3 Step 2)

- [ ] **Step 3: Add mobile CTA call button** (same as Task 3 Step 3)

- [ ] **Step 4: Add missing service bullets to Estate Planning section**

Find the `<article ... id="planning">` service row. In its `<ul>`, add after "Dependent support obligations":

```html
<li>Pet provisions</li>
<li>ODSP beneficiary gifts</li>
```

- [ ] **Step 5: Replace inline style overrides with utility classes**

```html
<!-- Find: -->
<section class="section" style="background: var(--color-white);">
<!-- Replace with: -->
<section class="section section--white">

<!-- Find: -->
<section class="section" style="background: var(--color-cream-dark);">
<!-- Replace with: -->
<section class="section section--parchment-dark">

<!-- Find (CTA eyebrow): -->
<span class="eyebrow" style="color: var(--color-gold-light);">
<!-- Replace with: -->
<span class="eyebrow eyebrow--light">
```

- [ ] **Step 6: Push updated services.html**

```
mcp__github-tharros__create_or_update_file
  path: services.html
  message: "feat: Quiet Forest — content additions, utility classes"
```

---

## Task 6: contact.html — Form a11y + inline styles

**Files:**
- Modify: `contact.html`

- [ ] **Step 1: Read current contact.html** (note SHA)

- [ ] **Step 2: Update Google Fonts link** (same swap as Task 3 Step 2)

- [ ] **Step 3: Update mobile nav CTA**

Contact page already shows a call button in the nav CTA. Replace its `.nav__cta`:
```html
<div class="nav__cta">
  <a href="tel:+16136911020" class="btn btn--primary">Call (613) 691-1020</a>
  <a href="tel:+16136911020" class="btn btn--primary btn--call">Call Us</a>
  <button class="hamburger" type="button" aria-label="Toggle navigation menu" aria-expanded="false"><span></span><span></span><span></span></button>
</div>
```

- [ ] **Step 4: Add `aria-describedby` and `field-error` spans to required inputs**

Update `first`, `last`, and `email` inputs:

```html
<div class="form-group">
  <label for="first">First Name</label>
  <input type="text" id="first" name="first" required autocomplete="given-name" aria-describedby="first-error">
  <span class="field-error" id="first-error" role="alert"></span>
</div>

<div class="form-group">
  <label for="last">Last Name</label>
  <input type="text" id="last" name="last" required autocomplete="family-name" aria-describedby="last-error">
  <span class="field-error" id="last-error" role="alert"></span>
</div>

<div class="form-group">
  <label for="email">Email</label>
  <input type="email" id="email" name="email" required autocomplete="email" aria-describedby="email-error">
  <span class="field-error" id="email-error" role="alert"></span>
</div>
```

- [ ] **Step 5: Add `inputmode="tel"` to phone input**

```html
<!-- Find: -->
<input type="tel" id="phone" name="phone" autocomplete="tel">
<!-- Replace with: -->
<input type="tel" id="phone" name="phone" autocomplete="tel" inputmode="tel">
```

- [ ] **Step 6: Add `form-status` region above submit button**

```html
<!-- Insert immediately before: <button type="submit" ...> -->
<p class="form-status" aria-live="polite" id="form-status"></p>
```

- [ ] **Step 7: Replace inline style overrides with utility classes**

```html
<!-- Find (contact-info section): -->
<section class="section" style="background: var(--color-cream);">
<!-- Replace with: -->
<section class="section section--parchment">

<!-- Find (contact-info eyebrow): -->
<span class="eyebrow" style="color: var(--color-gold-light);">
<!-- Replace with: -->
<span class="eyebrow eyebrow--light">

<!-- Find (CTA eyebrow): -->
<span class="eyebrow" style="color: var(--color-gold-light);">
<!-- Replace with: -->
<span class="eyebrow eyebrow--light">
```

- [ ] **Step 8: Push updated contact.html**

```
mcp__github-tharros__create_or_update_file
  path: contact.html
  message: "feat: Quiet Forest — form a11y, inputmode, utility classes"
```

---

## Task 7: js/main.js — Form validation + focus trap

**Files:**
- Modify: `js/main.js`

- [ ] **Step 1: Read current js/main.js** (note SHA)

- [ ] **Step 2: Write complete new main.js**

Replace the entire file with:

```javascript
(function () {
  // Header scroll
  const header = document.querySelector('.header');
  const onScroll = () => {
    if (window.scrollY > 20) header.classList.add('scrolled');
    else header.classList.remove('scrolled');
  };
  window.addEventListener('scroll', onScroll, { passive: true });
  onScroll();

  // Mobile menu + focus trap
  const hamburger = document.querySelector('.hamburger');
  const navMenu = document.querySelector('.nav__menu');

  const getFocusable = (el) =>
    [...el.querySelectorAll('a, button, input, select, textarea, [tabindex]:not([tabindex="-1"])')].filter(
      (e) => !e.disabled && e.offsetParent !== null
    );

  const setMenuOpen = (open) => {
    hamburger.classList.toggle('open', open);
    navMenu.classList.toggle('open', open);
    hamburger.setAttribute('aria-expanded', String(open));
    document.body.style.overflow = open ? 'hidden' : '';
  };

  if (hamburger && navMenu) {
    hamburger.setAttribute('aria-expanded', 'false');
    hamburger.setAttribute('aria-controls', 'primary-nav');
    hamburger.addEventListener('click', () => {
      setMenuOpen(!hamburger.classList.contains('open'));
    });
    navMenu.querySelectorAll('a').forEach(a => {
      a.addEventListener('click', () => setMenuOpen(false));
    });
    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape' && hamburger.classList.contains('open')) {
        setMenuOpen(false);
        hamburger.focus();
        return;
      }
      if (e.key === 'Tab' && hamburger.classList.contains('open')) {
        const focusable = getFocusable(navMenu);
        if (!focusable.length) return;
        const first = focusable[0];
        const last = focusable[focusable.length - 1];
        if (e.shiftKey && document.activeElement === first) {
          e.preventDefault();
          last.focus();
        } else if (!e.shiftKey && document.activeElement === last) {
          e.preventDefault();
          first.focus();
        }
      }
    });
  }

  // Reveal animation
  const reveals = document.querySelectorAll('.reveal');
  if ('IntersectionObserver' in window && reveals.length) {
    const io = new IntersectionObserver((entries) => {
      entries.forEach((e) => {
        if (e.isIntersecting) {
          e.target.classList.add('is-visible');
          io.unobserve(e.target);
        }
      });
    }, { threshold: 0.12, rootMargin: '0px 0px -40px 0px' });
    reveals.forEach((el) => io.observe(el));
  } else {
    reveals.forEach((el) => el.classList.add('is-visible'));
  }

  // Form validation
  const form = document.querySelector('form[data-contact]');
  if (form) {
    const statusEl = document.getElementById('form-status');

    const validators = {
      first: (v) => v.trim() ? '' : 'Please enter your first name.',
      last: (v) => v.trim() ? '' : 'Please enter your last name.',
      email: (v) => {
        if (!v.trim()) return 'Please enter your email address.';
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v) ? '' : 'Please enter a valid email address.';
      }
    };

    const showError = (fieldId, message) => {
      const input = document.getElementById(fieldId);
      const errorEl = document.getElementById(fieldId + '-error');
      if (!input || !errorEl) return;
      errorEl.textContent = message;
      input.setAttribute('aria-invalid', message ? 'true' : 'false');
    };

    const validateField = (fieldId) => {
      const input = document.getElementById(fieldId);
      if (!input || !validators[fieldId]) return true;
      const error = validators[fieldId](input.value);
      showError(fieldId, error);
      return !error;
    };

    ['first', 'last', 'email'].forEach(function (fieldId) {
      const input = document.getElementById(fieldId);
      if (input) input.addEventListener('blur', function () { validateField(fieldId); });
    });

    form.addEventListener('submit', function (e) {
      e.preventDefault();
      const fields = ['first', 'last', 'email'];
      const results = fields.map(validateField);
      const valid = results.every(Boolean);
      if (!valid) {
        const firstInvalid = fields.find(function (f) {
          const input = document.getElementById(f);
          return input && input.getAttribute('aria-invalid') === 'true';
        });
        if (firstInvalid) document.getElementById(firstInvalid).focus();
        return;
      }
      if (statusEl) statusEl.textContent = 'Thank you — we will be in touch within one business day.';
      const btn = form.querySelector('button[type="submit"]');
      if (btn) { btn.textContent = 'Message Sent'; btn.disabled = true; }
    });
  }

  // Year
  const yearEl = document.querySelector('[data-year]');
  if (yearEl) yearEl.textContent = new Date().getFullYear();
})();
```

- [ ] **Step 3: Push updated main.js**

```
mcp__github-tharros__create_or_update_file
  path: js/main.js
  message: "feat: form validation, focus trap, aria feedback"
  sha: <sha from Step 1>
```

---

## Task 8: Verify + final check

- [ ] **Step 1: Check all 6 files are updated**

Fetch each file and confirm key markers are present:

| File | Check for |
|------|-----------|
| `css/styles.css` | `--color-forest`, `"Lora"`, `.founder-quote`, `.service-card--primary`, no `--color-navy` |
| `js/main.js` | `getFocusable`, `validators`, `showError`, `form-status` |
| `index.html` | `<section class="founder-quote">`, `services-primary`, `services-secondary`, `<picture>` |
| `about.html` | `credential-card__num`, `section--parchment`, `<picture>` |
| `services.html` | `Pet provisions`, `ODSP beneficiary gifts`, `section--white` |
| `contact.html` | `aria-describedby="first-error"`, `form-status`, `inputmode="tel"` |

- [ ] **Step 2: Flag the WebP manual step**

`images/erin-mcnamara.webp` must be generated from the source JPEG and committed to the repo. The `<picture>` elements will gracefully fall back to JPEG until this file exists. Note this for the client or handle via CDN image optimisation at deployment.

---

## Notes

- **No local environment:** All file operations go through `mcp__github-tharros__get_file_contents` (read) and `mcp__github-tharros__create_or_update_file` (write). Always read first to get the current SHA before writing.
- **CSS is one file:** Tasks 1 and 2 both modify `css/styles.css`. Read a fresh SHA at the start of each task.
- **WebP fallback:** `<picture>` with a missing `.webp` source will silently fall back to the JPEG — no broken image risk.
- **Token renames are critical order-sensitive:** Do `gold-light` before `gold`, and `cream-dark` before `cream` in the find-replace pass, otherwise you'll produce double-partial names like `var(--color-honeyl-light)`.
