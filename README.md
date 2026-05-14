# McNamara Law Services — Website

A modern, sleek marketing site for McNamara Law Services, an Ottawa estate law firm led by Erin McNamara, B.A., LL.B.

## Structure

- `index.html` — Home (hero, services overview, about, testimonial, process, service areas, CTA)
- `about.html` — About Erin McNamara, credentials and philosophy
- `services.html` — Detailed practice areas + ways to meet
- `contact.html` — Contact form, office details, embedded map
- `css/styles.css` — Design system (custom properties, layout, components, responsive)
- `js/main.js` — Sticky header, mobile nav, reveal-on-scroll, form handling
- `images/` — Erin McNamara headshot and original logo asset

## Design

- Palette: deep navy `#0b2545`, gold accent `#b08742`, cream `#faf7f1`
- Typography: Cormorant Garamond (display serif) + Inter (sans body)
- Responsive single-page-per-section layout with subtle reveal animations
- No build step — open `index.html` directly or serve the folder with any static server

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```
