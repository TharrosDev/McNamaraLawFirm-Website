# McNamara Law Services — Website

A modern, sleek marketing site for McNamara Law Services, an Ottawa estate law firm led by Erin McNamara, B.A., LL.B.

## Structure

```
index.html      Home: hero, services overview, about, testimonial, process, service areas, CTA
about.html      Erin's bio, credential cards, philosophy quote
services.html   Seven detailed practice areas + three ways to meet
contact.html    Form, contact details, embedded map
404.html        Custom not-found page
robots.txt      Allow all, points to sitemap
sitemap.xml     Static sitemap of canonical pages
css/styles.css  Design system (custom properties, components, responsive)
js/main.js      Sticky header, mobile nav, reveal-on-scroll, form handling
images/         Erin McNamara headshot
```

## Design

- Palette: deep navy `#0b2545`, gold accent `#b08742`, cream `#faf7f1`
- Typography: Cormorant Garamond (display serif) + Inter (sans body)
- Responsive layout with reveal-on-scroll animations (honours `prefers-reduced-motion`)
- Sticky header with blur-on-scroll, mobile drawer nav with proper `aria-expanded`
- Skip-to-content link, `aria-current` on active nav links, visible focus rings
- Open Graph + Twitter Card meta on every page; LegalService / Person JSON-LD on home and about
- No build step — open `index.html` directly or serve the folder with any static server

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Outstanding before launch

- Hook the contact form to a real endpoint (Formspree, Netlify Forms, or a backend) — currently front-end only.
- Verify the headline figures on the trust strip (`20+ years`, `1,000s of plans drafted`) with the firm before going live.
- If a real bitmap logo is provided, swap the SVG favicon and the "M" monogram for the firm's mark.
