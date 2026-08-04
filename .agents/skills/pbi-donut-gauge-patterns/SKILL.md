---
name: pbi-donut-gauge-patterns
description: Custom Donut Ring Gauge & Center Hole KPI Callout Design Patterns in Power BI based on Bas Visual Design repo. Use when building thin ring percentage gauges, donut KPI cards, or target fill completion visuals in Power BI reports.
---

# Power BI Donut Ring Gauge & Center KPI Patterns (`pbi-donut-gauge-patterns`)

Extracted directly from the **Bas Visual Design** sample repository (`D:\Power-bi-design\Bas-visual-design\donut-chart-design`), this skill documents how to transform standard Power BI Donut Charts into minimalist **Thin Ring Gauges** with centered KPI callouts.

---

## 🏗️ 1. Architecture Overview

The Donut Ring Gauge relies on 3 design elements:
1. **Fill-to-100 DAX Pair:** Active percentage measure paired with a `100% - Active` complement measure.
2. **Thin Hole Radius (`innerRadiusRatio: 81L`):** Expands the center hole to 81% of the chart radius, creating a sleek ring instead of a thick pie block.
3. **Center Hole KPI Overlay (`cardVisual` / `card`):** Places a metric callout value and status text directly inside the donut's center hole.

---

## 📊 2. DAX Measure Setup

Define the active metric and its 100% complement:

```dax
-- Active Target Score (e.g. 75%)
Health Score = DIVIDE( [Achieved Points], [Total Max Points] )

-- Complement Fill-to-100 Measure
Health Score - Fill to 100 = 1 - [Health Score]
```

---

## 🎛️ 3. PBIR Visual Container JSON Configuration (`donutChart`)

Configures the `donutChart` visual with `innerRadiusRatio: 81L`, hidden legends, and custom data point colors:

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.11.0/schema.json",
  "name": "donut_ring_gauge",
  "position": {
    "x": 40,
    "y": 80,
    "z": 6000,
    "height": 365,
    "width": 415,
    "tabOrder": 2000
  },
  "visual": {
    "visualType": "donutChart",
    "query": {
      "queryState": {
        "Y": {
          "projections": [
            {
              "field": {
                "Measure": {
                  "Expression": { "SourceRef": { "Entity": "Metrics" } },
                  "Property": "Health Score"
                }
              },
              "queryRef": "Metrics.Health Score",
              "nativeQueryRef": "Health Score"
            },
            {
              "field": {
                "Measure": {
                  "Expression": { "SourceRef": { "Entity": "Metrics" } },
                  "Property": "Health Score - Fill to 100"
                }
              },
              "queryRef": "Metrics.Health Score - Fill to 100",
              "nativeQueryRef": "Health Score - Fill to 100"
            }
          ]
        }
      }
    },
    "objects": {
      "legend": [{ "properties": { "show": { "expr": { "Literal": { "Value": "false" } } } } }],
      "labels": [{ "properties": { "show": { "expr": { "Literal": { "Value": "false" } } } } }],
      "slices": [
        {
          "properties": {
            "innerRadiusRatio": { "expr": { "Literal": { "Value": "81L" } } }
          }
        }
      ],
      "dataPoint": [
        {
          "properties": {
            "fill": { "solid": { "color": { "expr": { "Literal": { "Value": "'#0F5C55'" } } } } }
          },
          "selector": { "metadata": "Metrics.Health Score" }
        },
        {
          "properties": {
            "fill": { "solid": { "color": { "expr": { "Literal": { "Value": "'#F3F4F7'" } } } } }
          },
          "selector": { "metadata": "Metrics.Health Score - Fill to 100" }
        }
      ]
    }
  }
}
```

---

## 🎨 4. Center Hole KPI Card Overlay (`cardVisual`)

Position a `cardVisual` directly over the donut's center hole (e.g. `x: 120, y: 160` centered) displaying the big percentage callout:

```json
{
  "visualType": "cardVisual",
  "position": {
    "x": 120,
    "y": 160,
    "z": 7000,
    "height": 180,
    "width": 255
  },
  "visualContainerObjects": {
    "background": [{ "properties": { "show": { "expr": { "Literal": { "Value": "false" } } } } }],
    "border": [{ "properties": { "show": { "expr": { "Literal": { "Value": "false" } } } } }],
    "dropShadow": [{ "properties": { "show": { "expr": { "Literal": { "Value": "false" } } } } }]
  }
}
```
