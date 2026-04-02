# Lanbow V3 — Layout System

Page structure, navigation, grid system, and responsive breakpoints.

---

## PAGE STRUCTURE (Top Nav Bar + No Sidebar)

**V3 removes the icon sidebar.** The layout is simpler: a top nav bar spanning full width, then a full-width content area below.

```
┌──────────────────────────────────────────────────────────────┐
│  Top Nav Bar (1440×~40, absolute positioned at top)          │
│  Logo | Line | Nav links (Home, AI Cake, Bills) | User Info  │
│  Bottom border: #E2E0DF                                      │
├──────────────────────────────────────────────────────────────┤
│  Page Background (#FBF9F4, opacity ~0.98)                    │
│  ┌──────────────────────────────────────────────────────────┐│
│  │  Background Texture Layer (FULL-SCREEN noise on #F3F3F3) ││
│  │  (底层全屏铺满，所有内容叠加其上)                          ││
│  └──────────────────────────────────────────────────────────┘│
│  ┌──────────────────────────────────────────────────────────┐│
│  │  Main Content Shell (VERTICAL)                           ││
│  │  ┌──────────────────────────────────────────────────────┐││
│  │  │  Container (VERTICAL, itemSpacing: 16px)             │││
│  │  │  padding: top 24, left 48, right 48, bottom 40       │││
│  │  │                                                       │││
│  │  │  ├── H1 Page Title (Abhaya Libre Bold 36px)          │││
│  │  │  ├── Main Content Section (HORIZONTAL, gap 16px)     │││
│  │  │  │   ├── Left Column (FILL, VERTICAL, gap 16px)      │││
│  │  │  │   │   ├── Card A (White, padding 24, sharp)       │││
│  │  │  │   │   └── Card B (White, padding 24, sharp)       │││
│  │  │  │   └── Right Column (FIXED ~280px, White, sharp)   │││
│  │  │  └── ... more sections                                │││
│  │  └──────────────────────────────────────────────────────┘││
│  └──────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────┘
```

**Build order (Figma mode):**
1. Page frame: 1440×H, fill: #FBF9F4 (opacity ~0.98)
2. Background texture layer: **全屏** Rectangle (1440×H) with #F3F3F3 fill + NOISE effect (color: rgba(0,0,0,0.16)), 作为最底层背景
3. Main Content Shell frame: VERTICAL, full size, no fill (transparent)
4. Container frame: VERTICAL, itemSpacing 16px, padding top 24, left 48, right 48, bottom 40, layoutSizingHorizontal: FILL
5. Top Nav Bar: absolute positioned at top of page frame (see Nav Bar section)
6. Content sections inside Container

---

## TOP NAV BAR

```
Nav Bar (FRAME, absolute position, 1440×~40)
  fill: transparent (or invisible White)
  stroke-bottom: #E2E0DF, 1px
  ├── Logo area (logo_pro or Lanbow logo mark)
  ├── Vertical Line separator (instance or 1px line)
  └── Nav Content (HORIZONTAL, SPACE_BETWEEN, 16px gap, center-aligned)
      ├── Left: Nav Links (HORIZONTAL, gap 36px, center-aligned)
      │   ├── "Home" — Inter Semi Bold 16px, #2B2933, opacity 1.0 (active)
      │   ├── "AI Cake" — Inter Semi Bold 16px, #2B2933, opacity 0.5 (inactive)
      │   └── "Bills" — Inter Semi Bold 16px, #2B2933, opacity 0.5 (inactive)
      └── Right: User Info (HORIZONTAL, gap 8px, center-aligned)
          ├── Avatar (small)
          └── "USER NAME" — Inter, Grey-06
```

**Active nav state**: opacity 1.0, inactive: opacity 0.5. No underline, no background highlight — purely opacity-based.

---

## GRID SYSTEM

### Desktop (≥1280px)
- Container max-width: 1400px (with 48px side padding → ~1304px usable)
- Two-column layout: Left column FILL + Right column FIXED (~280px)
- Report cards: equal-width columns in horizontal row, gap 16px
- Column gap: 16px

---

## RESPONSIVE BREAKPOINTS

### Breakpoint Tokens

| Name | Min-width | CSS Media Query | Figma frame width |
|---|---|---|---|
| `xs` (Mobile) | 0px | Default (mobile-first) | 375px |
| `sm` (Large mobile) | 640px | `@media (min-width: 640px)` | 640px |
| `md` (Tablet) | 768px | `@media (min-width: 768px)` | 768px |
| `lg` (Small desktop) | 1024px | `@media (min-width: 1024px)` | 1024px |
| `xl` (Desktop) | 1280px | `@media (min-width: 1280px)` | 1440px |

**Note:** The primary Figma design is at 1440px (xl). Other breakpoints are for frontend implementation. When designing in Figma, create additional frames at 768px and 375px for key pages if responsive deliverables are requested.

### CSS Variables (Responsive)

```css
:root {
  /* Responsive container padding — overridden per breakpoint */
  --container-padding-top: 16px;
  --container-padding-x: 16px;
  --container-padding-bottom: 24px;
  --container-gap: 12px;
  --container-max-width: 100%;
}

@media (min-width: 640px) {
  :root {
    --container-padding-x: 24px;
    --container-padding-bottom: 32px;
    --container-gap: 16px;
  }
}

@media (min-width: 768px) {
  :root {
    --container-padding-top: 20px;
    --container-padding-x: 32px;
    --container-padding-bottom: 36px;
  }
}

@media (min-width: 1024px) {
  :root {
    --container-padding-x: 40px;
    --container-padding-bottom: 40px;
    --container-max-width: 1400px;
  }
}

@media (min-width: 1280px) {
  :root {
    --container-padding-top: 24px;
    --container-padding-x: 48px;
    --container-padding-bottom: 40px;
    --container-gap: 16px;
  }
}
```

### Tailwind Config (Responsive)

```js
screens: {
  'sm': '640px',
  'md': '768px',
  'lg': '1024px',
  'xl': '1280px',
}
```

### Layout Behavior per Breakpoint

#### xl (≥1280px) — Desktop (Primary Design)
```
Nav: Full top nav bar, horizontal links, user info visible
Layout: Two-column (left FILL + right 280px fixed)
Container: max-width 1400px, padding 24/48/48/40
Cards: Side by side, full padding 24px
Sub-cards: Horizontal row (3-4 per row)
Typography: Full scale (H1 36px, H2 28px)
```

#### lg (≥1024px) — Small Desktop / Large Tablet Landscape
```
Nav: Full top nav bar (same as xl)
Layout: Two-column, right column shrinks to 240px
Container: padding 20/40/40/40
Cards: Side by side, padding 20px
Sub-cards: Horizontal row (2-3 per row)
Typography: H1 32px, H2 26px
```

#### md (≥768px) — Tablet Portrait
```
Nav: Full top nav bar, nav links condensed (gap 24px instead of 36px)
Layout: SINGLE column — right sidebar stacks below left content
Container: padding 20/32/36/32
Cards: Full width, padding 20px
Sub-cards: 2 per row
Typography: H1 28px, H2 24px
Right sidebar: Full width, horizontal scroll for notifications (optional) or stacked
```

#### sm (≥640px) — Large Mobile
```
Nav: Hamburger menu (☰) replaces horizontal links
      Logo + hamburger left, avatar right
      Menu opens as full-width dropdown panel below nav
Layout: Single column
Container: padding 16/24/32/24
Cards: Full width, padding 16px
Sub-cards: 1 per row, full width
Typography: H1 26px, H2 22px
KPI values: 28px (down from 32px)
```

#### xs (<640px) — Mobile
```
Nav: Hamburger menu, simplified
      Logo + hamburger left, avatar right
      Full-screen overlay menu on open
Layout: Single column
Container: padding 16/16/24/16
Cards: Full width, padding 16px
Sub-cards: 1 per row, full width
Typography: H1 24px, H2 20px, body stays 12px
KPI values: 24px (down from 32px)
Nav links in menu: Inter Semi Bold 18px, vertical stack, gap 24px
```

### Responsive Typography Scale

| Level | xl (≥1280) | lg (≥1024) | md (≥768) | sm (≥640) | xs (<640) |
|---|---|---|---|---|---|
| H1 | 36px | 32px | 28px | 26px | 24px |
| H2 | 28px | 26px | 24px | 22px | 20px |
| H3 | 20px | 20px | 18px | 18px | 16px |
| KPI large | 32px | 32px | 28px | 28px | 24px |
| KPI secondary | 20px | 20px | 18px | 16px | 16px |
| Label | 16px | 16px | 14px | 14px | 14px |
| Body | 12px | 12px | 12px | 12px | 13px* |
| Nav link | 16px | 16px | 14px | 18px** | 18px** |

*Mobile body slightly larger for touch readability
**Mobile nav links larger in hamburger menu for touch targets

### Responsive CSS Utility (Frontend)

```css
/* Typography responsive scaling */
.h1-title { font-size: 24px; }
.h2-title { font-size: 20px; }
.kpi-value { font-size: 24px; }

@media (min-width: 640px) {
  .h1-title { font-size: 26px; }
  .h2-title { font-size: 22px; }
  .kpi-value { font-size: 28px; }
}

@media (min-width: 768px) {
  .h1-title { font-size: 28px; }
  .h2-title { font-size: 24px; }
  .kpi-value { font-size: 28px; }
}

@media (min-width: 1024px) {
  .h1-title { font-size: 32px; }
  .h2-title { font-size: 26px; }
  .kpi-value { font-size: 32px; }
}

@media (min-width: 1280px) {
  .h1-title { font-size: 36px; }
  .h2-title { font-size: 28px; }
  .kpi-value { font-size: 32px; }
}

/* Grid responsive */
.content-grid {
  display: flex;
  flex-direction: column; /* mobile-first: stack */
  gap: var(--container-gap);
}

@media (min-width: 1024px) {
  .content-grid {
    flex-direction: row; /* desktop: side by side */
  }
  .right-column {
    width: 240px;
    flex-shrink: 0;
  }
}

@media (min-width: 1280px) {
  .right-column {
    width: 280px;
  }
}

/* Sub-cards grid */
.subcards-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-base);
}

@media (min-width: 640px) {
  .subcards-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1280px) {
  .subcards-grid {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }
}
```

### Mobile Nav — Hamburger Menu Pattern

```html
<!-- Mobile nav (sm and below) -->
<nav class="lanbow-nav">
  <div class="nav-left">
    <button class="nav-hamburger" aria-label="Menu">
      <i class="ph ph-list" style="font-size: 24px;"></i>
    </button>
    <div class="nav-logo"><!-- Logo --></div>
  </div>
  <div class="nav-right">
    <div class="nav-avatar"></div>
  </div>
</nav>

<!-- Mobile menu overlay -->
<div class="mobile-menu" role="dialog" aria-modal="true">
  <div class="mobile-menu-content">
    <a href="#" class="mobile-menu-link active">Home</a>
    <a href="#" class="mobile-menu-link">AI Cake</a>
    <a href="#" class="mobile-menu-link">Bills</a>
  </div>
</div>
```

```css
.nav-hamburger {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: var(--space-xs);
  color: var(--grey-01);
}

@media (max-width: 639px) {
  .nav-hamburger { display: flex; }
  .nav-links { display: none; }
}

.mobile-menu {
  position: fixed;
  inset: 40px 0 0 0; /* below nav */
  background: var(--page-bg);
  z-index: 99;
  padding: var(--space-xl);
  display: none;
}
.mobile-menu.open { display: block; }

.mobile-menu-link {
  font-family: var(--font-ui);
  font-weight: 600;
  font-size: 18px;
  color: var(--nav-text);
  text-decoration: none;
  display: block;
  padding: var(--space-md) 0;
  opacity: 0.5;
  border-bottom: 1px solid var(--nav-border);
}
.mobile-menu-link.active {
  opacity: 1;
}
```

### Responsive Touch Target Rules (Mobile)

- **Minimum touch target**: 44×44px (WCAG 2.5.5)
- **Button padding**: at least `12px 20px` on mobile (up from `10px 24px` on desktop)
- **Nav links in menu**: full-width tap area, `padding: 12px 0`
- **Card tap area**: entire card surface is tappable on mobile (no small "View All" button — card itself is the link)
- **Icon size**: minimum 20×20px on mobile (up from 16px inline minimum)
