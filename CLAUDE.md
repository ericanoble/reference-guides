# CLAUDE.md — Project Design System & Specification

> **This file is the single source of truth for all pages in this project.**
> Claude Code must read this file before creating or editing any HTML file.
> Do not deviate from these tokens, components, or conventions without explicit instruction.

---

## 1. PROJECT OVERVIEW

### Summary
A static multi-page reference guide site for Cricut Maker 3 classroom workflows. Each page covers a specific production process (e.g. sticker sheets, vinyl cutting, card making) and provides teachers with multimodal, step-by-step instructions including process maps, expandable steps, interactive checklists, flowcharts, callout boxes, video links, and troubleshooting tables.

### Deployment Target
**GitHub Pages** — static site, no server, no build step, no framework. All pages are plain `.html` files with inline `<style>` blocks (see section 4 for the rationale and rules).

### Audience
School staff and students. Tone is clear, professional, and practical.

### Core Tech Stack
- **Pure HTML5 + CSS3** — no frameworks, no preprocessors
- **Vanilla JavaScript** — inline `<script>` at bottom of `<body>`, no external JS libraries
- **No external CSS CDNs** — all styles are self-contained (no Tailwind, no Bootstrap)
- **No Google Fonts** — system font stack only (see Typography)
- **No icon libraries** — emoji used for icons throughout

---

## 2. BUILD, RUN, & DEPLOY COMMANDS

### Static Site — No Build Step
There is no build process. Edit `.html` files directly and open in a browser.

### Local Preview
```bash
# From the repository root — serves on http://localhost:8000
python -m http.server 8000

# Alternative (Node.js)
npx serve .
```

### Deploy to GitHub Pages
```bash
# Standard commit and push to the branch configured for GitHub Pages (typically main or gh-pages)
git add .
git commit -m "describe your change"
git push origin main
```

> If using a `gh-pages` branch:
> ```bash
> git subtree push --prefix . origin gh-pages
> ```

### File Naming
- All filenames: **lowercase, hyphenated**, no spaces. e.g. `sticker-sheets.html`, `vinyl-cutting.html`
- The site root index: `index.html`

---

## 3. DESIGN SYSTEM & VISUAL TOKENS

All tokens are defined as CSS custom properties on `:root`. **Copy this block verbatim into every page's `<style>` tag.**

```css
:root {
  /* ─── COLOUR PALETTE ─── */
  --teal:        #00838F;   /* Primary brand — interactive elements, accents */
  --teal-dark:   #006472;   /* Hover states, section labels, tip callout text */
  --teal-light:  #E0F4F6;   /* Teal surface backgrounds (callout, pmap, step open) */
  --teal-mid:    #B2E5EA;   /* Teal borders */
  --green:       #2E7D32;   /* Success / action states */
  --green-light: #E8F5E9;   /* Success surface background */
  --amber:       #B45309;   /* Warning text / icons — WCAG AA compliant on amber-light */
  --amber-light: #FFF3E0;   /* Warning surface background */
  --red:         #C62828;   /* Danger / critical / required */
  --red-light:   #FFEBEE;   /* Danger surface background */
  --purple:      #6A1B9A;   /* Optional / info states */
  --purple-light:#F3E5F5;   /* Info surface background */
  --ink:         #1A2332;   /* Primary text — all body copy, headings */
  --ink-soft:    #3D4F63;   /* Secondary text — subtitles, captions, muted labels */
  --border:      #D0DDE6;   /* All borders and dividers */
  --surface:     #F7FAFB;   /* Page background, card backgrounds */
  --white:       #FFFFFF;   /* Card surfaces, step body backgrounds */

  /* ─── BORDER RADIUS ─── */
  --radius:      12px;      /* Cards, step headers, checklist cards */
  --radius-sm:   8px;       /* Smaller components — tags, chips, flowchart nodes */

  /* ─── SHADOWS ─── */
  --shadow:      0 2px 12px rgba(0,131,143,.12);   /* Hover state on step headers */
  --shadow-md:   0 6px 24px rgba(0,131,143,.16);   /* Hover state on video cards */
}
```

### Colour Accessibility (WCAG 2.2 Level AA)
All colour combinations in this system have been verified to meet 4.5:1 contrast for normal text and 3:1 for large text. **Do not substitute colours without re-verifying contrast ratios.** Critical verified pairs:

| Foreground | Background | Ratio | Used for |
|---|---|---|---|
| `--ink` `#1A2332` | `--surface` `#F7FAFB` | 15.8:1 | Body text |
| `--ink` `#1A2332` | `--white` `#FFFFFF` | 16.7:1 | Step body text |
| `--ink-soft` `#3D4F63` | `--white` `#FFFFFF` | 8.4:1 | Subtitles, captions |
| `--teal-dark` `#006472` | `--surface` `#F7FAFB` | 6.5:1 | Section eyebrow labels |
| `--teal-dark` `#006472` | `--teal-light` `#E0F4F6` | 6.0:1 | Tip callout labels |
| `--amber` `#B45309` | `--amber-light` `#FFF3E0` | 4.6:1 | Warn callout labels |
| `--red` `#C62828` | `--red-light` `#FFEBEE` | 4.9:1 | Danger callout labels |
| `--purple` `#6A1B9A` | `--purple-light` `#F3E5F5` | 7.8:1 | Info callout labels |
| `--green` `#2E7D32` | `--green-light` `#E8F5E9` | 4.6:1 | Success callout labels |
| `#5C4A00` | `#FFF9C4` | 8.1:1 | One-time tag chip |
| `#0D47A1` | `#BBDEFB` | 6.2:1 | op-print chip |
| `#1B5E20` | `#C8E6C9` | 5.9:1 | op-die chip |
| `#FFFFFF` | `--teal` `#00838F` | 4.5:1 | Step number circles |
| `#FFFFFF` | `--ink` `#1A2332` | 16.7:1 | Table headers, footer |

### Typography

**Font Stack**
```css
font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
```
No external font imports. System fonts only.

**Type Scale**

| Element | Size | Weight | Line Height | Notes |
|---|---|---|---|---|
| Hero `h1` | `clamp(26px, 5vw, 42px)` | 800 | 1.25 | Fluid, letter-spacing `-0.02em` |
| Section title `.section-title` | `22px` | 700 | 1.25 | `color: var(--ink)` |
| Step title `.step-title` | `17px` | 700 | 1.25 | Reduced to `15px` at ≤600px |
| Body / sub-steps | `16px` / `15px` | 400 | 1.6 | `color: var(--ink)` |
| Hero paragraph | `17px` | 400 | 1.6 | White, max-width `700px` centered |
| Callout body | `14px` | 400 | 1.55 | `color: var(--ink)` |
| Callout label `strong` | `13px` | 700 | — | Uppercase, `letter-spacing: .06em` |
| Step subtitle | `13px` | 400 | — | `color: var(--ink-soft)` |
| Section eyebrow `.section-label` | `12px` | 700 | — | Uppercase, `letter-spacing: .12em`, `color: var(--teal-dark)` |
| Cards, tags, chips, captions | `12px` min | 400–700 | — | **Never below 12px** |

> **Minimum font size rule: 12px absolute floor across all elements.** This was established during WCAG accessibility review. Do not create any text element smaller than 12px.

### Spacing & Layout

**Container**
```css
.container { max-width: 1140px; margin: 0 auto; padding: 0 20px; }
```
This is the only layout wrapper. Every page section uses `<div class="container">` inside it.

**Section Padding** (desktop)
```css
padding: 36px 32px;   /* Standard section padding */
padding: 48px 32px 56px;  /* Hero only */
padding: 28px 32px;   /* Footer */
```

**Grid Conventions — all grids use `auto-fit` / `minmax` for automatic responsiveness:**
```css
/* Process map / icon cards */
grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));

/* Needs / equipment cards */
grid-template-columns: repeat(auto-fit, minmax(170px, 1fr));

/* Video cards */
grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));

/* Checklist groups */
grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
```

**Spacing Utilities**
```css
.mt8  { margin-top: 8px; }
.mt12 { margin-top: 12px; }
.mt16 { margin-top: 16px; }
```

---

## 4. GLOBAL HTML/CSS STRUCTURAL RULES

### Style Architecture — Inline `<style>` Per Page
All CSS lives in a `<style>` block in the `<head>` of each individual `.html` file. There is no shared `styles.css` file. This is intentional for GitHub Pages deployment simplicity — each page is fully self-contained with zero external dependencies.

> **When creating new pages:** copy the full `<style>` block from `cricut-sticker-guide.html` as the base. Remove page-specific components (e.g. `.layer-diagram`, `.process-map`) if the new page does not use them. Keep all tokens, reset, layout, callout, step, footer, and responsive rules.

### External Dependencies
**None.** No CDN links. No `<link>` tags to external stylesheets. No external `<script>` tags. All pages must be fully functional offline once cloned.

### Mandatory `<head>` Block
Every page must include exactly this `<head>` structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PAGE TITLE — Cricut Maker 3 Guides</title>
<style>
  /* full :root token block + page CSS here */
</style>
</head>
```

### Global Header / Navigation Bar
Every page (including `index.html`) must include a **site navigation bar** directly above the hero. This provides wayfinding between the index and sub-pages. Template:

```html
<!-- SITE NAV -->
<nav class="site-nav" aria-label="Site navigation">
  <div class="container">
    <a href="index.html" class="nav-home">🏠 Guides Home</a>
    <div class="nav-links">
      <a href="cricut-sticker-guide.html" class="nav-link">Sticker Sheets</a>
      <!-- Add further sub-page links here as pages are created -->
    </div>
  </div>
</nav>
```

CSS for the site nav (add to every page's `<style>` block):

```css
.site-nav {
  background: var(--ink);
  padding: 10px 20px;
  position: sticky;
  top: 0;
  z-index: 100;
}
.site-nav .container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 8px;
}
.nav-home {
  color: white;
  text-decoration: none;
  font-weight: 700;
  font-size: 14px;
}
.nav-home:hover { color: var(--teal-light); }
.nav-links {
  display: flex;
  gap: 4px;
  flex-wrap: wrap;
}
.nav-link {
  color: rgba(255,255,255,0.75);
  text-decoration: none;
  font-size: 13px;
  padding: 4px 10px;
  border-radius: 6px;
  transition: background .15s, color .15s;
}
.nav-link:hover {
  background: rgba(255,255,255,0.1);
  color: white;
}
.nav-link.active {
  background: var(--teal);
  color: white;
}
@media (max-width: 600px) {
  .site-nav { padding: 8px 16px; }
  .nav-home { font-size: 13px; }
}
```

> **Active state:** On each page, add `class="nav-link active"` to the link that corresponds to the current page.

### Global Footer
Every page must include the same footer structure:

```html
<!-- FOOTER -->
<footer class="footer">
  <div class="container">
    <p>Cricut Maker 3 Guides · [School/Department Name]</p>
    <p style="margin-top:6px;">
      <a href="index.html">← Back to all guides</a>
    </p>
  </div>
</footer>
```

CSS (already in the shared style block):

```css
.footer {
  background: var(--ink);
  color: rgba(255,255,255,0.75);
  padding: 28px 32px;
  text-align: center;
  font-size: 13px;
}
.footer a { color: var(--teal-mid); }
```

> Use `<footer>` semantic element — not `<div class="footer">`.

### Hero Block
Every sub-page uses a hero section directly below the site nav. The gradient, badge, h1, subtitle paragraph, and chip row are all reusable:

```html
<!-- HERO -->
<div class="hero">
  <div class="container">
    <div class="hero-badge">Reference Guide</div>
    <h1>EMOJI Page Title</h1>
    <p>Subtitle · Subtitle · Subtitle</p>
    <div class="hero-chips">
      <span class="chip">🔧 Tool or material</span>
      <!-- repeat as needed -->
    </div>
  </div>
</div>
```

The hero gradient is: `linear-gradient(135deg, var(--teal-dark) 0%, var(--teal) 60%, #007580 100%)`
The SVG dot pattern overlay in `hero::before` should be copied verbatim — it is a decorative texture only.

### `<body>` Page Structure — Correct Semantic Element Order

```html
<body>
  <nav class="site-nav">…</nav>          <!-- sticky site navigation -->
  <div class="hero">…</div>              <!-- page hero — not <header> -->
  <main>
    <!-- all page content sections go inside <main> -->
    <section class="process-section">…</section>
    <section class="needs-section">…</section>
    <section class="flowchart-section">…</section>
    <section class="steps-section">…</section>
    <section class="video-section">…</section>
    <section class="checklist-section">…</section>
    <section class="trouble-section">…</section>
  </main>
  <footer class="footer">…</footer>
</body>
```

> **All pages use this semantic structure**, including `cricut-sticker-guide.html` (retrofitted 2026-06-26 — see Task 3). New pages must follow it exactly.

### File Architecture

```
/                          ← repository root
├── index.html             ← site home / guide directory
├── cricut-sticker-guide.html
├── [future-guide].html
├── CLAUDE.md              ← this file
└── assets/
    └── (optional — only if images or downloadable files are added)
```

No subdirectories for HTML pages. No `css/` folder (all CSS is inline per page). No `js/` folder (all JS is inline per page).

---

## 5. CODE STYLE & STANDARDS

### Semantic HTML Requirements
Every page **must** use correct semantic HTML5 elements:

| Element | Required use |
|---|---|
| `<nav>` | Site navigation bar |
| `<main>` | All content sections below the hero |
| `<section>` | Each named content section (process map, steps, checklist, etc.) |
| `<footer>` | Global page footer |
| `<h1>` | Page title (one per page, inside `.hero`) |
| `<h2>` | Section titles (`.section-title`) — use if converting from `<div>` |
| `<button>` | Expandable step headers — not `<div onclick>` |
| `<table>` | Troubleshooting and concept comparison data only — not for layout |
| `<label>` + `<input type="checkbox">` | Checklist items |
| `aria-expanded` | Must be toggled on `<button>` accordion headers |
| `aria-label` | Required on `<nav>` elements |

### CSS Class Naming Conventions

**Pattern:** `block-element` or `block--modifier` (simplified BEM-style, no double underscores needed)

| Class pattern | Purpose |
|---|---|
| `.section-label` | Small uppercase eyebrow label above section titles |
| `.section-title` | Primary heading within a section |
| `.container` | Max-width layout wrapper |
| `.hero`, `.hero-badge`, `.hero-chips`, `.chip` | Hero block components |
| `.callout.warn/danger/tip/info/success` | Five callout box variants |
| `.step-group`, `.step-header`, `.step-body`, `.step-num`, `.step-meta`, `.step-title`, `.step-subtitle`, `.step-tags`, `.step-toggle` | Accordion step system |
| `.step-header.critical-step` | Red-accent step variant |
| `.step-header.optional-step` | Purple-accent step variant |
| `.tag.one-time/required/optional/critical` | Step header tag chips |
| `.sub-steps` | Lettered sub-step `<ol>` inside `.step-body` |
| `.need-card`, `.need-icon`, `.need-name`, `.need-detail` | Equipment/materials card |
| `.pmap-step`, `.pmap-num` | Process map clickable step |
| `.pmap-step.critical` | Red-variant process map step |
| `.fc-node`, `.fc-arrow`, `.fc-node-label` | Flowchart components |
| `.fc-node.setup/design/critical-fc/action/optional-fc` | Flowchart node colour variants |
| `.video-card`, `.video-thumb`, `.play-btn`, `.video-info`, `.video-tag`, `.video-title`, `.video-channel` | Video link card |
| `.checklist-card`, `.checklist-card-header`, `.check-item`, `.check-label` | Checklist components |
| `.checklist-card-header.print-hdr/cut-hdr/finish-hdr` | Checklist header colour variants |
| `.trouble-table` | Troubleshooting data table |
| `.mt8`, `.mt12`, `.mt16` | Spacing utility classes |
| `.site-nav`, `.nav-home`, `.nav-links`, `.nav-link`, `.nav-link.active` | Site navigation |
| `.guide-grid`, `.guide-card`, `.guide-card.soon`, `.guide-icon`, `.guide-title`, `.guide-desc`, `.guide-cta` | Home-page guide directory cards (`index.html`) |
| `.section-intro` | Intro paragraph under a section title (`index.html`) |

### JavaScript Patterns
All JavaScript is **vanilla, inline, in a `<script>` tag at the bottom of `<body>`** before `</body>`. No external libraries.

The three reusable JS patterns established in this project:

**1. Accordion step toggle**
```javascript
function toggleStep(header) {
  const wasOpen = header.classList.contains('open');
  document.querySelectorAll('.step-header.open').forEach(h => {
    h.classList.remove('open');
    h.setAttribute('aria-expanded', 'false');
    const body = h.nextElementSibling;
    if (body) body.classList.remove('open');
  });
  if (!wasOpen) {
    header.classList.add('open');
    header.setAttribute('aria-expanded', 'true');
    const body = header.nextElementSibling;
    if (body) body.classList.add('open');
    setTimeout(() => {
      header.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
    }, 50);
  }
}
```

**2. Progress bar + completion message**
```javascript
const TOTAL = N; // set to actual checkbox count on the page
function updateProgress() {
  const checked = document.querySelectorAll('.check-item input[type=checkbox]:checked').length;
  const pct = Math.round((checked / TOTAL) * 100);
  document.getElementById('progress-fill').style.width = pct + '%';
  document.getElementById('progress-text').textContent = checked + ' / ' + TOTAL + ' complete';
  const msg = document.getElementById('completion-msg');
  if (checked === TOTAL) {
    msg.classList.add('show');
    msg.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
  } else {
    msg.classList.remove('show');
  }
}
```

**3. Auto-open step from URL hash + initialise on load**
```javascript
function openStepFromHash() {
  const hash = window.location.hash;
  if (!hash) return;
  const target = document.querySelector(hash);
  if (!target) return;
  const header = target.querySelector('.step-header');
  if (header && !header.classList.contains('open')) {
    toggleStep(header);
  }
}
window.addEventListener('hashchange', openStepFromHash);
window.addEventListener('DOMContentLoaded', () => {
  openStepFromHash();
  updateProgress();
});
```

### Responsive Design Rules
- **Primary mobile breakpoint:** `max-width: 600px`
- **Troubleshooting table stacking breakpoint:** `max-width: 700px` (separate rule — stacks 3-column table into labelled cards)
- `overflow-x: hidden` on both `html` and `body` — prevents horizontal scroll from any overflowing child
- `word-break: break-word; overflow-wrap: break-word` applied globally within the 600px breakpoint
- All grids use `auto-fit` / `minmax` — no fixed column counts
- Inner element padding is explicitly reduced at ≤600px (`.step-body`, `.step-header`, `.callout`, `.concept-box-header`)
- `prefers-reduced-motion` media query disables all transitions and animations

### Callout Box Usage Guide
Use the correct variant for the content:

| Class | Colour | Icon | Use when |
|---|---|---|---|
| `.callout.tip` | Teal | 💡 | Helpful advice, shortcuts, best practice |
| `.callout.warn` | Amber | ⚠️ | Risk of poor outcome if ignored |
| `.callout.danger` | Red | 🚨 | Will cause failure or damage if ignored |
| `.callout.info` | Purple | ℹ️ | Conceptual explanation, background knowledge |
| `.callout.success` | Green | ✅ | Confirmation that something is complete/correct |

Template:
```html
<div class="callout [variant]">
  <div class="callout-icon">EMOJI</div>
  <div class="callout-body">
    <strong>SHORT LABEL IN CAPS</strong>
    Body text explaining the callout content.
  </div>
</div>
```

### Troubleshooting Table Mobile Behaviour
The troubleshooting table transforms at ≤700px from a 3-column table into stacked labelled cards using `display: block` on all table elements and CSS `::before` pseudo-elements to inject column labels. When adding new pages with troubleshooting tables, copy the full mobile stacking CSS block from `cricut-sticker-guide.html`.

---

## 6. NEXT STEPS & BACKLOG

### Task 1 — Create `index.html` (Site Home / Guide Directory) — ✅ COMPLETE (2026-06-26)
Built per the spec below. Implementation note: the guide cards use a dedicated `.guide-grid` / `.guide-card` component (same `minmax(260px, 1fr)` sizing and card styling as `.need-card`), and the live Sticker Sheets card is shown alongside clearly-marked, non-linked "Coming soon" cards for the backlog guides.

Original spec — an overarching front page that:
- Includes the site nav (with no `.active` link, since we are on the home page)
- Has a hero block with site title and brief description of what the guides cover
- Displays a **guide card grid** — one card per sub-page, using `.needs-grid` style layout (`minmax(260px, 1fr)`) with:
  - An emoji icon
  - Guide title
  - One-line description
  - A clear CTA link ("Open Guide →")
- Includes a brief "How to use these guides" section
- Uses the global footer
- Does **not** need process maps, step accordions, checklists, or troubleshooting tables

### Task 2 — Generate New Sub-Page Boilerplate
When creating a new guide sub-page (e.g. `vinyl-cutting.html`):

1. Copy the full `<head>` and `<style>` block from `cricut-sticker-guide.html`
2. Add the `.site-nav` CSS block (section 4 above)
3. Remove any page-specific CSS not needed (e.g. `.layer-diagram` if no layer diagram is used)
4. Use the semantic page structure from section 4 (`<nav>`, `<main>`, `<section>`, `<footer>`)
5. Add `.nav-link.active` on the correct nav link for this page
6. Replace all content within sections — keep all class names and component structures identical
7. Set `const TOTAL = N` in the JS to the actual number of checkboxes on the new page
8. Update the `<title>` tag to match the new guide

### Task 3 — Retrofit `cricut-sticker-guide.html` — ✅ COMPLETE (2026-06-26)
`cricut-sticker-guide.html` now matches the semantic structure in section 4:
- ✅ Added the `.site-nav` navigation bar (with `.nav-link.active` on the Sticker Sheets link)
- ✅ Wrapped all content sections in `<main>` and converted each `<div class="[section]">` to `<section class="[section]">`
- ✅ Changed `<div class="footer">` to `<footer class="footer">`
- ✅ Converted the seven `<div class="section-title">` headings to `<h2 class="section-title">`

> Visual styling, tokens, copy, and JavaScript were left unchanged; all interactivity (accordion, hash deep-linking, checklist progress) was verified intact after the migration.

### Backlog Ideas (not yet scoped)
- `vinyl-cutting.html` — HTV and adhesive vinyl guide
- `card-making.html` — score and cut card workflow
- `sublimation.html` — if sublimation printer is added
- 'applique.html' - fabric iron-on appliqué workflow
- Print-optimised `@media print` stylesheet for each guide
- Breadcrumb trail component for deep-linked steps
