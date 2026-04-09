# /lanbow-v3 — Lanbow V3 UI Design Generator (Dual Mode)

Generate high-fidelity UI that matches the Lanbow V3 design system. Supports two modes:
- **Figma mode**: Create designs in Figma via Vibma MCP
- **Frontend mode**: Generate production-ready HTML/CSS/React code

This is an evolution of the original Lanbow design with serif typography (Abhaya Libre + Newsreader), sharp corners (0px radius everywhere), Phosphor icon library, and a warm off-white background aesthetic.

---

## MODULAR FILE STRUCTURE

This skill is split into focused modules. **Load only the files relevant to the current task:**

| File | Contents | When to read |
|---|---|---|
| `tokens.md` | Colors, typography, spacing, shadows, radius | Always — foundational tokens |
| `layout.md` | Page structure, nav bar, grid, **responsive breakpoints** | When building pages/layouts |
| `components.md` | Card styles, KPI, notifications, page templates | When building UI components |
| `frontend.md` | CSS classes, React examples, noise texture, icons | Frontend mode only |
| `motion.md` | Microinteractions, animations, transitions | When adding motion/interaction |

**RULE: Read `tokens.md` for every task. Read other files only as needed for the specific request.**

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

**Framer Motion** (if adding interactions):
```bash
npm install framer-motion
```

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

### P3: Depth Through Layering — Noise Texture + Elevation
V3 adds a subtle noise texture over the background layer for tactile depth:
- Background rectangle: #F3F3F3 fill
- Effect: NOISE (color: rgba(0,0,0,0.16), visible: true)
- This sits behind all content, adding subtle grain

**V3 elevation system**: Minimal shadow use — depth is primarily communicated through background color layering (#FBF9F4 page → #FFFFFF card → #F8F6F2 sub-card). Shadows are reserved for floating layers only. See `tokens.md` for full shadow token definitions.

### P4: Consistent Spacing Grid
All spacing derives from a 4px base grid. See `tokens.md` for the full scale (2px–48px) and usage guide. Key values: tight 4px, compact 8px, item 12px, component 16px, section 24px, module 40px.

### P5: Gestalt Hierarchy (Size Contrast + Proximity)
**CRITICAL — apply on EVERY page, then review.**
- **Size Contrast**: Section titles MUST be visually distinct from body — at least 2px size difference AND weight difference.
- **Proximity**: Title → its content gap MUST be smaller than section → next section gap. This groups related content visually.
  - Title → content: 16–20px (tight)
  - Section → next section: 24–32px (loose)

### P6: Restrained Aesthetic
- No gradients (except login brand panel)
- No decorative illustrations in dashboard — use data and whitespace
- No colored backgrounds on cards — always White
- Borders over fills for secondary elements (tags, chips, secondary buttons)
- Left-align everything except: centered empty states, centered modal content, right-aligned numbers in tables

### P7: Button Color Discipline

| Type | Fill | Text | Stroke | Usage |
|---|---|---|---|---|
| Primary | Grey-01 | White | none | Main CTA (1 per visible area) |
| Secondary | transparent | Grey-01 | Grey-01 1px | Alternative actions |
| Text/Link | transparent | LANBOW-Cyan | none | Inline actions, "View More" |
| Destructive | Red | White | none | Delete confirmation (modals only) |
| Disabled | Grey-12 | Grey-08 | none | Unavailable actions |

### P8: Form Field Consistency
- Height: 40px
- **Corner radius: 0px** (V3 override — was 6px in V1)
- Stroke: Grey-12 1px (default) → Grey-01 (focus)
- Fill: White
- Label: bullet point (•) prefix + 14-Medium, Grey-01, 8px gap above input
- Placeholder: 14-Regular, Grey-08
- Error: 12-Regular, Red, replaces helper text
- Table row with images: min row height 80px (48px image + 16px padding top/bottom)

### P8.1: Card Bottom Padding — Visual Weight Rule
Bottom padding depends on visual weight of the last element, not layout properties:
- **Visually fills bottom** (full-width fields, textareas, dense lists) → bottom padding **40px**
- **Visually sparse bottom** (right-aligned totals, buttons, short text) → bottom padding **24px**

### P9: Whitespace Over Nested Borders
**Do NOT nest bordered containers inside cards.** Use whitespace + background-color differentiation instead.
- Tables inside cards: NO outer border — use Background fill on header + thin divider lines between rows
- Sub-sections: vertical spacing (24px+), NOT bordered sub-cards
- Selection groups: Background-filled cards (#F8F6F2 in V3) with 0px radius, NOT stroked cards
- Only allowed borders inside a card: input strokes, 1px divider lines (Grey-12), table row separators

### P10: Selection Indicators
**Single-select (radio-style)**: Circular check icon. Card fill: Selected token, 0px radius (V3). Selected card adds subtle Stroke border (1.5px).
**Multi-select (checkbox-style)**: Square 16×16 checkbox. Checked = Grey-01 fill + white checkmark; Unchecked = Grey-12 stroke + transparent fill.

**Grid Layout with WRAP**: Always set BOTH `itemSpacing` (horizontal) and `counterAxisSpacing` (vertical).

### P11: Data Value Contrast Rule
Value MUST be significantly larger and bolder than its label:
- Value font size ≥ 1.5× label size
- Value = Bold weight, Label = Medium/Regular
- Value = Grey-01, Label = Grey-08

| Context | Label | Value | Ratio |
|---|---|---|---|
| KPI hero | 12-Medium, Grey-08 | 32-SemiBold Newsreader, Grey-01 | 2.67× |
| KPI compact | 12-Medium, Grey-08 | 24-SemiBold Newsreader, Grey-01 | 2× |
| Data card | 12-Medium, Grey-08 | 20-Bold, Grey-01 | 1.67× |

Layout: VERTICAL (label top, value below, gap 2–4px). Avoid horizontal label-value for numeric data.

### P12: Icon Stroke Integrity
**NEVER resize icon components via width/height.** Use icons at native size (24×24 / 16×16). If no 16px variant exists, use 24×24 rather than a distorted shrink. Phosphor Regular icons in V3 use filled paths, so this primarily applies to custom SVG icons.

### P13: Card Internal Padding Standard
Strict tiers — no arbitrary values (e.g. 28px is BANNED):

| Type | Padding |
|---|---|
| Small (dropdowns, chips) | 12px |
| Standard (content cards) | 24px (V3 default) |
| Modal/Dialog | 24px |
| Large (magazine/hero) | 40px |

### P14: Title-Divider Optical Balance
When a card has Title → Divider → Content: spacing above title (paddingTop) MUST equal spacing below it to the divider line.

### P15: No Double Padding Nesting
When a child is the first/last inside a padded parent, the child MUST NOT add its own padding in the same direction. This prevents double-spacing.

### P16: Secondary Button Standard
**V3 override: cornerRadius = 0px** (was 8px in V1). All else same as V1:
- Fill: transparent, Stroke: Grey-01 1px, Text: 14-Medium Grey-01
- Primary and secondary buttons in the same row MUST have identical padding → identical height (standard V padding = 10px → 36px height)

### P17: Select / Filter Dropdown Standard
**V3 override: cornerRadius = 0px** (was 8px in V1).
- Fill: White, Stroke: Grey-12 1px, Text: 14-Regular Grey-01
- Chevron: 16×16 SVG, Grey-08, stroke-width 1.33px, right-aligned
- Padding: H 12px, V 10px (~36px height)

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
5. Build content sections following the card/typography patterns (see `components.md`)
6. Add decorative elements (Cyan accent lines, etc.)

### Step 3b: Build Each Page (Frontend Mode)
For each page/component:
1. Ensure CSS variables and Google Fonts are imported (see `frontend.md` for full CSS variable block)
2. Use semantic HTML structure matching the layout system (see `layout.md`)
3. Apply CSS variables for all colors/spacing — never hardcode
4. Use Phosphor Icons via React component or web font
5. Follow the component patterns in `frontend.md`

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
- [ ] Shadows only on floating layers (dropdown, popover, tooltip, modal overlay)
- [ ] Responsive: layout adapts at breakpoints if building full pages

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
| Shadow system | Undefined | Minimal — floating layers only |
| Responsive | Desktop only | 5 breakpoints, mobile-aware |
