---
name: pbi-modern-visuals-patterns
description: Microsoft Power BI Modern Visual PBIR Templates and Patterns (New Card Visual, Advanced Slicer, Page Navigator, Visual Calculations, and AI Narratives) extracted from Microsoft's official Power BI Visuals sample repository. Use when building or formatting New Card Visuals (cardVisual), Advanced Button Slicers (advancedSlicerVisual), Page Navigators (pageNavigator), or Analytics Reference Lines in PBIR format.
---

# Microsoft Power BI Modern Visual Patterns (`pbi-modern-visuals-patterns`)

Extracted directly from Microsoft's official **Power BI Visuals (`Power BI Visuals.pbip`)** sample project (`D:\Power-bi-design\ตัวอย่างกราฟ pbip`), this skill documents PBIR JSON schemas and implementation patterns for modern Power BI visual types.

---

## 🎴 1. New Card Visual (`cardVisual`)

The New Card Visual (`cardVisual`) allows displaying multiple metrics inside a single card container with rich layouts, callouts, sub-labels, and background formatting.

### Schema Definition
```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.11.0/schema.json",
  "name": "modern_card_visual",
  "position": {
    "x": 280,
    "y": 90,
    "z": 1000,
    "height": 120,
    "width": 1200,
    "tabOrder": 1000
  },
  "visual": {
    "visualType": "cardVisual",
    "query": {
      "queryState": {
        "Data": {
          "projections": [
            {
              "field": {
                "Measure": {
                  "Expression": { "SourceRef": { "Entity": "DAX" } },
                  "Property": "Average Price"
                }
              },
              "queryRef": "DAX.Average Price",
              "nativeQueryRef": "Average Price"
            },
            {
              "field": {
                "Measure": {
                  "Expression": { "SourceRef": { "Entity": "DAX" } },
                  "Property": "Avg Price WoW%"
                }
              },
              "queryRef": "DAX.Avg Price WoW%",
              "nativeQueryRef": "Weekly % Change"
            }
          ]
        }
      }
    },
    "objects": {
      "callout": [
        {
          "properties": {
            "fontFamily": { "expr": { "Literal": { "Value": "'Segoe UI Semibold'" } } },
            "fontSize": { "expr": { "Literal": { "Value": "22D" } } },
            "color": { "solid": { "color": { "expr": { "Literal": { "Value": "'#0F172A'" } } } } }
          }
        }
      ]
    },
    "visualContainerObjects": {
      "background": [
        {
          "properties": {
            "show": { "expr": { "Literal": { "Value": "true" } } },
            "color": { "solid": { "color": { "expr": { "Literal": { "Value": "'#FFFFFF'" } } } } },
            "transparency": { "expr": { "Literal": { "Value": "0D" } } }
          }
        }
      ],
      "border": [
        {
          "properties": {
            "show": { "expr": { "Literal": { "Value": "true" } } },
            "color": { "solid": { "color": { "expr": { "Literal": { "Value": "'#CBD5E1'" } } } } },
            "radius": { "expr": { "Literal": { "Value": "12D" } } }
          }
        }
      ],
      "dropShadow": [
        {
          "properties": {
            "show": { "expr": { "Literal": { "Value": "true" } } },
            "preset": { "expr": { "Literal": { "Value": "'BottomRight'" } } },
            "shadowBlur": { "expr": { "Literal": { "Value": "8D" } } }
          }
        }
      ]
    }
  }
}
```

---

## 🔘 2. Advanced Button Slicer (`advancedSlicerVisual`)

The Advanced Slicer (`advancedSlicerVisual`) provides grid/tile button selection with custom orientation, row/column counts, and tile shapes.

### Key Layout Options
- `orientation`: `0D` = Vertical, `1D` = Horizontal Grid
- `maxTiles`: Number of visible button tiles

```json
{
  "visualType": "advancedSlicerVisual",
  "query": {
    "queryState": {
      "Values": {
        "projections": [
          {
            "field": {
              "Column": {
                "Expression": { "SourceRef": { "Entity": "Dm_Category" } },
                "Property": "Category Eng"
              }
            },
            "queryRef": "Dm_Category.Category Eng",
            "nativeQueryRef": "Category Eng"
          }
        ]
      }
    }
  },
  "objects": {
    "layout": [
      {
        "properties": {
          "orientation": { "expr": { "Literal": { "Value": "1D" } } },
          "maxTiles": { "expr": { "Literal": { "Value": "6L" } } }
        }
      }
    ]
  }
}
```

---

## 🧭 3. Page Navigator (`pageNavigator`) & Bookmark Navigator

Native page navigation bar that renders buttons for all report pages automatically.

```json
{
  "visualType": "pageNavigator",
  "objects": {
    "pages": [
      {
        "properties": {
          "showHiddenPages": { "expr": { "Literal": { "Value": "false" } } },
          "showTooltipPages": { "expr": { "Literal": { "Value": "false" } } }
        }
      }
    ]
  }
}
```

---

## 📈 4. Analytics Reference Lines & Forecast

Configures constant lines, min/max/average lines, and trend lines in line and combo charts:

```json
"objects": {
  "y1AxisReferenceLine": [
    {
      "properties": {
        "show": { "expr": { "Literal": { "Value": "true" } } },
        "style": { "expr": { "Literal": { "Value": "'dashed'" } } },
        "color": { "solid": { "color": { "expr": { "Literal": { "Value": "'#D97706'" } } } } },
        "transparency": { "expr": { "Literal": { "Value": "0D" } } },
        "displayName": { "expr": { "Literal": { "Value": "'Market Benchmark'" } } }
      }
    }
  ]
}
```

---

## ⚡ 5. Visual Calculations (`visualCalculations`)

Visual Calculations allow writing context-aware DAX directly at the visual container level (e.g. `RUNNINGSUM()`, `PREVIOUS()`, `MOVINGAVERAGE()`):

```dax
// Visual Calculation Example for Price Momentum
Rolling_30D_Avg = MOVINGAVERAGE([Average Price], 30)
YoY_Growth = [Average Price] - OFFSET(-1, HIGHESTPARENT)
```
