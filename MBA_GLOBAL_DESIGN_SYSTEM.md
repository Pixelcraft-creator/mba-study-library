# MBA Study Library — Global Design System
**Canonical visual, UX, technical, and content specification for every course in the MBA Study Library.**

Reverse-engineered from the HRM Study Book (Arab Academy GSB) — the approved reference implementation. When this document conflicts with HRM's live code, the live code wins; this document should then be updated to match.

This is the **platform-level** standard. Each course additionally maintains its own `PROJECT_STANDARDS.md`, which documents only how that course applies these global rules — it must never re-specify or fork the global design tokens.

---

## Table of Contents

0. [Platform Hierarchy](#0-platform-hierarchy)
1. [Design Tokens](#1-design-tokens)
2. [Typography](#2-typography)
3. [Spacing & Sizing](#3-spacing--sizing)
4. [Color Usage Rules](#4-color-usage-rules)
5. [Component Library](#5-component-library)
6. [Layout System](#6-layout-system)
7. [Navigation System](#7-navigation-system)
8. [Dark Mode](#8-dark-mode)
9. [Responsive Behavior](#9-responsive-behavior)
10. [Content Architecture](#10-content-architecture)
11. [Technical Architecture & Course Isolation](#11-technical-architecture--course-isolation)
12. [JavaScript Patterns](#12-javascript-patterns)
13. [QA Standards](#13-qa-standards)
14. [Deployment Standards](#14-deployment-standards)
15. [Global vs. Course-Specific Rules](#15-global-vs-course-specific-rules)
16. [Course Onboarding Workflow](#16-course-onboarding-workflow)
17. [Standards Freeze](#17-standards-freeze)

---

## 0. Platform Hierarchy

The MBA Study Library is not a single course — it is a permanent, multi-year platform built one course at a time.

```
MBA STUDY LIBRARY                    (this repo: mba-study-library — index.html, this doc, global styles.css)
│
├── YEAR 1
│   ├── SEMESTER 1 (completed / previous semester)
│   │   ├── Contemporary Management            — planned, not yet built
│   │   ├── Statistics Analysis                — planned, not yet built
│   │   ├── Managerial Economics                — planned, not yet built
│   │   └── Accounting & Financial Reporting     — planned, not yet built
│   │
│   └── SEMESTER 2 (current semester)
│       ├── Marketing Management                — planned, not yet built
│       ├── Operations Management                — planned, not yet built (previous OM project is legacy — not a reference)
│       ├── Human Resource Management             — available, golden reference (Pixelcraft-creator/HRM-Study-Book)
│       └── Managerial Finance                    — in progress (managerial-finance-mba-guide, not yet deployed)
│
└── YEAR 2
    └── MAJOR / SPECIALIZATION        (not yet started — infrastructure only, do not invent a major name or its courses)
```

This is the authoritative, enrolled-course structure for Year 1. It is a fixed list, not a placeholder pattern — do not add, remove, or reorder these 8 courses without new instruction. Statuses (planned / in progress / available) are verified against actual repositories and deployments, not assumed — re-verify before trusting a status you didn't just check yourself.

Below Course, the existing per-course hierarchy applies unchanged:

```
Course → Course Study Book → Module → Topic
```

### What the global platform controls
Branding, global navigation, the Year/Semester/Major structure, course discovery (the library landing page), course status tracking, the global design system (this document + `styles.css`), accessibility, responsive behavior, dark mode, and shared component definitions.

### What each course controls
Its own source material, module structure, formulas/calculations/examples/cases/exams, course-specific diagrams, and course-specific learning-flow adaptation (§15, "Global vs. Course-Specific"). Course content must never leak into the global platform repo, and the global repo must never hold course-specific content.

### Course isolation (non-negotiable)
- Every course lives in its **own repository**, with its own Git history and its own GitHub Pages deployment.
- Every course keeps its **own local copy** of `styles.css` and this document's relevant rules — never a live cross-repo link. Course isolation means a course must build and deploy correctly even if every other course's repo is deleted.
- The only place courses "connect" is the library landing page (`mba-study-library/index.html`), which links out to each course's own deployed URL. It does not embed or iframe course content.
- When the global standard changes, the change is made once here, then manually propagated to each course's local copy — this is a deliberate, reviewed step, not automatic.

### Adding a future course
Follow the Course Build Workflow: course identified → source files provided → source scan → course map → module map → content-gap/overlap analysis → build → course QA → freeze course standard → Git → deploy → production QA. One course at a time. Never rebuild the whole library when one course changes.

---

## 1. Design Tokens

All tokens live in `:root {}` in `styles.css`. Never hard-code these values anywhere — always use the CSS variable.

### Brand Colors
| Token | Light Value | Purpose |
|-------|------------|---------|
| `--navy` | `#1a2744` | Primary headings, hero gradients, table headers, TOC links, card titles |
| `--navy-light` | `#243360` | Hero gradient end, sub-titles |
| `--crimson` | `#8B1A1A` | Section border accents, bullet markers, exam cards, crimson badges |
| `--crimson-light` | `#a82020` | Hover states, gradient ends |
| `--gold` | `#B8860B` | Key-point boxes, gold badges, `.home-btn` header button |
| `--gold-light` | `#d4a017` | Hover gold, dark-mode card titles |
| `--teal` | `#1a6b6b` | Insight boxes, check-list checkmarks, teal badges |
| `--teal-light` | `#218080` | Teal hover/gradient ends |

### Semantic Tokens (theme-aware)
| Token | Light | Dark |
|-------|-------|------|
| `--bg` | `#f7f8fc` | `#0f1524` |
| `--bg-card` | `#ffffff` | `#1a2233` |
| `--text` | `#1c1c2e` | `#e8eaf6` |
| `--text-muted` | `#5a5a7a` | `#9ba3c4` |
| `--border` | `#dde2f0` | `#2a3555` |

### Shadow Tokens
| Token | Value |
|-------|-------|
| `--shadow` | `0 2px 12px rgba(26,39,68,0.10)` |
| `--shadow-lg` | `0 6px 32px rgba(26,39,68,0.16)` |

Dark overrides: `rgba(0,0,0,0.4)` and `rgba(0,0,0,0.5)` respectively.

### Shape Tokens
| Token | Value |
|-------|-------|
| `--radius` | `10px` — standard cards, badges, most components |
| `--radius-lg` | `16px` — hero blocks, module cards on index |

### Layout Token
| Token | Value |
|-------|-------|
| `--max-w` | `1100px` — all pages, centered |

---

## 2. Typography

### Font Stack
```css
--font-main: 'Segoe UI', system-ui, -apple-system, sans-serif;
--font-mono: 'Courier New', Courier, monospace;
```
No external font imports. Everything uses system fonts only.

### Scale
| Role | Size | Weight | Notes |
|------|------|--------|-------|
| Body | `16px` | 400 | Line-height `1.7` |
| Hero H1 (module) | `clamp(1.6rem, 4vw, 2.4rem)` | 800 | |
| Hero H1 (index) | `clamp(2rem, 5vw, 3rem)` | 900 | |
| Section title `h2` | `1.45rem` | 700 | Left border 4px crimson |
| Sub-title `h3` | `1.1rem` | 700 | Navy-light color |
| Card title | `1.15rem` | 700 | Crimson bottom border |
| Body large | `1.05rem` | 400 | Subtitles, lead text |
| Body small | `0.93rem` | 400 | Glossary, table content |
| Labels / overlines | `0.72–0.85rem` | 600–700 | Uppercase, `letter-spacing: 1.5–2px` |
| Badges | `0.75rem` | 600 | |
| Mono (formula) | `1rem` | 600 | `--font-mono` |

### Heading Conventions
- Exactly **1 `<h1>` per page** — always inside the hero block.
- `<h2>` with `class="section-title"` for major content sections; must have an `id` for TOC anchoring.
- `<h3>` with `class="sub-title"` for subsections inside cards.
- `<h4>` inside comparison cards — no additional class needed.

---

## 3. Spacing & Sizing

### Page
| Context | Value |
|---------|-------|
| Max content width | `1100px` centered |
| Page padding (desktop) | `40px 24px 80px` |
| Page padding (≤768px) | `24px 16px 60px` |
| Section gap (h2 margin) | `36px 0 18px` |
| Card margin-bottom | `28px` |
| Card padding | `28px` |
| Card padding (≤480px) | `18px` |

### Hero
| Context | Value |
|---------|-------|
| Module hero padding | `56px 40px 48px` |
| Index hero padding | `70px 40px 60px` |
| Hero padding (≤768px) | `36px 22px 30px` |

### Header
| Context | Value |
|---------|-------|
| Header inner padding | `14px 24px` |
| Header inner padding (≤768px) | `10px 16px` |
| Nav button padding | `6px 14px` |

### Navigation footer
| Context | Value |
|---------|-------|
| Top margin | `56px` |
| Top padding | `28px` |
| Border top | `2px solid var(--border)` |

---

## 4. Color Usage Rules

### Gradient Rules
- **Hero gradients:** always go left `var(--navy)` → right brand color (crimson for modules; a different mid/end-stop may be chosen per course per §15, but the navy start-stop is fixed).
- **Hero decorative circles:** `background: rgba(255,255,255,0.03–0.04)`, `border-radius: 50%`, positioned off-canvas corners.
- **Card gradients (definition/exam/case boxes):** `rgba()` tints at 0.03–0.10 opacity only — never solid fills on content backgrounds.

### Accent Color Assignment
| Accent | Use case |
|--------|---------|
| Navy | Definition boxes (left border), primary data |
| Crimson | Section titles (left border), exam cards, `.red-accent` compare cards, bullets (`▸`) |
| Gold | Key points (left border), `.gold-accent` compare cards, home button |
| Teal | Insight boxes, case boxes, check-list marks (`✓`), `.teal-accent` compare cards |
| Green (`#228B22`) | Status "Available" pills, positive matrix cells |

### Dark Mode Color Shift
In dark mode, `.card-title` color shifts from `var(--navy)` to `var(--gold-light)` — both `@media` and `[data-theme="dark"]` selectors are required.

---

## 5. Component Library

All components below are defined in each course's local `styles.css` (kept identical to this global copy) and available via `<link rel="stylesheet" href="styles.css">`. Page-specific additions go in a page's own `<style>` block — never modify the shared `styles.css` for one-page needs.

### Header
```html
<header class="site-header">
  <div class="header-inner">
    <a href="index.html" class="header-brand">
      <span class="header-logo">FIN</span>
      <span class="header-title">Study Book</span>
    </a>
    <nav class="header-nav">
      <a href="index.html" class="nav-btn home-btn">⌂ Course Map</a>
      <button onclick="toggleTheme()" class="nav-btn">◑ Theme</button>
    </nav>
  </div>
</header>
```
- `header-logo`: displays the course abbreviation in gold (`var(--gold-light)`), weight 800.
- `home-btn`: gold background, dark text — always the primary CTA in the header.
- Index/landing pages show the Theme button only (no `home-btn`, since it IS home).

### Module Hero
```html
<div class="module-hero">
  <div class="hero-tag">Module 0X · Lecture N · DD Month YYYY</div>
  <h1>Module Title</h1>
  <div class="subtitle">One or two sentences describing scope and value.</div>
  <div class="hero-meta">
    <span class="hero-badge">📖 Source</span>
    <span class="hero-badge">⏱ Lecture N</span>
    <span class="hero-badge">📚 Textbook Ch. N</span>
  </div>
</div>
```
- Gradient: `linear-gradient(135deg, var(--navy) 0%, var(--crimson) 100%)`.
- Always include the decorative `::before` pseudo-element (absolute circle, top-right).
- `hero-tag` uses the `rgba(255,255,255,0.15)` pill style with `letter-spacing: 1px`.

### Card (`.card`)
```html
<div class="card">
  <div class="card-title"><span class="icon">🔷</span> Title</div>
  <!-- content -->
</div>
```
- `card-title` always has a `2px solid var(--crimson)` bottom border.
- Emoji icon precedes the title text in a `.icon` span.
- Card padding: `28px`. Shadow: `var(--shadow)`. Radius: `var(--radius)`.

### Section Heading
```html
<div class="section-label">SECTION LABEL</div>   <!-- optional overline -->
<h2 id="anchor-id" class="section-title">Section Title</h2>
```
- Every `h2` in the TOC **must** have an `id` matching the TOC `href`.

### Definition Box (`.def-box`)
Navy left border, navy-to-crimson subtle gradient background. Use for every key term introduced in a module.
```html
<div class="def-box"><div class="def-term">Term</div><div class="def-text">Definition text.</div></div>
```

### Key Point (`.key-point`)
Gold left border, gold tint background. Use for the single most important takeaway from a concept.
```html
<div class="key-point"><strong>Label:</strong> Content.</div>
```

### Insight Box (`.insight-box`)
Teal border and teal-tint background. Use for real-world application notes, cross-references, and case connections.
```html
<div class="insight-box"><div class="insight-title">🔗 Title</div><p>Content.</p></div>
```

### Table (`.table-wrap` + `<table>`)
Always wrap in `.table-wrap` for horizontal scroll on mobile. `thead`: navy background, white text.

### Comparison Cards (`.compare-card`)
Accent variants: `.blue-accent`, `.red-accent`, `.gold-accent`, `.teal-accent` (top border color).

### Process Flow (`.process-flow`)
Numbered horizontal steps; `step-num` crimson circle. Stacks vertically on mobile.

### Grids
`.grid-2` (auto-fit, minmax(300px,1fr)), `.grid-3` (auto-fit, minmax(220px,1fr)) — both collapse to 1 column at ≤768px.

### Formula Box (`.formula-box`)
```html
<div class="formula-box">
  <div class="formula-title">FORMULA NAME</div>
  <div class="formula">A = B + C</div>
  <div class="formula-note">Explanatory note.</div>
</div>
```
Navy 2px border, mono font, centered. This is the primary quantitative-course component — Finance, Operations, and any calculation-heavy course lean on this heavily.

### Stat Tiles, Exam Cards, Active Recall, Case Box, Glossary, Badges, Status Pills, TOC Box, 2×2 Matrix
Unchanged from the HRM reference implementation — see the live `styles.css` for exact markup; class names and structure are fixed platform-wide (§15, "Absolutely Do Not Change"). Course-specific diagram components (e.g. HRM's `.ulrich-grid`/`.addie-flow`) are additive, not global.

### Print
On `@media print`: header, nav, module-nav, and toggle arrows hidden. Cards get `break-inside: avoid`. Recall answers show. Font reduced to 12px.

---

## 6. Layout System

### Library Landing Page (`mba-study-library/index.html`)
```
site-header (Theme toggle only)
page-wrap
  ├── .module-hero (Library welcome — "MBA Study Library")
  ├── h2.section-title "Year 1"
  │     ├── h3.sub-title "Semester 1" → .grid-3 of course cards (Available / Coming Soon)
  │     └── h3.sub-title "Semester 2" → .grid-3 of course cards
  └── h2.section-title "Year 2 — Major / Specialization"
        └── .grid-3 of "Coming Soon" placeholder cards (no invented names)
```

### Course Index Page (per-course, e.g. `managerial-finance-mba-guide/index.html`)
```
site-header (Theme toggle only — this page IS the course home)
page-wrap
  ├── .module-hero (course welcome, links back to library index)
  ├── .card (course info / grading, if applicable)
  ├── h2.section-title "Modules"
  ├── .modules-grid (module cards, available + coming-soon)
  └── .grid-2 (resources, if any)
```

### Module Page Structure
```
site-header (⌂ Course Map home-btn + Theme toggle)
page-wrap
  ├── .module-hero
  ├── .toc-box
  ├── #objectives .card (Learning Objectives)
  ├── content sections (h2.section-title + .card / .def-box / .key-point / .formula-box / etc.)
  ├── #exam section (≥4 .exam-card)
  ├── #recall section (≥6 .recall-item)
  ├── #glossary section (.glossary-item list)
  └── .module-nav (prev + next links, or disabled state if the neighbor module doesn't exist yet)
```

---

## 7. Navigation System

### Sticky Header
`position: sticky; top: 0; z-index: 100`. Gradient `linear-gradient(135deg, var(--navy) 0%, var(--navy-light) 100%)`.

### Header Nav Buttons
| Button | Style | Present on |
|--------|-------|-----------|
| `⌂ Course Map` | `.nav-btn.home-btn` — gold bg | All course pages except that course's own index |
| `◑ Theme` | `.nav-btn` — ghost style | All pages |

### Module Footer Navigation (`.module-nav`)
Prev link: arrow on left. Next link: `flex-direction: row-reverse`, arrow on right. **If the neighbor module does not exist yet, render a disabled, non-clickable state — never link to a file that doesn't exist.**

### TOC Anchoring
Fixed anchor names across all modules: `id="objectives"`, `id="exam"`, `id="recall"`, `id="glossary"`.

---

## 8. Dark Mode

Two parallel mechanisms — both must be present: system preference (`@media (prefers-color-scheme: dark)`) and manual toggle (`data-theme` attribute), with `localStorage` persistence under a **course-specific key** (e.g. `fin-theme`, `hrm-theme`) to avoid cross-course persistence conflicts.

```js
function toggleTheme() {
  const root = document.documentElement;
  const current = root.getAttribute('data-theme');
  root.setAttribute('data-theme', current === 'dark' ? 'light' : 'dark');
  localStorage.setItem('fin-theme', root.getAttribute('data-theme'));
}
(function() {
  const saved = localStorage.getItem('fin-theme');
  if (saved) document.documentElement.setAttribute('data-theme', saved);
})();
```
The IIFE runs before the page renders to prevent flash-of-wrong-theme.

---

## 9. Responsive Behavior

| Breakpoint | Applied to |
|-----------|-----------|
| `≤768px` | Tablet/mobile: single-column grids, stacked flows, smaller table cells |
| `≤480px` | Small phone: reduced hero H1, reduced card padding, stacked hero-meta |

All wide content must be inside `.table-wrap` or a flex/grid container with `overflow-x: auto`. The `<body>` must never scroll horizontally.

---

## 10. Content Architecture

### The Learning Arc (module content pattern)
Not every section is required for every module — mark non-applicable sections as absent rather than filling them with invented content.

```
1. Module Hero (tag, H1, subtitle, badges)
2. Table of Contents (.toc-box, anchor list)
3. Learning Objectives ("After studying this module, you should be able to:" + 4–6 action-verb items)
4. Core Concepts (.def-box per term, .key-point for the headline takeaway)
5. Frameworks / Models / Formulas (course-adapted — see §15)
6. Worked Examples & Real-World Application (.case-box, .insight-box, .stat-tile)
7. Case / Exam Application (optional — only when source material provides a case)
8. Exam Preparation (id="exam", ≥4 .exam-card)
9. Active Recall (id="recall", ≥6 .recall-item)
10. Glossary (id="glossary")
11. Module Navigation (.module-nav, prev + next)
```

### Source Policy (all courses)
- The lecture/source material is authoritative. Never invent content, formulas, or examples.
- The course textbook (where provided) is a **secondary, cross-referencing** source — use it to verify formulas, cite chapter/section numbers, and add rigor, never to replace or override what was actually taught.
- Preserve course-specific terminology exactly as taught, even where it differs from textbook phrasing — note the divergence rather than silently reconciling it.
- If source material contains a genuine numerical or formula error, flag it explicitly (a distinct, visually flagged callout) rather than silently correcting or silently preserving it.

---

## 11. Technical Architecture & Course Isolation

### Repository Layout
```
mba-study-library/                    ← THIS repo: global platform
├── index.html                        ← Library landing page (Year/Semester/Major)
├── styles.css                        ← Canonical global stylesheet (source of truth)
├── MBA_GLOBAL_DESIGN_SYSTEM.md        ← This file
└── .gitignore

[course-abbr]-study-book/  (or similarly named, one repo per course)
├── index.html                        ← Course Map (this course's landing page)
├── styles.css                        ← Local copy of the global stylesheet
├── m01-[slug].html … m0N-[slug].html ← Module pages
├── midterm.html / cases.html         ← Resource pages, if applicable
├── PROJECT_STANDARDS.md              ← Course-specific application of the global rules
├── MODULE_BUILD_WORKFLOW.md          ← This course's step-by-step build guide
└── .gitignore
```

### Course Isolation Rules
- No course repo references another course's files, assets, or repo — ever.
- No course repo references the `mba-study-library` repo at build/runtime — the relationship is one-directional (library links out to courses), never the reverse.
- Each course deploys independently via its own GitHub Pages.
- Updating the global standard is a manual, reviewed propagation to each course — never a live shared dependency.

### CSS Architecture
- `styles.css` is the single source of truth for shared styles, in the library repo canonically and mirrored locally in every course repo.
- Naming convention: BEM-inspired but flat (`.card-title`, `.flow-step`, `.formula-box`).
- No `!important` except in the print media query for visibility overrides.

### Dependency Policy
Zero external dependencies. No CDN links, no Google Fonts, no JS libraries, no images, no iframes. System font stack only.

---

## 12. JavaScript Patterns

Three functions required on every page — recall toggle (module pages only), theme toggle (every page), theme-persistence IIFE (every page, before render). See §8 for the theme functions.

```js
function toggleRecall(el) {
  el.classList.toggle('open');
  el.nextElementSibling.classList.toggle('visible');
}
```
`.recall-q` and `.recall-a` must be **direct siblings** — the function uses `nextElementSibling`.

No other JavaScript is used. No scroll effects, no animations, no lazy loading.

---

## 13. QA Standards

A module or resource page is considered complete when it passes:

**Structure:** exactly one `<h1>`; no duplicate `id`s; every TOC `href="#anchor"` resolves within the same file; `<html lang="en">`; `<meta charset="UTF-8">`; viewport meta; `<link rel="stylesheet" href="styles.css">`; title pattern `M0X: Title — [Course] Study Book`.

**Navigation:** every cross-file `href` points to a file that actually exists in the repo (never a fake/placeholder link); module footer prev/next point to correct neighbors or render disabled if the neighbor doesn't exist yet; header home-btn present on all non-index course pages.

**Content:** learning objectives present (`id="objectives"`); ≥1 `.def-box` per key term; `id="exam"` with ≥4 `.exam-card`; `id="recall"` with ≥6 `.recall-item`; `id="glossary"` present; no invented content — every fact, formula, and case traces to source material.

**Visual/Responsive:** no hardcoded hex colors outside `styles.css` tokens; every wide table/diagram in `.table-wrap` or `overflow-x:auto`; no horizontal scroll at 375px; dark mode contrast correct; print layout correct.

**JavaScript:** `toggleTheme()` + IIFE present; `toggleRecall()` present if the page has `.recall-item`s; sibling relationship intact.

---

## 14. Deployment Standards

**Platform:** GitHub Pages, static hosting from `main` branch root, no build step.

**Repository pattern:** one repo per course, `[username]/[Course]-Study-Book`, single `main` branch. The library landing page is its own repo, `[username]/mba-study-library`.

```bash
cd /path/to/course-repo
git add [changed-files]
git commit -m "Add M0X: Title"
git push origin main
```

Verify after deploy: `curl -s -o /dev/null -w "%{http_code}" https://[user].github.io/[repo]/file.html` → expect `200`. Check the library index's links to each course after any course deploy.

---

## 15. Global vs. Course-Specific Rules

### Always Global
Zero external dependencies · single `styles.css` per repo (kept identical to this canonical copy) · system font stack · two-mechanism dark mode · `localStorage` theme persistence · sticky header, max-width 1100px · single `<h1>` per page · no JS libraries · `.table-wrap` for every table · every course page links back to that course's own index, and every course index is discoverable from the library landing page.

### Adapt per Course
| Element | What to change |
|---------|----------------|
| `header-logo` text | Course abbreviation (HRM, FIN, OM, MKT, …) |
| `localStorage` key | `[abbr]-theme` |
| Hero gradient end-stop | May shift per course; navy start-stop is fixed |
| Course-specific diagram components | e.g. HRM's `.ulrich-grid`/`.addie-flow`; Finance's needs lean on `.formula-box` far more heavily |
| `PROJECT_STANDARDS.md` | Course info, module status table |
| Learning-method adaptation within the fixed architecture | Finance: **Formula → Calculation → Interpretation → Managerial Decision**. HRM: **Concept → Framework → Case → Application → Exam**. Operations: **Process → Model → Calculation → Scenario → Decision**. Marketing: **Framework → Case → Application → Strategy**. The section skeleton (§10) stays constant; how each section is filled adapts to the subject. |

### Absolutely Do Not Change
CSS custom property **names** · `styles.css` class **names** · `toggleRecall`/`toggleTheme` function signatures · the `recall-q`/`recall-a` sibling relationship · the `data-theme` attribute name · the global color token values (a course may choose *where* to apply navy vs. crimson vs. gold vs. teal, but may not introduce a new palette).

---

## 16. Course Onboarding Workflow

The fixed process for adding any future course to the library:

1. **Identify the course.** Name must match the authoritative course list in §0 — never invented.
2. **Provide the course's source-material folder.** The library and other courses are never a source for a new course's content.
3. **Run source inspection** — read the material once, do not re-scan repeatedly.
4. **Create the course-specific module map** from that inspection.
5. **Build one Golden Module Template** (the course's first module, fully built).
6. **Validate the template against this document** — tokens, components, section skeleton (§10), no invented visual identity.
7. **Freeze the course's visual/UX shell** — from here on, only content changes, not design.
8. **Build the remaining modules**, one at a time, reusing the frozen template verbatim.
9. **Integrate exam materials, case studies, exercises, and other course-specific resources** as the source material provides them.
10. **Run one consolidated course QA** (§13) — not per-module, not endless.
11. **Deploy the course independently** to its own GitHub Pages URL (§14).
12. **Register the course's real public URL in the library's `index.html`** — replace its "planned" card with a live one. This is the *only* change the library itself needs; the library is never rebuilt to add a course.

## 17. Standards Freeze

As of 2026-08-10, the following are frozen and must not be redesigned without explicit new instruction:
- The platform hierarchy (§0) and the 8-course Year 1 structure.
- The global design tokens, typography, and component library (§§1–5), sourced from the HRM Study Book reference implementation.
- The course onboarding workflow (§16).

"Frozen" means: course-specific *content* work continues normally, but the visual identity, navigation philosophy, and architectural shape of the platform do not change as a side effect of building a course. A genuine defect (a broken link, a missing QA check) may still be fixed; a preference-driven redesign may not.

---

*Document created: 2026-08-08 (as course-level doc in HRM Study Book). Promoted to platform-level global standard: 2026-08-10. Course hierarchy corrected to the authoritative 8-course Year 1 list and onboarding/freeze sections added: 2026-08-10.*
*Source of truth: `mba-study-library/styles.css`, kept in sync with the HRM Study Book reference implementation.*
