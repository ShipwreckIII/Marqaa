# مِرقاة — Marqaa

A static landing page for **مِرقاة**, a premium academic advisory service based in Amman, Jordan. The page is fully in Arabic (RTL) and targets principals of elite private schools.

## Overview

مِرقاة (meaning "a rung on a ladder") connects distinguished private schools with senior academic experts who have decades of experience inside Jordan's higher education system. The service is invitation-only and limited to a small number of partner schools per year.

## Stack

- Pure HTML + CSS + vanilla JavaScript — single `index.html` file
- No frameworks, no build tools, no dependencies
- Google Fonts CDN: **Amiri**, **Scheherazade New**, **Tajawal**

## Page Sections

| # | Section | Background |
|---|---------|------------|
| 1 | Hero — animated gold particle canvas, logo, tagline | Navy |
| 2 | About — what مِرقاة is and the meaning of the name | Cream |
| 3 | Pain Points — challenges students and families face | Navy |
| 4 | Benefits — what students gain from the service | Cream |
| 5 | Experts — anonymous team credentials and institutional pillars | Navy |
| 6 | Services — three service types (no pricing) | Cream |
| 7 | Exclusivity — invitation-only model | Navy |
| 8 | Contact — WhatsApp CTA | Charcoal |
| 9 | Footer | Charcoal |

## Design

- **Direction:** RTL throughout (`dir="rtl"` on `<html>`)
- **Colors:** Deep navy `#0D1F3C` · Rich gold `#C9A84C` · Cream `#F5F0E8` · Charcoal `#1A1A2E`
- **Typography:** Amiri (display headings) · Tajawal (body) · Scheherazade New (tashkeel-heavy headings)
- **Aesthetic:** Luxury Arabic institutional branding — gold accents, geometric Islamic ornaments, deliberate whitespace
- **Animations:** Canvas-based gold particle system in hero · Intersection Observer scroll-reveal on all sections

## Usage

No build step required. Open `index.html` directly in a browser or serve with any static file server:

```bash
npx serve .
# or
python -m http.server 8000
```

## Notes

- The name مِرقاة is always written with full Arabic diacritics (tashkeel) — never without
- Expert identities are not disclosed on the page; they are revealed upon signing the partnership agreement
- The WhatsApp link in the contact section uses a placeholder number and should be updated before deployment
