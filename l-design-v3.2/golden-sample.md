# Lanbow V3 — Golden Sample

完整的 Dashboard 页面输出示例，作为质量对照基准。

---

## FRONTEND MODE — Dashboard HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Lanbow V3 Dashboard</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Abhaya+Libre:wght@400;700&family=Inter:wght@100;400;500;600;700&family=Newsreader:wght@600&display=swap" rel="stylesheet">
  <script src="https://unpkg.com/@phosphor-icons/web"></script>
  <style>
    /* === Tokens === */
    :root {
      --lanbow-cyan: #00B1A2;
      --grey-01: #181818;
      --grey-06: #626262;
      --grey-08: #999999;
      --grey-12: #EEEEEE;
      --background: #F3F3F3;
      --white: #FFFFFF;
      --selected: rgba(0,0,0,0.02);
      --stroke: rgba(0,0,0,0.04);
      --red: #FF2D55;
      --page-bg: #FBF9F4;
      --nav-text: #2B2933;
      --nav-border: #E2E0DF;
      --card-warm-grey: #F8F6F2;
      --font-display: 'Abhaya Libre', serif;
      --font-data: 'Newsreader', serif;
      --font-ui: 'Inter', sans-serif;
      --space-xs: 4px;
      --space-sm: 8px;
      --space-md: 12px;
      --space-base: 16px;
      --space-lg: 20px;
      --space-xl: 24px;
      --space-2xl: 32px;
      --space-3xl: 40px;
      --space-4xl: 48px;
      --shadow-none: none;
      --shadow-md: 0 4px 12px rgba(0,0,0,0.06), 0 2px 4px rgba(0,0,0,0.04);
      --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
    }

    /* === Reset === */
    * { margin: 0; padding: 0; box-sizing: border-box; }

    /* === Page === */
    .lanbow-page {
      min-height: 100vh;
      background-color: var(--page-bg);
      position: relative;
    }

    /* === Nav === */
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
    }
    .nav-left { gap: var(--space-xl); }
    .nav-right { gap: var(--space-sm); }
    .nav-logo {
      font-family: var(--font-display);
      font-weight: 700;
      font-size: 16px;
      color: var(--grey-01);
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
    .nav-link {
      font-family: var(--font-ui);
      font-weight: 600;
      font-size: 16px;
      color: var(--nav-text);
      text-decoration: none;
      opacity: 0.5;
      transition: opacity 0.15s var(--ease-out);
    }
    .nav-link:hover { opacity: 0.8; }
    .nav-link.active { opacity: 1; }
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

    /* === Container === */
    .lanbow-container {
      max-width: 1400px;
      margin: 0 auto;
      padding: var(--space-xl) var(--space-4xl) var(--space-3xl);
      display: flex;
      flex-direction: column;
      gap: var(--space-base);
    }

    /* === Typography === */
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
      font-weight: 100;
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
    .trend-text {
      font-family: var(--font-ui);
      font-size: 12px;
      color: var(--lanbow-cyan);
      margin-left: var(--space-sm);
    }

    /* === Layout === */
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

    /* === Card === */
    .card {
      background: var(--white);
      padding: var(--space-xl);
      border-radius: 0;  /* V3: sharp corners */
      box-shadow: var(--shadow-none);  /* V3: no shadow on grounded cards */
    }
    .card-warm {
      background: var(--card-warm-grey);
      padding: var(--space-base) var(--space-lg);
      border-radius: 0;
    }

    /* === Components === */
    .kpi-row {
      display: flex;
      align-items: baseline;
      gap: var(--space-xs);
      margin-top: var(--space-sm);
    }
    .subcards-row {
      display: flex;
      gap: var(--space-base);
    }
    .subcards-row > * {
      flex: 1;
    }
    .report-card {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      min-height: 200px;
    }
    .report-card-content {
      display: flex;
      flex-direction: column;
      gap: var(--space-base);
    }
    .report-card-footer {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .report-card-meta {
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .badge-new {
      font-family: var(--font-ui);
      font-weight: 700;
      font-size: 9px;
      color: var(--lanbow-cyan);
    }
    .divider {
      height: 1px;
      background: var(--grey-12);
    }
    .notification-item {
      display: flex;
      flex-direction: column;
      gap: var(--space-xs);
    }
    .notification-title {
      font-family: var(--font-ui);
      font-weight: 700;
      font-size: 16px;
      color: var(--grey-01);
    }
    .notification-list {
      display: flex;
      flex-direction: column;
      gap: var(--space-xl);
    }
    .btn-view-all {
      font-family: var(--font-ui);
      font-weight: 700;
      font-size: 10px;
      color: var(--grey-01);
      border: none;
      background: none;
      padding: 0 0 4px;
      border-bottom: 1px solid var(--grey-01);
      cursor: pointer;
    }
    .section-header {
      display: flex;
      flex-direction: column;
      gap: var(--space-xl);
    }
  </style>
</head>
<body>
  <div class="lanbow-page">
    <!-- Top Nav -->
    <nav class="lanbow-nav">
      <div class="nav-left">
        <div class="nav-logo">Lanbow</div>
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
      <h1 class="h1-title">Project Alpha</h1>

      <div class="content-grid">
        <!-- Left Column -->
        <div class="left-column">
          <!-- KPI Card -->
          <div class="card">
            <div class="section-header">
              <h2 class="h2-title">Target Overview</h2>
              <div style="display: flex; gap: var(--space-3xl);">
                <div>
                  <span class="label-text">Spend Progress</span>
                  <div class="kpi-row">
                    <span class="kpi-value">$248,900</span>
                    <span class="kpi-secondary">/$500,000</span>
                  </div>
                </div>
                <div>
                  <span class="label-text">ROI Trend</span>
                  <div class="kpi-row">
                    <span class="kpi-value">1.85</span>
                    <span class="trend-text">&#8593; 0.12 vs last week</span>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Reports Card -->
          <div class="card">
            <h2 class="h2-title">Reports</h2>
            <div class="subcards-row" style="margin-top: var(--space-xl);">
              <div class="card-warm report-card">
                <div class="report-card-content">
                  <h3 class="h3-title">Daily Report</h3>
                  <div class="divider"></div>
                  <p class="body-text">Campaign performance metrics and spend analysis for today's active campaigns.</p>
                </div>
                <div class="report-card-footer">
                  <div class="report-card-meta">
                    <span class="badge-new">New</span>
                    <span class="date-text">2026/03/29</span>
                  </div>
                  <i class="ph ph-arrow-right" style="font-size: 16px; color: var(--grey-01);"></i>
                </div>
              </div>
              <div class="card-warm report-card">
                <div class="report-card-content">
                  <h3 class="h3-title">Weekly Report</h3>
                  <div class="divider"></div>
                  <p class="body-text">Aggregated weekly insights across all channels with trend analysis.</p>
                </div>
                <div class="report-card-footer">
                  <div class="report-card-meta">
                    <span class="date-text">2026/03/24</span>
                  </div>
                  <i class="ph ph-arrow-right" style="font-size: 16px; color: var(--grey-01);"></i>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Right Column -->
        <aside class="right-column">
          <div class="card" style="height: 100%;">
            <div style="display: flex; flex-direction: column; gap: var(--space-xl); height: 100%;">
              <h2 class="h2-title">Notifications</h2>
              <div class="divider"></div>
              <div class="notification-list" style="flex: 1;">
                <div class="notification-item">
                  <span class="meta-text">10:42 AM &middot; System Alert</span>
                  <span class="notification-title">Budget threshold reached</span>
                  <span class="body-text">Campaign "Summer Sale" has exceeded 80% of monthly budget allocation.</span>
                </div>
                <div class="notification-item">
                  <span class="meta-text">09:15 AM &middot; Performance</span>
                  <span class="notification-title">ROI improvement detected</span>
                  <span class="body-text">Channel optimization has improved overall ROI by 12% this week.</span>
                </div>
              </div>
              <button class="btn-view-all">View All</button>
            </div>
          </div>
        </aside>
      </div>
    </main>
  </div>
</body>
</html>
```

---

## QUALITY CHECKLIST FOR THIS SAMPLE

Verify these are all correct:

- [x] Corner radius: 0px everywhere (cards, buttons, inputs)
- [x] H1: Abhaya Libre Bold 36px
- [x] H2: Abhaya Libre Bold 28px
- [x] H3: Inter Thin 20px
- [x] KPI: Newsreader SemiBold 32px / 20px
- [x] Date: Abhaya Libre Regular 12px
- [x] Body: Inter Regular 12px
- [x] Meta: Inter Regular 9px, Grey-06
- [x] Page background: #FBF9F4 (not white)
- [x] Cards: White, no shadow, no radius
- [x] Sub-cards: #F8F6F2 warm grey
- [x] Nav: opacity 1.0 active, 0.5 inactive
- [x] Container padding: 24/48/48/40
- [x] Container gap: 16px
- [x] No sidebar icon menu
- [x] Icons: Phosphor (ph- prefix)
- [x] All colors via CSS variables
- [x] Shadows only on floating layers (none used here)

---

## FIGMA MODE — Structure Reference

For the same dashboard in Figma, the node tree should look like:

```
Dashboard (AUTO_LAYOUT, VERTICAL, 1440×auto, fill: #FBF9F4)
├── Background Texture (RECTANGLE, 1440×2000, fill: #F3F3F3, effect: NOISE)
├── Container (AUTO_LAYOUT, VERTICAL, FILL, gap: 16, padding: 24/48/48/40)
│   ├── H1 "Project Alpha" (TEXT, Abhaya Libre Bold 36px, fill: Grey-01)
│   ├── Content Grid (AUTO_LAYOUT, HORIZONTAL, FILL, gap: 16)
│   │   ├── Left Column (AUTO_LAYOUT, VERTICAL, FILL, gap: 16)
│   │   │   ├── KPI Card (AUTO_LAYOUT, VERTICAL, FILL, fill: White, padding: 24)
│   │   │   │   ├── H2 "Target Overview" (Abhaya Libre Bold 28px)
│   │   │   │   └── KPI Row (HORIZONTAL, gap: 40)
│   │   │   │       ├── KPI Group (VERTICAL)
│   │   │   │       │   ├── Label "Spend Progress" (Inter Regular 16px)
│   │   │   │       │   └── Value Row (HORIZONTAL, baseline)
│   │   │   │       │       ├── "$248,900" (Newsreader SemiBold 32px)
│   │   │   │       │       └── "/$500,000" (Newsreader SemiBold 20px)
│   │   │   │       └── KPI Group (VERTICAL)
│   │   │   │           ├── Label "ROI Trend" (Inter Regular 16px)
│   │   │   │           └── Value Row
│   │   │   │               ├── "1.85" (Newsreader SemiBold 32px)
│   │   │   │               └── "↑ 0.12 vs last week" (Inter Regular 12px, Cyan)
│   │   │   └── Reports Card (AUTO_LAYOUT, VERTICAL, FILL, fill: White, padding: 24)
│   │   │       ├── H2 "Reports"
│   │   │       └── Sub-cards Row (HORIZONTAL, gap: 16)
│   │   │           ├── Report Card (FILL, fill: #F8F6F2, padding: 16/20, minH: 200)
│   │   │           └── Report Card (FILL, fill: #F8F6F2, padding: 16/20, minH: 200)
│   │   └── Right Column (AUTO_LAYOUT, VERTICAL, FIXED 280px, fill: White, padding: 24)
│   │       ├── H2 "Notifications"
│   │       ├── Divider (RECTANGLE, FILL×1, fill: Grey-12)
│   │       ├── Notification Items (VERTICAL, gap: 24)
│   │       └── "View All" button
├── Top Nav Bar (AUTO_LAYOUT, HORIZONTAL, ABSOLUTE, 1440×40, stroke-bottom: #E2E0DF)
│   ├── Nav Left (HORIZONTAL, gap: 24)
│   │   ├── Logo "Lanbow"
│   │   ├── Divider (1×16)
│   │   └── Nav Links (HORIZONTAL, gap: 36)
│   │       ├── "Home" (Inter SemiBold 16px, opacity: 1.0)
│   │       ├── "AI Cake" (opacity: 0.5)
│   │       └── "Bills" (opacity: 0.5)
│   └── Nav Right (HORIZONTAL, gap: 8)
│       ├── Avatar (24×24, circle)
│       └── "USER NAME" (Inter 13px, Grey-06)
```
