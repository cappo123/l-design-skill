# Lanbow V3 — Design Tokens

All foundational design tokens: colors, typography, spacing, shadows, radius.

---

## COLOR TOKENS

### Figma Variables (Collection: "Lanbow Colors", collectionId: VariableCollectionId:10337:7721, modeId: 10337:0)

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

**Frontend mode RULE: Use CSS variables (`var(--grey-01)`) or Tailwind classes (`text-grey-01`). NEVER hardcode hex values inline — always reference the token. See `frontend.md` for the complete CSS variables block and Tailwind config (single source of truth for all code output).**

---

## RADIUS: ALL ZERO

**V3 uses 0px corner radius on EVERYTHING.** No rounded corners anywhere — cards, buttons, inputs, modals, panels, tags — all sharp/square.

- Do NOT bind radius variables from the Lanbow Radius collection
- Set `cornerRadius: 0` on all frames, or simply omit (default is 0)
- Exception: avatars remain circular (radius-round 9999px)

**Frontend: See `frontend.md` for CSS variables and Tailwind config.**

---

## SHADOW / ELEVATION SYSTEM

### Design Philosophy

Lanbow V3 communicates depth primarily through **background color layering**, not shadows:

```
Layer 0 (Page)     → #FBF9F4  page background
Layer 1 (Card)     → #FFFFFF  white card surface
Layer 2 (Sub-card) → #F8F6F2  warm grey nested surface
```

**Shadows are reserved ONLY for floating/overlay elements** — content that overlaps other content. Grounded elements (cards, sections, panels) use NO shadow.

### Shadow Tokens

| Token | CSS Value | Figma Effect | Usage |
|---|---|---|---|
| `--shadow-none` | `none` | No effect | Default for all grounded elements: cards, sections, panels, buttons |
| `--shadow-sm` | `0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04)` | Drop shadow: X0 Y1 B3 rgba(0,0,0,6%) + X0 Y1 B2 rgba(0,0,0,4%) | Tooltip, small popover |
| `--shadow-md` | `0 4px 12px rgba(0,0,0,0.06), 0 2px 4px rgba(0,0,0,0.04)` | Drop shadow: X0 Y4 B12 rgba(0,0,0,6%) + X0 Y2 B4 rgba(0,0,0,4%) | Dropdown menu, date picker popup, autocomplete |
| `--shadow-lg` | `0 8px 24px rgba(0,0,0,0.08), 0 4px 8px rgba(0,0,0,0.04)` | Drop shadow: X0 Y8 B24 rgba(0,0,0,8%) + X0 Y4 B8 rgba(0,0,0,4%) | Modal, dialog, command palette |
| `--shadow-overlay` | `0 16px 48px rgba(0,0,0,0.10), 0 8px 16px rgba(0,0,0,0.06)` | Drop shadow: X0 Y16 B48 rgba(0,0,0,10%) + X0 Y8 B16 rgba(0,0,0,6%) | Full-screen overlay panels, side drawers |

**Frontend: See `frontend.md` for CSS variables and Tailwind config.**

### Usage Rules (MANDATORY)

| Element | Shadow | Why |
|---|---|---|
| Card (white, grounded) | `--shadow-none` | Depth via color layering, not shadow |
| Sub-card (warm grey) | `--shadow-none` | Nested within card, no elevation needed |
| Button (all variants) | `--shadow-none` | Flat V3 aesthetic |
| Input / Form field | `--shadow-none` | Border-based focus, no shadow |
| Nav bar | `--shadow-none` | Uses bottom border for separation |
| Tooltip | `--shadow-sm` | Small floating element, close to trigger |
| Dropdown menu | `--shadow-md` | Floating above content, needs clear separation |
| Popover / Date picker | `--shadow-md` | Same as dropdown |
| Modal / Dialog | `--shadow-lg` | Major floating layer, needs prominence |
| Toast notification | `--shadow-md` | Floating but smaller than modal |
| Side drawer / Panel | `--shadow-overlay` | Full-height overlay, maximum depth |
| Command palette | `--shadow-lg` | Centered overlay like modal |

**NEVER add shadow to grounded elements.** If a card "needs" shadow, reconsider: it likely needs a border (`--stroke`) or background contrast instead.

---

## SPACING SYSTEM

### Token Scale

| Token | Value | CSS Variable | Tailwind |
|---|---|---|---|
| `space-2xs` | 2px | `--space-2xs` | `gap-0.5` / `p-0.5` |
| `space-xs` | 4px | `--space-xs` | `gap-1` / `p-1` |
| `space-sm` | 8px | `--space-sm` | `gap-2` / `p-2` |
| `space-md` | 12px | `--space-md` | `gap-3` / `p-3` |
| `space-base` | 16px | `--space-base` | `gap-4` / `p-4` |
| `space-lg` | 20px | `--space-lg` | `gap-5` / `p-5` |
| `space-xl` | 24px | `--space-xl` | `gap-6` / `p-6` |
| `space-2xl` | 32px | `--space-2xl` | `gap-8` / `p-8` |
| `space-3xl` | 40px | `--space-3xl` | `gap-10` / `p-10` |
| `space-4xl` | 48px | `--space-4xl` | `gap-12` / `p-12` |

**Frontend: See `frontend.md` for CSS variables and Tailwind config.**

### Spacing Usage Guide — When to Use Each Value

#### 2px (`--space-2xs`) — Micro nudge
- Gap between icon and badge/dot indicator
- Optical alignment adjustments
- Gap between stacked thin borders

#### 4px (`--space-xs`) — Tight inline
- Gap between icon and adjacent label text (e.g., `ph:bell` + "3")
- Vertical gap between meta text and title within notification items
- Gap between small badge/tag elements in a row
- Inner padding of tiny badges (e.g., "New" badge top/bottom)

#### 8px (`--space-sm`) — Compact content
- Gap between avatar and username in nav
- Horizontal gap between legend dot and legend text
- Vertical gap between closely related items within a card (e.g., label → value)
- Padding for small tags/chips
- Gap between horizontally stacked small buttons

#### 12px (`--space-md`) — Default internal rhythm
- Gap between skeleton loading bars
- Gap between form label and input field
- Vertical gap between paragraph-level items within a list
- Gap between chart axis tick labels and chart edge

#### 16px (`--space-base`) — Standard structural gap
- **Column gap** between left/right columns in content grid
- **Section gap** (Container itemSpacing) between major content blocks
- Gap between cards stacked vertically in a column
- Gap between sub-cards in a horizontal row (report cards)
- Gap between H1 title and first content section
- Horizontal padding for compact sub-cards (left/right 20px, top/bottom 16px — top/bottom uses base)

#### 20px (`--space-lg`) — Spacious content
- Horizontal padding within sub-cards (left/right)
- Gap between notification title and notification body
- Padding for medium-density card content areas

#### 24px (`--space-xl`) — Standard card padding
- **Card internal padding** (all sides) for standard white cards
- Container top padding (distance from nav bar to first content)
- Gap between nav logo area and nav links
- Vertical gap between section title and section content within a card
- Gap between major card internal sections (e.g., KPI area → chart area)

#### 32px (`--space-2xl`) — Section breathing room
- Gap between large content blocks that need visual separation
- Vertical margin between completely independent page sections
- Padding for hero/feature card content areas
- Distance between page title group and main content grid (when extra emphasis needed)

#### 40px (`--space-3xl`) — Page-level spacing
- **Container bottom padding**
- Vertical distance above the footer area
- Gap between major page zones (above/below a full-width divider)

#### 48px (`--space-4xl`) — Container edge
- **Container horizontal padding** (left/right)
- Major page-level indentation
- Breathing room at layout edges

### Spacing Decision Flowchart

```
Is this inside a single component?
├─ YES → Is it between icon/text or tightly coupled pairs?
│        ├─ YES → 4px (xs) or 8px (sm)
│        └─ NO  → Is it between label and content?
│                 ├─ YES → 12px (md)
│                 └─ NO  → 8px (sm) for tight, 16px (base) for standard
├─ NO → Is this between sibling components (cards, sections)?
│       ├─ YES → 16px (base) — the universal structural gap
│       └─ NO  → Is this card/container internal padding?
│               ├─ YES → 24px (xl) standard, 16-20px compact variant
│               └─ NO  → Is this page/container edge spacing?
│                       ├─ YES → 24-48px (xl to 4xl) per container rules
│                       └─ NO  → Default to 16px (base)
```

### Key Spacing Constants (Quick Reference)

| Location | Value | Token |
|---|---|---|
| Container padding-top | 24px | `--space-xl` |
| Container padding-left/right | 48px | `--space-4xl` |
| Container padding-bottom | 40px | `--space-3xl` |
| Container itemSpacing | 16px | `--space-base` |
| Card padding (standard) | 24px | `--space-xl` |
| Card padding (compact sub-card) | 16px top/bottom, 20px left/right | `--space-base` / `--space-lg` |
| Column gap | 16px | `--space-base` |
| Notification item gap | 24px | `--space-xl` |
| Nav link gap | 36px | Custom |

---

## TYPOGRAPHY SYSTEM

### Three Font Families

| Font Family | CSS Variable | Role | Available Styles |
|---|---|---|---|
| **Abhaya Libre** | `--font-display` | Display/headings, dates | Bold, SemiBold, Medium, Regular |
| **Newsreader** | `--font-data` | Numeric data/KPI values | SemiBold |
| **Inter** | `--font-ui` | UI text, body, labels, nav | Bold, Semi Bold, Medium, Thin, Regular |

**Frontend: See `frontend.md` for CSS variables and Tailwind config.**

### Typography Hierarchy

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

**CRITICAL — Font family + fontStyle mapping (Figma mode):**
- `fontFamily: "Abhaya Libre"` with `fontStyle: "Bold"` or `fontStyle: "Regular"`
- `fontFamily: "Newsreader"` with `fontStyle: "SemiBold"`
- `fontFamily: "Inter"` with `fontStyle: "Bold"`, `"Semi Bold"`, `"Medium"`, `"Thin"`, or `"Regular"`
