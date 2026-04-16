# Lanbow V3 — Figma Mode API Patterns

Vibma MCP 调用模板，用于在 Figma 中构建 Lanbow V3 设计。

**前置条件**: 已通过 `mcp__Vibma__connection(method: "create")` + `mcp__Vibma__connection(method: "get")` 建立连接。

---

## VARIABLE LOOKUP STRATEGY

**不要硬编码 Variable ID。** 按名称查找：

```
1. mcp__Vibma__variable_collections(method: "list")
   → 找到 "Lanbow Colors" 的 collectionId

2. mcp__Vibma__variables(method: "list", collectionId: "<id>")
   → 找到各 token 的 variable name

3. 使用 fillVariableName / strokeVariableName 绑定
   → 比 fillVariableId 更稳定，跨文件可用
```

**常用 token name → 用途映射:**

| Variable Name | Usage |
|---|---|
| `LANBOW-Cyan` | Brand primary, CTA, links |
| `Grey-01` | Primary text, headings, primary button fill |
| `Grey-06` | Secondary text, captions |
| `Grey-08` | Placeholder, disabled |
| `Grey-12` | Borders, dividers |
| `Background` | Noise texture base (#F3F3F3) |
| `White` | Card backgrounds |
| `Selected` | Hover states, table headers |
| `Stroke` | Card borders |
| `Red` | Error, danger |
| `Orange` | Warning |

**V3 硬编码颜色**（不在 Variable Collection 中）:

| Hex | Usage | 在代码中直接用 hex |
|---|---|---|
| `#FBF9F4` | Page background | frames.create fill |
| `#F8F6F2` | Sub-card / warm grey | frames.create fill |
| `#2B2933` | Nav text | text.create fill |
| `#E2E0DF` | Nav border | stroke color |

---

## PAGE FRAME

```js
// 1. 创建页面 frame
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Dashboard",
  layoutMode: "VERTICAL",
  width: 1440,
  primaryAxisSizingMode: "AUTO",  // 高度自适应
  fills: [{ type: "SOLID", color: "#FBF9F4", opacity: 0.98 }],
  itemSpacing: 0,
  paddingLeft: 0, paddingRight: 0, paddingTop: 0, paddingBottom: 0,
}])
```

---

## BACKGROUND NOISE TEXTURE

```js
// 2. 全屏噪点背景 (page frame 的第一个子层)
mcp__Vibma__frames(method: "create", type: "rectangle", items: [{
  name: "Background Texture",
  parentId: "<pageFrameId>",
  layoutSizingHorizontal: "FILL",
  height: 2000,  // 足够高，或后续 update
  fills: [{ type: "SOLID", color: "#F3F3F3" }],
  effects: [{ type: "NOISE", visible: true, color: { r: 0, g: 0, b: 0, a: 0.16 } }],
}])
```

---

## CONTAINER (Main Content)

```js
// 3. Container with correct V3 padding
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Container",
  parentId: "<pageFrameId>",
  layoutMode: "VERTICAL",
  layoutSizingHorizontal: "FILL",
  primaryAxisSizingMode: "AUTO",
  itemSpacing: 16,        // --space-base
  paddingTop: 24,         // --space-xl (distance from nav)
  paddingLeft: 48,        // --space-4xl
  paddingRight: 48,       // --space-4xl
  paddingBottom: 40,      // --space-3xl
}])
```

---

## TOP NAV BAR

```js
// 4. Nav bar (absolute positioned)
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Top Nav Bar",
  parentId: "<pageFrameId>",
  layoutMode: "HORIZONTAL",
  width: 1440,
  height: 40,
  layoutPositioning: "ABSOLUTE",
  x: 0, y: 0,
  counterAxisAlignItems: "CENTER",
  primaryAxisAlignItems: "SPACE_BETWEEN",
  paddingLeft: 24, paddingRight: 24,
  itemSpacing: 16,
  strokes: [{ type: "SOLID", color: "#E2E0DF" }],
  strokeWeight: 1,
  strokeAlign: "INSIDE",
  // Only bottom border — use individual stroke weights if available:
  strokeTopWeight: 0, strokeRightWeight: 0, strokeBottomWeight: 1, strokeLeftWeight: 0,
}])

// Nav links
mcp__Vibma__text(method: "create", items: [{
  name: "Nav Home",
  parentId: "<navLeftId>",
  content: "Home",
  fontFamily: "Inter",
  fontStyle: "Semi Bold",
  fontSize: 16,
  fills: [{ type: "SOLID", color: "#2B2933" }],
  opacity: 1.0,  // active
}])

mcp__Vibma__text(method: "create", items: [{
  name: "Nav AI Cake",
  parentId: "<navLeftId>",
  content: "AI Cake",
  fontFamily: "Inter",
  fontStyle: "Semi Bold",
  fontSize: 16,
  fills: [{ type: "SOLID", color: "#2B2933" }],
  opacity: 0.5,  // inactive
}])
```

---

## TYPOGRAPHY — TEXT NODES

### H1 Page Title
```js
mcp__Vibma__text(method: "create", items: [{
  name: "H1 Title",
  parentId: "<containerId>",
  content: "Project Name",
  fontFamily: "Abhaya Libre",
  fontStyle: "Bold",
  fontSize: 36,
  fillVariableName: "Grey-01",
}])
```

### H2 Section Title
```js
mcp__Vibma__text(method: "create", items: [{
  name: "H2 Section",
  parentId: "<cardId>",
  content: "Target Overview",
  fontFamily: "Abhaya Libre",
  fontStyle: "Bold",
  fontSize: 28,
  fillVariableName: "Grey-01",
}])
```

### H3 Card Title (Inter Thin)
```js
mcp__Vibma__text(method: "create", items: [{
  name: "H3 Card Title",
  parentId: "<subCardId>",
  content: "Daily Report",
  fontFamily: "Inter",
  fontStyle: "Thin",
  fontSize: 20,
  fillVariableName: "Grey-01",
}])
```

### KPI Value (Newsreader)
```js
mcp__Vibma__text(method: "create", items: [{
  name: "KPI Value",
  parentId: "<kpiRowId>",
  content: "$248,900",
  fontFamily: "Newsreader",
  fontStyle: "SemiBold",
  fontSize: 32,
  fillVariableName: "Grey-01",
}])
```

### Date (Abhaya Libre Regular)
```js
mcp__Vibma__text(method: "create", items: [{
  name: "Date",
  parentId: "<footerId>",
  content: "2026/03/29",
  fontFamily: "Abhaya Libre",
  fontStyle: "Regular",
  fontSize: 12,
  fillVariableName: "Grey-01",
}])
```

### Body Text
```js
mcp__Vibma__text(method: "create", items: [{
  name: "Body",
  parentId: "<contentId>",
  content: "Description text here...",
  fontFamily: "Inter",
  fontStyle: "Regular",
  fontSize: 12,
  fillVariableName: "Grey-01",
  lineHeight: { value: 150, unit: "PERCENT" },
}])
```

---

## CARD — White Standard

```js
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Card",
  parentId: "<columnId>",
  layoutMode: "VERTICAL",
  layoutSizingHorizontal: "FILL",
  primaryAxisSizingMode: "AUTO",
  itemSpacing: 24,
  paddingTop: 24, paddingRight: 24, paddingBottom: 24, paddingLeft: 24,
  cornerRadius: 0,
  fillVariableName: "White",
}])
```

---

## CARD — Warm Grey Sub-Card

```js
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Report Card",
  parentId: "<subCardsRowId>",
  layoutMode: "VERTICAL",
  layoutSizingHorizontal: "FILL",
  primaryAxisSizingMode: "AUTO",
  primaryAxisAlignItems: "SPACE_BETWEEN",
  minHeight: 200,
  itemSpacing: 16,
  paddingTop: 16, paddingBottom: 16, paddingLeft: 20, paddingRight: 20,
  cornerRadius: 0,
  fills: [{ type: "SOLID", color: "#F8F6F2" }],
}])
```

---

## TWO-COLUMN LAYOUT

```js
// Main content section (horizontal)
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Content Grid",
  parentId: "<containerId>",
  layoutMode: "HORIZONTAL",
  layoutSizingHorizontal: "FILL",
  primaryAxisSizingMode: "AUTO",
  itemSpacing: 16,
}])

// Left column (FILL)
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Left Column",
  parentId: "<contentGridId>",
  layoutMode: "VERTICAL",
  layoutSizingHorizontal: "FILL",
  primaryAxisSizingMode: "AUTO",
  itemSpacing: 16,
}])

// Right column (FIXED 280px)
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Right Column",
  parentId: "<contentGridId>",
  layoutMode: "VERTICAL",
  width: 280,
  layoutSizingHorizontal: "FIXED",
  primaryAxisSizingMode: "AUTO",
  itemSpacing: 24,
  paddingTop: 24, paddingRight: 24, paddingBottom: 24, paddingLeft: 24,
  cornerRadius: 0,
  fillVariableName: "White",
}])
```

---

## BUTTONS

### Primary Button
```js
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Button Primary",
  parentId: "<parentId>",
  layoutMode: "HORIZONTAL",
  counterAxisAlignItems: "CENTER",
  primaryAxisAlignItems: "CENTER",
  paddingTop: 10, paddingBottom: 10, paddingLeft: 24, paddingRight: 24,
  cornerRadius: 0,
  fillVariableName: "Grey-01",
}])
// Then create text child:
mcp__Vibma__text(method: "create", items: [{
  parentId: "<buttonId>",
  content: "Submit",
  fontFamily: "Inter",
  fontStyle: "Medium",
  fontSize: 14,
  fillVariableName: "White",
}])
```

### Secondary Button
```js
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Button Secondary",
  parentId: "<parentId>",
  layoutMode: "HORIZONTAL",
  counterAxisAlignItems: "CENTER",
  primaryAxisAlignItems: "CENTER",
  paddingTop: 10, paddingBottom: 10, paddingLeft: 24, paddingRight: 24,
  cornerRadius: 0,
  strokeVariableName: "Grey-01",
  strokeWeight: 1,
  strokeAlign: "INSIDE",
}])
mcp__Vibma__text(method: "create", items: [{
  parentId: "<buttonId>",
  content: "Cancel",
  fontFamily: "Inter",
  fontStyle: "Medium",
  fontSize: 14,
  fillVariableName: "Grey-01",
}])
```

### "View All" Text Button
```js
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "View All",
  parentId: "<parentId>",
  layoutMode: "HORIZONTAL",
  paddingBottom: 4,
  strokes: [{ type: "SOLID", color: "#181818" }],
  strokeWeight: 1,
  strokeTopWeight: 0, strokeRightWeight: 0, strokeBottomWeight: 1, strokeLeftWeight: 0,
}])
mcp__Vibma__text(method: "create", items: [{
  parentId: "<viewAllId>",
  content: "View All",
  fontFamily: "Inter",
  fontStyle: "Bold",
  fontSize: 10,
  fillVariableName: "Grey-01",
}])
```

---

## DIVIDER LINE

```js
mcp__Vibma__frames(method: "create", type: "rectangle", items: [{
  name: "Divider",
  parentId: "<parentId>",
  layoutSizingHorizontal: "FILL",
  height: 1,
  fillVariableName: "Grey-12",
}])
```

---

## ICONS — Phosphor via Vibma

```js
// Search for an icon
mcp__Vibma__icons(method: "search", query: "arrow right", prefix: "ph")

// Create icon node
mcp__Vibma__icons(method: "create",
  icon: "ph:arrow-right",
  size: 24,
  parentId: "<parentId>",
  colorVariableName: "Grey-01",
)

// Common icons:
// ph:house, ph:chart-bar, ph:gear, ph:bell, ph:arrow-right
// ph:caret-down, ph:list, ph:plus, ph:x, ph:check
// ph:magnifying-glass, ph:funnel, ph:calendar, ph:user
```

---

## NOTIFICATION ITEM PATTERN

```js
// Container for one notification
const notifItem = mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Notification Item",
  parentId: "<notifListId>",
  layoutMode: "VERTICAL",
  layoutSizingHorizontal: "FILL",
  itemSpacing: 4,
}])

// Meta line
mcp__Vibma__text(method: "create", items: [{
  parentId: "<notifItemId>",
  content: "10:42 AM · System Alert",
  fontFamily: "Inter",
  fontStyle: "Regular",
  fontSize: 9,
  fillVariableName: "Grey-06",
}])

// Title
mcp__Vibma__text(method: "create", items: [{
  parentId: "<notifItemId>",
  content: "Budget threshold reached",
  fontFamily: "Inter",
  fontStyle: "Bold",
  fontSize: 16,
  fillVariableName: "Grey-01",
}])

// Body
mcp__Vibma__text(method: "create", items: [{
  parentId: "<notifItemId>",
  content: "Campaign spending has exceeded 80% of monthly budget...",
  fontFamily: "Inter",
  fontStyle: "Regular",
  fontSize: 12,
  fillVariableName: "Grey-01",
  lineHeight: { value: 150, unit: "PERCENT" },
}])
```

---

## FORM INPUT FIELD

```js
// Input container
mcp__Vibma__frames(method: "create", type: "auto_layout", items: [{
  name: "Input Field",
  parentId: "<formId>",
  layoutMode: "HORIZONTAL",
  layoutSizingHorizontal: "FILL",
  height: 40,
  counterAxisAlignItems: "CENTER",
  paddingLeft: 12, paddingRight: 12,
  cornerRadius: 0,
  fillVariableName: "White",
  strokeVariableName: "Grey-12",
  strokeWeight: 1,
  strokeAlign: "INSIDE",
}])

// Placeholder text
mcp__Vibma__text(method: "create", items: [{
  parentId: "<inputId>",
  content: "Enter value...",
  fontFamily: "Inter",
  fontStyle: "Regular",
  fontSize: 14,
  fillVariableName: "Grey-08",
}])
```

---

## TROUBLESHOOTING

| 问题 | 解决方案 |
|---|---|
| `fillVariableName` 不生效 | 先 `variable_collections.list` 确认集合存在，确认 variable name 拼写正确 |
| 字体不可用 | `mcp__Vibma__fonts(method: "list", query: "Abhaya")` 检查字体是否已安装 |
| 连接超时 | 用户需运行 `npx @ufira/vibma-tunnel`，然后重新 `connection.create` + `connection.get` |
| 噪点效果不显示 | NOISE effect 需要在 Rectangle 上设置，不能在 Frame 上 |
| 子节点未出现 | 确认 `parentId` 正确，检查是否被绝对定位的 Nav Bar 遮挡 |
