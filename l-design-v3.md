# /lanbow-v3 — Lanbow V3 UI Design Generator (Dual Mode)

Generate high-fidelity UI that matches the Lanbow V3 design system. Supports two modes:
- **Figma mode**: Create designs in Figma via Vibma MCP
- **Frontend mode**: Generate production-ready HTML/CSS/React code

This is an evolution of the original Lanbow design with serif typography (Abhaya Libre + Newsreader), sharp corners (0px radius everywhere), Phosphor icon library, and a warm off-white background aesthetic.

---

## MODE DETECTION

Determine the mode based on context:
- **Figma mode**: User mentions Figma, design, mockup, prototype, or Vibma is connected. Use Figma-specific APIs (Variable IDs, `layoutMode`, `fillVariableId`, etc.)
- **Frontend mode**: User mentions code, React, HTML, CSS, Tailwind, component, page, frontend, vibe, or working directory contains web project files. Generate code with CSS variables and proper imports.

If unclear, ask: "Do you want me to create this in Figma or generate frontend code?"

---

## SETUP

### Figma Mode
```
mcp__Vibma__connection(method: "create") → mcp__Vibma__connection(method: "get")
```
If get times out, tell user to start relay: `npx @ufira/vibma-tunnel`

### Frontend Mode
Ensure the following are available in the project:

**Google Fonts** (add to `<head>` or CSS `@import`):
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Abhaya+Libre:wght@400;500;600;700;800&family=Inter:wght@100;300;400;500;600;700&family=Newsreader:wght@600&display=swap" rel="stylesheet">
```

**Phosphor Icons** (choose one):
```bash
# React
npm install @phosphor-icons/react

# Vue
npm install @phosphor-icons/vue

# CDN (vanilla HTML)
# <script src="https://unpkg.com/@phosphor-icons/web"></script>
```

**CSS Variables** (add to project root stylesheet):
```css
:root {
  /* === Lanbow Colors === */
  --lanbow-cyan: #00B1A2;
  --grey-01: #181818;
  --grey-06: #626262;
  --grey-08: #999999;
  --grey-12: #EEEEEE;
  --background: #F3F3F3;
  --white: #FFFFFF;
  --selected: rgba(0, 0, 0, 0.02);
  --stroke: rgba(0, 0, 0, 0.04);
  --red: #FF2D55;
  --orange: #FF9500;

  /* === V3 Specific Colors === */
  --page-bg: #FBF9F4;
  --nav-text: #2B2933;
  --nav-border: #E2E0DF;
  --card-warm-grey: #F8F6F2;

  /* === Typography === */
  --font-display: 'Abhaya Libre', serif;
  --font-data: 'Newsreader', serif;
  --font-ui: 'Inter', sans-serif;

  /* === Spacing === */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 12px;
  --space-base: 16px;
  --space-lg: 20px;
  --space-xl: 24px;
  --space-2xl: 32px;
  --space-3xl: 40px;
  --space-4xl: 48px;

  /* === Container === */
  --container-max-width: 1400px;
  --container-padding-top: 24px;
  --container-padding-x: 48px;
  --container-padding-bottom: 40px;
  --container-gap: 16px;
}
```

**Tailwind Config** (if using Tailwind — extend `tailwind.config.js`):
```js
module.exports = {
  theme: {
    extend: {
      colors: {
        'lanbow-cyan': '#00B1A2',
        'grey-01': '#181818',
        'grey-06': '#626262',
        'grey-08': '#999999',
        'grey-12': '#EEEEEE',
        'bg-main': '#F3F3F3',
        'page-bg': '#FBF9F4',
        'nav-text': '#2B2933',
        'nav-border': '#E2E0DF',
        'card-warm': '#F8F6F2',
        'selected': 'rgba(0,0,0,0.02)',
        'stroke': 'rgba(0,0,0,0.04)',
        'danger': '#FF2D55',
        'warning': '#FF9500',
      },
      fontFamily: {
        display: ['"Abhaya Libre"', 'serif'],
        data: ['"Newsreader"', 'serif'],
        ui: ['"Inter"', 'sans-serif'],
      },
      borderRadius: {
        none: '0px', /* V3: all corners sharp */
      },
    },
  },
}
```

---

## DESIGN TOKENS

### Color Variables (Collection: "Lanbow Colors", collectionId: VariableCollectionId:10337:7721, modeId: 10337:0)

**Same as Lanbow V1 — no changes to the color system.**

| Token | Variable ID | Hex | Usage |
|---|---|---|---|
| LANBOW-Cyan | VariableID:10337:7723 | #00B1A2 | Brand primary, CTA, active states, links |
| Grey-01 | VariableID:10337:7724 | #181818 | Primary text, headings |
| Grey-06 | VariableID:10337:7725 | #626262 | Secondary text, captions, user name |
| Grey-08 | VariableID:10337:7726 | #999999 | Placeholder text, disabled states |
| Grey-12 | VariableID:10337:7727 | #EEEEEE | Borders, dividers, subtle lines |
| Background | VariableID:10337:7728 | #F3F3F3 | Large background areas (used for noise texture base) |
| White | VariableID:10337:7729 | #FFFFFF | Card backgrounds, surface |
| Selected | VariableID:10337:7730 | rgba(0,0,0,2%) | Subtle fills: table headers, hover states, tag bg |
| Stroke | VariableID:10337:7731 | rgba(0,0,0,4%) | Card borders, container strokes |
| Red | VariableID:10337:7732 | #FF2D55 | Error, danger, delete |
| Orange | VariableID:10337:7733 | #FF9500 | Warning, attention |

**Additional V3 colors (hardcoded, not in variable collection):**
| Color | Hex | Usage |
|---|---|---|
| Page Background | #FBF9F4 | Warm off-white page fill (opacity ~0.98) |
| Nav Text | #2B2933 | Top nav text color |
| Nav Border | #E2E0DF | Top nav bottom border |
| Card Warm Grey | #F8F6F2 | Report cards, sub-card backgrounds |

**Figma mode RULE: ALWAYS use `fillVariableId` / `strokeVariableId` / `set_variable_binding` for Lanbow Colors collection tokens. Use hardcoded hex ONLY for V3-specific colors above.**

**Frontend mode RULE: Use CSS variables (`var(--grey-01)`) or Tailwind classes (`text-grey-01`). NEVER hardcode hex values inline — always reference the token.**

### Radius: ALL ZERO

**V3 uses 0px corner radius on EVERYTHING.** No rounded corners anywhere — cards, buttons, inputs, modals, panels, tags — all sharp/square.

- Do NOT bind radius variables from the Lanbow Radius collection
- Set `cornerRadius: 0` on all frames, or simply omit (default is 0)
- Exception: avatars remain circular (radius-round 9999px)

### Text Styles — V3 Typography System

V3 introduces three font families with distinct roles:

| Font Family | Role | Available Styles |
|---|---|---|
| **Abhaya Libre** | Display/headings, dates | Bold, SemiBold, Medium, Regular |
| **Newsreader** | Numeric data/KPI values | SemiBold |
| **Inter** | UI text, body, labels, nav | Bold, Semi Bold, Medium, Thin, Regular |

#### Typography Hierarchy

| Level | Font | Style | Size | Color | Usage |
|---|---|---|---|---|---|
| H1 Page Title | Abhaya Libre | Bold | 36px | Grey-01 | Top of page, one per screen |
| H2 Section Title | Abhaya Libre | Bold | 28px | Grey-01 | Section headers: "Target Overview", "Reports", "Notifications" |
| H3 Card Title | Inter | Thin | 20px | Grey-01 | Report card titles: "Daily Report", "Weekly Report" |
| Label | Inter | Regular | 16px | Grey-01 | Sub-labels: "Spend Progress", "ROI Trend" |
| KPI Value (large) | Newsreader | SemiBold | 32px | Grey-01 | Hero metrics: "$248,900", "1.85" |
| KPI Value (secondary) | Newsreader | SemiBold | 20px | Grey-01 | Secondary metrics: "/$500,000" |
| Notification Title | Inter | Bold | 16px | Grey-01 | Notification card headings |
| Body | Inter | Regular | 12px | Grey-01 | Default reading text, descriptions |
| Date | Abhaya Libre | Regular | 12px | Grey-01 | Dates: "2026/03/29" |
| Meta / Timestamp | Inter | Regular | 9px | Grey-06 | "10:42 AM · System Alert" |
| Badge | Inter | Bold | 9px | varies | "New" badge text |
| Button Text | Inter | Bold | 10px | Grey-01 | "View All" style buttons |
| Nav Link | Inter | Semi Bold | 16px | #2B2933 | Top nav items |
| Chart Axis | Inter | Regular | 12px | Grey-01 | Chart axis labels |
| Trend Text | Inter | Regular | 12px | LANBOW-Cyan | "↑ 0.12 vs last week" |

**CRITICAL — Font family + fontStyle mapping:**
- `fontFamily: "Abhaya Libre"` with `fontStyle: "Bold"` or `fontStyle: "Regular"`
- `fontFamily: "Newsreader"` with `fontStyle: "SemiBold"`
- `fontFamily: "Inter"` with `fontStyle: "Bold"`, `"Semi Bold"`, `"Medium"`, `"Thin"`, or `"Regular"`

---

## LAYOUT SYSTEM

### Page Structure (Top Nav Bar + No Sidebar)

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

**Build order:**
1. Page frame: 1440×H, fill: #FBF9F4 (opacity ~0.98)
2. Background texture layer: **全屏** Rectangle (1440×H) with #F3F3F3 fill + NOISE effect (color: rgba(0,0,0,0.16)), 作为最底层背景
3. Main Content Shell frame: VERTICAL, full size, no fill (transparent)
4. Container frame: VERTICAL, itemSpacing 16px, padding top 24, left 48, right 48, bottom 40, layoutSizingHorizontal: FILL
5. Top Nav Bar: absolute positioned at top of page frame (see Nav Bar section)
6. Content sections inside Container

### Top Nav Bar

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

### Spacing Constants
- **Container padding: 24px top, 48px left/right, 40px bottom** (different from V1's 40/80/80/40)
- Section gap (Container itemSpacing): 16px
- Card internal padding: 24px (standard), 20px/16px (compact report cards)
- Column gap: 16px
- Content gap within cards: 8-24px depending on density
- Notification item gap: 22px (with divider)

### Grid System
- Container max-width: 1400px (with 48px side padding → ~1304px usable)
- Two-column layout: Left column FILL + Right column FIXED (~280px)
- Report cards: equal-width columns in horizontal row, gap 16px

---

## DESIGN PRINCIPLES

### P1: Color Ratio Rule (85 / 10 / 5)
Same as V1 — greyscale dominant, Cyan accent only, semantic Red/Orange.

**V3 addition**: The warm background (#FBF9F4) and warm card grey (#F8F6F2) add warmth to the greyscale palette without introducing new hue.

### P2: Typography Creates Hierarchy — Serif + Sans-Serif Contrast
V3 adds a new dimension: **serif vs sans-serif** contrast.
- **Serif fonts (Abhaya Libre, Newsreader)**: Used for headings and data values — creates editorial/premium feel
- **Sans-serif (Inter)**: Used for UI labels, body text, navigation — keeps readability for functional elements
- This dual-font approach creates natural hierarchy even at similar sizes

### P3: Depth Through Layering — Noise Texture
V3 adds a subtle noise texture over the background layer for tactile depth:
- Background rectangle: #F3F3F3 fill
- Effect: NOISE (color: rgba(0,0,0,0.16), visible: true)
- This sits behind all content, adding subtle grain

### P4-P15: Same as V1
All other design principles from V1 carry over — spacing grid, Gestalt hierarchy, button discipline, form consistency, card padding rules, no nested borders, selection indicators, data value contrast, icon stroke integrity, card internal padding, title-divider balance, no double padding nesting, secondary button standard, select/filter dropdown standard.

**Exception to P8 (Form Field Consistency):** Corner radius on inputs is now 0px instead of radius-medium (6px).

**Exception to P16 (Secondary Button):** Corner radius on buttons is now 0px instead of radius-large (8px).

### V3-Specific: Button Style

```
"View All" style button (text button with underline stroke):
  fill: NONE
  stroke-bottom: Grey-01, ~1px
  padding: H 0, V bottom 4px
  text: Inter Bold, 10px, Grey-01
  No background, no full border — just text + bottom stroke line

Primary button:
  fill: Grey-01
  stroke: none
  cornerRadius: 0
  padding: H 24px, V 10px
  text: Inter Medium/Bold 14px, White

Secondary button:
  fill: NONE
  stroke: Grey-01, 1px
  cornerRadius: 0
  padding: H 24px, V 10px
  text: Inter Medium/Bold 14px, Grey-01
```

---

## ICON LIBRARY — Phosphor Icons (Regular)

### Source
All icons come from **Phosphor Icons Regular** variant: https://icones.js.org/collection/ph?variant=Regular

### Usage Rules (MANDATORY)
- **ALL icons MUST use Phosphor Regular** — NEVER use the old Lanbow icon library (component page 10:175)
- **How to use**: Create SVG nodes from Phosphor icon paths. Use the `ph:icon-name` naming convention.
- **Default size**: 24×24px for standalone icons, 16×16px for inline/compact contexts, 14×14px for small inline
- **Default stroke/fill**: Phosphor Regular icons use filled paths (not strokes). Apply Grey-01 (#181818) as fill color.
- **For interactive/navigation icons**: Use the component instance approach if Phosphor components are available in the file, otherwise create via SVG.

### Common Phosphor Icons (Regular variant)

**Navigation**: `ph:house`, `ph:chart-bar`, `ph:gear`, `ph:arrows-clockwise`, `ph:caret-down`, `ph:caret-right`, `ph:caret-left`, `ph:arrow-right`, `ph:arrow-left`

**Actions**: `ph:plus`, `ph:minus`, `ph:x`, `ph:check`, `ph:pencil`, `ph:trash`, `ph:copy`, `ph:download`, `ph:upload`, `ph:share`, `ph:export`, `ph:funnel`, `ph:magnifying-glass`

**Data**: `ph:chart-line-up`, `ph:chart-pie-slice`, `ph:chart-bar`, `ph:trend-up`, `ph:trend-down`, `ph:currency-dollar`, `ph:calendar`, `ph:clock`

**Communication**: `ph:bell`, `ph:envelope`, `ph:chat-circle`, `ph:paper-plane-tilt`

**Files**: `ph:file`, `ph:folder`, `ph:clipboard`, `ph:note`

**Users**: `ph:user`, `ph:users`, `ph:user-circle`

**Status**: `ph:warning`, `ph:info`, `ph:check-circle`, `ph:x-circle`, `ph:shield-check`

**Media**: `ph:image`, `ph:camera`, `ph:play`, `ph:pause`

### Icon in Component (from reference design)
The reference design uses Phosphor component instances with properties:
- `Format`: "Stroke" or "Outline"
- `Weight`: "Regular"

When cloning from an existing Phosphor component in the file, use `componentProperties` to set these.

---

## PAGE TYPE TEMPLATES

### Type A: Dashboard Page (V3 Style)
Used for: main app screens, data views, project home
```
Page (1440×H, fill: #FBF9F4, opacity ~0.98)
├── Background Texture (Rectangle, #F3F3F3, NOISE effect, behind content)
├── Main Content Shell (VERTICAL, full size)
│   └── Container (VERTICAL, itemSpacing: 16, padding: 24/48/48/40, maxWidth: 1400)
│       ├── H1 Header (HORIZONTAL, HUG, center-aligned, gap 16)
│       │   ├── Title: Abhaya Libre Bold 36px, #1C1917
│       │   └── Switch icon (Phosphor, 24×24)
│       ├── Main Section (HORIZONTAL, gap 16, FILL width)
│       │   ├── Left Column (FILL, VERTICAL, gap 16)
│       │   │   ├── Featured Card (White fill, padding 24, no radius)
│       │   │   │   ├── Section Title: Abhaya Libre Bold 28px
│       │   │   │   ├── KPI Cards / Charts
│       │   │   │   └── Data visualizations
│       │   │   └── Secondary Card (White fill, padding 24, no radius)
│       │   │       ├── Section Title: Abhaya Libre Bold 28px
│       │   │       └── Sub-cards (HORIZONTAL row, #F8F6F2 fill, padding 20/16)
│       │   └── Right Sidebar Card (FIXED ~280px, White fill, padding 24, no radius)
│       │       ├── Section Title: Abhaya Libre Bold 28px
│       │       ├── Divider line
│       │       ├── Notification items (VERTICAL, gap 22)
│       │       └── "View All" button (bottom-aligned)
├── Top Nav Bar (absolute, at top)
│   ├── Logo
│   ├── Line separator
│   └── Nav links + User info
└── Decorative Cyan line (subtle accent, optional)
```

### Card Styles

**Standard White Card:**
```
fill: White (#FFFFFF)
cornerRadius: 0
stroke: none (or Stroke token if border needed)
padding: 24px all sides
layoutMode: VERTICAL
itemSpacing: 8-24px
```

**Report Sub-Card (warm grey):**
```
fill: #F8F6F2
cornerRadius: 0
padding: left 20, right 20, top 16, bottom 16
layoutMode: VERTICAL
primaryAxisAlignItems: SPACE_BETWEEN
├── Content area (VERTICAL, gap 16)
│   ├── Title: Inter Thin 20px (or Abhaya Libre Medium 24px)
│   ├── Divider: thin rectangle
│   └── Body: Inter Regular 12px
└── Footer (HORIZONTAL, SPACE_BETWEEN)
    ├── Badge ("New") + Date (Abhaya Libre Regular 12px)
    └── Arrow icon (Phosphor ph:arrow-right)
```

**Notification Item:**
```
Container (VERTICAL, gap 22, with divider lines between items)
├── Each item:
│   ├── Meta line: Inter Regular 9px, Grey-06 ("10:42 AM · System Alert")
│   ├── Title: Inter Bold 16px, Grey-01
│   └── Body: Inter Regular 12px, Grey-01
```

---

## KPI / Data Display

### KPI with Spend Progress
```
├── Label: Inter Regular 16px, Grey-01 ("Spend Progress")
├── Value row (HORIZONTAL, baseline-aligned):
│   ├── Main value: Newsreader SemiBold 32px, Grey-01 ("$248,900")
│   └── Total: Newsreader SemiBold 20px, Grey-01 ("/$500,000")
├── Chart area
└── Legend row (HORIZONTAL, gap)
    ├── Dot (8×8, LANBOW-Cyan) + label (Inter Regular 12px)
    └── Dot (8×8, Grey) + label (Inter Regular 12px)
```

### KPI with Trend
```
├── Label: Inter Regular 16px, Grey-01 ("ROI Trend")
├── Value row (HORIZONTAL, baseline-aligned):
│   ├── Main value: Newsreader SemiBold 32px, Grey-01 ("1.85")
│   └── Trend: Inter Regular 12px, LANBOW-Cyan ("↑ 0.12 vs last week")
└── Chart area with axis labels (Inter Regular 12px)
```

---

## EXECUTION PROTOCOL

### Step 1: Understand Requirements
Same as V1 — ask clarifying questions about screens, user role, data context, primary action.
Also determine: **Figma mode** or **Frontend mode**?

### Step 2: Plan Before Creating
State the plan explicitly with page names and purposes.

### Step 3a: Build Each Page (Figma Mode)
For each page:
1. Create page frame (1440×H, #FBF9F4 fill)
2. Add background texture layer (optional noise)
3. Build Main Content Shell → Container with correct padding
4. Add Top Nav Bar (absolute positioned)
5. Build content sections following the card/typography patterns above
6. Add decorative elements (Cyan accent lines, etc.)

### Step 3b: Build Each Page (Frontend Mode)
For each page/component:
1. Ensure CSS variables and Google Fonts are imported (see SETUP)
2. Use semantic HTML structure matching the layout system
3. Apply CSS variables for all colors/spacing — never hardcode
4. Use Phosphor Icons via React component or web font
5. Follow the component patterns below

### Step 4: Self-Review Checklist
After creating each page, verify:
- [ ] All corner radius = 0 (no rounded corners anywhere except avatars)
- [ ] Headings use Abhaya Libre (Bold 36px for H1, Bold 28px for H2)
- [ ] KPI values use Newsreader SemiBold
- [ ] Body/UI text uses Inter
- [ ] Dates use Abhaya Libre Regular 12px
- [ ] Icons are from Phosphor Regular library
- [ ] Page background is #FBF9F4 (not pure white)
- [ ] Cards are White with sharp corners
- [ ] Sub-cards use #F8F6F2 warm grey
- [ ] Nav bar has opacity-based active/inactive states
- [ ] Color tokens are properly referenced (Figma: variable bindings, Frontend: CSS vars)
- [ ] Container padding is 24/48/48/40 (not 40/80/80/40)
- [ ] No sidebar icon menu (V3 removed it)

---

## FRONTEND MODE — Component Patterns

### Typography Utility Classes (CSS)
```css
/* Add to your global stylesheet alongside the CSS variables */
.h1-title {
  font-family: var(--font-display);
  font-weight: 700;
  font-size: 36px;
  color: var(--grey-01);
  line-height: 1.2;
}
.h2-title {
  font-family: var(--font-display);
  font-weight: 700;
  font-size: 28px;
  color: var(--grey-01);
  line-height: 1.3;
}
.h3-title {
  font-family: var(--font-ui);
  font-weight: 100; /* Thin */
  font-size: 20px;
  color: var(--grey-01);
  line-height: 1.4;
}
.kpi-value {
  font-family: var(--font-data);
  font-weight: 600;
  font-size: 32px;
  color: var(--grey-01);
  line-height: 1.25;
}
.kpi-secondary {
  font-family: var(--font-data);
  font-weight: 600;
  font-size: 20px;
  color: var(--grey-01);
  line-height: 1.25;
}
.label-text {
  font-family: var(--font-ui);
  font-weight: 400;
  font-size: 16px;
  color: var(--grey-01);
}
.body-text {
  font-family: var(--font-ui);
  font-weight: 400;
  font-size: 12px;
  color: var(--grey-01);
  line-height: 1.5;
}
.date-text {
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 12px;
  color: var(--grey-01);
}
.meta-text {
  font-family: var(--font-ui);
  font-weight: 400;
  font-size: 9px;
  color: var(--grey-06);
}
.nav-link {
  font-family: var(--font-ui);
  font-weight: 600;
  font-size: 16px;
  color: var(--nav-text);
  text-decoration: none;
  opacity: 0.5;
  transition: opacity 0.2s;
}
.nav-link.active, .nav-link:hover {
  opacity: 1;
}
```

### Page Layout (HTML/CSS)
```html
<div class="lanbow-page">
  <!-- Top Nav Bar -->
  <nav class="lanbow-nav">
    <div class="nav-left">
      <div class="nav-logo"><!-- Logo SVG --></div>
      <div class="nav-divider"></div>
      <div class="nav-links">
        <a href="#" class="nav-link active">Home</a>
        <a href="#" class="nav-link">AI Cake</a>
        <a href="#" class="nav-link">Bills</a>
      </div>
    </div>
    <div class="nav-right">
      <div class="nav-avatar"></div>
      <span class="nav-username">USER NAME</span>
    </div>
  </nav>

  <!-- Main Content -->
  <main class="lanbow-container">
    <h1 class="h1-title">Page Title</h1>
    <div class="content-grid">
      <div class="left-column">
        <div class="card"><!-- Card content --></div>
      </div>
      <aside class="right-column">
        <div class="card"><!-- Sidebar content --></div>
      </aside>
    </div>
  </main>
</div>
```

```css
.lanbow-page {
  min-height: 100vh;
  background-color: var(--page-bg);
  position: relative;
}

/* Nav */
.lanbow-nav {
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 24px;
  height: 40px;
  border-bottom: 1px solid var(--nav-border);
  background: transparent;
  backdrop-filter: blur(8px);
}
.nav-left, .nav-right {
  display: flex;
  align-items: center;
  gap: 24px;
}
.nav-divider {
  width: 1px;
  height: 16px;
  background: var(--nav-border);
}
.nav-links {
  display: flex;
  align-items: center;
  gap: 36px;
}
.nav-right {
  gap: 8px;
}
.nav-avatar {
  width: 24px;
  height: 24px;
  border-radius: 9999px;
  background: var(--grey-12);
}
.nav-username {
  font-family: var(--font-ui);
  font-size: 13px;
  color: var(--grey-06);
}

/* Container */
.lanbow-container {
  max-width: var(--container-max-width);
  margin: 0 auto;
  padding: var(--container-padding-top) var(--container-padding-x) var(--container-padding-bottom);
  display: flex;
  flex-direction: column;
  gap: var(--container-gap);
}

/* Grid */
.content-grid {
  display: flex;
  gap: var(--space-base);
}
.left-column {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--space-base);
}
.right-column {
  width: 280px;
  flex-shrink: 0;
}

/* Card */
.card {
  background: var(--white);
  padding: var(--space-xl);
  border-radius: 0; /* V3: sharp corners */
}
.card-warm {
  background: var(--card-warm-grey);
  padding: 16px 20px;
  border-radius: 0;
}
```

### React Component Example — KPI Card
```tsx
import { ArrowUp } from '@phosphor-icons/react'

function KpiCard({ label, value, total, trend }: {
  label: string
  value: string
  total?: string
  trend?: { value: string; positive: boolean }
}) {
  return (
    <div className="card">
      <span className="label-text">{label}</span>
      <div style={{ display: 'flex', alignItems: 'baseline', gap: 4, marginTop: 8 }}>
        <span className="kpi-value">{value}</span>
        {total && <span className="kpi-secondary">/{total}</span>}
        {trend && (
          <span style={{
            fontFamily: 'var(--font-ui)',
            fontSize: 12,
            color: trend.positive ? 'var(--lanbow-cyan)' : 'var(--danger)',
            marginLeft: 8,
          }}>
            {trend.positive ? '↑' : '↓'} {trend.value}
          </span>
        )}
      </div>
    </div>
  )
}
```

### React Component Example — Notification Item
```tsx
function NotificationItem({ time, title, body }: {
  time: string
  title: string
  body: string
}) {
  return (
    <div style={{ display: 'flex', flexDirection: 'column', gap: 4 }}>
      <span className="meta-text">{time}</span>
      <span style={{
        fontFamily: 'var(--font-ui)',
        fontWeight: 700,
        fontSize: 16,
        color: 'var(--grey-01)',
      }}>{title}</span>
      <span className="body-text">{body}</span>
    </div>
  )
}
```

### React Component Example — Report Sub-Card
```tsx
import { ArrowRight } from '@phosphor-icons/react'

function ReportCard({ title, body, date, isNew }: {
  title: string
  body: string
  date: string
  isNew?: boolean
}) {
  return (
    <div className="card-warm" style={{
      display: 'flex',
      flexDirection: 'column',
      justifyContent: 'space-between',
      minHeight: 200,
    }}>
      <div style={{ display: 'flex', flexDirection: 'column', gap: 16 }}>
        <h3 className="h3-title">{title}</h3>
        <div style={{ height: 1, background: 'var(--grey-12)' }} />
        <p className="body-text">{body}</p>
      </div>
      <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
        <div style={{ display: 'flex', alignItems: 'center', gap: 6 }}>
          {isNew && (
            <span style={{
              fontFamily: 'var(--font-ui)',
              fontWeight: 700,
              fontSize: 9,
              color: 'var(--lanbow-cyan)',
            }}>New</span>
          )}
          <span className="date-text">{date}</span>
        </div>
        <ArrowRight size={16} color="var(--grey-01)" />
      </div>
    </div>
  )
}
```

### Phosphor Icons — Frontend Usage

**React:**
```tsx
import { House, ChartBar, Gear, Bell, ArrowRight, CaretDown } from '@phosphor-icons/react'

// Regular weight (default), size matches design
<House size={24} color="var(--grey-01)" />
<Bell size={20} color="var(--grey-01)" />
<CaretDown size={12} color="var(--grey-08)" />
```

**Vanilla HTML (via CDN):**
```html
<i class="ph ph-house" style="font-size: 24px; color: var(--grey-01);"></i>
<i class="ph ph-bell" style="font-size: 20px; color: var(--grey-01);"></i>
```

### Noise Texture Background

**Figma 原始参数**:
- 页面底色: `#FBF9F4`, opacity ~0.98
- 噪点层: 全屏 Rectangle `#F3F3F3` fill + Effect `NOISE` (color: `rgba(0,0,0,0.16)`)
- 噪点铺满整个页面作为最底层背景纹理

**Figma mode 构建**:
1. 页面 frame fill: `#FBF9F4`
2. 第一个子层: 全屏 Rectangle，fill `#F3F3F3`，添加 effect `{ type: "NOISE", visible: true, color: rgba(0,0,0,0.16) }`
3. 后续内容层叠加在噪点层之上

**Frontend 方案 A — SVG Filter 模拟（纯 CSS，无额外资源）:**
```css
.lanbow-page {
  background-color: var(--page-bg);
  position: relative;
  min-height: 100vh;
}
.lanbow-page::before {
  content: '';
  position: fixed;
  inset: 0;
  z-index: 0;
  background-color: var(--background);
  pointer-events: none;
  filter: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.16'/%3E%3C/svg%3E");
}
.lanbow-page > * {
  position: relative;
  z-index: 1;
}
```

**Frontend 方案 B — 导出纹理图片平铺（推荐，效果最接近 Figma）:**

1. 从 Figma 导出噪点矩形为 PNG（约 200×200px 即可，确保导出含 noise effect）
2. 保存为 `noise-texture.png` 放入项目 assets
3. CSS 引用：
```css
.lanbow-page {
  background-color: var(--page-bg);
  position: relative;
  min-height: 100vh;
}
.lanbow-page::before {
  content: '';
  position: fixed;
  inset: 0;
  z-index: 0;
  background: url('/assets/noise-texture.png') repeat;
  pointer-events: none;
}
.lanbow-page > * {
  position: relative;
  z-index: 1;
}
```

方案 B 更精确还原 Figma NOISE effect 的颗粒感，推荐用于生产环境。方案 A 适合快速原型。

---

## FRONTEND MODE — Microinteractions & Motion Design

Lanbow V3 的动效风格: **克制、精准、有目的**。符合设计系统的 restrained aesthetic，动效不是装饰而是反馈。

### Motion Principles

| 原则 | 说明 |
|---|---|
| **Purposeful** | 每个动效必须传达信息：确认操作、引导注意力、维持上下文 |
| **Restrained** | 匹配 Lanbow 克制的美学，避免花哨动画 |
| **Physics-based** | 优先使用 spring 动画而非线性，更自然 |
| **Accessible** | 始终尊重 `prefers-reduced-motion` |

### Timing Guidelines

| Duration | Use Case |
|---|---|
| 100-150ms | Micro-feedback: hover, click, toggle |
| 200-300ms | Small transitions: dropdown, tooltip |
| 300-500ms | Medium transitions: modal, page change, card expand |
| 500ms+ | Complex choreography: page load stagger |

### Easing Functions (CSS Variables)

```css
:root {
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);      /* Decelerate — entering elements */
  --ease-in: cubic-bezier(0.55, 0, 1, 0.45);       /* Accelerate — exiting elements */
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);   /* Both — moving between states */
  --spring: cubic-bezier(0.34, 1.56, 0.64, 1);     /* Overshoot — playful/bouncy */
}
```

### Accessibility: Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

```tsx
function useReducedMotion() {
  const [prefersReduced, setPrefersReduced] = useState(false);
  useEffect(() => {
    const mq = window.matchMedia('(prefers-reduced-motion: reduce)');
    setPrefersReduced(mq.matches);
    const handler = (e: MediaQueryListEvent) => setPrefersReduced(e.matches);
    mq.addEventListener('change', handler);
    return () => mq.removeEventListener('change', handler);
  }, []);
  return prefersReduced;
}
```

---

### 1. Button Microinteractions

**Primary Button — Press Feedback (Lanbow V3 style: sharp, subtle)**:
```tsx
import { motion } from 'framer-motion'

function LanbowButton({ children, onClick, variant = 'primary' }) {
  const styles = {
    primary: 'bg-grey-01 text-white',
    secondary: 'bg-transparent border border-grey-01 text-grey-01',
  }

  return (
    <motion.button
      onClick={onClick}
      whileHover={{ opacity: 0.85 }}
      whileTap={{ scale: 0.97 }}
      transition={{ type: 'spring', stiffness: 400, damping: 17 }}
      className={`px-6 py-2.5 font-ui text-sm font-medium ${styles[variant]}`}
      style={{ borderRadius: 0 }}  /* V3: sharp corners */
    >
      {children}
    </motion.button>
  )
}
```

**Loading Button — State Transition**:
```tsx
import { motion, AnimatePresence } from 'framer-motion'

function LoadingButton({ isLoading, children, onClick }) {
  return (
    <motion.button
      onClick={onClick}
      disabled={isLoading}
      whileTap={!isLoading ? { scale: 0.97 } : {}}
      className="relative px-6 py-2.5 bg-[var(--grey-01)] text-white font-ui text-sm overflow-hidden"
      style={{ borderRadius: 0 }}
    >
      <AnimatePresence mode="wait">
        {isLoading ? (
          <motion.span
            key="loading"
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -10 }}
            className="flex items-center gap-2"
          >
            <svg className="w-4 h-4 animate-spin" viewBox="0 0 24 24">
              <circle cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="3"
                fill="none" strokeDasharray="62.83" strokeDashoffset="15" strokeLinecap="round" />
            </svg>
            Processing...
          </motion.span>
        ) : (
          <motion.span
            key="idle"
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -10 }}
          >
            {children}
          </motion.span>
        )}
      </AnimatePresence>
    </motion.button>
  )
}
```

**"View All" Text Button — Underline Hover**:
```css
.btn-view-all {
  font-family: var(--font-ui);
  font-weight: 700;
  font-size: 10px;
  color: var(--grey-01);
  text-decoration: none;
  border: none;
  background: none;
  padding: 0 0 4px;
  border-bottom: 1px solid var(--grey-01);
  cursor: pointer;
  transition: opacity 0.15s var(--ease-out);
}
.btn-view-all:hover {
  opacity: 0.6;
}
```

---

### 2. Card Interactions

**Card Hover — Subtle Lift (V3: no shadow, use translate only)**:
```css
.card {
  background: var(--white);
  padding: var(--space-xl);
  border-radius: 0;
  transition: transform 0.2s var(--ease-out);
}
.card:hover {
  transform: translateY(-2px);
}
```

**Report Sub-Card — Click Feedback**:
```tsx
function ReportCardInteractive({ title, body, date, isNew, onClick }) {
  return (
    <motion.div
      onClick={onClick}
      whileHover={{ y: -2 }}
      whileTap={{ scale: 0.985 }}
      transition={{ type: 'spring', stiffness: 400, damping: 25 }}
      className="card-warm cursor-pointer"
      style={{
        background: 'var(--card-warm-grey)',
        padding: '16px 20px',
        borderRadius: 0,
        display: 'flex',
        flexDirection: 'column',
        justifyContent: 'space-between',
        minHeight: 200,
      }}
    >
      {/* card content */}
    </motion.div>
  )
}
```

---

### 3. Page Load — Staggered Reveal

```tsx
import { motion } from 'framer-motion'

const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: { staggerChildren: 0.08, delayChildren: 0.1 },
  },
}

const itemVariants = {
  hidden: { opacity: 0, y: 16 },
  visible: {
    opacity: 1,
    y: 0,
    transition: { duration: 0.4, ease: [0.16, 1, 0.3, 1] },
  },
}

function DashboardPage() {
  return (
    <motion.div
      className="lanbow-container"
      variants={containerVariants}
      initial="hidden"
      animate="visible"
    >
      <motion.h1 variants={itemVariants} className="h1-title">
        Project Name
      </motion.h1>
      <motion.div variants={itemVariants} className="content-grid">
        {/* cards stagger in naturally */}
      </motion.div>
    </motion.div>
  )
}
```

---

### 4. Scroll Animations

**Fade-In on Scroll (Intersection Observer)**:
```tsx
function useInView({ threshold = 0, triggerOnce = false } = {}) {
  const ref = useRef(null);
  const [isInView, setIsInView] = useState(false);

  useEffect(() => {
    const el = ref.current;
    if (!el) return;
    const observer = new IntersectionObserver(
      ([entry]) => {
        const inView = entry.isIntersecting;
        setIsInView(inView);
        if (inView && triggerOnce) observer.unobserve(el);
      },
      { threshold }
    );
    observer.observe(el);
    return () => observer.disconnect();
  }, [threshold, triggerOnce]);

  return [ref, isInView];
}

function FadeInSection({ children }) {
  const [ref, isInView] = useInView({ threshold: 0.2, triggerOnce: true });
  return (
    <div
      ref={ref}
      className={`transition-all duration-700 ${
        isInView ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-8'
      }`}
    >
      {children}
    </div>
  );
}
```

**Scroll Progress Bar (Cyan accent)**:
```tsx
import { motion, useScroll, useSpring } from 'framer-motion'

function ScrollProgress() {
  const { scrollYProgress } = useScroll();
  const scaleX = useSpring(scrollYProgress, {
    stiffness: 100, damping: 30, restDelta: 0.001,
  });
  return (
    <motion.div
      className="fixed top-0 left-0 right-0 h-[2px] origin-left z-50"
      style={{ scaleX, backgroundColor: 'var(--lanbow-cyan)' }}
    />
  );
}
```

---

### 5. Navigation Interactions

**Top Nav — Active Link Indicator (opacity transition)**:
```tsx
function LanbowNav({ items, activeItem }) {
  return (
    <nav className="lanbow-nav">
      <div className="nav-left">
        <div className="nav-logo">{/* Logo */}</div>
        <div className="nav-divider" />
        <div className="nav-links">
          {items.map((item) => (
            <motion.a
              key={item.href}
              href={item.href}
              className="nav-link"
              animate={{ opacity: item.id === activeItem ? 1 : 0.5 }}
              whileHover={{ opacity: 0.8 }}
              transition={{ duration: 0.15 }}
            >
              {item.label}
            </motion.a>
          ))}
        </div>
      </div>
    </nav>
  );
}
```

---

### 6. Notification & Feedback

**Toast Notification (Lanbow V3 style: sharp corners, Grey-01 bg)**:
```tsx
import { motion, AnimatePresence } from 'framer-motion'

function ToastContainer({ toasts, onDismiss }) {
  return (
    <div className="fixed bottom-6 right-6 space-y-2 z-50">
      <AnimatePresence>
        {toasts.map((toast) => (
          <motion.div
            key={toast.id}
            initial={{ opacity: 0, y: 20, scale: 0.95 }}
            animate={{ opacity: 1, y: 0, scale: 1 }}
            exit={{ opacity: 0, x: 100, scale: 0.95 }}
            transition={{ type: 'spring', stiffness: 400, damping: 25 }}
            onClick={() => onDismiss(toast.id)}
            style={{
              background: 'var(--grey-01)',
              color: 'var(--white)',
              fontFamily: 'var(--font-ui)',
              fontSize: 13,
              padding: '12px 20px',
              borderRadius: 0,
              cursor: 'pointer',
              minWidth: 240,
            }}
          >
            {toast.message}
          </motion.div>
        ))}
      </AnimatePresence>
    </div>
  );
}
```

---

### 7. Form Interactions

**Input Focus — Bottom Border Highlight (V3: sharp, minimal)**:
```css
.lanbow-input {
  font-family: var(--font-ui);
  font-size: 14px;
  color: var(--grey-01);
  background: var(--white);
  border: 1px solid var(--grey-12);
  border-radius: 0;
  padding: 10px 12px;
  width: 100%;
  outline: none;
  transition: border-color 0.15s var(--ease-out);
}
.lanbow-input:focus {
  border-color: var(--grey-01);
}
.lanbow-input::placeholder {
  color: var(--grey-08);
}
```

**Shake on Error**:
```tsx
import { motion, useAnimation } from 'framer-motion'

function ShakeInput({ error, ...props }) {
  const controls = useAnimation();
  useEffect(() => {
    if (error) {
      controls.start({
        x: [0, -8, 8, -8, 8, 0],
        transition: { duration: 0.4 },
      });
    }
  }, [error, controls]);

  return (
    <motion.div animate={controls}>
      <input
        {...props}
        className="lanbow-input"
        style={{ borderColor: error ? 'var(--red)' : undefined }}
      />
      {error && (
        <motion.p
          initial={{ opacity: 0, y: -8 }}
          animate={{ opacity: 1, y: 0 }}
          style={{
            fontFamily: 'var(--font-ui)', fontSize: 12,
            color: 'var(--red)', marginTop: 4,
          }}
        >
          {error}
        </motion.p>
      )}
    </motion.div>
  );
}
```

---

### 8. KPI Data — Number Count-Up

```tsx
import { motion, useMotionValue, useTransform, animate } from 'framer-motion'
import { useEffect } from 'react'

function CountUp({ value, duration = 1.5 }) {
  const count = useMotionValue(0);
  const rounded = useTransform(count, (v) =>
    v.toLocaleString('en-US', { maximumFractionDigits: 0 })
  );

  useEffect(() => {
    const controls = animate(count, value, {
      duration,
      ease: [0.16, 1, 0.3, 1],
    });
    return controls.stop;
  }, [value]);

  return (
    <motion.span className="kpi-value">{rounded}</motion.span>
  );
}

// Usage: <CountUp value={248900} />
```

---

### 9. Page Transitions

```tsx
import { AnimatePresence, motion } from 'framer-motion'

const pageVariants = {
  initial: { opacity: 0, y: 12 },
  enter: { opacity: 1, y: 0 },
  exit: { opacity: 0, y: -12 },
};

function LanbowPageTransition({ children, pageKey }) {
  return (
    <AnimatePresence mode="wait">
      <motion.div
        key={pageKey}
        variants={pageVariants}
        initial="initial"
        animate="enter"
        exit="exit"
        transition={{ duration: 0.3, ease: [0.16, 1, 0.3, 1] }}
      >
        {children}
      </motion.div>
    </AnimatePresence>
  );
}
```

---

### 10. Skeleton Loading (V3 Style)

```tsx
function CardSkeleton() {
  return (
    <div className="card" style={{ padding: 'var(--space-xl)' }}>
      <div className="animate-pulse" style={{ display: 'flex', flexDirection: 'column', gap: 12 }}>
        <div style={{ height: 28, width: '40%', background: 'var(--grey-12)' }} />
        <div style={{ height: 40, width: '60%', background: 'var(--grey-12)' }} />
        <div style={{ height: 160, width: '100%', background: 'var(--card-warm-grey)' }} />
        <div style={{ display: 'flex', gap: 16 }}>
          <div style={{ height: 12, width: 60, background: 'var(--grey-12)' }} />
          <div style={{ height: 12, width: 60, background: 'var(--grey-12)' }} />
        </div>
      </div>
    </div>
  );
}
```

### CSS Keyframe Animations

```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(12px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.4; }
}

.animate-fadeInUp {
  animation: fadeInUp 0.4s var(--ease-out) both;
}
.animate-pulse {
  animation: pulse 1.8s ease-in-out infinite;
}

/* Staggered children */
.stagger > *:nth-child(1) { animation-delay: 0ms; }
.stagger > *:nth-child(2) { animation-delay: 80ms; }
.stagger > *:nth-child(3) { animation-delay: 160ms; }
.stagger > *:nth-child(4) { animation-delay: 240ms; }
.stagger > *:nth-child(5) { animation-delay: 320ms; }
```

### Performance Best Practices

1. **只动 `transform` 和 `opacity`** — 保证 60fps，避免动 `width/height/top/left`
2. **谨慎使用 `will-change`** — 仅在确实需要 GPU 加速的元素上
3. **Spring > Linear** — 使用弹性动画更自然，匹配 Lanbow 的品质感
4. **可中断** — 允许用户在动画过程中操作，不要阻塞交互
5. **渐进增强** — 无 JS 时也能正常使用，动效是增强而非必需

---

## KEY DIFFERENCES FROM V1 (Quick Reference)

| Aspect | V1 (lanbow-design) | V3 (lanbow-v3) |
|---|---|---|
| Heading font | Inter Bold | Abhaya Libre Bold |
| KPI value font | Inter Bold | Newsreader SemiBold |
| Date font | Inter Regular | Abhaya Libre Regular |
| Corner radius | Variable (2-24px) | 0px everywhere |
| Page background | White (#FFFFFF) | Warm off-white (#FBF9F4) |
| Sub-card fill | Background (#F3F3F3) | Warm grey (#F8F6F2) |
| Sidebar | 56px icon sidebar | None (removed) |
| Main content radius | 24px (radius-3xl) | 0px |
| Container padding | 40/80/80/40 | 24/48/48/40 |
| Container gap | 24px | 16px |
| Icon library | Lanbow custom (page 10:175) | Phosphor Regular |
| Nav bar | Component instance (6443:11918) | Custom top bar with logo + links |
| Nav active state | Cyan color | Opacity 1.0 vs 0.5 |
| Button style | radius-large (8px) | Sharp corners (0px) |
| Background texture | None | Noise effect on #F3F3F3 |
