---
name: pbi-flow-chart-patterns
description: Native Flow Chart & Multi-Stage Step Chart Patterns in Power BI using Stacked Column Charts, TMDL UNION Step Dimensions, and Multilevel Dynamic Format Strings based on Bas Visual Design repo. Use when building budget distribution flows, process flow charts, or multi-level waterfall breakdown charts in Power BI reports.
---

# Power BI Native Flow Chart & Step Patterns (`pbi-flow-chart-patterns`)

Extracted directly from the **Bas Visual Design** sample repository (`D:\Power-bi-design\Bas-visual-design\flow-chart`), this skill documents how to construct native **Flow Charts & Multi-Stage Step Charts** in Power BI using standard Stacked Column Charts and TMDL DAX Unification without needing third-party custom visuals.

---

## 🏗️ 1. Architecture Overview

Native Flow Charts use a 3-tier architecture:
1. **Unified Step Dimension (`Steps Chart`):** Combines Step 1 (Total), Step 2 (Category/Department), and Step 3 (Sub-team/Item) into a single virtual step table.
2. **Native Stacked Column Rendering (`columnChart`):** Renders flow branches dynamically across Step 1, Step 2, and Step 3 columns.
3. **Multilevel Dynamic Format Strings (`formatStringDefinition`):** Formats values dynamically based on hierarchy depth (L1 `$M`, L2 `$M`, L3 `$K`).

---

## 📊 2. Data Model Setup (TMDL / DAX)

Create a calculated step dimension table that merges all hierarchy levels using `UNION` and `SELECTCOLUMNS`:

```tmdl
table 'Steps Chart'
    measure 'Budget Chart' = SUM( 'Steps Chart'[Budget Amount ($)] ) 
        formatStringDefinition = ```
            VAR _L1Value = FORMAT( CALCULATE([Budget Chart], REMOVEFILTERS('Steps Chart'[Team]), 'Steps Chart'[Team] <> "Total"), "#,##0,,.0M" )
            VAR _Department = SELECTEDVALUE('Steps Chart'[Team])
            VAR _L2Value = FORMAT( CALCULATE([Budget Chart], REMOVEFILTERS('Steps Chart'[Team]), 'Steps Chart'[Department] = _Department), "#,##0,,.0M" )
            RETURN
            """" & SWITCH( SELECTEDVALUE('Steps Chart'[Team]), "HR", _L2Value, "IT", _L2Value, "Total", _L1Value, FORMAT([Budget Chart], "#,##0,,.0M") )
            ```

    column Step
        dataType: int64
        sourceColumn: [Step]

    column Department
        dataType: string
        sourceColumn: [Department]

    column Team
        dataType: string
        sourceColumn: [Team]
        sortByColumn: 'Team Sort'

    partition 'Steps Chart' = calculated
        mode: import
        source = ```
            UNION(
                -- Step 1: Total Level
                SELECTCOLUMNS(Budget, "Step", 1, "Department", Budget[Department], "Team", [Team], "Budget Amount ($)", [Budget Amount ($)], "Team Sort", Budget[TeamSort]),
                -- Step 2: Department Breakdown
                SELECTCOLUMNS(Budget, "Step", 2, "Department", Budget[Department], "Team", [Team], "Budget Amount ($)", [Budget Amount ($)], "Team Sort", Budget[TeamSort]),
                -- Step 3: Sub-Team Granular Level
                SELECTCOLUMNS(Budget, "Step", 3, "Department", Budget[Department], "Team", [Team], "Budget Amount ($)", [Budget Amount ($)], "Team Sort", Budget[TeamSort])
            )
            ```
```

---

## 🎛️ 3. PBIR Visual Container JSON Configuration (`columnChart`)

Bind `columnChart` with `Category` = `Steps Chart.Step` and `Series` = `Steps Chart.Team`:

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.11.0/schema.json",
  "name": "flow_chart_stacked_column",
  "position": {
    "x": 800,
    "y": 300,
    "z": 1000,
    "height": 700,
    "width": 800,
    "tabOrder": 1000
  },
  "visual": {
    "visualType": "columnChart",
    "query": {
      "queryState": {
        "Category": {
          "projections": [
            {
              "field": {
                "Column": {
                  "Expression": { "SourceRef": { "Entity": "Steps Chart" } },
                  "Property": "Step"
                }
              },
              "queryRef": "Steps Chart.Step",
              "nativeQueryRef": "Step",
              "active": true
            }
          ]
        },
        "Series": {
          "projections": [
            {
              "field": {
                "Column": {
                  "Expression": { "SourceRef": { "Entity": "Steps Chart" } },
                  "Property": "Team"
                }
              },
              "queryRef": "Steps Chart.Team",
              "nativeQueryRef": "Team"
            }
          ]
        },
        "Y": {
          "projections": [
            {
              "field": {
                "Measure": {
                  "Expression": { "SourceRef": { "Entity": "Steps Chart" } },
                  "Property": "Budget Chart"
                }
              },
              "queryRef": "Steps Chart.Budget Chart",
              "nativeQueryRef": "Budget Chart"
            }
          ]
        }
      }
    },
    "objects": {
      "categoryAxis": [{ "properties": { "showAxisTitle": { "expr": { "Literal": { "Value": "false" } } } } }],
      "valueAxis": [{ "properties": { "showAxisTitle": { "expr": { "Literal": { "Value": "false" } } } } }]
    }
  }
}
```
