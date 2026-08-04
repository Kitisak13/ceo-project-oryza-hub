---
name: pbi-modern-visuals-patterns
description: Microsoft Power BI Modern Visual PBIR Templates and Patterns (New Card Visual, Advanced Slicer, Page Navigator, Visual Calculations, UX Trend Highlighting, Searchable Card Grids, Data Flags, and AI Narratives) extracted from Microsoft's official Power BI Visuals sample repository and Bas Visual Design repo. Use when building or formatting New Card Visuals (cardVisual), Advanced Button Slicers (advancedSlicerVisual), Searchable Card Grids (cardVisual + textSlicer), Data Flags, Page Navigators (pageNavigator), Analytics Reference Lines, or Visual Calculations UX in PBIR format.
---

# Microsoft Power BI Modern Visual Patterns (`pbi-modern-visuals-patterns`)

Extracted directly from Microsoft's official **Power BI Visuals (`Power BI Visuals.pbip`)** sample project (`D:\Power-bi-design\ตัวอย่างกราฟ pbip`) and the **Bas Visual Design** repository (`D:\Power-bi-design\Bas-visual-design\data-flag`), this skill documents PBIR JSON schemas and implementation patterns for modern Power BI visual types.

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

---

## 🚀 6. Executive UX Patterns: Dynamic Trend Highlighting

Combine `MOVINGAVERAGE()` with conditional visual DAX to automatically highlight data points that exceed the moving trend:

```dax
-- Step 1: Calculate N-period Moving Average
Moving average = MOVINGAVERAGE([Total Sales], 3)

-- Step 2: Dynamic Above-Trend Highlighting Series
Above MA = IF( [Total Sales] > [Moving average] , [Total Sales] )
```

---

## 🔍 7. Searchable Card Grid Pattern (`cardVisual` + `textSlicer`)

Combine `cardVisual` multi-tile layouts with `textSlicer` to build live-searchable catalog grids for items, products, or trader profiles:

```json
{
  "visualType": "cardVisual",
  "objects": {
    "image": [
      {
        "properties": {
          "show": { "expr": { "Literal": { "Value": "true" } } },
          "imageType": { "expr": { "Literal": { "Value": "'imageUrl'" } } },
          "position": { "expr": { "Literal": { "Value": "'Left'" } } }
        }
      }
    ],
    "layout": [
      {
        "properties": {
          "maxTiles": { "expr": { "Literal": { "Value": "9L" } } }
        }
      }
    ]
  }
}
```

---

## 🚩 8. Data Flag & Milestone Banner Pattern (`data-flag`)

Render floating milestone flags (e.g. Q1, Q2, Peak Trading Season) above charts without overcrowding monthly labels:

### DAX Conditional Flag Measure
```dax
Data Labels Value = 
    VAR _Quarter = MAX( dimDate[Quarter] )
    VAR _ConditionVisibility = MONTH( MAX( dimDate[Date] ) ) IN {1, 4, 7, 10}
    RETURN
        IF( _ConditionVisibility, _Quarter )
```

### Overhead Y-Axis Clearance Cushion
Scale upper Y-axis max by `1.65x` (`MAXX(...) * 1.65`) so floating milestone banners sit cleanly above trend lines without clipping!
