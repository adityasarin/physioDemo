# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static single-page website for **Dr. Mohammed Wasim Raja Physiotherapy and Slimming Clinic**, located at Shop 17, Mahagun Mart, Sector 78, Noida. No build step, no framework, no package manager — open `index.html` directly in a browser.

**Live domain:** `https://drwasimphysiotherapy.in/`

## File Structure

```
index.html        — entire page (HTML + inline JSON-LD structured data)
css/style.css     — all styles (mobile-first, CSS custom properties)
js/main.js        — all interactivity (vanilla JS, IIFE, no dependencies)
images/           — clinic/certificate photos
```

## Design System

CSS custom properties are defined at `:root` in `style.css`:

| Token | Value | Use |
|---|---|---|
| `--navy` | `#0D3B5E` | Primary brand color |
| `--teal` | `#1a8a9a` | Accent |
| `--gold` | `#e8a020` | Highlights / CTAs |
| `--green` | `#25d366` | WhatsApp button |

Fonts: **Poppins** (headings) and **Inter** (body), both from Google Fonts.

## Key Content Rules

- **Review count** appears in multiple places — the hero badge, the JSON-LD `reviewCount` field, and the testimonials section. Keep them in sync when updating.
- **Phone numbers:** `+91 9013103095` (primary, WhatsApp) and `+91 120 4245219` (landline). Update both in JSON-LD and visible markup together.
- **Opening hours** are specified in both the visible schedule grid and the JSON-LD `openingHoursSpecification` blocks. Keep both in sync.
- The canonical URL, OG tags, and JSON-LD `@id` / `url` fields all point to `https://drwasimphysiotherapy.in/` — don't change these without deploying to that domain.

## SEO / Structured Data

`index.html` contains an inline `<script type="application/ld+json">` with a `@graph` of three schemas:
1. `MedicalBusiness` + `LocalBusiness` — clinic entity with address, hours, ratings, services
2. `Physician` — Dr. Wasim Raja's credentials and role
3. `FAQPage` — FAQ accordion entries mirrored in JSON-LD

Whenever FAQ questions/answers are edited in the HTML accordion, mirror the change in the JSON-LD `FAQPage` block, and vice versa.

## JS Behaviors

All JS lives in `js/main.js` as a single IIFE. Behaviors:
- **Mobile nav** — toggle `#nav-menu.open` on `#nav-toggle` click; close on outside click or nav-link click
- **Header shrink** — adds `.scrolled` to `#site-header` after 40 px scroll
- **FAQ accordion** — `aria-expanded` + `hidden` attribute pattern; only one item open at a time
- **Scroll reveal** — `IntersectionObserver` adds `.visible` to `.reveal`, `.reveal-left`, `.reveal-right`
- **Active nav highlight** — `IntersectionObserver` at 40 % threshold toggles `.active` on `.nav-link`
- **Parallax** — `data-parallax-speed` attribute on hero element; disabled on mobile (`< 768px`)
