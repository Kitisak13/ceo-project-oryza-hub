---
name: pbi-new-card-and-slicer-visuals
description: Standardized patterns for Power BI New Card Visual (cardVisual) and New Button Slicer (advancedSlicerVisual), including button states, icon integration, sub-labels, grid layouts, and active filter indicator badges.
---

# Power BI New Card & Button Slicer Visuals

Power BI's core visual framework includes the **New Card Visual** (`cardVisual`) and **New Button Slicer** (`advancedSlicerVisual`). These visuals support rich micro-layouts, images/icons, sub-labels, badge overlays, and dynamic interaction states (Default, Hover, Press, Selected).

## Visual Identifier Types

- `cardVisual` - New multi-card visual with layout control, callouts, titles, subtitles, and badges.
- `advancedSlicerVisual` - New button slicer supporting grid buttons, image URLs, icons, and state styling.

---

## Key Configurations & DAX Patterns

### 1. Dynamic Active Filter Indicator Badge

Display an active filter badge on top of slicer headers or card containers (e.g. `Filters (3)` or `Channel: Store, Online`).

```dax
measure ShowFilters1 = 
VAR MaxFilters = 3
VAR ___f = FILTERS ( 'dimChannel'[ChannelDescription] )
VAR ___r = COUNTROWS ( ___f )
VAR ___t = TOPN ( MaxFilters, ___f, 'dimChannel'[ChannelDescription] )
VAR ___d = CONCATENATEX ( ___t, 'dimChannel'[ChannelDescription], ", " )
RETURN
    IF (
        ISFILTERED ( 'dimChannel'[ChannelDescription] ),
        IF ( ___r > MaxFilters, ___d & " (+" & ___r - MaxFilters & ")", ___d ),
        "All Channels"
    )
```

```dax
measure 'CF Isfiltered' = 
IF ( ISFILTERED( 'dimChannel'[ChannelDescription] ), "#0051D5", "#C5C6D2" )
```

---

### 2. Slicer Button Image & Icon Measures

Return dynamic image URLs or SVG icons inside New Button Slicer tiles based on row context.

```dax
measure 'Icon Product Category' = MAX( imgProductCategory[Img url] )

measure 'Color Product Category' = 
VAR _IsSelected = ISFILTERED( 'dimProduct'[Category] )
RETURN IF( _IsSelected, "#0051D5", "#F7F9FB" )
```

---

### 3. Visual JSON Structure: `advancedSlicerVisual`

```json
{
  "visualType": "advancedSlicerVisual",
  "objects": {
    "layout": [
      {
        "properties": {
          "orientation": { "expr": { "Literal": { "Value": "0D" } } },
          "rowCount": { "expr": { "Literal": { "Value": "1L" } } },
          "columnCount": { "expr": { "Literal": { "Value": "4L" } } },
          "style": { "expr": { "Literal": { "Value": "'Cards'" } } }
        }
      }
    ],
    "fillCustom": [
      {
        "properties": {
          "fillColor": { "solid": { "color": { "expr": { "Literal": { "Value": "'#FFFFFF'" } } } } }
        },
        "selector": { "id": "default" }
      },
      {
        "properties": {
          "fillColor": { "solid": { "color": { "expr": { "Literal": { "Value": "'#0051D5'" } } } } }
        },
        "selector": { "id": "selection:selected" }
      }
    ]
  }
}
```

---

### 4. Visual JSON Structure: `cardVisual`

```json
{
  "visualType": "cardVisual",
  "objects": {
    "cardLayout": [
      {
        "properties": {
          "orientation": { "expr": { "Literal": { "Value": "0D" } } },
          "maxCards": { "expr": { "Literal": { "Value": "4L" } } }
        }
      }
    ],
    "calloutValues": [
      {
        "properties": {
          "fontFamily": { "expr": { "Literal": { "Value": "'Inter'" } } },
          "fontSize": { "expr": { "Literal": { "Value": "24D" } } },
          "bold": { "expr": { "Literal": { "Value": "true" } } }
        }
      }
    ]
  }
}
```

## Best Practices
- Combine `cardVisual` callouts with DAX sub-label measures to present KPI comparison metrics (e.g. `+12.4% vs LY`) directly inside card boundaries.
- For `advancedSlicerVisual`, use fixed pixel padding and corner radius `8` or `12` to align with system design standards.
