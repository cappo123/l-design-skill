# /l-coding — L Frontend Code Generator

Generate production-grade frontend code (React/Next.js + Tailwind CSS) that precisely implements the L design system. Every component, color, spacing, and interaction pattern matches the L Figma spec 1:1.

---

/* 增加一行注释 删掉橙色 */

## SETUP: CSS CUSTOM PROPERTIES

**Paste this into your global CSS file (e.g., `globals.css` or `app.css`).**
All components reference these tokens — NEVER hardcode hex values.

```css
:root {
  /* ── Colors ── */
  --l-cyan: #00B1A2;         /* Brand primary: active states, links, positive trends ONLY */
  --grey-01: #181818;             /* Primary text, headings, primary button fill */
  --grey-06: #626262;             /* Secondary text, captions */
  --grey-08: #999999;             /* Placeholder text, disabled states, tertiary text */
  --grey-12: #EEEEEE;            /* Borders, dividers, subtle lines */
  --bg: #F3F3F3;                  /* ONLY for large page-level backgrounds */
  --white: #FFFFFF;               /* Card surfaces, inputs, modals */
  --selected: rgba(0,0,0,0.02);  /* Subtle fills: table headers, hover states, option cards, tag bg */
  --stroke: rgba(0,0,0,0.04);    /* Card borders, container strokes */
  --red: #FF2D55;                 /* Error, danger, destructive ONLY */
  --c7: #2A6365;                  /* Magazine card title accent (dark teal) */

  /* ── Radius ── */
  --radius-none: 0px;
  --radius-xs: 2px;               /* Tags, badges */
  --radius-sm: 4px;               /* Small buttons, chips, checkboxes */
  --radius-md: 6px;               /* Inputs, small cards */
  --radius-lg: 8px;               /* Buttons, dropdowns, project cards */
  --radius-xl: 12px;              /* Cards, modals */
  --radius-2xl: 16px;             /* Large cards, panels */
  --radius-3xl: 24px;             /* Main content container, hero cards */
  --radius-round: 9999px;         /* Avatars, pills, toggle knobs */

  /* ── Spacing (4px base grid) ── */
  --sp-tight: 4px;                /* Label-to-input, icon internal */
  --sp-compact: 8px;              /* Icon-text gap, tag padding */
  --sp-item: 12px;                /* Items within a group */
  --sp-component: 16px;           /* Between components, grid gap (small) */
  --sp-section: 24px;             /* Between sections, card padding */
  --sp-module: 40px;              /* Major section breaks, content margin top/bottom */
  --sp-flow: 80px;                /* Content margin left/right from grey bg */

  /* ── Shadows ── */
  --shadow-panel: -8px 0 24px rgba(0,0,0,0.04);   /* Floating side panels */
  --shadow-dropdown: 0 4px 16px rgba(0,0,0,0.08);  /* Dropdown menus */
  --shadow-card-magazine: 0 40px 48px -32px rgba(0,0,0,0.08); /* Magazine cards */

  /* ── Transitions ── */
  --transition-fast: 150ms ease;
  --transition-normal: 250ms ease;
}
```

### Tailwind v4 Integration

If using Tailwind v4 with `@theme`, add to your CSS:
```css
@theme {
  --color-l-cyan: #00B1A2;
  --color-grey-01: #181818;
  --color-grey-06: #626262;
  --color-grey-08: #999999;
  --color-grey-12: #EEEEEE;
  --color-bg: #F3F3F3;
  --color-selected: rgba(0,0,0,0.02);
  --color-stroke: rgba(0,0,0,0.04);
  --color-red: #FF2D55;
  --color-orange: #FF9500;

  --radius-xs: 2px;
  --radius-sm: 4px;
  --radius-md: 6px;
  --radius-lg: 8px;
  --radius-xl: 12px;
  --radius-2xl: 16px;
  --radius-3xl: 24px;
}
```

Usage: `className="bg-l-cyan text-grey-01 rounded-xl"`

---

## TYPOGRAPHY SYSTEM

**Font family**: Inter (primary), system-ui fallback.

```css
/* Base typography utility classes */
.text-32-bold    { font-size: 32px; font-weight: 700; line-height: 1.25; }
.text-28-bold    { font-size: 28px; font-weight: 700; line-height: 1.3; }
.text-24-bold    { font-size: 24px; font-weight: 700; line-height: 1.33; }
.text-20-bold    { font-size: 20px; font-weight: 700; line-height: 1.4; }
.text-18-bold    { font-size: 18px; font-weight: 700; line-height: 1.44; }
.text-16-bold    { font-size: 16px; font-weight: 700; line-height: 1.5; }
.text-14-bold    { font-size: 14px; font-weight: 700; line-height: 1.43; }
.text-12-bold    { font-size: 12px; font-weight: 700; line-height: 1.33; }

.text-32-medium  { font-size: 32px; font-weight: 500; line-height: 1.25; }
.text-24-medium  { font-size: 24px; font-weight: 500; line-height: 1.33; }
.text-20-medium  { font-size: 20px; font-weight: 500; line-height: 1.4; }
.text-18-medium  { font-size: 18px; font-weight: 500; line-height: 1.44; }
.text-16-medium  { font-size: 16px; font-weight: 500; line-height: 1.5; }
.text-14-medium  { font-size: 14px; font-weight: 500; line-height: 1.43; }
.text-12-medium  { font-size: 12px; font-weight: 500; line-height: 1.33; }

.text-16-regular { font-size: 16px; font-weight: 400; line-height: 1.5; }
.text-14-regular { font-size: 14px; font-weight: 400; line-height: 1.43; }
.text-12-regular { font-size: 12px; font-weight: 400; line-height: 1.33; }
.text-10-regular { font-size: 10px; font-weight: 400; line-height: 1.4; }
```

**Hierarchy mapping (P2 — typography creates hierarchy, NOT color):**

| Level | Class | Color Var | Usage |
|---|---|---|---|
| Page title | `text-24-bold` | `--grey-01` | One per screen, top of page |
| Section title | `text-20-bold` | `--grey-01` | Card headers, panel titles |
| Subsection | `text-16-bold` | `--grey-01` | Widget labels, table section titles |
| Label | `text-14-medium` | `--grey-01` | Form labels, nav items, button text |
| Body | `text-14-regular` | `--grey-01` | Default reading text, table cells |
| Secondary | `text-12-regular` | `--grey-06` | Timestamps, helper text, captions |
| Placeholder | `text-14-regular` | `--grey-08` | Input placeholders, disabled text |
| Tiny | `text-10-regular` | `--grey-08` | Footnotes, legal text |

---

## PAGE LAYOUT SYSTEM

### Standard Dashboard Page (Nav + Icon Sidebar + Content)

```tsx
<div className="w-[1440px] min-h-screen bg-white flex flex-col">
  {/* Top Navigation Bar — 50px, full width */}
  <NavBar />

  {/* Body: sidebar + content */}
  <div className="flex flex-1">
    {/* Collapsed Icon Sidebar — 56px fixed */}
    <Sidebar />

    {/* Main Content Area — grey background, rounded container */}
    <main
      className="flex-1 bg-[var(--bg)] flex flex-col gap-6"
      style={{
        borderRadius: 'var(--radius-3xl)',
        padding: '40px 80px',  /* CRITICAL: 40/80/80/40 content margins */
      }}
    >
      {/* Page content goes here */}
      {children}
    </main>
  </div>
</div>
```

**CRITICAL content margin rule: `padding: 40px 80px`**
- Top/bottom: 40px from grey bg edge
- Left/right: 80px from grey bg edge
- Content width = container width − 160px (e.g., 1384 − 160 = 1224px)

### Spacing Constants

```
Content margin from grey bg:  40px top/bottom, 80px left/right  (MOST IMPORTANT)
Section gap:                  24px (between cards/sections)
Card internal padding:        20px (standard), 24px (modals)
Component gap:                16px (between components within a card)
Item gap:                     12px (items within a group)
Tight gap:                    8px (icon-text, tag padding)
```

### Fixed Sizing

```
Nav bar height:      50px
Sidebar width:       56px
Table row height:    48px (text-only), 80px+ (with images/thumbnails)
Input height:        40px
Button height:       36px (default), 40px (large modals)
```

---

## DESIGN PRINCIPLES (P1–P17)

These are **mandatory rules** — every component and page MUST comply.

### P1: Color Ratio (85/10/5)
- **~85% Greyscale**: `--grey-01` through `--grey-12`, `--white`, `--bg`
- **~10% Brand**: `--l-cyan` — ONLY active states, links, positive indicators
- **~5% Semantic**: `--red` for errors, `--orange` for warnings — NEVER decorative
- **NEVER use cyan as button background** — buttons are `--grey-01` bg + white text

### P2: Typography Creates Hierarchy (Not Color)
Size + weight contrast, not color variation. See Typography table above.

### P3: Depth Through Layering (Not Shadow)
```css
/* CORRECT: layered backgrounds */
.page-bg  { background: var(--bg); }           /* Grey base layer */
.card     { background: var(--white); border: 1px solid var(--stroke); }  /* White card on grey */

/* WRONG: heavy shadows */
.card     { box-shadow: 0 8px 32px rgba(0,0,0,0.15); }  /* ❌ Too dramatic */
```
Exception: floating panels and dropdowns may use subtle shadows.

### P4: Consistent Spacing Grid (4px base)
All spacing values derive from 4px multiples: 4, 8, 12, 16, 20, 24, 32, 40, 80.
**NEVER use arbitrary values** like 5px, 7px, 13px, 28px, 35px.

### P5: Gestalt Hierarchy (Size Contrast + Proximity)
- Title-to-content gap: 16–20px (tight → shows belonging)
- Section-to-section gap: 24–32px (loose → shows separation)
- Title MUST be ≥2px larger AND bolder than body text

### P6: Restrained Aesthetic
- No gradients (except login brand panel)
- No decorative illustrations in dashboard
- No colored card backgrounds — always `--white`
- Left-align everything except: centered empty states, centered modals, right-aligned numbers

### P7: Button Color Discipline

```tsx
/* Primary — main CTA (one per visible area) */
<button className="bg-[var(--grey-01)] text-white rounded-lg px-6 py-2.5 text-14-medium">
  Confirm
</button>

/* Secondary — alternative actions */
<button className="bg-transparent border border-[var(--grey-01)] text-[var(--grey-01)] rounded-lg px-6 py-2.5 text-14-medium">
  Cancel
</button>

/* Destructive — delete confirmation ONLY */
<button className="bg-[var(--red)] text-white rounded-lg px-6 py-2.5 text-14-medium">
  Delete
</button>

/* Disabled */
<button className="bg-[var(--grey-12)] text-[var(--grey-08)] rounded-lg px-6 py-2.5 text-14-medium cursor-not-allowed">
  Submit
</button>

/* Text/Link — inline actions */
<button className="text-[var(--l-cyan)] text-14-medium bg-transparent border-none">
  View More
</button>
```

**CRITICAL: Button Height Consistency Rule**
Primary and secondary buttons in the **same row** MUST have identical padding → identical height.
Standard: `py-2.5` (10px) → produces 36px height with 14px text. Use everywhere.
**NEVER mix `py-2` and `py-2.5` in the same button group.**

### P8: Form Field Standard

```tsx
<div className="flex flex-col gap-1">
  <label className="text-14-medium text-[var(--grey-01)]">
    • Field Label
  </label>
  <input
    className="h-10 px-3 rounded-md border border-[var(--grey-12)] bg-white
               text-14-regular text-[var(--grey-01)]
               placeholder:text-[var(--grey-08)]
               focus:border-[var(--grey-01)] focus:outline-none
               transition-colors duration-150"
    placeholder="Placeholder text"
  />
  <span className="text-12-regular text-[var(--grey-08)]">Helper text</span>
</div>
```

### P8.1: Card Bottom Padding — Visual Weight Rule
- Visually packed bottom (full-width fields, textareas, dense lists) → `pb-10` (40px)
- Visually sparse bottom (right-aligned totals, buttons) → `pb-6` (24px)

### P9: Whitespace Over Nested Borders
```tsx
{/* ❌ WRONG: card-inside-card */}
<div className="border rounded-xl p-5">
  <table className="border rounded-lg">...</table>
</div>

{/* ✅ CORRECT: whitespace + background differentiation */}
<div className="bg-white rounded-xl p-5">
  <div className="bg-[var(--selected)] rounded-lg px-4 py-3">  {/* Header row */}
    ...
  </div>
  <div className="divide-y divide-[var(--grey-12)]">  {/* Data rows */}
    ...
  </div>
</div>
```

### P10: Selection Indicators

**Single-select (radio-style) — 16×16 circular:**
```tsx
{/* Selected */}
<svg width="16" height="16" viewBox="0 0 16 16" fill="none">
  <circle cx="8" cy="8" r="8" fill="var(--grey-01)"/>
  <path d="M5 8L7 10L11 6" stroke="white" strokeWidth="1.33" strokeLinecap="round" strokeLinejoin="round"/>
</svg>

{/* Unselected */}
<svg width="16" height="16" viewBox="0 0 16 16" fill="none">
  <circle cx="8" cy="8" r="7" stroke="#CCCCCC" strokeWidth="1.33"/>
</svg>
```

**Multi-select (checkbox-style) — 16×16 rounded square:**
```tsx
{/* Checked */}
<svg width="16" height="16" viewBox="0 0 16 16" fill="none">
  <rect width="16" height="16" rx="4" fill="var(--grey-01)"/>
  <path d="M4 8L7 11L12 5" stroke="white" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round"/>
</svg>

{/* Unchecked */}
<svg width="16" height="16" viewBox="0 0 16 16" fill="none">
  <rect x="0.5" y="0.5" width="15" height="15" rx="3.5" fill="rgba(255,255,255,0.5)" stroke="var(--grey-12)"/>
</svg>

{/* Disabled */}
<svg width="16" height="16" viewBox="0 0 16 16" fill="none" opacity="0.35">
  <rect x="0.5" y="0.5" width="15" height="15" rx="3.5" fill="rgba(255,255,255,0.5)" stroke="var(--grey-12)"/>
</svg>
```

### P11: Data Value Contrast Rule
**Minimum value font ≥ 1.5× label font. Value MUST be Bold, label uses Medium/Regular.**

```tsx
{/* ✅ CORRECT: strong contrast */}
<div className="flex flex-col gap-0.5">
  <span className="text-12-medium text-[var(--grey-08)]">Spent</span>
  <span className="text-20-bold text-[var(--grey-01)]">$ 4,350</span>
</div>

{/* ❌ WRONG: flat hierarchy */}
<div className="flex items-center gap-2">
  <span className="text-14-medium">Spent</span>
  <span className="text-14-medium">$ 4,350</span>  {/* No contrast! */}
</div>
```

| Context | Label | Value | Ratio |
|---|---|---|---|
| KPI hero | `text-12-medium grey-08` | `text-32-bold grey-01` | 2.67× |
| KPI compact | `text-12-medium grey-08` | `text-24-bold grey-01` | 2× |
| Card metric | `text-12-medium grey-08` | `text-20-bold grey-01` | 1.67× |
| Table inline | `text-12-medium grey-08` | `text-16-bold grey-01` | 1.33× min |

### P12: Icon Stroke Integrity
Use SVG icons with proper stroke widths per size:

| Size | Stroke | Usage |
|---|---|---|
| 24×24 | 2px | Large UI, standalone icons |
| 20×20 | 1.67px | Prominent inline |
| 16×16 | 1.33px | Default UI, buttons, nav |
| 12×12 | 1px | Compact, badges, tags |

**NEVER scale icons via CSS `transform: scale()` — it distorts stroke width.** Use the correct SVG viewBox and strokeWidth for each size.

### P13: Card Internal Padding Standard
**No arbitrary values (28px is BANNED).**

| Context | Padding | Tailwind |
|---|---|---|
| Dropdown menus, chips | 12px | `p-3` |
| Content cards, form cards | 20px | `p-5` |
| Modal/dialog overlays | 24px | `p-6` |
| Magazine/hero cards | 40px | `p-10` |

### P14: Title-Divider Optical Balance
When card has Title → Divider → Content:
- `paddingTop` MUST equal gap below title to divider
- Correct: both 16px → balanced
- Wrong: paddingTop 24px, gap 16px → title floats up

### P15: No Double Padding
If parent has padding and child is first/last, child has 0 padding in that direction.

### P16: Button Standard

```css
/* Both buttons: SAME padding = SAME height */
.btn-primary {
  background: var(--grey-01);
  color: white;
  border: none;
  border-radius: var(--radius-lg);
  padding: 10px 24px;          /* V: 10px → height 36px with 14px text */
  font: 500 14px/1.43 Inter;
}

.btn-secondary {
  background: transparent;
  color: var(--grey-01);
  border: 1px solid var(--grey-01);  /* Grey-01, NOT Grey-12 */
  border-radius: var(--radius-lg);
  padding: 10px 24px;          /* SAME as primary */
  font: 500 14px/1.43 Inter;
}
```

### P17: Select / Filter Dropdown

```tsx
<div className="flex items-center gap-1.5 h-9 px-3 bg-white border border-[var(--grey-12)] rounded-lg cursor-pointer">
  <span className="text-14-regular text-[var(--grey-01)]">Last 15 days</span>
  <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
    <path d="M4 6L8 10L12 6" stroke="var(--grey-08)" strokeWidth="1.33" strokeLinecap="round" strokeLinejoin="round"/>
  </svg>
</div>
```

---

## COMPONENT LIBRARY

### Card

```tsx
function Card({ children, className = '', noBorder = false }) {
  return (
    <div className={`
      bg-white rounded-xl p-5
      ${noBorder ? '' : 'border border-[var(--stroke)]'}
      ${className}
    `}>
      {children}
    </div>
  );
}
```

### Data Table

```tsx
function DataTable({ columns, rows }) {
  return (
    <div className="bg-white rounded-xl overflow-hidden">
      {/* Header */}
      <div className="flex bg-[var(--selected)] px-5 h-10 items-center">
        {columns.map(col => (
          <div key={col.key} className={`text-14-bold text-[var(--grey-01)] ${col.className}`}>
            {col.label}
          </div>
        ))}
      </div>

      {/* Rows with zebra striping (alternating, non-full-width) */}
      {rows.map((row, i) => (
        <div key={i}>
          {/* Divider line */}
          <div className="mx-5 h-px bg-[var(--stroke)]" />

          {/* Row — odd rows get subtle bg, even rows stay white */}
          <div className={`flex px-5 h-12 items-center ${
            i % 2 === 1 ? 'mx-3 bg-[var(--selected)] rounded-lg' : ''
          }`}>
            {columns.map(col => (
              <div key={col.key} className={`text-14-regular text-[var(--grey-01)] ${col.className}`}>
                {row[col.key]}
              </div>
            ))}
          </div>
        </div>
      ))}
    </div>
  );
}
```

**Table Rules:**
- No outer border on table (P9)
- Header: `bg-[var(--selected)]` (NOT `--bg`)
- Dividers: `bg-[var(--stroke)]` with `mx-5` (not full-width)
- Zebra rows: alternating `bg-[var(--selected)]` with `mx-3 rounded-lg` (not full bleed)
- Row height: 48px text-only, 80px+ with images

### Input Field

```tsx
function InputField({ label, placeholder, helperText, error, ...props }) {
  return (
    <div className="flex flex-col gap-1">
      {label && (
        <label className="text-14-medium text-[var(--grey-01)]">• {label}</label>
      )}
      <input
        className={`
          h-10 px-3 rounded-md bg-white text-14-regular text-[var(--grey-01)]
          placeholder:text-[var(--grey-08)]
          border ${error ? 'border-[var(--red)]' : 'border-[var(--grey-12)]'}
          focus:border-[var(--grey-01)] focus:outline-none
          transition-colors duration-150
        `}
        placeholder={placeholder}
        {...props}
      />
      {error && <span className="text-12-regular text-[var(--red)]">{error}</span>}
      {helperText && !error && <span className="text-12-regular text-[var(--grey-08)]">{helperText}</span>}
    </div>
  );
}
```

### Buttons

```tsx
function Button({ variant = 'primary', size = 'default', disabled, children, ...props }) {
  const base = 'inline-flex items-center justify-center gap-2 rounded-lg text-14-medium transition-colors duration-150';

  const variants = {
    primary: 'bg-[var(--grey-01)] text-white hover:opacity-90',
    secondary: 'bg-transparent border border-[var(--grey-01)] text-[var(--grey-01)] hover:bg-[var(--selected)]',
    destructive: 'bg-[var(--red)] text-white hover:opacity-90',
    ghost: 'bg-transparent text-[var(--l-cyan)] hover:underline',
  };

  const sizes = {
    default: 'h-9 px-6',   /* 36px */
    large: 'h-10 px-6',    /* 40px */
  };

  const disabledStyle = 'bg-[var(--grey-12)] text-[var(--grey-08)] border-none cursor-not-allowed';

  return (
    <button
      className={`${base} ${disabled ? disabledStyle : variants[variant]} ${sizes[size]}`}
      disabled={disabled}
      {...props}
    >
      {children}
    </button>
  );
}
```

### Select / Dropdown

```tsx
function Select({ value, options, onChange, placeholder = 'Select...' }) {
  return (
    <div className="relative inline-flex items-center gap-1.5 h-9 px-3 bg-white border border-[var(--grey-12)] rounded-lg cursor-pointer">
      <span className={`text-14-regular ${value ? 'text-[var(--grey-01)]' : 'text-[var(--grey-08)]'}`}>
        {value || placeholder}
      </span>
      <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
        <path d="M4 6L8 10L12 6" stroke="var(--grey-08)" strokeWidth="1.33" strokeLinecap="round" strokeLinejoin="round"/>
      </svg>
    </div>
  );
}
```

### Tag / Chip

```tsx
function Tag({ children, active, onRemove }) {
  return (
    <span className={`
      inline-flex items-center gap-1 h-6 px-2 rounded-full text-12-medium
      ${active
        ? 'bg-[var(--grey-01)] text-white'
        : 'bg-[var(--selected)] text-[var(--grey-01)]'
      }
    `}>
      {children}
      {onRemove && (
        <svg onClick={onRemove} className="cursor-pointer" width="12" height="12" viewBox="0 0 12 12" fill="none">
          <path d="M3 3L9 9M9 3L3 9" stroke="currentColor" strokeWidth="1" strokeLinecap="round"/>
        </svg>
      )}
    </span>
  );
}
```

### Toggle Switch

```tsx
function Toggle({ checked, onChange }) {
  return (
    <button
      role="switch"
      aria-checked={checked}
      onClick={() => onChange?.(!checked)}
      className={`
        relative w-10 h-5 rounded-full transition-colors duration-200
        ${checked ? 'bg-[var(--l-cyan)]' : 'bg-[var(--grey-12)]'}
      `}
    >
      <span className={`
        absolute top-0.5 w-4 h-4 rounded-full bg-white transition-transform duration-200
        ${checked ? 'translate-x-5' : 'translate-x-0.5'}
      `} />
    </button>
  );
}
```

### Avatar

```tsx
function Avatar({ src, name, size = 'md' }) {
  const sizes = { sm: 'w-6 h-6 text-10-regular', md: 'w-8 h-8 text-14-medium', lg: 'w-10 h-10 text-16-medium' };
  return (
    <div className={`${sizes[size]} rounded-full bg-[var(--bg)] text-[var(--grey-06)] flex items-center justify-center overflow-hidden`}>
      {src ? <img src={src} alt={name} className="w-full h-full object-cover" /> : name?.[0]?.toUpperCase()}
    </div>
  );
}
```

### Confirmation Dialog

```tsx
function ConfirmDialog({ title, description, onConfirm, onCancel, confirmLabel = 'Confirm', cancelLabel = 'Cancel' }) {
  return (
    <div className="fixed inset-0 bg-black/20 flex items-center justify-center z-50">
      <div className="w-[520px] bg-white rounded-2xl p-6 flex flex-col gap-6">
        <div className="flex items-center justify-between">
          <h3 className="text-20-bold text-[var(--grey-01)]">{title}</h3>
          <button onClick={onCancel} className="text-[var(--grey-08)] hover:text-[var(--grey-01)]">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
              <path d="M6 6L18 18M18 6L6 18" stroke="currentColor" strokeWidth="2" strokeLinecap="round"/>
            </svg>
          </button>
        </div>
        <p className="text-14-regular text-[var(--grey-06)]">{description}</p>
        <div className="flex justify-end gap-3">
          <Button variant="secondary" onClick={onCancel}>{cancelLabel}</Button>
          <Button variant="primary" onClick={onConfirm}>{confirmLabel}</Button>
        </div>
      </div>
    </div>
  );
}
```

### Right Floating Panel (Drawer)

```tsx
function FloatingPanel({ title, children, footer, onClose }) {
  return (
    <div className="absolute top-1 right-1 bottom-1 w-[400px] bg-white rounded-[20px] flex flex-col overflow-hidden"
         style={{ boxShadow: 'var(--shadow-panel)', backdropFilter: 'blur(32px)' }}>
      {/* Sticky header */}
      <div className="flex items-center justify-between px-6 py-4 border-b border-[var(--stroke)]">
        <h2 className="text-20-bold text-[var(--grey-01)]">{title}</h2>
        <button onClick={onClose}>
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <path d="M6 6L18 18M18 6L6 18" stroke="var(--grey-01)" strokeWidth="2" strokeLinecap="round"/>
          </svg>
        </button>
      </div>

      {/* Scrollable content */}
      <div className="flex-1 overflow-y-auto p-6 flex flex-col gap-6">
        {children}
      </div>

      {/* Sticky footer */}
      {footer && (
        <div className="flex justify-end gap-3 px-6 py-4 border-t border-[var(--stroke)]">
          {footer}
        </div>
      )}
    </div>
  );
}
```

**CRITICAL: ALL right-side drawers MUST use this floating panel pattern. NEVER use full-screen modal overlay + drawer. L drawers float within the grey bg area.**

---

## PAGE TYPE TEMPLATES

### Type A: Dashboard Page

```tsx
export default function DashboardPage() {
  return (
    <div className="w-[1440px] min-h-screen bg-white flex flex-col">
      <NavBar />
      <div className="flex flex-1">
        <Sidebar />
        <main className="flex-1 bg-[var(--bg)] rounded-3xl flex flex-col gap-6"
              style={{ padding: '40px 80px' }}>

          {/* Page Header */}
          <div className="flex items-center justify-between">
            <h1 className="text-24-bold text-[var(--grey-01)]">Page Title</h1>
            <div className="flex gap-3">
              <Button variant="secondary">Export</Button>
              <Button variant="primary">+ Add New</Button>
            </div>
          </div>

          {/* Filter Bar */}
          <div className="flex items-center gap-2">
            <InputField placeholder="Search..." className="flex-1" />
            <Select placeholder="Status" />
            <Select placeholder="Date Range" />
          </div>

          {/* Content Card with Table */}
          <Card noBorder>
            <DataTable columns={[...]} rows={[...]} />
          </Card>

        </main>
      </div>
    </div>
  );
}
```

### Type C: Auth / Login Page

```tsx
export default function LoginPage() {
  return (
    <div className="flex w-[1440px] h-screen">
      {/* Left brand panel — ONLY place gradients are allowed */}
      <div className="w-[45%] bg-[#1A1A1A] flex flex-col justify-between p-12">
        <Logo variant="white" />
        <div>{/* Brand imagery */}</div>
      </div>

      {/* Right form panel */}
      <div className="flex-1 bg-white flex items-center justify-center">
        <div className="w-[400px] flex flex-col gap-6">
          <h1 className="text-24-bold text-[var(--grey-01)]">Sign In</h1>
          <InputField label="Email" placeholder="your@email.com" />
          <InputField label="Password" placeholder="••••••••" type="password" />
          <Button variant="primary" size="large" className="w-full">Sign In</Button>
          <p className="text-12-regular text-[var(--grey-06)] text-center">
            Don't have an account? <a className="text-[var(--l-cyan)]">Sign Up</a>
          </p>
        </div>
      </div>
    </div>
  );
}
```

---

## EXECUTION PROTOCOL

### Step 1: Understand Requirements
Same as Figma skill — clarify screens, user roles, data context, primary actions.

### Step 2: Generate Code
- Use React functional components + Tailwind CSS classes
- ALL colors via CSS custom properties (NEVER hardcoded hex in JSX)
- ALL spacing from the 4px grid system
- ALL typography from the defined text style classes
- Apply P1–P17 design principles throughout

### Step 3: Self-Review Checklist
Before declaring done, verify:
- [ ] No hardcoded hex colors — all using `var(--xxx)` or Tailwind theme tokens
- [ ] No arbitrary spacing — all values from 4px grid (4/8/12/16/20/24/32/40/80)
- [ ] Typography hierarchy: titles visually distinct from body (P2, P5)
- [ ] Button heights match in every button group (P16)
- [ ] No nested borders (P9)
- [ ] Color discipline: only greys + cyan/red/orange for states (P1)
- [ ] Form fields consistent (P8): 40px height, radius-md, Grey-12 stroke
- [ ] Card padding from P13 tiers — no arbitrary values
- [ ] Data values ≥ 1.5× label font size (P11)
- [ ] Icons use correct strokeWidth per size (P12)

---

## ICON USAGE IN CODE

Use inline SVGs with correct stroke width per display size. Example:

```tsx
// 16×16 icon with 1.33px stroke (default UI size)
function SettingsIcon({ className = '' }) {
  return (
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" className={className}
         stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
      <path d="M12 15a3 3 0 100-6 3 3 0 000 6z"/>
      <path d="M19.4 15a1.65 1.65 0 00.33 1.82l..." />
    </svg>
  );
}
```

For icon libraries, use [Lucide React](https://lucide.dev/) which matches the L icon style (24×24, 2px stroke line icons). Set `strokeWidth` per P12 size rules.

```tsx
import { Settings, BarChart3, ChevronDown } from 'lucide-react';

// 16px context → strokeWidth 1.33
<Settings size={16} strokeWidth={1.33} />

// 24px context → strokeWidth 2 (default)
<Settings size={24} />
```

---

## EXAMPLE INVOCATIONS

```
/l-coding Build the Account Settings page with profile form, notification toggles, and billing section
/l-coding Create a transaction history table with filters, pagination, and status badges
/l-coding Build the recharge flow: amount selection → payment method → processing → result modal
```
