# /lanbow-design — Lanbow UI Design Generator

Generate high-fidelity Figma design pages that match the Lanbow design system. Reads PRD/requirements, asks clarifying questions when needed, then creates complete Figma screens using proper design tokens.

---

## SETUP: ALWAYS DO FIRST

```
mcp__Vibma__join_channel → mcp__Vibma__ping
```
If ping fails, tell user to start relay: `npx @ufira/vibma-tunnel`

---

## DESIGN TOKENS

### Color Variables (Collection: "Lanbow Colors", collectionId: VariableCollectionId:10337:7721, modeId: 10337:0)

| Token | Variable ID | Hex | Usage |
|---|---|---|---|
| LANBOW-Cyan | VariableID:10337:7723 | #00B1A2 | Brand primary, CTA, active states, links |
| Grey-01 | VariableID:10337:7724 | #181818 | Primary text, headings |
| Grey-06 | VariableID:10337:7725 | #626262 | Secondary text, captions |
| Grey-08 | VariableID:10337:7726 | #999999 | Placeholder text, disabled states |
| Grey-12 | VariableID:10337:7727 | #EEEEEE | Borders, dividers, subtle lines |
| Background | VariableID:10337:7728 | #F3F3F3 | **ONLY for large background areas**: Main Content grey bg rect, page-level background fills. NEVER for small UI elements. |
| White | VariableID:10337:7729 | #FFFFFF | Card backgrounds, surface |
| Selected | VariableID:10337:7730 | rgba(0,0,0,2%) | **Default for all subtle fills on small UI elements**: table header rows, option cards, image placeholders, textarea fills, hover states, tag backgrounds, section divider fills. Use this instead of Background for anything smaller than a full-page area. |
| Stroke | VariableID:10337:7731 | rgba(0,0,0,4%) | Card borders, container strokes |
| Red | VariableID:10337:7732 | #FF2D55 | Error, danger, delete |
| Orange | VariableID:10337:7733 | #FF9500 | Warning, attention |

**RULE: NEVER use hardcoded hex colors. Always use `fillVariableId` / `strokeVariableId` / `set_variable_binding`.**

### Radius Variables (Collection: "Lanbow Radius", collectionId: VariableCollectionId:10337:7722, modeId: 10337:1)

| Token | Variable ID | Value | Usage |
|---|---|---|---|
| radius-none | VariableID:10337:7734 | 0px | Sharp corners |
| radius-extra-small | VariableID:10337:7735 | 2px | Tags, badges |
| radius-small | VariableID:10337:7736 | 4px | Buttons small, chips |
| radius-medium | VariableID:10337:7737 | 6px | Inputs, small cards |
| radius-large | VariableID:10337:7738 | 8px | Buttons, dropdowns |
| radius-extra-large | VariableID:10337:7739 | 12px | Cards, modals |
| radius-2xl | VariableID:10337:7740 | 16px | Large cards, panels |
| radius-round | VariableID:10337:7741 | 9999px | Avatars, pills, circular buttons |
| radius-3xl | VariableID:10337:13593 | 24px | Main content container, hero cards |

**RULE: Bind radius via `set_variable_binding` with field `topLeftRadius`, `topRightRadius`, `bottomLeftRadius`, `bottomRightRadius`.**

### Text Styles (use `textStyleName` property)

| Style Name | Size | Weight | Usage |
|---|---|---|---|
| 32-Bold | 32px | Bold | Page titles, hero headings |
| 28-Bold | 28px | Bold | Section headings |
| 24-Bold | 24px | Bold | Card titles, dialog headings |
| 20-Bold | 20px | Bold | Subsection titles |
| 18-Bold | 18px | Bold | Prominent labels |
| 16-Bold | 16px | Bold | Emphasis text, button labels |
| 14-Bold | 14px | Bold | Table headers, strong labels |
| 12-Bold | 12px | Bold | Small labels, badges |
| 32-Medium | 32px | Medium | Large display text |
| 28-Medium | 28px | Medium | Sub-headings |
| 24-Medium | 24px | Medium | Feature text |
| 20-Medium | 20px | Medium | Body large |
| 18-Medium | 18px | Medium | Body emphasized |
| 16-Medium | 16px | Medium | Navigation, prominent body |
| 14-Medium | 14px | Medium | Default body text |
| 12-Medium | 12px | Medium | Small body, captions |
| 32-Regular | 32px | Regular | Display body |
| 24-Regular | 24px | Regular | Large body |
| 20-Regular | 20px | Regular | Body text |
| 18-Regular | 18px | Regular | Comfortable reading |
| 16-Regular | 16px | Regular | Standard body |
| 14-Regular | 14px | Regular | Secondary body |
| 12-Regular | 12px | Regular | Captions, meta |
| 10-Regular | 10px | Regular | Footnotes, tiny labels |

---

## LAYOUT SYSTEM

### Page Structure (Top Nav + Icon Sidebar)

```
┌──────────────────────────────────────────────────────┐
│  nav_bar_Lanbow (1440×50, componentId: 6443:11918)   │
│  Transparent — requires pure white underneath        │
├────┬─────────────────────────────────────────────────┤
│icon│  Grey bg area (Background fill, radius-3xl 24px)│
│side│                                                 │
│bar │  Content margins from grey bg edges:            │
│56px│    top: 40px                                    │
│    │    left: 80px                                   │
│comp│    right: 80px                                  │
│ID: │    bottom: 40px                                 │
│7841│                                                 │
│:   │  Content width = bg width − 160px              │
│3046│  (e.g., 1384 − 160 = 1224px)                   │
│4   │                                                 │
└────┴─────────────────────────────────────────────────┘
```

**CRITICAL: Content margin rule — 40/80/80/40**
- Title (24-Bold) sits at: grey bg top + 40px, grey bg left + 80px
- All page content shares the same 80px left/right margins
- This creates generous breathing room inside the grey bg

**Build order:**
1. Page frame: 1440×H, fill: pure **White** (VariableID:10337:7729)
   **Page height is NOT fixed at 900px** — if content requires more space, increase the canvas height (e.g., 1100px, 1400px) to ensure all content is fully visible. Never clip or compress content to fit 900px.
2. Body frame: HORIZONTAL, at (0, 50), layoutSizingHorizontal: FILL
3. `side_menu_hide` instance: componentId `7841:30464`, 56×height
4. Main Content frame: Background fill (VariableID:10337:7728), radius-3xl (24px), layoutSizingHorizontal: FILL
   - VERTICAL auto-layout
   - **padding: top 40, left 80, right 80, bottom 40**
   - itemSpacing: 24px
5. `nav_bar_Lanbow` instance: componentId `6443:11918`, at (0,0), 1440×50
6. Content modules inside Main Content, all using layoutSizingHorizontal: FILL

### Spacing Constants
- **Content margin from grey bg: 40px top/bottom, 80px left/right** (MOST IMPORTANT)
- Card internal padding: 20px (standard), 24px (large)
- Section gap: 24px (itemSpacing between cards/sections)
- Component gap: 16px
- Row/item gap: 12px
- Tight gap: 8px
- Icon-text gap: 8px
- Nav item height: 40px
- Table row height: 48px
- Input height: 40px
- Button height: 36px (default), 40px (large)
- Top nav bar height: 50px
- Icon sidebar width: 56px

### Grid System
- Main content width: grey bg width − 160px (80px × 2 margins)
- e.g., 1384 − 160 = 1224px usable content width
- Card grid: typically 2-col or 3-col, gap 16px-24px
- Stats cards: 4-col for KPI row
- Full-width: data tables, charts — use layoutSizingHorizontal: FILL

---

## DESIGN PRINCIPLES (Cross-Page Consistency Rules)

These principles ensure any NEW page — even without reusing existing components — looks consistent with the Lanbow system.

### P1: Color Ratio Rule (85 / 10 / 5)
- **~85% Greyscale**: White, Background, Grey-01 through Grey-12 form the entire base
- **~10% Brand Accent**: LANBOW-Cyan — ONLY for active states, links, positive trend indicators, and text highlights
- **~5% Semantic**: Red for errors/delete, Orange for warnings — never decorative
- **NEVER use Cyan as button fill** — buttons are Grey-01 fill + White text. Cyan is text/icon-only.

### P2: Typography Creates Hierarchy (Not Color)
Distinguish content levels through size + weight contrast, never through color variation:

| Level | Style | Token | Usage |
|---|---|---|---|
| Page title | 24-Bold | Grey-01 | Top of page, one per screen |
| Section title | 20-Bold | Grey-01 | Card headers, panel titles |
| Subsection | 16-Bold | Grey-01 | Table titles, widget labels |
| Label | 14-Medium | Grey-01 | Form labels, nav items, button text |
| Body | 14-Regular | Grey-01 | Default reading text, table cells |
| Secondary | 12-Regular | Grey-06 | Timestamps, helper text, captions |
| Placeholder | 14-Regular | Grey-08 | Input placeholders, disabled text |
| Tiny | 10-Regular | Grey-08 | Footnotes, legal text |

### P3: Depth Through Layering (Not Shadow)
- Create visual depth via Background (#F3F3F3) → White card stacking
- Stroke borders (`rgba(0,0,0,4%)`) for card edges — NOT drop shadows
- Exception: floating panels (sidebar drawer) may use subtle shadow for elevation
- Exception: tooltips use shadow for separation from chart content

### P4: Consistent Spacing Grid
All spacing derives from an 4px base grid, with these standard steps:

| Token | Value | Usage |
|---|---|---|
| tight | 4px | Label-to-input, icon internal padding |
| compact | 8px | Icon-text gap, tag padding, tight stacks |
| item | 12px | Items within a group, list rows |
| component | 16px | Between components, grid gap (small) |
| section | 24px | Between sections, card padding, page padding |
| module | 40px | Chart internal padding, major section breaks |
| flow | 80px | Between flow modules on detail pages |

### P5: Gestalt Hierarchy (Size Contrast + Proximity)
**CRITICAL — apply this principle on EVERY page, then review before declaring done.**

**Size Contrast Rule**: Section titles MUST be visually distinct from body text. If body text is 14px, titles must be ≥16-Bold (ideally 16-Bold or 18-Bold). A 14-Medium title next to 14-Regular body creates NO hierarchy — this is a common mistake. Always ensure ≥2px size difference AND weight difference between title and body.

**Proximity Rule**: The gap between a section title and its content MUST be **smaller** than the gap between two separate sections. This groups related content visually.
- Title → its content: 16-20px (tight, shows belonging)
- Section → next section: 24-32px (loose, shows separation)
- Example: "Basic Information" title → field grid = 20px gap; "Basic Information" card → "Product List" card = 24px gap

**Application**: After completing any page, scan ALL title-body pairs and verify:
1. Is the title visually larger/bolder than its body? (size contrast ✓)
2. Is the title closer to its own content than to the previous section? (proximity ✓)

### P6: Restrained Aesthetic
- **No gradients** anywhere except login page brand panel
- **No decorative illustrations** in dashboard — use data and whitespace
- **No colored backgrounds on cards** — always White
- **Borders over fills** for secondary elements (tags, chips, secondary buttons)
- **Left-align everything** except: centered empty states, centered modal content, right-aligned numbers in tables

### P7: Button Color Discipline

| Button Type | Fill | Text Color | Stroke | When to Use |
|---|---|---|---|---|
| Primary | Grey-01 | White | none | Main CTA per section (1 per visible area) |
| Secondary | White | Grey-01 | Grey-12 | Alternative actions alongside primary |
| Text/Link | transparent | LANBOW-Cyan | none | Inline actions, "View More", navigation |
| Destructive | Red | White | none | Delete confirmation ONLY (inside modals) |
| Disabled | Grey-12 | Grey-08 | none | Unavailable actions |

### P8: Form Field Consistency
Every input field across all pages follows:
- Height: 40px
- Corner radius: radius-medium (6px)
- Stroke: Grey-12, 1px (default) → Grey-01 (focus)
- Fill: White
- Label: bullet point (•) prefix + 14-Medium, Grey-01, gap 8px above input
- Placeholder: 14-Regular, Grey-08
- Helper text: 12-Regular, Grey-08, gap 4px below
- Error text: 12-Regular, Red, replaces helper text
- Dropdown: same as input + chevron-down icon (16×16) right-aligned

**Table row height rule**: Data rows containing images/thumbnails MUST have sufficient vertical padding — at least **16px above and below** the image. For a 48×48 image: row height = 48 + 16 + 16 = **80px minimum**. Text-only rows: 48px is sufficient.

### P8.1: Card Bottom Padding — Visual Weight Rule
**Card bottom padding is NOT always equal to top padding.** It depends on the **visual perception** of how "full" the bottom edge looks — NOT on the technical `layoutSizingHorizontal` property.

Judge by **visual appearance**: does the last element LOOK like it fills and packs the bottom edge?

- **Visually fills the bottom** (full-width form fields, textareas, selection card rows, dense full-width list items) → bottom padding = **40px**. The eye perceives a "wall" of content pressing against the card edge.
- **Visually does NOT fill the bottom** (right-aligned totals, buttons, short text, sparse single-line items) → bottom padding = **24px**. The surrounding whitespace already provides visual relief.

Key distinction: A table technically fills the card width, but if its **last row** is a right-aligned "Total: ¥7,093.00" with most of the row empty, it visually does NOT fill the bottom → use 24px.

Example:
```
Card ending with full-width textarea → paddingBottom: 40px (visually packed)
Card ending with full-width form row → paddingBottom: 40px (visually packed)
Card ending with right-aligned total → paddingBottom: 24px (visually sparse)
Card ending with selection card row → paddingBottom: 40px (visually packed)
Card ending with buttons → paddingBottom: 24px (visually sparse)
```

### P9: Whitespace Over Nested Borders
**CRITICAL — avoid nesting bordered containers inside cards.**

When content (tables, sub-sections, option groups) sits inside a white card, do NOT add a second stroke border around it. Instead, use whitespace and subtle background-color differentiation to create visual separation.

**Bad pattern** (nested borders):
```
Card (White, stroke border)
  └── Table (White, stroke border)    ← AVOID: card-inside-card
  └── Option group (White, stroke border)  ← AVOID
```

**Good pattern** (whitespace + background):
```
Card (White, shadow)
  └── Table header row (Background fill #F3F3F3, no border)
  └── Table data rows (White, bottom divider line only)
  └── Option cards (Background fill #F3F3F3, no stroke, radius-extra-large)
```

Rules:
- Tables inside cards: NO outer border on table frame. Use Background fill on header row + thin divider lines between data rows.
- Sub-sections inside cards: Use vertical spacing (24px+) between sections, NOT bordered sub-cards.
- Selection groups inside cards: Use Background-filled cards (F3F3F3) with radius, NOT White cards with stroke.
- The only borders allowed inside a card are: input field strokes (form controls), thin 1px divider lines (Grey-12), and table row separators.

### P10: Selection Indicators — Single-Select vs Multi-Select
**Lanbow uses two distinct indicator styles depending on the selection mode.**

#### Single-Select (Radio-style) — Circular Check Icon
Used in **card-style selection** where only ONE option can be chosen (payment method, data scope, plan tier).

**Card structure:**
```
Option Card (HORIZONTAL, FILL, SPACE_BETWEEN, center-aligned)
  fill: Selected (VariableID:10337:7730, rgba(0,0,0,2%))
  cornerRadius: radius-extra-large (12px)
  padding: 16px all sides
  ├── Label text: 14-Medium, Grey-01
  └── Check indicator (16×16 circular SVG, stroke-width 1.33px):
      Selected: filled circle (r=8), Grey-01 fill + white checkmark path (stroke-width 1.33, round cap)
      Unselected: empty circle (r=7), #CCCCCC stroke 1.33px, no fill
```

**Selected card state**: Adds subtle stroke border using Stroke token (VariableID:10337:7731, rgba(0,0,0,4%), weight 1.5px). Do NOT use Grey-01 for card stroke.

**Layout**: Cards in HORIZONTAL auto-layout row, FILL sizing, 16px gap.

**When to use**: Payment methods, shipping options, plan selection, data scope, channel selection — any discrete choice set with 2-5 options.

#### Multi-Select (Checkbox-style) — Square Checkbox Icon
Used in **tables, permission matrices, feature toggles** where MULTIPLE items can be selected independently.

**Component IDs:**
- **Checked**: `icon_select_sel` (componentId: 2419:3384) — 16×16, cornerRadius 4px, Grey-01 fill + white checkmark vector
- **Unchecked**: `icon_checkbox_def` (componentId: 3912:27340) — 16×16, cornerRadius 4px, semi-transparent white fill + Grey-12 stroke
- **Disabled**: Clone `icon_checkbox_def` + set opacity 0.35

**Usage**: Clone from component library. Source component (2419:3384) has been fixed to Grey-01 — future clones should inherit correctly. Always verify after cloning.
```
Checked:  clone_node { nodeId: "2419:3384", parentId: col_frame_id }
          → Verify inner Rectangle fill is Grey-01. If not, override: set_variable_binding fills/0/color → Grey-01 (VariableID:10337:7724)
Unchecked: clone_node { nodeId: "3912:27340", parentId: col_frame_id }
Disabled:  clone_node { nodeId: "3912:27340", parentId: col_frame_id } → patch opacity: 0.35
```

**🚫 #8366FF PERMANENTLY BANNED** — This purple/violet color is **permanently removed** from Lanbow. It must NEVER appear in any design. The source component `icon_select_sel` (2419:3384) has been patched to Grey-01 at the source. If purple ever reappears after cloning, immediately override to Grey-01. No exceptions.

**CRITICAL — Do NOT**:
- Resize these components (stroke integrity rule P12)
- Create custom COMPONENT/SVG nodes to approximate the look — always clone from source

**When to use**: Permission matrices, bulk selection tables, feature toggles, multi-select filter panels — any context where multiple independent boolean choices coexist.

#### Grid Layout with WRAP — Vertical Gap Rule
When using `layoutWrap: WRAP` for card grids, **both gaps must be set explicitly**:
- `itemSpacing` = horizontal gap between cards in same row
- `counterAxisSpacing` = vertical gap between rows (this is the commonly forgotten one)

These are **separate properties** — setting `itemSpacing: 16` does NOT set the vertical gap. Always set both:
```
patch_nodes: { layout: { itemSpacing: 16, counterAxisSpacing: 16 } }
```

### P11: Data Value Contrast Rule (Label vs Value Hierarchy)
**CRITICAL — applies to ALL metric/KPI displays, card data rows, and stat blocks.**

When displaying a label-value pair (e.g., "Spent" → "$ 4,350"), the value MUST be **significantly larger and bolder** than the label to create immediate visual hierarchy. A flat, same-size label + value is one of the most common mistakes in data-heavy UI.

**Minimum contrast requirements:**
- **Value font size ≥ 1.5× label font size** (e.g., label 12px → value ≥ 18px)
- **Value MUST use Bold weight**, label uses Medium or Regular
- **Value uses Grey-01**, label uses Grey-08 (not Grey-06 — push the contrast further)

**Standard pairings:**

| Context | Label Style | Value Style | Ratio |
|---|---|---|---|
| KPI card (hero) | 12-Medium, Grey-08 | 32-Bold, Grey-01 | 2.67× |
| KPI card (compact) | 12-Medium, Grey-08 | 24-Bold, Grey-01 | 2× |
| Project/data card | 12-Medium, Grey-08 | 20-Bold, Grey-01 | 1.67× |
| Table inline metric | 12-Medium, Grey-08 | 16-Bold, Grey-01 | 1.33× (minimum) |

**Layout**: Label + value should be VERTICAL (label on top, value below, gap 2-4px). This gives the value maximum visual prominence. Avoid HORIZONTAL label-value pairs for numeric data — they flatten the hierarchy.

**Anti-pattern (DO NOT):**
```
❌ "Spent" (14-Medium) + "$ 4,350" (14-Medium)  ← no contrast, unreadable
❌ "Spent" (12-Medium) + "$ 4,350" (14-Medium)  ← too similar, weak hierarchy
```

**Correct pattern:**
```
✅ "Spent" (12-Medium, Grey-08)
   "$ 4,350" (20-Bold, Grey-01)  ← clear hierarchy, scannable
```

### P12: Icon Stroke Integrity Rule
**NEVER resize icon components via width/height patching.** Figma's resize changes the frame bounds but does NOT scale stroke weights, causing icons to appear with disproportionately thick lines at smaller sizes.

**Correct approach:**
- Use icons at their **native 24×24 size** (2px stroke). This is suitable for card headers, action bars, and prominent UI.
- For truly small contexts (inline text, compact lists), use 16×16 icons but **only if the source component has 1.33px strokes** designed for that size. Do NOT shrink 24×24 icons.
- If no 16px variant exists, prefer 24×24 over a distorted 16×16.
- **Chevron-down for dropdowns/menus**: Use 16×16 SVG with 1.33px stroke, Grey-08 (#999999) color. NOT cloned from 24×24 icon library.

### P13: Card Internal Padding Standard
**Strict padding tiers — no arbitrary values (e.g. 28px is BANNED).**

| Component Type | Padding | Usage |
|---|---|---|
| Small (dropdown menus, chips) | 12px | Account menu dropdown, filter pills |
| Standard (content cards, form cards) | 20px | Recharge cards, data cards, table cards |
| Modal/Dialog overlays | 24px | Payment result modals, card update modal, billing modals |
| Large (magazine cards) | 40px | Campaign proposal cards, hero content |

### P14: Title-Divider Optical Balance
When a card/modal has **Title → Divider Line → Content** structure, the spacing above the title (paddingTop) MUST equal the spacing below it (itemSpacing to the Line). If card paddingTop = 20px, then itemSpacing must also be ≥ 20px for the title section, or reduce paddingTop to match itemSpacing.

**Anti-pattern**: paddingTop: 24px + itemSpacing: 16px → unbalanced (title floats toward top).
**Correct**: paddingTop: 16px + itemSpacing: 16px → balanced.

### P15: No Double Padding Nesting
When a child frame is the **first or last child** inside a parent with padding, the child MUST NOT have its own margin/padding in the same direction. This causes double spacing.

**Anti-pattern**: Card paddingTop: 20px → child paddingTop: 8px → total 28px (too much).
**Correct**: Card paddingTop: 20px → child paddingTop: 0px → clean 20px.

### P16: Secondary Button Standard (from Lanbow 2.0 library)
**Secondary buttons use transparent fill + Grey-01 stroke. NEVER use Grey-12 or light grey stroke.**

```
Secondary button:
  fill: NONE (transparent, visible: false)
  stroke: Grey-01 (VariableID:10337:7724), weight: 1px
  cornerRadius: radius-large (8px)
  padding: H 24px, V 10px (height 36px)
  text: 14-Medium, Grey-01

Primary button (for reference):
  fill: Grey-01 (VariableID:10337:7724)
  stroke: none
  cornerRadius: radius-large (8px)
  padding: H 24px, V 10px (height 36px)
  text: 14-Medium, White
```

**CRITICAL: Button Height Consistency Rule**
- Primary and secondary buttons in the **same row** MUST have identical padding → identical height.
- Standard V padding = **10px** (produces 36px height with 14-Medium text). Use this everywhere.
- NEVER mix V 8px and V 10px in the same button group — this causes visible height mismatch.
- After creating/modifying buttons, always **visually verify** that paired buttons are equal height.

### P17: Select / Filter Dropdown Standard
**Match Lanbow 2.0 Select component style. Do NOT freestyle filter UI.**

```
Select / Filter dropdown:
  fill: White (VariableID:10337:7729)
  stroke: Grey-12 (VariableID:10337:7727), weight: 1px
  cornerRadius: radius-large (8px)
  padding: H 12px, V 10px (height ~36px)
  text: 14-Regular, Grey-01
  chevron: 16×16 SVG, Grey-08 (#999999), stroke-width 1.33px
  itemSpacing: 6px (between text and chevron)
```

**Chevron SVG template** (use this exact SVG for all dropdowns/selects):
```svg
<svg width='16' height='16' viewBox='0 0 16 16' fill='none'>
  <path d='M4 6L8 10L12 6' stroke='#999999' stroke-width='1.33' stroke-linecap='round' stroke-linejoin='round'/>
</svg>
```

---

## PAGE TYPE TEMPLATES

### Type A: Dashboard Page (Nav + Icon Sidebar)
Used for: main app screens, data views, project lists, task tables
```
Page (1440×H, White fill — VariableID:10337:7729)
├── nav_bar_Lanbow (0,0) — 1440×50 (componentId: 6443:11918)
├── Body frame (0,50) — HORIZONTAL, 1440×(H-50)
│   ├── side_menu_hide — 56×(H-50) (componentId: 7841:30464)
│   └── Main Content — FILL width, Background fill (VariableID:10337:7728)
│       cornerRadius: radius-3xl (24px)
│       padding: top 40, left 80, right 80, bottom 40
│       layoutMode: VERTICAL, itemSpacing: 24px
│       ├── Page Header — HORIZONTAL, SPACE_BETWEEN, FILL width
│       │   ├── Left: title (24-Bold, Grey-01) + subtitle (14-Regular, Grey-06)
│       │   └── Right: action buttons (Export secondary + Primary CTA)
│       ├── Filter Bar — HORIZONTAL, gap 8px, FILL width
│       │   ├── Search input (FILL) + filter dropdowns (FIXED width)
│       │   └── Each filter: White fill, radius-large, Stroke border
│       │       with chevron-down icon (16×16, 1.33px stroke, Grey-08)
│       └── Table Card — Data Table pattern (see COMMON COMPONENTS)
│           layoutSizingHorizontal: FILL, layoutSizingVertical: HUG
```

### Type B: Settings Page (Text Sidebar + Content)
Used for: account settings, preferences, integrations
```
Page (1440×H, White fill)
├── nav_bar_Lanbow (0,0) — 1440×50
├── bg rect (56,50) — Background, radius-3xl
├── side_menu_hide (0,50) — 56px
└── Content area
    ├── Text nav sidebar (~200px, White bg)
    │   ├── Nav items: 14-Medium, Grey-06 (default), Grey-01 (active)
    │   └── Active indicator: left border or subtle bg tint
    └── Main content (~1100px)
        ├── Section title: 20-Bold, Grey-01
        └── Form fields / settings cards
```

### Type C: Auth/Login Page (Split Layout)
Used for: login, signup, password reset, verification
```
Page (1440×900)
├── Left panel (~40-50% width)
│   ├── Fill: near-black (#1A1A1A or brand dark)
│   ├── LANBOW logo: White, top-left area
│   └── Brand imagery: abstract/dark, decorative only
└── Right panel (~50-60% width)
    ├── Fill: White
    ├── Centered form container (~400px wide)
    │   ├── Title: 24-Bold, Grey-01
    │   ├── Social login buttons (if any): icon + text, White fill, Grey-12 stroke
    │   ├── Form fields (vertical stack, gap 24px)
    │   └── Primary CTA: full-width, Grey-01 fill, 40px height
    └── Footer links: 12-Regular, Grey-06
```

### Type D: Detail Page (Content + Right Floating Panel)
Used for: campaign details, proposal review, item editing, **order detail drawers, any right-side drawer/panel**.

**CRITICAL RULE: ALL right-side drawers/panels/sheets MUST use this floating panel style. NEVER use modal overlay (dark mask) + full-height drawer. This is a Lanbow design principle — drawers float within the grey bg area, not on top of the entire screen.**

```
Page (1440×H, layoutMode: NONE for absolute positioning)
├── Standard nav + Body (side_menu + Main Content)
├── Left content area: flow modules, cards, data
└── Right floating panel
    ├── Position: 4px inset from Main Content (grey bg) edges
    │   x = MainContent.right - 4 - panelWidth
    │   y = MainContent.top + 4
    │   height = MainContent.height - 8
    ├── Width: 400px (standard), flexible per content
    ├── White fill, cornerRadius: 20
    ├── Effects:
    │   - BACKGROUND_BLUR: radius 32
    │   - DROP_SHADOW: x=-8, y=0, blur=24, rgba(0,0,0,4%)
    ├── clipsContent: true
    ├── layoutMode: VERTICAL, itemSpacing: 0
    │
    ├── Title bar (sticky top)
    │   HORIZONTAL, SPACE_BETWEEN, padding 20/24
    │   Left: title text (20-Bold), Right: x-close icon (24×24)
    │   White fill background
    │
    ├── Scrollable content area (FILL height)
    │   VERTICAL, padding 20/24, itemSpacing 24
    │   Contains: info cards, mini tables, timeline, form fields
    │   **RULE: Content cards inside panel use NO stroke borders.
    │   Use spacing (itemSpacing 24) between cards for visual hierarchy,
    │   NOT borders. Cards have White fill, no stroke, no shadow.
    │   CRITICAL: When removing borders from a card, ALSO remove:
    │   - internal padding (set to 0) — parent already provides padding
    │   - cornerRadius (set to 0) — invisible radius wastes space
    │   Otherwise "ghost indentation" occurs (double padding).**
    │
    └── Footer bar (sticky bottom)
        HORIZONTAL, MAX aligned, padding 16/24
        White fill, top border (Stroke token, 1px)
        Button group: Ghost + Secondary + Primary
```

### Type E: Empty State
Used for: first-time views, no data, no assets
```
Centered within content area:
├── Icon: 48×48 or 64×64, Grey-08 (optional)
├── Gap: 16px
├── Title: 16-Bold or 20-Bold, Grey-01
├── Gap: 8px
├── Description: 14-Regular, Grey-06, max-width 400px, center-aligned
├── Gap: 24px
└── CTA button: primary style, centered
```

### Type F: Notification Panel
Used for: notification dropdown, message list
```
Panel (floating, right-aligned from nav)
├── White fill, radius-extra-large (12px), shadow
├── Header: "Notifications" title + "Mark all as Read" link
├── Notification items (vertical list, gap 0, separator between)
│   ├── Each item: padding 16px, HORIZONTAL
│   │   ├── Title: 14-Medium, Grey-01
│   │   ├── Timestamp: 12-Regular, Grey-08, right-aligned
│   │   └── Preview text: 14-Regular, Grey-06 (truncated 1 line)
│   └── Unread indicator: subtle Background tint on row
└── Footer: "View All" link, centered, 14-Medium, LANBOW-Cyan
```

---

## HIGH-FREQUENCY REUSABLE COMPONENTS

### Tab Navigation (Segmented Control)
```
Container: HORIZONTAL, Background fill, radius-large (8px), padding 4px
  ├── Active tab: White fill, Grey-12 stroke, radius-medium (6px)
  │   Icon (16×16, LANBOW-Cyan) + 14-Medium, Grey-01
  │   layoutSizingHorizontal: FILL
  └── Inactive tab: transparent, no stroke
      Icon (16×16, Grey-06) + 14-Regular, Grey-06
      layoutSizingHorizontal: FILL
```

### Action Bar (Filter + Buttons, top of card)
```
HORIZONTAL, SPACE_BETWEEN, full width
  ├── Left: filter controls (date picker, search, dropdowns)
  └── Right: action buttons (Export, Add New, etc.)
  Height: 40px per element, gap 12px between elements
```

### Breadcrumb / Status Indicator
```
HORIZONTAL, gap 8px, center-aligned
  ├── Green dot: 8×8 ellipse, LANBOW-Cyan fill
  └── Label: 14-Medium, Grey-01 (e.g., "Ad Strategy Formulation")
```

### Tag / Chip
```
frame: height 24-32px, padding H 8-12px, radius-round (9999px)
  Default: Background fill, 12-Medium Grey-01
  Active/Selected: Grey-01 fill, 12-Medium White
  Removable: + x-close icon (12×12) right side, gap 4px
```

### Dropdown Select
```
frame: height 40px, padding H 12px, radius-medium (6px)
  fill: White, stroke: Grey-12 (1px)
  HORIZONTAL, SPACE_BETWEEN, center-aligned
  ├── Value text: 14-Regular, Grey-01 (or Grey-08 if placeholder)
  └── chevron-down icon: 16×16, Grey-08
```

### Toggle Switch
```
Container: 40×20px, radius-round
  Off: Grey-12 fill, knob left
  On: LANBOW-Cyan fill, knob right
  Knob: 16×16 circle, White fill, centered vertically
```

### Avatar
```
circle: 32×32 (default), 40×40 (large), 24×24 (small)
  Fill: Background (placeholder) or image
  Radius: radius-round (9999px)
  Fallback: first initial, 14-Medium Grey-06, centered
```

### Progress / Status Dot
```
8×8 ellipse, radius-round
  Active/Success: LANBOW-Cyan
  Warning: Orange
  Error: Red
  Inactive: Grey-12
  Used before status labels with gap 8px
```

### Confirmation Dialog (Quick Pattern)
```
Modal: ~520px wide, White fill, radius-2xl (16px), padding 24px
  ├── Title: 20-Bold, Grey-01 + X close (24×24, right)
  ├── Gap: 8px
  ├── Description: 14-Regular, Grey-06
  ├── Gap: 24px
  └── Buttons: right-aligned, HORIZONTAL, gap 12px
      ├── Cancel: Secondary button (160×40)
      └── Confirm: Primary button (160×40)
```

---

## DESIGN LANGUAGE

### Aesthetic: Rational · Restrained · Nordic
- **Clean whitespace** — let content breathe, don't pack elements
- **Monochromatic base** — Grey-01 text on White/Background
- **Single accent** — LANBOW-Cyan for active states and links ONLY. Never as button fill.
- **No gradients** — flat fills only (exception: login brand panel)
- **Typography-led** — hierarchy through font weight/size, not color
- **Borders over shadows** — use Stroke variable for card borders instead of drop shadows

### Info-Flow Structure Pattern
Each page follows a top-down information hierarchy:
```
1. Page context header (breadcrumb/status + title + primary action)
2. Summary/KPI bar (key metrics at a glance)
3. Primary content area (main data/visualization)
4. Secondary details (related info, recent activity)
```
This mirrors how information flows in editorial/magazine layouts — from macro to micro.

---

## ICON LIBRARY

### Usage Rules (MANDATORY)
- **ALL icons MUST come from the Lanbow icon library** (component page, scopeNodeId: `"10:175"`). NEVER create custom shapes, draw paths, or use non-library icons.
- **How to use**: Clone the icon COMPONENT (not instance) via `clone_node`, then resize the clone. Do NOT clone instances — clone the component node directly (type: COMPONENT).
- **Search icons**: Use `mcp__Vibma__search_nodes` with `scopeNodeId: "10:175"` and `query: "icon-name"`. Pick the result with `type: "COMPONENT"`.
- **Default color**: Grey-01 (VariableID:10337:7724, #181818) — bind via `set_variable_binding` field `strokes/0/color` on the inner VECTOR node (NOT `fills/0/color` — icon fills are invisible, only strokes render)

### Size & Stroke Scaling

| Display Size | Source Size | Scale Factor | Stroke Weight | Usage |
|---|---|---|---|---|
| 24×24px | 24×24 | 1× | 2px (original) | Large UI, standalone icons, chart toolbar |
| 20×20px | 24×24 | 0.833× | ~1.67px | Prominent inline icons |
| 16×16px | 24×24 | 0.667× | ~1.33px | Default UI, buttons, table cells, nav items |
| 12×12px | 24×24 | 0.5× | 1px | Compact UI, badges, tags |

**MANDATORY IMPLEMENTATION STEPS** (do this for EVERY icon):
1. Clone the COMPONENT (not instance) via `clone_node` with target `parentId`
2. Resize the clone to target size (e.g., 16×16) via `patch_nodes`
3. Navigate to the inner vector: clone → child INSTANCE → child VECTOR (named "Icon")
4. **Set strokeWeight on the VECTOR node** via `patch_nodes` → `stroke: { weight: 1.33 }` (for 16×16)
5. **Set stroke color on the VECTOR node** via `set_variable_binding` → field `strokes/0/color`
   - Default: Grey-01 (VariableID:10337:7724)
   - On dark backgrounds (e.g., Grey-01 button): White (VariableID:10337:7729)
   - Placeholder/secondary context: Grey-08 (VariableID:10337:7726)
6. Use `insert_child` with `index: 0` to place icon before text labels

**CRITICAL**: Figma does NOT auto-scale strokeWeight when you resize a component clone. You MUST manually set it. Without step 4, icons at 16×16 will still have 2px strokes and look visually wrong (too thick).
- Not resizing after cloning (leaving at 24×24 when 16×16 is needed)

### Icon Categories (Line Icons, 24×24 source)

**Charts** — Data visualization icons
`bar-chart-01~12`, `candlestick-chart-01~02`, `chart-breakout-circle`, `chart-breakout-square`, `line-chart-down-01~03`, `line-chart-up-01~03`, `pie-chart-01~04`, `presentation-chart-01~03`, `treemap-01~02`, `trending-down-01~02`, `trending-up-01~02`

**Layout** — UI structure icons
`columns-01~03`, `grid-01~03`, `layout-alt-01~04`, `layout-bottom`, `layout-left`, `layout-right`, `layout-top`, `rows-01~03`, `sidebar`

**Security** — Auth & protection icons
`certificate-01~02`, `fingerprint-01~04`, `key-01~02`, `lock-01~04`, `lock-keyed-01~02`, `lock-unlocked-01~04`, `passport`, `shield-01~03`, `shield-dollar`, `shield-off`, `shield-plus`, `shield-tick`, `shield-zap`

**Education** — Learning icons
`award-01~05`, `backpack`, `book-closed`, `book-open-01~02`, `bookmark`, `certificate-01~02`, `graduation-hat-01~02`, `microscope`, `puzzle-piece-01~02`, `ruler`, `scale-01~03`

**Development** — Code & tech icons
`brackets`, `brackets-check`, `brackets-ellipses`, `brackets-minus`, `brackets-plus`, `brackets-slash`, `brackets-x`, `code-01~02`, `code-browser`, `code-circle-01~03`, `code-square-01~02`, `cpu-chip-01~02`, `database-01~03`, `git-branch-01~02`, `git-commit`, `git-merge`, `git-pull-request`, `terminal`, `variable`

**Arrows** — Navigation & direction icons
`arrow-circle-broken-down`, `arrow-circle-broken-left`, `arrow-circle-broken-right`, `arrow-circle-broken-up`, `arrow-circle-down`, `arrow-circle-left`, `arrow-circle-right`, `arrow-circle-up`, `arrow-down`, `arrow-down-left`, `arrow-down-right`, `arrow-left`, `arrow-narrow-down`, `arrow-narrow-left`, `arrow-narrow-right`, `arrow-narrow-up`, `arrow-right`, `arrow-up`, `arrow-up-left`, `arrow-up-right`, `chevron-down`, `chevron-left`, `chevron-right`, `chevron-up`, `chevrons-down`, `chevrons-left`, `chevrons-right`, `chevrons-up`, `corner-down-left`, `corner-down-right`, `corner-left-down`, `corner-left-up`, `corner-right-down`, `corner-right-up`, `corner-up-left`, `corner-up-right`, `expand-01~06`, `flip-backward`, `flip-forward`, `refresh-ccw-01~05`, `refresh-cw-01~05`, `reverse-left`, `reverse-right`, `switch-horizontal-01~02`, `switch-vertical-01~02`

**Finance & eCommerce** — Money & commerce icons
`bank`, `bitcoin`, `chart-bar-square`, `coins-01~04`, `coins-hand`, `coins-stacked-01~04`, `coins-swap-01~02`, `credit-card-01~02`, `credit-card-check`, `credit-card-down`, `credit-card-download`, `credit-card-edit`, `credit-card-lock`, `credit-card-minus`, `credit-card-plus`, `credit-card-refresh`, `credit-card-search`, `credit-card-shield`, `credit-card-up`, `credit-card-upload`, `credit-card-x`, `cryptocurrency-01~04`, `currency-bitcoin`, `currency-bitcoin-circle`, `currency-dollar`, `currency-dollar-circle`, `currency-ethereum`, `currency-ethereum-circle`, `currency-euro`, `currency-euro-circle`, `currency-pound`, `currency-ruble`, `currency-rupee`, `currency-yen`, `diamond-01~02`, `gift-01~02`, `piggy-bank-01~02`, `receipt`, `receipt-check`, `safe`, `sale-01~04`, `scales-01~02`, `shopping-bag-01~03`, `shopping-cart-01~03`, `tag-01~03`, `wallet-01~05`

**General** — Universal UI icons
`activity`, `activity-heart`, `anchor`, `archive`, `asterisk-01~02`, `at-sign`, `bookmark`, `bookmark-add`, `bookmark-check`, `bookmark-minus`, `bookmark-x`, `building-01~08`, `check`, `check-circle`, `check-circle-broken`, `check-done-01~02`, `check-heart`, `check-square`, `check-square-broken`, `check-verified-01~03`, `cloud-blank-01~02`, `copy-01~07`, `divide-01~03`, `dots-grid`, `dots-horizontal`, `dots-vertical`, `download-01~04`, `download-cloud-01~02`, `edit-01~05`, `equal`, `equal-not`, `eye`, `eye-off`, `filter-funnel-01~02`, `filter-lines`, `google-chrome`, `hash-01~02`, `heart`, `heart-circle`, `heart-hand`, `heart-hexagon`, `heart-octagon`, `heart-rounded`, `heart-square`, `hearts`, `help-circle`, `help-hexagon`, `help-octagon`, `help-square`, `home-01~05`, `home-line`, `home-smile`, `info-circle`, `info-hexagon`, `info-octagon`, `info-square`, `life-buoy-01~02`, `link-01~05`, `link-broken-01~02`, `link-external-01~02`, `loading-01~03`, `log-in-01~04`, `log-out-01~04`, `medical-circle`, `medical-cross`, `medical-square`, `menu-01~05`, `minus`, `minus-circle`, `minus-square`, `percent-01~03`, `pin-01~02`, `placeholder`, `plus`, `plus-circle`, `plus-square`, `save-01~03`, `search-lg`, `search-md`, `search-refraction`, `search-sm`, `settings-01~04`, `share-01~07`, `slash-circle-01~02`, `slash-divider`, `slash-octagon`, `speedometer-01~04`, `target-01~05`, `toggle-01-left`, `toggle-01-right`, `toggle-02-left`, `toggle-02-right`, `toggle-03-left`, `toggle-03-right`, `tool-01~02`, `translate-01~02`, `trash-01~04`, `upload-01~04`, `upload-cloud-01~02`, `virus`, `x`, `x-circle`, `x-close`, `x-square`, `zap`, `zap-circle`, `zap-fast`, `zap-off`, `zap-square`

**Alerts & feedback** — Status icons
`alert-circle`, `alert-hexagon`, `alert-octagon`, `alert-square`, `alert-triangle`, `bell-01~04`, `bell-minus`, `bell-off-01~03`, `bell-plus`, `bell-ringing-01~04`, `notification-box`, `notification-message`

**Shapes** — Geometric icons
`circle`, `cube-01~04`, `cube-outline`, `dice-1~6`, `hexagon-01~02`, `octagon`, `pentagon`, `square`, `star-01~07`, `triangle`

**Users** — People icons
`face-content`, `face-frown`, `face-happy`, `face-neutral`, `face-sad`, `face-smile`, `face-wink`, `user-01~03`, `user-check-01~02`, `user-circle`, `user-down-01~02`, `user-edit`, `user-left-01~02`, `user-minus-01~02`, `user-plus-01~02`, `user-right-01~02`, `user-square`, `user-up-01~02`, `user-x-01~02`, `users-01~03`, `users-check`, `users-down`, `users-edit`, `users-left`, `users-minus`, `users-plus`, `users-right`, `users-up`, `users-x`

**Communication** — Messaging & contact icons
`annotation`, `annotation-alert`, `annotation-check`, `annotation-dots`, `annotation-heart`, `annotation-info`, `annotation-plus`, `annotation-question`, `annotation-x`, `inbox-01~02`, `mail-01~05`, `message-alert-circle`, `message-alert-square`, `message-chat-circle`, `message-chat-square`, `message-check-circle`, `message-check-square`, `message-circle-01~02`, `message-dots-circle`, `message-dots-square`, `message-heart-circle`, `message-heart-square`, `message-notification-circle`, `message-notification-square`, `message-plus-circle`, `message-plus-square`, `message-question-circle`, `message-question-square`, `message-smile-circle`, `message-smile-square`, `message-square-01~02`, `message-text-circle-01~02`, `message-text-square-01~02`, `message-x-circle`, `message-x-square`, `phone`, `phone-call-01~02`, `phone-hang-up`, `phone-incoming-01~02`, `phone-outgoing-01~02`, `phone-pause`, `phone-plus`, `phone-x`, `send-01~03`

**Media & devices** — Device & media icons
`airplay`, `airpods`, `battery-charging-01~02`, `battery-empty`, `battery-full`, `battery-low`, `battery-mid`, `bluetooth-connect`, `bluetooth-off`, `bluetooth-on`, `bluetooth-signal`, `chrome-cast`, `clapperboard`, `disc-01~02`, `fast-backward`, `fast-forward`, `film-01~03`, `gaming-pad-01~02`, `hard-drive`, `headphones-01~02`, `keyboard-01~02`, `laptop-01~02`, `lightbulb-01~05`, `microphone-01~02`, `microphone-off-01~02`, `modem-01~02`, `monitor-01~05`, `mouse`, `music-note-01~02`, `music-note-plus`, `pause-circle`, `pause-square`, `phone-01~02`, `play`, `play-circle`, `play-square`, `podcast`, `power-01~03`, `printer`, `recording-01~03`, `repeat-01~04`, `rss-01~02`, `shuffle-01~02`, `signal-01~03`, `simcard`, `skip-back`, `skip-forward`, `sliders-01~04`, `speaker-01~03`, `stop`, `stop-circle`, `stop-square`, `tablet-01~02`, `tv-01~03`, `usb-flash-drive`, `video-recorder`, `video-recorder-off`, `voicemail`, `volume-max`, `volume-min`, `volume-minus`, `volume-plus`, `volume-x`, `webcam-01~02`, `wifi`, `wifi-off`, `youtube`

**Images** — Photo & image icons
`camera-01~03`, `camera-lens`, `camera-off`, `camera-plus`, `colors`, `flash`, `flash-off`, `image-01~05`, `image-check`, `image-down`, `image-left`, `image-plus`, `image-right`, `image-up`, `image-user`, `image-user-check`, `image-user-down`, `image-user-left`, `image-user-plus`, `image-user-right`, `image-user-up`, `image-user-x`, `image-x`

**Editor** — Text editing icons
`bold-01~02`, `code-snippet-01~02`, `cursor-01~04`, `cursor-box`, `cursor-click-01~02`, `eraser`, `italic-01~02`, `left-indent-01~02`, `letter-spacing-01~02`, `line-height`, `paragraph-spacing`, `paragraph-wrap`, `right-indent-01~02`, `strikethrough-01~02`, `text`, `text-input`, `type-01~02`, `type-strikethrough-01~02`, `underline-01~02`

**Time** — Clock & calendar icons
`alarm-clock`, `alarm-clock-check`, `alarm-clock-minus`, `alarm-clock-off`, `alarm-clock-plus`, `calendar`, `calendar-check-01~02`, `calendar-date`, `calendar-heart-01~02`, `calendar-minus-01~02`, `calendar-plus-01~02`, `clock`, `clock-check`, `clock-fast-forward`, `clock-plus`, `clock-refresh`, `clock-rewind`, `clock-snooze`, `clock-stopwatch`, `hourglass-01~03`, `watch-circle`, `watch-square`

**Files** — Document & folder icons
`box`, `clipboard`, `clipboard-attachment`, `clipboard-check`, `clipboard-download`, `clipboard-minus`, `clipboard-plus`, `clipboard-x`, `file-01~07`, `file-attachment-01~05`, `file-check-01~03`, `file-download-01~03`, `file-heart-01~03`, `file-minus-01~03`, `file-plus-01~03`, `file-question-01~03`, `file-search-01~03`, `file-x-01~03`, `folder`, `folder-check`, `folder-closed`, `folder-download`, `folder-lock`, `folder-minus`, `folder-plus`, `folder-question`, `folder-search`, `folder-x`, `paperclip`, `sticker-circle`, `sticker-square`

**Maps & travel** — Location & transport icons
`bus`, `car-01~02`, `compass-01~03`, `flag-01~06`, `globe-01~06`, `luggage-01~03`, `map-01~02`, `mark`, `marker-pin-01~06`, `navigation-pointer-01~02`, `navigation-pointer-off-01~02`, `passport`, `plane`, `rocket-01~02`, `route`, `ticket-01~02`, `train`, `tram`, `truck-01~02`

**Weather** — Weather condition icons
`cloud-01~03`, `cloud-lightning`, `cloud-moon`, `cloud-off`, `cloud-rain-01~03`, `cloud-raining-06`, `cloud-snow-01~03`, `cloud-sun-01~03`, `cloud-wind`, `droplets-01~03`, `hurricane-01~02`, `lightning-01~02`, `moon-01~02`, `moon-eclipse`, `moonstar`, `snowflake-01~02`, `stars-01~02`, `sun`, `sun-rising-01~02`, `sun-setting-01~02`, `sunrise`, `sunset`, `thermometer-01~03`, `thermometer-cold`, `thermometer-warm`, `tornado-01~02`, `waves`, `wind`, `wind-02`, `zap`

**Social** — Social platform icons (Original + Black styles)
Facebook, Instagram, X/Twitter, TikTok, Threads, YouTube, YouTube Shorts, WhatsApp, Telegram, Snapchat, Messenger, Discord, Reddit, GitHub, Figma, Notion, Slack, Spotify, Apple Music, Apple Podcasts, Google, Google Play, LinkedIn, Medium, Dribbble, Behance, Pinterest, VK, WeChat, Zoom, Skype, and more

---

## EXECUTION PROTOCOL

### Step 1: Understand Requirements
When user provides a PRD or feature description, ask clarifying questions if any of these are unclear:
- **How many screens?** (full multi-page flow vs single page vs partial update)
- **User role?** (admin, regular user, viewer — affects navigation and permissions shown)
- **Data context?** (what data is being displayed — affects table columns, chart types)
- **Primary action?** (what is the #1 thing user does on each screen)

### Step 2: Plan Before Creating
State the plan explicitly:
```
I'll create:
1. [Page name] — [purpose]
2. [Page name] — [purpose]
...
Then connect with flow arrows to show user journey.
```

### Step 3: Generate Each Page

**Page structure template:**
```
create_frame: page-level frame (1440×900, fillVariableId: Background)
  └── sidebar frame (256px wide, white fill, stroke border right)
       └── nav items (40px height each)
  └── main content frame (fills remaining, padding 24px, vertical layout)
       └── page header (title + breadcrumb + actions)
       └── content area (cards, tables, etc.)
```

**Card template:**
```
create_frame: card (width fills parent, cornerRadius: bind radius-3xl)
  fill: White variable
  stroke: Stroke variable (rgba 0,0,0,4%)
  padding: 20px
  vertical layout, gap: 16px
```

**Always bind tokens after creating nodes:**
```javascript
// Color fill
set_variable_binding: { nodeId, field: "fills/0/color", variableId: "VariableID:10337:7729" }
// Stroke
set_variable_binding: { nodeId, field: "strokes/0/color", variableId: "VariableID:10337:7731" }
// Corner radius
set_variable_binding: { nodeId, field: "topLeftRadius", variableId: "VariableID:10337:13593" }
// (repeat for all 4 corners)
```

### Step 4: Add Flow Connectors (multi-page only)
After creating all pages, connect them with Figma connector arrows to show navigation flow:
- Use `patch_nodes` to add connector lines between related frames
- Label each connector with the trigger action (e.g., "click Save", "submit form")

### Step 5: Regenerate on Change Request
When user requests changes: **NEVER patch individual nodes for structural changes**. Instead:
1. Delete the affected page/section
2. Recreate from scratch with the new requirements
3. Re-bind all tokens

Only use `patch_nodes` for minor text/color tweaks.

---

## COMMON COMPONENT PATTERNS

### Primary Button
```
frame: height 36px, padding H 16px, radius-large, fillVariableId: Grey-01 (#181818)
  text: "Label", textStyleName: "14-Medium", fontColorVariableId: White
  icon (if present): 16×16, White stroke — MUST set on inner VECTOR node
```
NOTE: Primary buttons use Grey-01 (near-black), NOT Cyan. Cyan is reserved for active states and links only.

### Secondary Button
```
frame: height 36px, padding H 16px, radius-large
  stroke: Grey-12, fill: White
  text: "Label", textStyleName: "14-Bold", fontColorVariableId: Grey-01
```

### Input Field
```
frame: height 40px, padding H 12px, radius-medium
  stroke: Grey-12 (default), fill: White
  placeholder text: 14-Regular, Grey-08
  label above: 12-Bold, Grey-01, margin-bottom 4px
```

### Data Table (Unified Card Pattern — matches Daily Report)

**CRITICAL: The table is a SINGLE white card that contains EVERYTHING — page header, filters, separator, table header, and data rows. NOT separate elements.**

```
Table Card container (THE ONLY card on the page):
  fill: White (VariableID:10337:7729)
  cornerRadius: radius-2xl (16px, VariableID:10337:7740) — bind all 4 corners
  clipsContent: true — clips internal elements to card radius
  stroke: none, shadow: none (clean flat card)
  layoutMode: VERTICAL, itemSpacing: 0
  paddingLeft: 24, paddingRight: 24, paddingTop: 0, paddingBottom: 0
  layoutSizingHorizontal: FILL, layoutSizingVertical: HUG
  **CRITICAL: L/R padding on the CARD level (not per-section) ensures ALL children — including row fills, zebra stripes, and separator lines — are indented consistently.**

  ├── Page Header (inside card, NOT outside)
  │   HORIZONTAL, SPACE_BETWEEN, layoutSizingHorizontal: FILL
  │   padding: top 24, left 0, right 0, bottom 16 (L/R from card)
  │   ├── Left: title (24-Bold, Grey-01) + subtitle (14-Regular, Grey-06)
  │   └── Right: action buttons
  │       ├── Export: Grey-01 fill, radius-large, download-01 icon (16×16 White) + 14-Medium White
  │       └── Primary CTA: Grey-01 fill, radius-large, icon (16×16 White) + 14-Medium White
  │
  ├── Filter Bar (inside card)
  │   HORIZONTAL, gap 8, layoutSizingHorizontal: FILL
  │   padding: left 0, right 0, bottom 0 (L/R from card)
  │   ├── Search input (FILL): White fill, Stroke border, radius-large
  │   │   search-sm icon (16×16, Grey-08) + placeholder 14-Regular Grey-08
  │   └── Filter dropdowns (FIXED ~140px each): White fill, Stroke border, radius-large
  │       SPACE_BETWEEN: label (14-Regular Grey-08) + chevron-down icon (16×16 Grey-08)
  │
  ├── Separator: Line component (componentId: 8:76) — see LINE SEPARATOR RULES below
  │
  ├── Table Header row:
  │   height: 40px, layoutSizingHorizontal: FILL
  │   fill: Selected (VariableID:10337:7730)
  │   cornerRadius: 0 on ALL corners — rely on card's clipsContent: true for clipping
  │   paddingLeft: 16, paddingRight: 16 (internal row padding for cell content breathing room)
  │   cells: 12-Regular, Grey-06 (VariableID:10337:7725)
  │   counterAxisAlignItems: CENTER
  │   First column (name): layoutSizingHorizontal FILL — absorbs remaining width
  │
  ├── Data rows (alternating with Line separators):
  │   height: 64px, layoutSizingHorizontal: FILL
  │   Odd rows: White fill (VariableID:10337:7729)
  │   Even rows: rgba(0,0,0,0.02) fill — subtle zebra stripe
  │   NO stroke on rows — use Line separators between rows instead
  │   paddingLeft: 16, paddingRight: 16, counterAxisAlignItems: CENTER
  │   First column (name): layoutSizingHorizontal FILL
  │   Name text: 14-Medium Grey-01
  │   Other text: 14-Regular Grey-01 (or Grey-06 for secondary)
  │
  ├── Line separators between rows:
  │   Line component (componentId: 8:76) — see LINE SEPARATOR RULES below
  │
  └── Bottom Spacer: empty frame, height 80px, FILL width
      Provides breathing room between last data row and card bottom edge

Status badges:
  radius-round (9999px), height 24px, padding H 10px
  "In Progress": LANBOW-Cyan fill at 10% opacity, text LANBOW-Cyan, 12-Medium
  "Completed"/"Pending": Grey-12 fill, text Grey-01, 12-Medium
  "Overdue": Red fill at 10% opacity, text Red, 12-Medium

Priority text:
  "High": Red (VariableID:10337:7732)
  "Medium"/"Low": Grey-06 (VariableID:10337:7725)
```

**LINE SEPARATOR RULES (CRITICAL — prevents invisible separators):**
- Clone Line component (componentId: 8:76) via `clone_node`
- Set the **parent clone's opacity to 1.0** (NOT 0.1) — Figma defaults clone opacity to 1.0 but double-check
- Navigate into the clone → find the inner Line/VECTOR node
- Set inner stroke color via `set_variable_binding` field `strokes/0/color` → Stroke token (VariableID:10337:7731, rgba 0,0,0,4%)
- **NEVER hardcode stroke color** (e.g., rgba 0,0,0,0.1) — always bind the Stroke variable token
- **NEVER set opacity on both parent AND inner stroke** — this creates double opacity reduction (e.g., 10% × 10% = 1% effective opacity → invisible)
- Set `layoutSizingHorizontal: FILL` on the separator so it stretches to card width
- **Indent lines to match row content**: Card-level paddingLeft/Right: 24 handles indentation. Set parent clone to `layoutMode: HORIZONTAL`, `paddingLeft: 0`, `paddingRight: 0`. Set inner LINE child to `layoutSizingHorizontal: FILL`. The card padding indents the separator automatically.
- **NO separator between filter bar and table header** — the table header directly follows the filter bar

**ZEBRA STRIPE RULES:**
- Odd rows (1, 3, 5...): White fill or transparent
- Even rows (2, 4, 6...): `rgba(0,0,0,0.02)` fill (2% opacity black) — very subtle alternating stripe
- Do NOT use the Selected variable for zebra (it's for hover/active states)

**TABLE HEADER CORNER RADIUS RULE:**
- Table header cornerRadius MUST be 0 on all 4 corners
- The parent card has `clipsContent: true` which clips the header to the card's rounded shape
- If you set cornerRadius on the header itself, it creates VISIBLE rounded corners inside the card (wrong)

**IMPORTANT LAYOUT NOTES:**
- Card has paddingLeft: 24, paddingRight: 24 — this indents ALL children (rows, headers, zebra fills, separators) uniformly
- Individual sections (header, filter) use paddingLeft/Right: 0 (card handles the L/R margin)
- Table header and data rows use paddingLeft: 16, paddingRight: 16 for internal cell breathing room
- Total effective indent: 24 (card) + 16 (row) = 40px from card edge to first text
- Line separators: paddingLeft/Right: 0 on parent (card padding handles indentation), inner LINE set to FILL
- First data column MUST be `layoutSizingHorizontal: FILL` to absorb variable width
- Other columns use FIXED widths (80-120px depending on content)
- After inserting rows, explicitly set `width` + `layoutSizingHorizontal: FILL` on each row — Figma may not auto-apply FILL
- Always add an 80px bottom spacer frame as the last child of the card (breathing room below last row)

### KPI Card
```
card (1/4 width): padding 20px, radius-extra-large, White
  metric value: 32-Bold, Grey-01
  label: 12-Medium, Grey-06
  trend indicator: 12-Medium + arrow icon, Cyan (up) or Red (down)
```

### Navigation Item (active)
```
frame: width fills sidebar, height 40px, padding L 12px, radius-large
  fill: Selected (rgba 0,0,0,2%) or Cyan tint
  icon: 16×16, LANBOW-Cyan
  label: 14-Medium, LANBOW-Cyan
```

### Navigation Item (default)
```
frame: same structure
  fill: transparent
  icon: 16×16, Grey-06
  label: 14-Medium, Grey-06
```

### Status Badge
```
frame: height 24px, padding H 8px, radius-round
  Success: fill green-tint, text 12-Medium Green
  Error: fill red-tint, text 12-Medium Red
  Warning: fill orange-tint, text 12-Medium Orange
  Info: fill cyan-tint, text 12-Medium LANBOW-Cyan
```

---

## LINE CHART PATTERN

Reference: `折线图` / `chart` in Figma (Lanbow v2.0 page)

### Overall Container
- **Background**: White (`VariableID:10337:7729`), cornerRadius 16px → bind `radius-2xl` (`VariableID:10337:7740`)
- **Shadow**: off by default (disabled drop-shadow)
- **Typical size**: 1072×720px (full-width within card), adjust width to content area

### Layout Structure (top → bottom)
```
chart container (White, radius-2xl, no padding — content uses absolute offsets)
  ├── Title row: y=32, x=40
  │   ├── Active tab: 20-Bold, Grey-01 (e.g., "Data Performance")
  │   └── Inactive tab: 20-Bold, Grey-08 (e.g., "Ad Sets"), gap=20px
  │
  ├── Filter (top-right): y=28, right-aligned x=(container-width − 40)
  │   ├── filter-funnel-01 icon (16×16, Grey-01)
  │   ├── Label: 16-Medium, Grey-01 (e.g., "Last 15 days")
  │   └── chevron-right icon (16×16, Grey-01)
  │   HORIZONTAL layout, gap=8px
  │
  ├── Metric selector row: y=112, x=40
  │   HORIZONTAL layout, gap=8px
  │   Each metric card: see below
  │
  ├── Chart area: y=236, x=40, fills remaining height − 40px bottom padding
  │   Contains: grid lines, axes, data curve, hover indicator, tooltip
  │
  └── Bottom padding: 40px
```

### Metric Selector Cards
Each card in the horizontal row above the chart:

| State | Size | Fill | Corner Radius | Label Style | Value Style |
|---|---|---|---|---|---|
| **Active** | 160×56px | Grey-01 (`VariableID:10337:7724`) | radius-large (8px) | 14-Medium, White | 20-Bold, White |
| **Inactive** | 160×56px | Selected (`VariableID:10337:7730`, rgba 0,0,0,2%) | radius-large (8px) | 14-Medium, Grey-08 | 20-Bold, Grey-01 |

- Vertical padding: 8px top/bottom
- Content center-aligned horizontally within card
- Currency prefix (if needed): 16-Bold, same color as value
- Trend indicator (e.g., "+19%"): 14-Medium, LANBOW-Cyan (positive) or Red (negative), right of label

### Chart Grid
- **Horizontal grid lines**: 5 lines, evenly spaced across chart height (every ~80px for 400px chart area)
  - Stroke: `rgba(0,0,0,0.1)` — 10% opacity black, 1px weight
  - Closest token: use hardcoded `rgba(0,0,0,0.1)` (no exact variable match; Grey-12 is too opaque)
- **No vertical grid lines** — clean horizontal-only grid

### Axes
- **Y-axis labels** (left edge): 12-Regular, Grey-06 (`VariableID:10337:7725`), left-aligned
  - Note: Figma design uses `#5b5b5b` which maps to Grey-06 (`#626262`)
- **X-axis labels** (bottom): 12-Regular, Grey-06, center-aligned
  - **Active date** (hover): 12-Medium, Grey-01 — highlights currently hovered data point
- **No axis lines** — the grid lines serve as visual reference instead

### Data Line (Curve)
- **Stroke color**: LANBOW-Cyan (`VariableID:10337:7723`, #00B1A2)
- **Stroke weight**: 2px
- **Curve type**: Smooth bezier (not straight segments)
- **Area fill** beneath curve: LANBOW-Cyan at ~10% opacity, gradient fading to transparent at bottom
  - Creates a subtle "filled area" effect below the line

### Hover Interaction State
- **Vertical crosshair**: 1px width, Grey-01 (#272727), opacity 60%
  - Full height of chart area (from top grid line to bottom)
- **Data point dot**: 6×6px ellipse
  - Fill: LANBOW-Cyan (`VariableID:10337:7723`)
  - Stroke: White (`VariableID:10337:7729`), 1px
  - Shadow: `drop-shadow(0 16px 4px rgba(131,102,255,0.08))`

### Tooltip
- **Container**: White fill, cornerRadius 4px → `radius-small` (`VariableID:10337:7736`)
- **Shadow**: `drop-shadow(0 0 20px rgba(71,69,86,0.1))`
- **Padding**: 8px horizontal, 4px vertical
- **Content**: Metric label + value (stacked vertically, gap 8px)
- **Arrow**: Small triangle pointing down, attached below tooltip body
- **Position**: Centered above the data point dot

### Token Mapping (hardcoded → Lanbow token)

| Figma Hardcoded | Closest Lanbow Token | Variable ID |
|---|---|---|
| #272727 | Grey-01 | VariableID:10337:7724 |
| #999999 | Grey-08 | VariableID:10337:7726 |
| #5b5b5b | Grey-06 | VariableID:10337:7725 |
| #00b1a2 | LANBOW-Cyan | VariableID:10337:7723 |
| rgba(0,0,0,0.04) | Selected | VariableID:10337:7730 |
| #ffffff | White | VariableID:10337:7729 |
| cornerRadius 16 | radius-2xl | VariableID:10337:7740 |
| cornerRadius 8 | radius-large | VariableID:10337:7738 |
| cornerRadius 4 | radius-small | VariableID:10337:7736 |

---

## REPORT TABLE PATTERN

Reference: `Reports` / `Daily Report` in Figma (Lanbow v2.0 page)

### Overall Container
- **Background**: White (`VariableID:10337:7729`)
- **Corner radius**: radius-2xl (`VariableID:10337:7740`, 16px)
- **No stroke, no shadow** — clean flat card

### Layout Structure (top → bottom)
```
report container (White, radius-2xl)
  ├── Header area: padding 24px
  │   ├── Left: title group (VERTICAL, gap 0)
  │   │   ├── Title: 24-Bold, Grey-01
  │   │   └── Subtitle: 12-Regular, Grey-08 (inline, baseline-aligned with title)
  │   └── Right: action group (HORIZONTAL, gap 12px, constraint MAX)
  │       ├── Date filter
  │       └── Export button
  │
  ├── Separator: Line component, full width
  │
  ├── Table header row (height 40px)
  │
  ├── Summary row: "Total" (height 64px)
  ├── YoY row: "Daily YoY" (height 64px)
  │
  └── Data rows (height 64px each, alternating bg)
```

### Header Elements

**Date Range Filter:**
- Fill: Selected (`VariableID:10337:7730`, rgba 0,0,0,2%)
- Corner radius: radius-large (`VariableID:10337:7738`, 8px)
- Padding: 12px horizontal, 4px vertical
- Layout: HORIZONTAL, gap 4px
- Icon: `calendar` from library (16×16, Grey-01)
- Text: 16-Medium, Grey-01

**Export Button:**
- Fill: Grey-01 (`VariableID:10337:7724`)
- Corner radius: radius-large (`VariableID:10337:7738`, 8px)
- Padding: 20px horizontal, vertically centered
- Layout: HORIZONTAL, gap 4px
- Icon: `download-01` from library (16×16, White)
- Text: 16-Medium, White (`VariableID:10337:7729`)

### Separator
- Use `Line` component (componentId: `8:76`), full width
- Opacity: 10% — set node opacity to 0.1

### Table Header Row
- Height: 40px
- Background: Selected (`VariableID:10337:7730`, rgba 0,0,0,2%)
- Column headers: 12-Regular, Grey-06 (`VariableID:10337:7725`)
- First column ("Day"): left-aligned
- Other columns: center-aligned
- Left content offset: 16px

### Table Rows

**Row dimensions:**
- Height: 64px per row
- Content vertically centered within row

**Row background alternating:**

| Row Type | Fill |
|---|---|
| Odd rows | Transparent (no fill) |
| Even rows | Selected at 50% (`rgba(0,0,0,1%)`) — very subtle alternating stripe |

**Row separator:**
- Line component at bottom of each row, opacity 10%
- Full width

**First column (Date/Label cell):**
- Width: ~100px, left-aligned, 16px left padding
- VERTICAL layout, gap 0
- Primary: 12-Regular, Grey-01 (e.g., "10-08", "Total")
- Secondary (optional tag): 12-Regular, Grey-08 (e.g., "T-1", "T-2")

**Data cells:**
- Text: 16-Regular, Grey-01 (`VariableID:10337:7724`)
- Currency values: right-aligned
- Numeric values: center-aligned
- Last column: right-aligned
- Empty values: show "--"

### Special Row: Daily YoY (Year-over-Year Comparison)

| Value Direction | Text Color | Token |
|---|---|---|
| Positive (+) | LANBOW-Cyan | `VariableID:10337:7723` |
| Negative (−) | Grey-08 | `VariableID:10337:7726` |
| Zero / neutral | Grey-01 | `VariableID:10337:7724` |

- Text style: 16-Regular
- Prefix: "+" for positive, "−" for negative
- Note: negative values use Grey-08 (muted), NOT Red — aligns with Lanbow's restrained color philosophy

### Column Group Headers (optional)
When table has grouped columns (e.g., multiple "Conversions" groups):
- Group label: 12-Regular, Grey-06, center-aligned above the sub-column group
- Sub-columns: 12-Regular, Grey-06 (e.g., "CVR", "CPA", "KYC")
- Column groups are visually separated by wider spacing (~40px gap between groups vs ~16px within groups)

---

## MODAL / DIALOG PATTERNS

Reference: `绑定Meta账号`, `移除二次确认`, `AI生图` in Figma (BPO Product page)

### Size Variants

| Type | Width | Height | Use case |
|------|-------|--------|----------|
| Small (confirmation) | 520px | ~256px | 二次确认、警告、简单提示 |
| Medium (form) | 568px | ~520px | 表单填写、账号绑定 |
| Large (complex) | 776px | ~710px | 多字段表单、AI 生成配置 |

### Universal Modal Rules
- **Background**: White `#ffffff`, `border-radius: 16px`
- **Shadow**: Drop shadow off by default (use backdrop overlay on parent instead)
- **Padding**: 24px all sides
- **Content width**: modal-width − 48px (24px × 2 margins)
- **Header**: Title left + X close button (24×24px) right, vertically centered, at top 24px
  - Title: Inter Bold 20px, `#272727`
  - Always at same vertical baseline as close icon
- **Vertical gap between fields**: 24px (Auto Layout vertical)
- **Section dividers**: 0px-height `Line` component between logical groups

### Button Rules (Modal Footer)

| Variant | Size | Fill | Stroke | Text |
|---------|------|------|--------|------|
| Primary (Confirm/Generate) | 160×40px | `#272727` solid | none | Funnel Sans Medium 16px, White |
| Secondary (Cancel) | 160×40px | transparent | `#272727` 1px | Funnel Sans Medium 16px, `#272727` |

- Both buttons: `border-radius: 8px`, `padding: 5px 40px`
- Layout: Cancel left, Confirm right; horizontal gap 12px
- Always right-aligned to modal edge (24px from right)

### Layout Patterns by Type

**Single-column form** (Small / Medium modals):
```
modal (568px wide)
  └── header: title + X [padding-top 24px, padding-H 24px]
  └── content frame (520px wide)
       └── vertical auto-layout, gap 24px
       └── input fields (full width 520px)
  └── footer: Confirm button (right-aligned)
```

**Two-column form** (Large modal, e.g. AI生图):
```
modal (776px wide)
  └── header: title + X [padding-top 24px, padding-H 24px]
  └── content frame (728px = two 352px cols + 24px gap)
       ├── left column (352px): primary inputs
       └── right column (352px): textarea / image inputs
  └── bottom bar (88px height)
       ├── parameter chips (left-aligned, 32px height)
       └── action button (right-aligned, 160×40px)
```

**Confirmation popup** (Small modal, e.g. 移除二次确认):
```
modal (520px wide, ~256px tall)
  └── header: title + X
  └── description text: Inter Regular 14px, #5b5b5b, margin-top 8px
  └── footer: [Cancel] [Confirm] — both 160×40px, right-aligned
```

### Bottom Bar (Large Modals)
- Height: 88px
- Background: gradient from `rgba(255,255,255,0)` → `#ffffff` (top to bottom, fade-in effect)
- Contains: left = parameter chips / status buttons; right = primary CTA
- Parameter chip: height 32px, padding H 12px, radius 8px, semi-transparent fill `rgba(0,0,0,0.02)`, icon + label + chevron-down

---

## SIDEBAR DRAWER PATTERN

Reference: `侧边栏_编辑_混合渠道` in Figma (BPO Product page)

### Overall Structure
- **Width**: 400px fixed
- **Position**: Right-pinned (`constraint: MAX`), vertical `STRETCH` (fills full page height)
- **Background**: White `#ffffff`, `border-radius: 20px` (left corners)
- **Shadow**: `box-shadow: -8px 0 24px rgba(0,0,0,0.04)`
- **Backdrop blur**: `backdrop-filter: blur(32px)`

### Three Zones

| Zone | Height | Background | Constraint |
|------|--------|------------|------------|
| Top bar (吸顶) | 74px | White | STRETCH width / MIN vertical → sticks to top |
| Content area | fills remaining | — | STRETCH width / MIN vertical → scrollable |
| Bottom bar (吸底) | 88px | White | STRETCH width / MAX vertical → sticks to bottom |

### Top Bar
- Padding: 24px all sides
- Layout: Horizontal — title text left, X close button right
- Title: Inter Medium 20px, `#272727`
- Close button: 24×24px X icon

### Content Area
- Left/right margin: 24px (content width = 400 − 48 = **352px**)
- Vertical Auto Layout, gap between modules: **24px**
- Overflows and scrolls (scrollbar: 6×~241px, opacity 10%, right edge)
- Section dividers: 0px-height `Line` component between groups

### Bottom Bar
- Height: 88px
- Primary action: 160×40px, right-aligned (`constraint: MAX`)
  - Fill `#272727`, radius 8px, padding `5px 20px`
- May contain secondary info/status group on the left

### Height Adaptation
- Top bar: pinned to top — never moves
- Bottom bar: pinned to bottom — never moves
- Content area: scrolls internally to accommodate overflow — never compress content

---

## MANDATORY SELF-REVIEW PROTOCOL

**This protocol is NON-NEGOTIABLE. After EVERY page creation or modification, you MUST complete ALL three phases before reporting "done" to the user. Skipping any phase is a failure.**

### Phase 1: Change Impact Analysis (BEFORE making changes)
For every change, ask: **"What else does this affect?"**

**Dependency chain rules** — when doing X, ALWAYS also check/do Y:
| Change | Must also check/fix |
|---|---|
| Remove border/stroke | Remove internal padding (→0), remove cornerRadius (→0), verify no double indentation with parent |
| Remove a wrapper frame | Re-check children positioning — were they relying on parent's padding/layout? |
| Change font size | Re-check spacing above/below — does hierarchy still hold? (P5 Gestalt) |
| Reposition elements | Verify ALL related elements are still aligned (center lines, baselines, edges) |
| Add/remove auto-layout | Check ALL children sizing — FILL/HUG may behave differently |
| Change container size | Check ALL children that use FILL/STRETCH — do they adapt correctly? |
| Clone a component | Verify: correct size, correct stroke weight, correct color on inner VECTOR |

### Phase 2: Geometric Verification (AFTER making changes)
Use `get_node_info` to mathematically verify alignment. **NEVER trust that coordinates "should be right" — always verify.**

**Verification formulas:**
- **Vertical center alignment**: For items A and B in same row, check `A.y + A.height/2 == B.y + B.height/2` (tolerance ±1px)
- **Even distribution**: For N items across width W with margin M, spacing = `(W - 2*M - N*itemWidth) / (N-1)`. Verify each gap is equal.
- **Line connects dots**: Line.x should == dot.right_edge, Line.x + Line.width should == next_dot.left_edge
- **No double padding**: If parent has padding P, children should have padding 0 for that axis (not P again)

### Phase 3: Visual Screenshot Review (MANDATORY — export and inspect)
1. **Export screenshot** of every completed/modified page at 0.5x scale
2. **Inspect the screenshot yourself** and look for these specific issues:

**Layout issues to catch:**
- [ ] Double indentation (nested padding that shouldn't exist)
- [ ] Uneven spacing between repeated elements (steps, grid items, columns)
- [ ] Elements that look "off-center" or misaligned
- [ ] Content clipped or cut off at edges

**Typography issues to catch:**
- [ ] Section titles that look the same size/weight as body text (P5 violation)
- [ ] Title-to-content gap ≥ section-to-section gap (proximity violation)
- [ ] Placeholder text accidentally left as real content

**Geometric issues to catch:**
- [ ] Connection lines not passing through centers of their endpoints
- [ ] Icons not vertically centered with adjacent text
- [ ] Inconsistent element sizes (some dots bigger than others, etc.)

**Residual artifact issues to catch:**
- [ ] Invisible borders still taking up space (strokeWeight > 0 but opacity 0)
- [ ] Padding/radius left over from removed borders ("ghost indentation")
- [ ] Hidden elements still affecting layout (visible: false but in auto-layout)

3. **If ANY issue is found**: fix it immediately, then re-export and re-inspect. Do NOT report done until clean.

### Token & Style Checks
- [ ] All fills use variable bindings (no hardcoded hex)
- [ ] All corner radii use variable bindings
- [ ] All text uses `textStyleName` (no hardcoded font sizes)
- [ ] Spacing is consistent (8/12/16/24px grid)
- [ ] Color discipline: only Greys + Cyan (+ Red/Orange for states)
- [ ] Icons at 16×16px default, correct stroke color on inner VECTOR node

### Page Height Rule
- Page height is NOT fixed at 900px — expand canvas height as needed to show all content
- Drawer panels, floating panels, and body frames must resize accordingly when page height changes

---

## EXAMPLE INVOCATIONS

```
/lanbow-design Create a user management page with table, search filter, and invite user modal
/lanbow-design Design the full onboarding flow: signup → setup profile → invite team → dashboard
/lanbow-design Add a settings page to the existing design: general, notifications, billing tabs
```
