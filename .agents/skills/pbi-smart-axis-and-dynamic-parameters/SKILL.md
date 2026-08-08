---
name: pbi-smart-axis-and-dynamic-parameters
description: Techniques for smart dynamic axis scale control (Axis_Min, Axis_Max), Field Parameters for dynamic measure/dimension switching, dynamic granularity, and dynamic chart titles (Power-title).
---

# Power BI Smart Axis Control & Dynamic Parameters

Dynamic Axis Control and Field Parameters allow users to customize chart scales, switch granularity (Day/Week/Month/Year), change dimensions/measures, and generate context-aware visual titles (`Power-title`).

---

## 1. Smart Axis Range Control (Dynamic Axis Min / Max)

Dynamically bound Y-Axis ranges to prevent compressed line charts or distorted bar charts when slicers filter down to low/high value ranges.

```dax
measure 'Axis Value Slicer' = SELECTEDVALUE( 'Axis Slicer'[Max Limit], 5000000 )

measure Axis_Min = 
VAR _MinVal = MIN( [Total Sales], [Forecast] )
RETURN IF( ISBLANK( _MinVal ), 0, _MinVal * 0.9 ) // 10% bottom padding

measure Axis_Max = 
VAR _MaxVal = MAX( [Total Sales], [Forecast] )
VAR _CustomLimit = [Axis Value Slicer]
RETURN IF( _MaxVal > _CustomLimit, _MaxVal * 1.1, _CustomLimit )
```

*In Power BI Visual Formatting > Y-Axis > Range Min/Max, map the **fx** conditional formatting button to `[Axis_Min]` and `[Axis_Max]`.*

---

## 2. Field Parameters for Dynamic Dimension & Measure Switching

Field Parameters allow end users to dynamically switch visual dimensions (e.g. Category, Region, Channel) or metrics (Sales, Profit, Units Sold).

### TMDL Syntax: Field Parameter Table

```tmdl
table 'Dynamic Dimension'
	lineageTag: 5b4d8a1c-9921-4f2a-b103-8833182103e9

	column 'Dynamic Dimension'
		dataType: string
		lineageTag: c9281a4b-1234-4bda-8000-111122223333
		sourceColumn: [Value1]
		sortByColumn: 'Order'

	column 'Order'
		dataType: int64
		hidden
		lineageTag: e8a91c2b-5678-4cba-9000-444455556666
		sourceColumn: [Value2]

	partition 'Dynamic Dimension' = m
		mode: import
		source = ```
			{
			    ("Category", NAMEOF('dimProduct'[Category]), 0),
			    ("Sub-Category", NAMEOF('dimProduct'[SubCategory]), 1),
			    ("Region", NAMEOF('dimGeography'[Region]), 2),
			    ("Sales Channel", NAMEOF('dimChannel'[ChannelDescription]), 3)
			}
			```
```

---

## 3. Dynamic Visual Titles (`Power-title`)

Generate dynamic, context-rich chart titles that display current selection, KPI total, and growth indicator symbols (`▲`, `▼`).

```dax
measure 'Chart Power Title' = 
VAR _MetricName = SELECTEDVALUE( 'Dynamic Measure'[Measure Name], "Sales" )
VAR _SelectedDim = SELECTEDVALUE( 'Dynamic Dimension'[Dynamic Dimension], "All Dimensions" )
VAR _CurrentVal = [Total Sales]
VAR _YoY = [Sales YoY%]
VAR _TrendSymbol = IF( _YoY >= 0, " ▲ ", " ▼ " )
VAR _FormattedYoY = FORMAT( _YoY, "+0.0%;-0.0%;0.0%" )
RETURN 
    _MetricName & " by " & _SelectedDim & " | " & 
    FORMAT( _CurrentVal, "$#,##0" ) & 
    " (" & _TrendSymbol & _FormattedYoY & " YoY)"
```

## Best Practices
- Bind visual Y-Axis Min/Max to measures using the **fx** button to maintain readable chart scaling regardless of slicer filter depth.
- Use `NAMEOF()` inside Field Parameter M code definitions to maintain schema lineage when columns are renamed.
