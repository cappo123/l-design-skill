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

## COPY RULES (ALL MODES)

These rules apply to **every UI text string** generated in both Figma mode and Frontend mode.

### Rule 1: No Emoji
Never add emoji to any UI copy — buttons, labels, titles, nav items, notifications, placeholders, helper text, or any other UI element. Emoji are prohibited unless explicitly present in the user's original design spec.

❌ `"✅ Confirm"` `"🔔 Notifications"` `"📊 Analytics"`
✅ `"Confirm"` `"Notifications"` `"Analytics"`

### Rule 2: English Only
All UI copy must be in English regardless of the language the user communicates in. When a user describes requirements in Chinese (or any other language), translate all interface text to natural English before output.

❌ `"确认"` `"通知"` `"数据分析"`
✅ `"Confirm"` `"Notifications"` `"Analytics"`

### Rule 3: Text Length Rules

**Title / Label class** (buttons, nav items, tags, card titles, section headers, tab labels):
- Maximum **3 words**
- Keep concise and action-oriented

❌ `"Add a New Campaign Item"` `"View All Notifications"` `"Export Report Data"`
✅ `"Add Item"` `"View All"` `"Export"`

**Content class** (descriptions, body text, table cells, notification body, helper text):
- No word count limit
- Must truncate at container boundary — overflow renders as `…`
- Frontend: use truncation utilities (see `frontend.md`)
- Figma: set text node to fixed-width with text truncation enabled

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

### P4-P15: Same as V1
All other design principles from V1 carry over — spacing grid, Gestalt hierarchy, button discipline, form consistency, card padding rules, no nested borders, selection indicators, data value contrast, icon stroke integrity, card internal padding, title-divider balance, no double padding nesting, secondary button standard, select/filter dropdown standard.

**Exception to P8 (Form Field Consistency):** Corner radius on inputs is now 0px instead of radius-medium (6px).

**Exception to P16 (Secondary Button):** Corner radius on buttons is now 0px instead of radius-large (8px).

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
- [ ] No emoji in any UI copy
- [ ] All UI copy is in English
- [ ] Title / label text ≤ 3 words (buttons, nav items, tags, section headers)
- [ ] Content text has truncation applied where overflow is possible
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
