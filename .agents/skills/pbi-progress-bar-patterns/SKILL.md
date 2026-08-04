---
name: pbi-progress-bar-patterns
description: Custom Progress Bar Visualization Patterns in Power BI using Native Line Charts, Visual Calculations (Visual DAX), and TMDL Series Tables based on Bas Visual Design repo. Use when building horizontal progress bars, goal completion gauges, or status percentage tracking visuals in Power BI reports.
---

# Power BI Progress Bar Visualization Patterns (`pbi-progress-bar-patterns`)

Extracted directly from the **Bas Visual Design** sample project (`D:\Power-bi-design\Bas-visual-design\progress-bar`), this skill documents how to construct sleek, dynamic progress bar visuals in Power BI using Native Line Charts and Visual Calculations without requiring custom visuals.

---

## 🏗️ 1. Architecture Overview

The Native Progress Bar relies on 3 components:
1. **X-Axis Series Table (`X-axis`):** Generates a continuous scale from `0.1` to `1.0` (or `0.05` steps).
2. **Track Line (`Dummy = 1`):** Renders a thick, semi-transparent background bar.
3. **Active Fill & Marker (`Visual Calculations`):** Highlights the active progress dynamically and positions an indicator pin with a percentage data label at the target progress head.

---

## 📊 2. Data Model Setup (TMDL / DAX)

Create a calculated series table for the X-axis:

```tmdl
table X-axis
    lineageTag: a49e5ed6-3480-4a5c-9f30-6d200c8b3962

    column Value
        lineageTag: 7e5919ea-9e16-4ed3-8994-0517f9aa8c6c
        summarizeBy: sum
        isNameInferred
        sourceColumn: [Value]

    partition X-axis = calculated
        mode: import
        source = GENERATESERIES(0.1, 1, 0.1)
```

---

## ⚡ 3. Visual Calculations (Visual DAX) Logic

Inside the Line Chart visual, add the target measure (e.g. `[Progress Status]`) to Tooltips, and define two **Native Visual Calculations** in the Y-axis:

```dax
-- Visual Calculation 1: Active Progress Line Fill
Highlight Progress = IF( [Progress Status] >= [Value] , 1 )

-- Visual Calculation 2: Progress Head Pin & Data Label
Placeholder Data Label = IF( [Progress Status] = [Value] , 1.3 )
```

---

## 🎛️ 4. PBIR Visual Container JSON Configuration

Bind the `lineChart` visual with thick strokes, background transparency, and marker positioning:

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.11.0/schema.json",
  "name": "progress_bar_linechart",
  "position": {
    "x": 800,
    "y": 500,
    "z": 1000,
    "height": 150,
    "width": 320,
    "tabOrder": 1000
  },
  "visual": {
    "visualType": "lineChart",
    "query": {
      "queryState": {
        "Category": {
          "projections": [
            {
              "field": {
                "Column": {
                  "Expression": { "SourceRef": { "Entity": "X-axis" } },
                  "Property": "Value"
                }
              },
              "queryRef": "X-axis.Value",
              "nativeQueryRef": "Value",
              "active": true
            }
          ]
        },
        "Tooltips": {
          "projections": [
            {
              "field": {
                "Measure": {
                  "Expression": { "SourceRef": { "Entity": "Metrics" } },
                  "Property": "Progress Status"
                }
              },
              "queryRef": "Metrics.Progress Status",
              "nativeQueryRef": "Progress Status"
            }
          ]
        },
        "Y": {
          "projections": [
            {
              "field": {
                "Measure": {
                  "Expression": { "SourceRef": { "Entity": "Metrics" } },
                  "Property": "Dummy"
                }
              },
              "queryRef": "Metrics.Dummy",
              "nativeQueryRef": "Dummy"
            },
            {
              "field": {
                "NativeVisualCalculation": {
                  "Language": "dax",
                  "Expression": "IF( [Progress Status] >= [Value] , 1 )",
                  "Name": "Highlight Progress"
                }
              },
              "queryRef": "select",
              "nativeQueryRef": "Highlight Progress"
            },
            {
              "field": {
                "NativeVisualCalculation": {
                  "Language": "dax",
                  "Expression": "IF( [Progress Status] = [Value] , 1.3 )",
                  "Name": "Placeholder Data Label"
                }
              },
              "queryRef": "select1",
              "nativeQueryRef": "Placeholder Data Label"
            }
          ]
        }
      }
    },
    "objects": {
      "categoryAxis": [{ "properties": { "show": { "expr": { "Literal": { "Value": "false" } } } } }],
      "valueAxis": [{ "properties": { "show": { "expr": { "Literal": { "Value": "false" } } }, "start": { "expr": { "Literal": { "Value": "0D" } } }, "end": { "expr": { "Literal": { "Value": "2D" } } } } }],
      "lineStyles": [
        {
          "properties": {
            "strokeWidth": { "expr": { "Literal": { "Value": "10D" } } },
            "strokeTransparency": { "expr": { "Literal": { "Value": "85D" } } }
          }
        }
      ]
    }
  }
}
```

---

## 🎨 Best Practices for Executive Dashboards

1. **Color Contrast:** Use `#E2E8F0` or 85% transparent grey for the track, and `#0F5C55` (Emerald) or `#2563EB` (Blue) for active fill.
2. **Clean Canvas:** Hide category/value axes, gridlines, and legends.
3. **Data Label Position:** Position the Head Pin Data Label `Above` or `Inside` for crisp readability.
