# مِرْقاة — Landing Page

A single-page Arabic RTL landing site for **مِرْقاة** (Marqaa), a premium educational consulting venture serving elite private secondary schools in Amman, Jordan.

This README is the entry point for any developer joining the project. Read it before making changes.

---

## Table of Contents

1. [Tech Stack](#tech-stack)
2. [Repository Structure](#repository-structure)
3. [Local Development](#local-development)
4. [Deployment](#deployment)
5. [Brand & Design System](#brand--design-system)
6. [Page Architecture](#page-architecture)
7. [Section-by-Section Guide](#section-by-section-guide)
8. [Common Editing Tasks](#common-editing-tasks)
9. [Arabic & RTL Considerations](#arabic--rtl-considerations)
10. [Compliance Rules](#compliance-rules)
11. [Browser Support](#browser-support)

---

## Tech Stack

This is a **fully static, dependency-free** site:

- **HTML5** — single `index.html` file
- **CSS3** — embedded in `<style>` block at the top of `index.html` (not externalized)
- **Vanilla JavaScript** — embedded in `<script>` block at the bottom of `index.html`
- **No frameworks.** No React, no Vue, no jQuery, no build step, no bundler, no `package.json`
- **External dependencies (CDN-only):**
  - Google Fonts: `Amiri`, `Tajawal`, `Scheherazade New`

**Why this stack:** Hosting on Cloudflare Pages, instant deploys, zero build complexity, sub-second page loads, and direct edit-in-browser DevTools workflow.

---

## Repository Structure

```
/
├── index.html            ← The entire site lives here
├── README.md             ← This file
└── assets/
    ├── images/           ← Brand visuals, photos
    ├── icons/            ← SVG icons (currently inline in HTML)
    └── fonts/            ← Reserved for self-hosted fonts (currently unused; using Google Fonts CDN)
```

The site is intentionally a **single file** to keep deploys atomic and review trivial. If you split CSS/JS into separate files later, update the deploy config accordingly.

---

## Local Development

No build step. Just open the file:

```bash
# Option 1 — open directly in browser
open index.html

# Option 2 — quick local server (recommended for testing fonts/CDN)
python3 -m http.server 8000
# then visit http://localhost:8000

# Option 3 — VS Code Live Server extension
# Right-click index.html → "Open with Live Server"
```

---

## Deployment

Auto-deployed via **Cloudflare Pages** connected to the GitHub repository.

- Push to `main` → automatic deploy
- No build command needed (static site)
- Output directory: repository root

To deploy manually or change the connected branch, see the Cloudflare Pages dashboard.

---

## Brand & Design System

### Color Palette

All colors are defined as CSS custom properties at the top of the `<style>` block:

| Token | Value | Usage |
|---|---|---|
| `--navy` | `#0D1F3C` | Primary dark background (hero, alternating sections) |
| `--gold` | `#C9A84C` | Primary brand accent — headings, ornaments, CTAs |
| `--gold-m` | `#E8D5A3` | Muted gold — soft accents, hover states |
| `--cream` | `#F5F0E8` | Light section backgrounds, cards on dark sections |
| `--charcoal` | `#1A1A2E` | Footer background, body text on light sections |
| `--gold-a10` | `rgba(201,168,76,.10)` | 10% gold — subtle backgrounds |
| `--gold-a22` | `rgba(201,168,76,.22)` | 22% gold — borders |
| `--gold-a45` | `rgba(201,168,76,.45)` | 45% gold — emphasis borders |

**Rule:** Never hardcode hex values inside selectors. Always reference the CSS variables. If a new color is genuinely needed, add it to `:root` first.

### Typography

Three Google Font families are loaded:

| Token | Family | Use case |
|---|---|---|
| `--f-disp` | `Amiri` (serif) | All display headings, logo, callouts — classical authority |
| `--f-body` | `Tajawal` (sans-serif) | All body text, UI labels, buttons |
| (inline) | `Scheherazade New` | Optional — heavy-tashkeel text where Amiri/Tajawal struggle |

**Type scale:** Most sizes use `clamp(min, vw-based, max)` to scale fluidly between mobile and desktop. Example: `font-size: clamp(2.15rem, 4.6vw, 3.3rem);`

When changing a font size, prefer adjusting the existing `clamp()` rather than replacing it with a fixed `px` value — this preserves responsiveness.

### Spacing & Layout

- **Container:** `.wrap` — `max-width: 1140px`, horizontal padding `2rem` (desktop) / `1.25rem` (mobile)
- **Section padding:** `96px 0` desktop, `70px 0` mobile (`@media (max-width: 768px)`)
- **Breakpoints:**
  - Mobile: `≤ 480px`
  - Tablet: `≤ 768px`
  - Small desktop: `≤ 1024px`
  - Default: above 1024px

### Ornamental Elements

The site uses recurring decorative patterns. When adding a new section, reuse these instead of inventing new ones:

- **`.g-rule`** — full-width thin gold gradient line (between sections)
- **`.orn`** — centered gold ornament: line + small diamond + diamond + small diamond + line
  - `.orn.cr` — cream-section variant (darker dots/lines for light backgrounds)
- **Section eyebrow** — small uppercase label above headings (use `.eyebrow` class). **Note:** all section eyebrows were removed in v3.2; reintroduce only if explicitly required.

### Animation

- **Scroll reveal** — apply `.rv` class to any element. Add delay variants `.d1`–`.d5` for staggered animations. Powered by `IntersectionObserver` at the bottom of the `<script>` block.
- **Glow keyframe** — `@keyframes glow` — applied to the hero logo and footer logo for a subtle gold text-shadow pulse.
- **Particle system** — the hero canvas runs a custom rising-gold-particles animation (~68 particles). See the `(function () { ... })()` block at the top of the `<script>`.

---

## Page Architecture

The page is a vertical scroll of nine sections, separated by `.g-rule` dividers:

```
① Hero                          (navy)
② About / من نحن                 (cream)
③ Diagnosed Problems            (navy)
④ Why Marqaa / لماذا مِرْقاة      (cream)
⑤ Sessions Accordion            (navy)  ← centerpiece
⑥ Topics Accordion              (cream)
⑦ Exclusivity                   (navy)
⑧ Contact / WhatsApp CTA         (cream)
⑨ Footer                        (charcoal)
```

The alternating navy ↔ cream rhythm is intentional — it gives the page a clear visual cadence and lets each section breathe.

---

## Section-by-Section Guide

### ① Hero (`<section id="hero">`)

The visual anchor of the page. Contains:

- **Canvas** (`#hero-canvas`) — animated gold particle system
- **Grain overlay** (`.h-grain`) — subtle SVG noise filter for texture
- **Radial glow** (`.h-glow`) — soft gold halo behind the logo
- **Logo** (`.h-logo`) — the brand mark "مِرْقاة" in Amiri
- **Tagline + sub-tagline** (`.h-tag`, `.h-sub`)
- **Scroll indicator** (`.scroll-ind`) — animated downward arrow

**Editing tips:**
- To adjust the logo size, edit `.h-logo { font-size: clamp(...) }`
- To add space between the logo and the tagline, increase the `gap` on `.h-content`
- To slow/speed the particle animation, adjust the velocity values (`.vy`, `.vx`) in the `P` class inside the script
- The logo container needs `overflow: visible`, `width: auto`, `white-space: nowrap` so the ة tail doesn't clip — do not change these

### ② About / من نحن (`<section id="about">`)

Contains the brand introduction paragraph and a gold-accented callout box.

**Compliance-critical:** This section talks about the team. All references to experts must use **past-tense verbs** when describing their relationship to government systems. See [Compliance Rules](#compliance-rules) below.

- `.about-body` — the main paragraph
- `.about-callout` — the highlighted gold-bordered quote box

### ③ Diagnosed Problems (`<section id="problems">`)

Seven cream cards on a navy background, arranged in a 2-column grid. The 7th card is centered in its own row using:

```css
.prob-card--last {
  grid-column: 1 / -1;
  justify-self: center;
  width: calc(50% - 9px);
}
```

**Editing tips:**
- To add an 8th card, remove the `.prob-card--last` class — the grid will re-balance to 4 rows of 2
- To remove a card, you may need to re-evaluate which card gets `.prob-card--last`

### ④ Why Marqaa / لماذا مِرْقاة (`<section id="why">`)

Five horizontal feature rows. Each row has:
- A small gold star icon (`.why-icon` with inline SVG)
- A bold label (`.why-text strong`)
- A description line (`.why-text span`)

Rows are separated by thin gold horizontal rules.

### ⑤ Sessions Accordion (`<section id="sessions">`)

The page's centerpiece. Five expandable cards, one per audience type (Grade 9, Grades 10–11, Grade 12, Parents, Counselors).

**Structure of each card (`.sess-card`):**
- `.sess-header` — always visible: grade label + session type + duration + chevron
- `.sess-body` — collapsible: contains `.sess-detail` with two `<dl>` blocks (الوصف + النتيجة المتوقعة)

**Behavior:** Only one card can be open at a time (enforced by the script — opening a card auto-closes the others).

**Editing the duration text:** All five durations should match. Currently set to "ساعتان".

**Below the accordion:** the `.sess-followup` line about the post-session Q&A.

### ⑥ Topics Accordion (`<section id="topics">`)

Seven collapsible clusters, each containing a list of sub-topics. Unlike the sessions section, **multiple clusters can be open simultaneously**.

**Structure of each cluster (`.acc-item`):**
- `.acc-header` — cluster title + chevron
- `.acc-body` → `.acc-content` → `.acc-list` (an unordered list with gold-diamond bullets)

**Adding a topic to a cluster:** Insert a new `<li>` inside the relevant `.acc-list`. The bullet styling is handled by `.acc-list li::before`.

**Adding a new cluster:** Duplicate an existing `.acc-item` block, change the `.acc-title`, and replace the `<li>` items.

### ⑦ Exclusivity (`<section id="exclusivity">`)

A statement section emphasizing partnership selectivity. Contains:
- A large display headline (`.excl-headline`)
- A smaller subtitle (`.excl-sub`)
- Three cream cards (`.excl-card`) in a 3-column grid (collapses to 2 columns on tablet, 1 on mobile)

### ⑧ Contact / WhatsApp CTA (`<section id="contact">`)

A clean section with a single WhatsApp button (`.wa-btn`) linking to `https://wa.me/962795487535`.

**To change the WhatsApp number:** edit the `href` on the `.wa-btn` anchor. The number must be in international format without `+` or spaces.

**No forms exist.** No email capture, no contact form. The single WhatsApp CTA is the only contact mechanism — this is intentional positioning.

### ⑨ Footer (`<footer>`)

Small centered block on charcoal background. Contains the brand logo and copyright line.

---

## Common Editing Tasks

### Change a text string

1. Open `index.html`
2. Find the string with Cmd/Ctrl+F
3. Edit in place
4. Save

There's no localization layer — Arabic text is hardcoded in the HTML.

### Change a color globally

Edit the value of the relevant CSS variable in `:root` at the top of the `<style>` block. The new color will propagate everywhere.

### Adjust font sizes

Each section heading and body text uses `clamp(min, vw-based, max)`. Adjust the three values together to keep responsive scaling intact:
- `min` — smallest size (mobile)
- middle — fluid value based on viewport width
- `max` — largest size (large desktop)

### Add a new section

1. Insert a `<section>` block in the desired position
2. Add an alternating background color (cream after navy, navy after cream)
3. Wrap content in `.wrap`
4. Add a `.g-rule` divider before/after as needed
5. Use `.s-title` (or `.s-title.dark`) for the heading
6. Use `.orn` (or `.orn.cr`) for ornamental dividers
7. Apply `.rv` (with `.d1`–`.d5` delay variants) for scroll-reveal animations

### Add a new accordion item (Topics section)

```html
<div class="acc-item" role="listitem">
  <div class="acc-header" role="button" aria-expanded="false" tabindex="0">
    <span class="acc-title">عنوان المجموعة الجديدة</span>
    <svg class="acc-chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <polyline points="6 9 12 15 18 9"/>
    </svg>
  </div>
  <div class="acc-body">
    <div class="acc-content">
      <ul class="acc-list">
        <li>عنصر أول</li>
        <li>عنصر ثاني</li>
      </ul>
    </div>
  </div>
</div>
```

### Update the WhatsApp number

Edit the `href` on `.wa-btn`:

```html
<a href="https://wa.me/9627XXXXXXXX" class="wa-btn">...
```

---

## Arabic & RTL Considerations

This is a fully RTL site. Several quirks to be aware of:

### Brand name spelling

Always render the brand name with full tashkeel: **مِرْقاة**

- Kasra under the meem (مِ)
- Sukoon over the ra (رْ)
- The bare form `مرقاة` (no diacritics) and the variant `مَرقاة` (fatha) are **incorrect** — never use them
- Before any commit, search for bare `مرقاة` and verify each occurrence is the diacritized form

### Tashkeel rendering

For text with heavy tashkeel, use:

```css
font-feature-settings: "calt" 1, "clig" 1;
text-rendering: optimizeLegibility;
```

These flags enable contextual ligatures and improve diacritic positioning. Already applied in the relevant rules — preserve them.

### Logo container clipping

The brand logo "مِرْقاة" can have its final ة clipped if its container has `overflow: hidden` or a constrained width. The hero logo container uses `overflow: visible`, `width: auto`, `white-space: nowrap` — keep these.

### RTL borders and shadows

When using directional borders (e.g., `border-left`, `border-right`), remember that in RTL the *visual* left/right is flipped. The current code uses physical properties (`border-right` for the right edge of cards in the cream-on-navy sections). If you switch to logical properties (`border-inline-start`, `border-inline-end`), test thoroughly.

### Punctuation

Use Arabic punctuation where appropriate: `،` (Arabic comma), `؛` (Arabic semicolon), `؟` (Arabic question mark). The current copy uses these correctly — preserve them.

### `dir="rtl"` and `lang="ar"`

Both attributes are set on the `<html>` tag. Do not remove them.

---

## Compliance Rules

These rules exist for legal/regulatory reasons in Jordan and **must not be relaxed** without explicit business approval.

### 1. Past-tense framing for the team

All references to the Marqaa team's experts and their relationship to the Jordanian higher education system **must use past-tense verbs** (عملوا, شغلوا, اكتسبوا, تأسست). Present-tense framing that implies current government office is forbidden.

**Approved callout line:**
> يضم فريق مِرْقاة خبراء عملوا في منظومة القبول الجامعي الموحد في الأردن

**Why:** Implying that a sitting government official is involved in private business could constitute *تكسب غير مشروع* (improper personal gain from public office) under Jordanian law.

### 2. No partner names in written materials

The names of individual team members and government-experienced partners are **not published** anywhere — not on this site, not in proposals, not in marketing materials. Identity is disclosed only in face-to-face school meetings.

### 3. No pricing on the site

Pricing is **never published** on this site, in proposals, or in stored records. All pricing is negotiated per school in direct conversation. If you see fixed pricing tiers anywhere in the codebase or content, treat as a bug and remove.

### 4. School cap framing

Public materials describe partner schools as "a limited number" or "عدد محدود". The exact cap (7–10 schools per year) is internal — do not publish the specific number.

### 5. No delivery platform mentions

Do not mention specific session delivery platforms, meeting software (Zoom, Teams, Google Meet, etc.), or logistical specifics. These are negotiated per school.

---

## Browser Support

Tested and supported:

- Chrome / Edge (latest 2 versions)
- Safari (latest 2 versions, including iOS Safari)
- Firefox (latest 2 versions)

Uses modern features (`IntersectionObserver`, CSS `clamp()`, CSS Grid, custom properties) — no IE11 support.

---

## Conventions

- **Indentation:** 2 spaces
- **Quotes:** double quotes for HTML attributes, single quotes for JS strings
- **Classnames:** kebab-case, BEM-ish (`.sess-card`, `.sess-header`, `.sess-detail`)
- **Comments:** decorative section dividers in CSS use `═══════════════` boxes — preserve the style when adding new sections
- **Commits:** prefix with type (`feat:`, `fix:`, `docs:`, `style:`) and reference the version (`v3.4`)

---

## Questions

If anything in this README is outdated or unclear after a change, update the README in the same commit. The README is the contract — keep it accurate.
