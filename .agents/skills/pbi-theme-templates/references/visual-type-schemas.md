# Power BI Theme Per-Visual JSON Schema Reference

This reference documents the exact JSON property keys and structure for all 34 native Power BI visual types, extracted from the local repository `D:\Power-bi-design\power-bi-theme\PowerBI-ThemeTemplates`.

---

## 1. Global Wildcard Defaults (`*.*`) & Canvas Structural Items
```json
{
  "name": "GlobalLevelTemplate",
  "visualStyles": {
    "*": {
      "*": {
        "title": [{
          "show": true,
          "fontColor": { "solid": { "color": "#0F172A" } },
          "background": { "solid": { "color": "#F1F5F9" } },
          "alignment": "left",
          "fontSize": 14,
          "fontFamily": "Segoe UI Semibold"
        }],
        "background": [{
          "show": true,
          "color": { "solid": { "color": "#FFFFFF" } },
          "transparency": 0
        }],
        "border": [{
          "show": true,
          "color": { "solid": { "color": "#CBD5E1" } },
          "radius": 12
        }],
        "visualHeader": [{
          "foreground": { "solid": { "color": "#0F172A" } },
          "border": { "solid": { "color": "#E2E8F0" } },
          "showTooltipButton": true
        }],
        "dropShadow": [{
          "show": true,
          "color": { "solid": { "color": "#0F172A" } },
          "position": "Outer",
          "preset": "BottomRight",
          "shadowBlur": 8,
          "shadowDistance": 4,
          "shadowSpread": 0,
          "transparency": 94
        }]
      }
    },
    "page": {
      "*": {
        "background": [{
          "color": { "solid": { "color": "#F8FAFC" } },
          "transparency": 0
        }],
        "filterCard": [
          {
            "$id": "Applied",
            "backgroundColor": { "solid": { "color": "#0F5C55" } },
            "foregroundColor": { "solid": { "color": "#FFFFFF" } },
            "borderColor": { "solid": { "color": "#CBD5E1" } },
            "textSize": 11,
            "fontFamily": "Segoe UI",
            "border": true
          },
          {
            "$id": "Available",
            "backgroundColor": { "solid": { "color": "#F1F5F9" } },
            "foregroundColor": { "solid": { "color": "#0F172A" } },
            "borderColor": { "solid": { "color": "#E2E8F0" } },
            "textSize": 10,
            "fontFamily": "Segoe UI",
            "border": true
          }
        ]
      }
    }
  }
}
```

---

## 2. Executive KPI & Cards (`card`, `multiRowCard`, `kpi`)

### KPI Card (`card`)
```json
"card": {
  "*": {
    "categoryLabels": [{
      "color": { "solid": { "color": "#64748B" } },
      "fontSize": 11,
      "fontFamily": "Segoe UI"
    }],
    "labels": [{
      "color": { "solid": { "color": "#0F172A" } },
      "fontSize": 22,
      "fontFamily": "Segoe UI Semibold"
    }]
  }
}
```

### Multi-Row Card (`multiRowCard`)
```json
"multiRowCard": {
  "*": {
    "cardTitle": [{
      "color": { "solid": { "color": "#0F5C55" } },
      "fontSize": 14,
      "fontFamily": "Segoe UI Semibold"
    }],
    "categoryLabels": [{
      "color": { "solid": { "color": "#64748B" } },
      "fontSize": 11,
      "fontFamily": "Segoe UI"
    }],
    "cardData": [{
      "color": { "solid": { "color": "#0F172A" } },
      "fontSize": 18,
      "fontFamily": "Segoe UI Semibold"
    }]
  }
}
```

### Standard KPI Visual (`kpi`)
```json
"kpi": {
  "*": {
    "indicator": [{
      "fontFamily": "Segoe UI Semibold",
      "fontSize": 24
    }],
    "trendLine": [{
      "show": true,
      "weight": 2.5
    }],
    "goals": [{
      "show": true,
      "fontFamily": "Segoe UI",
      "fontSize": 10
    }]
  }
}
```

---

## 3. Tables & Matrices (`tableEx`, `pivotTable`)

```json
"pivotTable": {
  "*": {
    "grid": [{
      "gridVertical": false,
      "gridHorizontal": true,
      "gridHorizontalColor": { "solid": { "color": "#E2E8F0" } },
      "rowPadding": 8
    }],
    "columnHeaders": [{
      "fontFamily": "Segoe UI Semibold",
      "fontSize": 11,
      "fontColor": { "solid": { "color": "#FFFFFF" } },
      "backColor": { "solid": { "color": "#0F5C55" } }
    }],
    "rowHeaders": [{
      "fontFamily": "Segoe UI Semibold",
      "fontSize": 11,
      "fontColor": { "solid": { "color": "#0F172A" } }
    }],
    "values": [{
      "fontSize": 11,
      "fontColor": { "solid": { "color": "#0F172A" } },
      "backColorSecondary": { "solid": { "color": "#F8FAFC" } }
    }]
  }
}
```

---

## 4. Combo Charts (`lineClusteredColumnComboChart`, `lineStackedColumnComboChart`)

```json
"lineClusteredColumnComboChart": {
  "*": {
    "legend": [{
      "show": true,
      "position": "Top",
      "fontFamily": "Segoe UI",
      "fontSize": 10
    }],
    "categoryAxis": [{
      "show": true,
      "gridlineStyle": "dotted",
      "gridlineColor": { "solid": { "color": "#E2E8F0" } }
    }],
    "valueAxis": [{
      "show": true,
      "alignZeros": true,
      "gridlineStyle": "dotted",
      "gridlineColor": { "solid": { "color": "#E2E8F0" } },
      "secShow": true,
      "secPosition": "Right"
    }],
    "lineStyles": [{
      "strokeWidth": 2.5,
      "showMarker": true,
      "markerShape": "circle",
      "markerSize": 4
    }]
  }
}
```

---

## 5. Tree & Advanced Visuals (`decompositionTree`, `treemap`, `waterfallChart`, `gauge`)

### Decomposition Tree (`decompositionTree`)
```json
"decompositionTree": {
  "*": {
    "tree": [{
      "density": "Compact",
      "connectorColor": { "solid": { "color": "#CBD5E1" } }
    }],
    "node": [{
      "fontFamily": "Segoe UI",
      "fontSize": 11
    }]
  }
}
```

### Waterfall Chart (`waterfallChart`)
```json
"waterfallChart": {
  "*": {
    "sentimentColors": [{
      "increaseColor": { "solid": { "color": "#166534" } },
      "decreaseColor": { "solid": { "color": "#991B1B" } },
      "totalColor": { "solid": { "color": "#0F5C55" } }
    }]
  }
}
```

---

## 6. Slicers (`slicer`)
```json
"slicer": {
  "*": {
    "border": [{
      "show": true,
      "radius": 8,
      "color": { "solid": { "color": "#CBD5E1" } }
    }],
    "header": [{
      "show": true,
      "fontColor": { "solid": { "color": "#0F172A" } },
      "background": { "solid": { "color": "#F1F5F9" } },
      "fontSize": 11,
      "fontFamily": "Segoe UI Semibold"
    }],
    "items": [{
      "fontColor": { "solid": { "color": "#0F172A" } },
      "fontSize": 11,
      "fontFamily": "Segoe UI"
    }]
  }
}
```
