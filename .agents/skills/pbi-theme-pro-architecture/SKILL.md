---
name: pbi-theme-pro-architecture
description: Professional Power BI Theme Architecture, Cascading Hierarchy, AI Visuals (Key Influencers), Filter Pane Outspace, and Action Buttons based on Apress Pro Power BI Theme Creation by Adam Aspin. Use when designing enterprise theme cascading rules, formatting AI/Script visuals, action buttons, filter card panes, or full production base themes.
---

# Pro Power BI Theme Architecture (`pbi-theme-pro-architecture`)

Based on the authoritative reference **"Pro Power BI Theme Creation"** by Adam Aspin (Apress Publishing), this skill governs enterprise theme cascading, font hierarchy, AI Visual formatting (Key Influencers), Filter Pane Outspace customization, and Action Button styling.

---

## 🏗️ 1. Theme Style Cascading & Font Scale Strategy

Theme cascading allows properties to flow from global wildcards (`*`) down to visual categories and specific visual properties without code duplication.

### Typography Scale (Text Classes Hierarchy)
Power BI supports 4 standard text classes that propagate automatically across all visual headers, titles, cards, and labels:

```json
"textClasses": {
  "title": {
    "fontFace": "Segoe UI Semibold",
    "fontSize": 16,
    "color": "#0F172A"
  },
  "header": {
    "fontFace": "Segoe UI Semibold",
    "fontSize": 14,
    "color": "#0F172A"
  },
  "label": {
    "fontFace": "Segoe UI",
    "fontSize": 11,
    "color": "#475569"
  },
  "callout": {
    "fontFace": "Segoe UI Semibold",
    "fontSize": 22,
    "color": "#0F5C55"
  }
}
```

---

## 🤖 2. AI Visuals Theme Formatting (`keyDriversVisual`)

Configures Power BI's AI **Key Influencers** visual (`keyDriversVisual`) with branded primary/secondary colors, canvas colors, and reference line highlights.

```json
"keyDriversVisual": {
  "*": {
    "general": [{ "responsive": true }],
    "keyDrivers": [{
      "allowKeyDrivers": true,
      "allowProfiles": true,
      "allowKeyDriversCounting": true,
      "countType": "relative"
    }],
    "keyInfluencersVisual": [{
      "primaryColor": { "solid": { "color": "#0F5C55" } },
      "primaryFontColor": { "solid": { "color": "#0F172A" } },
      "secondaryColor": { "solid": { "color": "#D97706" } },
      "secondaryFontColor": { "solid": { "color": "#1E293B" } },
      "canvasColor": { "solid": { "color": "#F8FAFC" } },
      "fontColor": { "solid": { "color": "#FFFFFF" } }
    }],
    "keyDriversDrillVisual": [{
      "defaultColor": { "solid": { "color": "#0F5C55" } },
      "referenceLineColor": { "solid": { "color": "#D97706" } }
    }]
  }
}
```

---

## 🎛️ 3. Filter Pane & Outspace Customization (`outspacePane` & `filterCard`)

Configures the right-hand **Filter Pane** (`outspacePane`) and individual **Applied / Available Filter Cards** (`filterCard`) so they match the executive report design.

```json
"page": {
  "*": {
    "outspacePane": [{
      "backgroundColor": { "solid": { "color": "#F1F5F9" } },
      "transparency": 0,
      "foregroundColor": { "solid": { "color": "#0F172A" } },
      "titleSize": 16,
      "headerSize": 11,
      "fontFamily": "Segoe UI Semibold",
      "border": true,
      "borderColor": { "solid": { "color": "#CBD5E1" } },
      "width": 200,
      "checkboxAndApplyColor": { "solid": { "color": "#0F5C55" } },
      "inputBoxColor": { "solid": { "color": "#FFFFFF" } }
    }],
    "filterCard": [
      {
        "$id": "Applied",
        "backgroundColor": { "solid": { "color": "#0F5C55" } },
        "transparency": 0,
        "border": true,
        "borderColor": { "solid": { "color": "#0F5C55" } },
        "foregroundColor": { "solid": { "color": "#FFFFFF" } },
        "textSize": 10,
        "fontFamily": "Segoe UI Semibold",
        "inputBoxColor": { "solid": { "color": "#134E4A" } }
      },
      {
        "$id": "Available",
        "backgroundColor": { "solid": { "color": "#FFFFFF" } },
        "transparency": 0,
        "border": true,
        "borderColor": { "solid": { "color": "#E2E8F0" } },
        "foregroundColor": { "solid": { "color": "#475569" } },
        "textSize": 10,
        "fontFamily": "Segoe UI",
        "inputBoxColor": { "solid": { "color": "#F8FAFC" } }
      }
    ]
  }
}
```

---

## 🔘 4. Action Buttons Theme Formatting (`actionButton`)

Configures interactive page buttons, bookmarks, and back buttons (`actionButton`).

```json
"actionButton": {
  "*": {
    "line": [{
      "color": { "solid": { "color": "#CBD5E1" } },
      "weight": 1.5
    }],
    "fill": [{
      "show": true,
      "transparency": 0,
      "fillColor": { "solid": { "color": "#FFFFFF" } }
    }],
    "text": [{
      "show": true,
      "fontColor": { "solid": { "color": "#0F172A" } },
      "fontFamily": "Segoe UI Semibold",
      "fontSize": 11
    }]
  }
}
```

---

## 🚀 Enterprise Theme Creation Protocol

When authoring enterprise themes based on this architecture:
1. Always start from a clean `textClasses` foundation.
2. Define universal `*.*` wildcard container defaults (background `#FFFFFF`, border `#CBD5E1` radius 12px, shadow blur 8px).
3. Customize Filter Pane (`outspacePane` and `filterCard.Applied`/`Available`) to ensure non-disruptive navigation.
4. Set AI Visual defaults (`keyDriversVisual`) when using Copilot or Key Influencers visuals.
