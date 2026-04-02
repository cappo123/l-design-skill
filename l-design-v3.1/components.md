# Lanbow V3 — Component Patterns

Card styles, KPI displays, notifications, page templates, and button styles.

---

## COPY RULES

All UI copy in this component system follows three mandatory rules:

| Rule | Description |
|---|---|
| **No Emoji** | Never add emoji to any UI element — buttons, labels, titles, nav, notifications, placeholders |
| **English Only** | All copy in English regardless of user's language. Translate Chinese/other language input to English |
| **Text Length** | Title/label class ≤ 3 words. Content class truncates at container boundary with `…` |

**Title / Label class**: buttons, nav items, tags, card titles, section headers
**Content class**: descriptions, body text, table cells, notification body, helper text

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
