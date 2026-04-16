# Lanbow V3 — Component Patterns

Card styles, KPI displays, notifications, page templates, and button styles.

---

## BUTTON STYLES

### V3-Specific Button Style

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

## TABLE / LIST STYLE

### Simple Data Table (inside White Card)

From the "Recharge Bills" pattern in Flow 3.0. A flat, borderless table with zebra striping.

```
Card (White, padding 24, no shadow, 0px radius)
├── Section Title: Abhaya Libre Bold 28px (H2), Grey-01
├── Divider: 1px, #E7E5E4 (or Grey-12)
├── Table (VERTICAL, FILL width, gap 0, clip content)
│   ├── Header Row (HORIZONTAL, center-aligned, FILL width)
│   │   height: 46px
│   │   fill: Selected token (invisible/transparent — no visible bg)
│   │   padding: left 16, right 16
│   │   ├── Column headers: Abhaya Libre Bold 20px, Grey-01
│   │   └── Each column: layoutSizingHorizontal FILL (equal width)
│   ├── Line: Stroke token (rgba(0,0,0,0.04)), 1px
│   ├── Data Row — odd (HORIZONTAL, center-aligned, FILL width)
│   │   height: 42px
│   │   fill: #F8F6F2 @ 40% opacity (warm zebra stripe)
│   │   padding: left 16, right 16
│   │   ├── Cell text: Inter Bold 14px, Grey-01
│   │   └── Each column: layoutSizingHorizontal FILL
│   ├── Data Row — even (same layout, NO fill — transparent)
│   ├── ... alternating rows
```

### Table Design Rules

| Property | Value | Token |
|---|---|---|
| Header row height | 46px | Custom (accommodates 20px text + padding) |
| Data row height | 42px | Custom (accommodates 14px text + padding) |
| Row horizontal padding | 16px | `--space-base` |
| Header text | Abhaya Libre Bold 20px | Matches H3-level hierarchy |
| Cell text | Inter Bold 14px | Standard data text |
| Text color | Grey-01 | `fillVariableName: "Grey-01"` |
| Zebra stripe (odd rows) | #F8F6F2 @ 40% opacity | card-warm-grey, reduced |
| Zebra stripe (even rows) | transparent | No fill |
| Row divider | Stroke token, 1px | Between header and first row; between row pairs (optional) |
| Column sizing | FILL (equal width) | All columns stretch equally |
| Card → table gap | 24px | `--space-xl` (after section title + divider) |

### Key Differences from Generic Tables
- **No outer border** on table — lives inside a White card, border-free per P9
- **No header background visible** — Selected token is near-transparent, header hierarchy comes from font contrast (Abhaya 20px vs Inter 14px)
- **Zebra uses warm grey** (#F8F6F2) not cold grey — consistent with V3 warm palette
- **Column headers use serif** (Abhaya Libre Bold) while data uses sans-serif (Inter Bold) — creates natural visual separation without needing color differentiation
- **All text is Grey-01** — no grey-06/grey-08 for secondary columns; hierarchy is purely typographic

### Frontend — Table CSS
```css
.lanbow-table {
  width: 100%;
  border-collapse: collapse;
  border-radius: 0;
}
.lanbow-table th {
  font-family: var(--font-display);
  font-weight: 700;
  font-size: 20px;
  color: var(--grey-01);
  text-align: left;
  padding: 0 var(--space-base);
  height: 46px;
  vertical-align: middle;
  border-bottom: 1px solid var(--stroke);
}
.lanbow-table td {
  font-family: var(--font-ui);
  font-weight: 700;
  font-size: 14px;
  color: var(--grey-01);
  text-align: left;
  padding: 0 var(--space-base);
  height: 42px;
  vertical-align: middle;
}
.lanbow-table tr:nth-child(odd) td {
  background: rgba(248, 246, 242, 0.4);
}
```

### Figma Mode — Table Build Template
```js
// Table container
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "table",
  parentId: "<cardId>",
  layoutMode: "VERTICAL",
  layoutSizingHorizontal: "FILL",
  primaryAxisSizingMode: "AUTO",
  clipsContent: true,
}])

// Header row
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "header-row",
  parentId: "<tableId>",
  layoutMode: "HORIZONTAL",
  layoutSizingHorizontal: "FILL",
  height: 46,
  layoutSizingVertical: "FIXED",
  counterAxisAlignItems: "CENTER",
  paddingLeft: 16, paddingRight: 16,
  fillVariableName: "Selected",
}])

// Header text (repeat per column)
mcp__Vibma__text(method: "create", items: [{
  parentId: "<headerRowId>",
  content: "Date",
  fontFamily: "Abhaya Libre",
  fontStyle: "Bold",
  fontSize: 20,
  fillVariableName: "Grey-01",
  layoutSizingHorizontal: "FILL",
}])

// Data row (odd — with zebra fill)
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "row-1",
  parentId: "<tableId>",
  layoutMode: "HORIZONTAL",
  layoutSizingHorizontal: "FILL",
  height: 42,
  layoutSizingVertical: "FIXED",
  counterAxisAlignItems: "CENTER",
  paddingLeft: 16, paddingRight: 16,
  fills: [{ type: "SOLID", color: "#F8F6F2", opacity: 0.4 }],
}])

// Data row (even — no fill)
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "row-2",
  parentId: "<tableId>",
  layoutMode: "HORIZONTAL",
  layoutSizingHorizontal: "FILL",
  height: 42,
  layoutSizingVertical: "FIXED",
  counterAxisAlignItems: "CENTER",
  paddingLeft: 16, paddingRight: 16,
}])

// Cell text (repeat per column)
mcp__Vibma__text(method: "create", items: [{
  parentId: "<rowId>",
  content: "2026-03-03",
  fontFamily: "Inter",
  fontStyle: "Bold",
  fontSize: 14,
  fillVariableName: "Grey-01",
  layoutSizingHorizontal: "FILL",
}])
```

---

## CARD STYLES

### Standard White Card
```
fill: White (#FFFFFF)
cornerRadius: 0
stroke: none (or Stroke token if border needed)
shadow: none (V3: no shadow on grounded cards)
padding: 24px all sides
layoutMode: VERTICAL
itemSpacing: 8-24px
```

### Report Sub-Card (warm grey)
```
fill: #F8F6F2
cornerRadius: 0
shadow: none
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

### Notification Item
```
Container (VERTICAL, gap 24, with divider lines between items)
├── Each item:
│   ├── Meta line: Inter Regular 9px, Grey-06 ("10:42 AM · System Alert")
│   ├── Title: Inter Bold 16px, Grey-01
│   └── Body: Inter Regular 12px, Grey-01
```

---

## KPI / DATA DISPLAY

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
│       │   │   ├── Featured Card (White fill, padding 24, no radius, no shadow)
│       │   │   │   ├── Section Title: Abhaya Libre Bold 28px
│       │   │   │   ├── KPI Cards / Charts
│       │   │   │   └── Data visualizations
│       │   │   └── Secondary Card (White fill, padding 24, no radius, no shadow)
│       │   │       ├── Section Title: Abhaya Libre Bold 28px
│       │   │       └── Sub-cards (HORIZONTAL row, #F8F6F2 fill, padding 20/16)
│       │   └── Right Sidebar Card (FIXED ~280px, White fill, padding 24, no radius, no shadow)
│       │       ├── Section Title: Abhaya Libre Bold 28px
│       │       ├── Divider line
│       │       ├── Notification items (VERTICAL, gap 24)
│       │       └── "View All" button (bottom-aligned)
├── Top Nav Bar (absolute, at top)
│   ├── Logo
│   ├── Line separator
│   └── Nav links + User info
└── Decorative Cyan line (subtle accent, optional)
```

---

## PAGE TYPE TEMPLATES

### Type A: Dashboard Page (V3 Style)
Used for: main app screens, data views, project home. See `layout.md` for full structure diagram.

### Type B: List Page
Used for: data tables, record lists, search results
```
Page (1440×H, #FBF9F4)
├── Top Nav Bar (absolute)
├── Container (VERTICAL, 24/48/48/40 padding, gap 16)
│   ├── Header Row (HORIZONTAL, SPACE_BETWEEN, center-aligned)
│   │   ├── H1: Abhaya Libre Bold 36px
│   │   └── Actions (HORIZONTAL, gap 8)
│   │       ├── Search Input (FILL, 40px height, Grey-12 border)
│   │       ├── Filter Dropdown (secondary button style)
│   │       └── Primary Button ("+ New Item")
│   └── Table Card (White, padding 0, no shadow)
│       ├── Table Header (HORIZONTAL, Selected fill, padding 12-16)
│       │   ├── Checkbox (16×16)
│       │   ├── Column headers: Inter Medium 12px, Grey-06
│       │   └── Sort icons (ph:caret-up-down, 14px)
│       ├── Table Rows (VERTICAL, divider Grey-12 between rows)
│       │   └── Row (HORIZONTAL, padding 12-16, hover: Selected fill)
│       │       ├── Checkbox
│       │       ├── Cell data: Inter Regular 12-14px, Grey-01
│       │       ├── Status badge (tag style, 0px radius)
│       │       └── Action icons (ph:pencil, ph:trash, 16px)
│       └── Pagination (HORIZONTAL, SPACE_BETWEEN, padding 12-16)
│           ├── "Showing 1-20 of 156": Inter Regular 12px, Grey-06
│           └── Page buttons (secondary button style, compact 8px padding)
```

### Type C: Detail Page
Used for: single record view, profile, project detail
```
Page (1440×H, #FBF9F4)
├── Top Nav Bar (absolute)
├── Container (VERTICAL, 24/48/48/40 padding, gap 16)
│   ├── Breadcrumb (HORIZONTAL, gap 8)
│   │   ├── "Projects": Inter Regular 14px, LANBOW-Cyan (link)
│   │   ├── "/": Inter Regular 14px, Grey-08
│   │   └── "Project Name": Inter Regular 14px, Grey-01 (current)
│   ├── Header Card (White, padding 24)
│   │   ├── Top Row (HORIZONTAL, SPACE_BETWEEN)
│   │   │   ├── H1: Abhaya Libre Bold 36px
│   │   │   └── Actions (buttons)
│   │   ├── Meta Row (HORIZONTAL, gap 24, margin-top 8)
│   │   │   ├── "Created: 2026/03/15" — date-text style
│   │   │   ├── "Status: Active" — badge style
│   │   │   └── "Owner: John" — body-text style
│   │   └── Optional: Tab bar (HORIZONTAL, gap 0)
│   │       ├── Active tab: Inter Medium 14px, Grey-01, bottom border 2px Grey-01
│   │       └── Inactive tab: Inter Regular 14px, Grey-08, no border
│   └── Content Grid (HORIZONTAL, gap 16)
│       ├── Main Column (FILL, VERTICAL, gap 16)
│       │   ├── Section Card (White, padding 24)
│       │   └── Section Card (White, padding 24)
│       └── Side Column (FIXED 320px, VERTICAL, gap 16)
│           ├── Info Card (White, padding 24)
│           └── Activity Card (White, padding 24)
```

### Type D: Form Page
Used for: create/edit forms, settings, onboarding
```
Page (1440×H, #FBF9F4)
├── Top Nav Bar (absolute)
├── Container (VERTICAL, 24/48/48/40 padding, gap 16)
│   ├── Header Row (HORIZONTAL, SPACE_BETWEEN)
│   │   ├── H1: Abhaya Libre Bold 36px
│   │   └── "Cancel" link: Inter Regular 14px, Grey-06
│   └── Form Card (White, padding 24, max-width 720px)
│       ├── Section Title: Abhaya Libre Bold 28px
│       ├── Form Fields (VERTICAL, gap 20)
│       │   ├── Field Group (VERTICAL, gap 8)
│       │   │   ├── Label: • Inter Medium 14px, Grey-01
│       │   │   └── Input: 40px height, 0px radius, Grey-12 border
│       │   ├── Field Group (2-column for short fields)
│       │   │   ├── Left Field (FILL)
│       │   │   └── Right Field (FILL)
│       │   └── Textarea: min-height 120px, same border/radius style
│       ├── Divider (1px Grey-12)
│       ├── Section Title: Abhaya Libre Bold 28px
│       ├── More fields...
│       └── Button Row (HORIZONTAL, gap 8, right-aligned)
│           ├── Secondary Button ("Cancel")
│           └── Primary Button ("Save")
```

### Type E: Login / Auth Page
Used for: login, register, password reset
```
Page (1440×900, HORIZONTAL)
├── Brand Panel (FIXED 560px, Grey-01 fill or gradient — exception to P6)
│   ├── Logo (White, centered or top-left with padding 40)
│   ├── Tagline: Abhaya Libre Bold 48px, White, centered
│   └── Optional: decorative pattern or product screenshot
└── Form Panel (FILL, #FBF9F4, centered content)
    └── Form Container (max-width 400px, centered vertically)
        ├── Welcome text: Abhaya Libre Bold 28px, Grey-01
        ├── Subtitle: Inter Regular 14px, Grey-06
        ├── Form Fields (VERTICAL, gap 16, margin-top 32)
        │   ├── Email field (with • label)
        │   ├── Password field (with • label + show/hide icon)
        │   └── "Forgot password?" — Inter Regular 12px, LANBOW-Cyan, right-aligned
        ├── Primary Button (FILL width, "Sign In")
        ├── Divider with text ("or continue with")
        └── Social login buttons (secondary style, FILL width)
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

**Navigation**: `ph:house`, `ph:chart-bar`, `ph:gear`, `ph:arrows-clockwise`, `ph:caret-down`, `ph:caret-right`, `ph:caret-left`, `ph:arrow-right`, `ph:arrow-left`, `ph:list` (hamburger)

**Actions**: `ph:plus`, `ph:minus`, `ph:x`, `ph:check`, `ph:pencil`, `ph:trash`, `ph:copy`, `ph:download`, `ph:upload`, `ph:share`, `ph:export`, `ph:funnel`, `ph:magnifying-glass`

**Data**: `ph:chart-line-up`, `ph:chart-pie-slice`, `ph:chart-bar`, `ph:trend-up`, `ph:trend-down`, `ph:currency-dollar`, `ph:calendar`, `ph:clock`

**Communication**: `ph:bell`, `ph:envelope`, `ph:chat-circle`, `ph:paper-plane-tilt`

**Files**: `ph:file`, `ph:folder`, `ph:clipboard`, `ph:note`

**Users**: `ph:user`, `ph:users`, `ph:user-circle`

**Status**: `ph:warning`, `ph:info`, `ph:check-circle`, `ph:x-circle`, `ph:shield-check`

**Media**: `ph:image`, `ph:camera`, `ph:play`, `ph:pause`

### Icon in Component (Figma mode)
When cloning from an existing Phosphor component in the Figma file, set `componentProperties`:
- `Weight`: "Regular"

**Note:** Phosphor Regular icons use **filled paths** (not stroke-based). The `fill` color should be set to Grey-01 (#181818). Do NOT confuse with stroke-based icon libraries.
