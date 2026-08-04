---
name: pbi-theme-templates
description: Advanced Power BI Theme JSON per-visual formatting templates and schema reference based on deldersveld/PowerBI-ThemeTemplates. Use when authoring, customizing, or upgrading Power BI report themes with detailed visualStyles properties for cards, line charts, bar charts, matrices, slicers, tables, textboxes, and shapes.
---

# Power BI Theme Templates Canon (`pbi-theme-templates`)

This skill provides comprehensive JSON schema templates and per-visual formatting specifications for Power BI report themes. Based on the community benchmark `deldersveld/PowerBI-ThemeTemplates` (located locally at `D:\Power-bi-design\power-bi-theme\PowerBI-ThemeTemplates`), it centralizes visual styling at the theme level to enforce strict visual hierarchy, brand consistency, and high-performance rendering without cluttering individual `visual.json` files.

> [!NOTE]
> For complete JSON property schemas covering all 34 native Power BI visual types, inspect [references/visual-type-schemas.md](file:///d:/OneDrive%20-%20CPCRT/Job/Power%20BI%20Project/pbip_oryza_price/.agents/skills/pbi-theme-templates/references/visual-type-schemas.md) or the individual template files in `D:\Power-bi-design\power-bi-theme\PowerBI-ThemeTemplates\`.

---

## 🏗️ Theme Architecture & Principles

1. **Theme-First Formatting (DRY - Don't Repeat Yourself):**
   - Place all repeated visual formatting (backgrounds, borders, shadows, headers, fonts, padding) in the theme JSON (`visualStyles`).
   - Reserve `visual.json` instance overrides strictly for one-off business logic or dynamic conditional formatting.

2. **Cascade Hierarchy:**
   `Power BI Defaults` -> `Theme Wildcards (*.*)` -> `Theme Visual-Type Defaults (card.*, lineChart.*)` -> `Visual Instance Overrides`

3. **Restrained Executive Palette:**
   - Primary Accent: 1-2 brand colors
   - Semantic Tokens: Good (`#166534`), Neutral (`#64748B`), Bad (`#991B1B`)
   - Neutral Canvas: Surface `#FFFFFF`, Background `#F8FAFC`, Border `#E2E8F0`

---

## 🎨 Master Visual Property Reference (`visualStyles`)

### 1. Wildcard Defaults (`*.*`)
Controls universal defaults for all visuals (word wrapping, rounded container borders, subtle elevation shadows).

```json
"*": {
  "*": {
    "*": [
      {
        "wordWrap": true
      }
    ],
    "background": [
      {
        "show": true,
        "color": { "solid": { "color": "#FFFFFF" } },
        "transparency": 0
      }
    ],
    "border": [
      {
        "show": true,
        "color": { "solid": { "color": "#E2E8F0" } },
        "radius": 12
      }
    ],
    "dropShadow": [
      {
        "show": true,
        "color": { "solid": { "color": "#0F172A" } },
        "position": "Outer",
        "preset": "BottomRight",
        "shadowBlur": 8,
        "shadowSpread": 0,
        "transparency": 94
      }
    ]
  }
}
```

---

### 2. Page Canvas (`page.*`)
```json
"page": {
  "*": {
    "background": [
      {
        "color": { "solid": { "color": "#F8FAFC" } },
        "transparency": 0
      }
    ]
  }
}
```

---

### 3. KPI Cards (`card.*`)
Configures executive KPI cards with readable callout font size (`22pt - 24pt`) and clean category labels (`11pt`).

```json
"card": {
  "*": {
    "categoryLabels": [
      {
        "color": { "solid": { "color": "#64748B" } },
        "fontSize": 11,
        "fontFamily": "Segoe UI"
      }
    ],
    "labels": [
      {
        "color": { "solid": { "color": "#0F172A" } },
        "fontSize": 22,
        "fontFamily": "Segoe UI Semibold"
      }
    ]
  }
}
```

---

### 4. Slicers (`slicer.*`)
Configures dropdown and list slicers with header background shading, rounded borders (`8px`), and clear font sizing.

```json
"slicer": {
  "*": {
    "border": [
      {
        "show": true,
        "radius": 8,
        "color": { "solid": { "color": "#CBD5E1" } }
      }
    ],
    "header": [
      {
        "show": true,
        "fontColor": { "solid": { "color": "#0F172A" } },
        "background": { "solid": { "color": "#F1F5F9" } },
        "fontSize": 11,
        "fontFamily": "Segoe UI Semibold"
      }
    ],
    "items": [
      {
        "fontColor": { "solid": { "color": "#0F172A" } },
        "fontSize": 11,
        "fontFamily": "Segoe UI"
      }
    ]
  }
}
```

---

### 5. Line Charts (`lineChart.*`)
Configures crisp line charts with 2.5px stroke width, subtle dotted gridlines (`#E2E8F0`), and clean axis labels.

```json
"lineChart": {
  "*": {
    "categoryAxis": [
      {
        "gridlineStyle": "dotted",
        "gridlineColor": { "solid": { "color": "#E2E8F0" } }
      }
    ],
    "valueAxis": [
      {
        "gridlineStyle": "dotted",
        "gridlineColor": { "solid": { "color": "#E2E8F0" } }
      }
    ],
    "lineStyles": [
      {
        "strokeWidth": 2.5
      }
    ]
  }
}
```

---

### 6. Matrices & Tables (`pivotTable.*` & `tableEx.*`)
Configures corporate matrix tables with branded header background (`#0F5C55`), bold white header text, zebra striping (`#F8FAFC`), and clean row padding.

```json
"pivotTable": {
  "*": {
    "grid": [
      {
        "gridVertical": false,
        "gridHorizontal": true,
        "gridHorizontalColor": { "solid": { "color": "#E2E8F0" } },
        "rowPadding": 8
      }
    ],
    "columnHeaders": [
      {
        "fontFamily": "Segoe UI Semibold",
        "fontSize": 11,
        "fontColor": { "solid": { "color": "#FFFFFF" } },
        "backColor": { "solid": { "color": "#0F5C55" } }
      }
    ],
    "values": [
      {
        "fontSize": 11,
        "fontColor": { "solid": { "color": "#0F172A" } },
        "backColorSecondary": { "solid": { "color": "#F8FAFC" } }
      }
    ]
  }
},
"tableEx": {
  "*": {
    "grid": [
      {
        "gridVertical": false,
        "gridHorizontal": true,
        "gridHorizontalColor": { "solid": { "color": "#E2E8F0" } },
        "rowPadding": 8
      }
    ],
    "columnHeaders": [
      {
        "fontFamily": "Segoe UI Semibold",
        "fontSize": 11,
        "fontColor": { "solid": { "color": "#FFFFFF" } },
        "backColor": { "solid": { "color": "#0F5C55" } }
      }
    ],
    "values": [
      {
        "fontSize": 11,
        "fontColor": { "solid": { "color": "#0F172A" } },
        "backColorSecondary": { "solid": { "color": "#F8FAFC" } }
      }
    ]
  }
}
```

---

### 7. Bar & Column Charts (`barChart.*` & `columnChart.*`)
```json
"barChart": {
  "*": {
    "categoryAxis": [
      {
        "gridlineStyle": "dotted",
        "gridlineColor": { "solid": { "color": "#E2E8F0" } }
      }
    ],
    "valueAxis": [
      {
        "gridlineStyle": "dotted",
        "gridlineColor": { "solid": { "color": "#E2E8F0" } }
      }
    ]
  }
},
"columnChart": {
  "*": {
    "categoryAxis": [
      {
        "gridlineStyle": "dotted",
        "gridlineColor": { "solid": { "color": "#E2E8F0" } }
      }
    ],
    "valueAxis": [
      {
        "gridlineStyle": "dotted",
        "gridlineColor": { "solid": { "color": "#E2E8F0" } }
      }
    ]
  }
}
```

---

## ⚠️ Critical Rule: Theme Path Binding Verification (`report.json` Integrity)

Whenever creating, editing, or renaming a Power BI report theme in PBIP/PBIR format:

1. **Verify Physical File vs `report.json` Alignment:**
   - Check the physical JSON filename in `StaticResources/RegisteredResources/` (e.g., `AgroExecutive_Core_Theme9779412528026522.json`).
   - Inspect `definition/report.json` and ensure the following fields match the physical filename EXACTLY:
     - `themeCollection.customTheme.name`
     - `resourcePackages[RegisteredResources].items[0].name`
     - `resourcePackages[RegisteredResources].items[0].path`
   - *Failure to match will cause Power BI Desktop to fail silently and fall back to plain text default styles.*

2. **Dual-Layer Fallback (Explicit `visualContainerObjects`):**
   - To guarantee visual rendering across all client machines, inject explicit `visualContainerObjects` (background `#FFFFFF`, border `#CBD5E1` radius 12px, shadow blur 8px) into key card and container `visual.json` definitions in addition to theme JSON properties.

---

## 🚀 Quality Checklist

When applying or updating Power BI report themes:
- [x] Verify physical filename in `RegisteredResources/` matches `report.json` bindings 100%.
- [x] Validate JSON syntax against Fabric Theme Schema.
- [x] Verify font callouts do not cause text clipping (`...`) in KPI cards.
- [x] Ensure contrast ratio between foreground text and backgrounds meets WCAG 2.1 (minimum 4.5:1).
- [x] Test matrix header background colors against dark and light themes.

