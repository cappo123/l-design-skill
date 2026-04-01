# Lanbow V3 — Frontend Mode Patterns

CSS classes, React component examples, noise texture, and icon usage for frontend implementation.

---

## COMPLETE CSS VARIABLES BLOCK

Add this to your project's root stylesheet. This is the single source of truth for all tokens in frontend mode.

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
  --space-2xs: 2px;
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 12px;
  --space-base: 16px;
  --space-lg: 20px;
  --space-xl: 24px;
  --space-2xl: 32px;
  --space-3xl: 40px;
  --space-4xl: 48px;

  /* === Container (responsive — see layout.md for breakpoint overrides) === */
  --container-max-width: 1400px;
  --container-padding-top: 24px;
  --container-padding-x: 48px;
  --container-padding-bottom: 40px;
  --container-gap: 16px;

  /* === Shadows (floating layers only) === */
  --shadow-none: none;
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
  --shadow-md: 0 4px 12px rgba(0,0,0,0.06), 0 2px 4px rgba(0,0,0,0.04);
  --shadow-lg: 0 8px 24px rgba(0,0,0,0.08), 0 4px 8px rgba(0,0,0,0.04);
  --shadow-overlay: 0 16px 48px rgba(0,0,0,0.10), 0 8px 16px rgba(0,0,0,0.06);

  /* === Easing === */
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in: cubic-bezier(0.55, 0, 1, 0.45);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
  --spring: cubic-bezier(0.34, 1.56, 0.64, 1);
}
```

---

## COMPLETE TAILWIND CONFIG

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
        none: '0px',
      },
      boxShadow: {
        'none': 'none',
        'sm': '0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04)',
        'md': '0 4px 12px rgba(0,0,0,0.06), 0 2px 4px rgba(0,0,0,0.04)',
        'lg': '0 8px 24px rgba(0,0,0,0.08), 0 4px 8px rgba(0,0,0,0.04)',
        'overlay': '0 16px 48px rgba(0,0,0,0.10), 0 8px 16px rgba(0,0,0,0.06)',
      },
    },
  },
}
```

---

## TYPOGRAPHY UTILITY CLASSES

```css
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

---

## PAGE LAYOUT (HTML/CSS)

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
  padding: 0 var(--space-xl);
  height: 40px;
  border-bottom: 1px solid var(--nav-border);
  background: transparent;
  backdrop-filter: blur(8px);
}
.nav-left, .nav-right {
  display: flex;
  align-items: center;
  gap: var(--space-xl);
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
  gap: var(--space-sm);
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
  flex-direction: column;
  gap: var(--space-base);
}
@media (min-width: 1024px) {
  .content-grid {
    flex-direction: row;
  }
}
.left-column {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--space-base);
}
.right-column {
  flex-shrink: 0;
}
@media (min-width: 1024px) {
  .right-column { width: 240px; }
}
@media (min-width: 1280px) {
  .right-column { width: 280px; }
}

/* Card */
.card {
  background: var(--white);
  padding: var(--space-xl);
  border-radius: 0;
  box-shadow: var(--shadow-none);
}
.card-warm {
  background: var(--card-warm-grey);
  padding: var(--space-base) var(--space-lg);
  border-radius: 0;
}
```

---

## REACT COMPONENT EXAMPLES

### KPI Card
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

### Notification Item
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

### Report Sub-Card
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

---

## PHOSPHOR ICONS — FRONTEND USAGE

**React:**
```tsx
import { House, ChartBar, Gear, Bell, ArrowRight, CaretDown, List } from '@phosphor-icons/react'

// Regular weight (default), size matches design
<House size={24} color="var(--grey-01)" />
<Bell size={20} color="var(--grey-01)" />
<CaretDown size={12} color="var(--grey-08)" />
<List size={24} color="var(--grey-01)" />  {/* hamburger menu */}
```

**Vanilla HTML (via CDN):**
```html
<i class="ph ph-house" style="font-size: 24px; color: var(--grey-01);"></i>
<i class="ph ph-bell" style="font-size: 20px; color: var(--grey-01);"></i>
```

---

## NOISE TEXTURE BACKGROUND

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
```css
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
