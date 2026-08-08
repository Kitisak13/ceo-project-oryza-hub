---
name: pbi-advanced-layout-and-ux-tricks
description: Advanced Power BI layout techniques including The Overlap Trick (visual layering), Info placeholders/dividers, dynamic ranking labels (#1, #2), paging slicers (# Items, # Pages), KPI gradient overlays, and slide-out slicer panels.
---

# Power BI Advanced Layout & UX Tricks

Modern Power BI dashboard design relies on micro-layout techniques, layered visual stacking, dynamic paging, and interactive panel UX.

---

## 1. The Overlap Trick (Visual Stacking & Custom Dual Cards)

Layering visuals (e.g. placing a transparent KPI card over a Bar Chart or background Container Shape) allows creating custom visual headers, floating callout badges, and dual-layer charts.

### Setup Rules for Visual Overlapping
1. **Background**: Set top visual background to **Transparent** (`show: false` or `transparency: 100`).
2. **Visual Header**: Turn off visual headers (`visualHeader: [{ "show": false }]`) on overlaid elements.
3. **Padding**: Set container padding to `0` to prevent alignment shift.
4. **Z-Order**: Manage Layer Order explicitly in Power BI Selection Pane (`Format > Selection`).

---

## 2. Dynamic Ranking & Info Placeholders (`#1, #2`)

Display dynamic rank badges inside bar chart category labels or visual headers.

```dax
measure 'Item Rank CY' = 
RANKX( ALLSELECTED( 'dimProduct'[Product] ), [Total Sales CY], , DESC, Dense )

measure 'Ranking Label' = 
"#" & [Item Rank CY] & " - " & SELECTEDVALUE( 'dimProduct'[Product] )
```

---

## 3. Dynamic Paging & Top N Items Slicers

Allow end users to dynamically control page size (`# Items` per page: 5, 10, 20) and page number (`# Pages`: Page 1, Page 2).

```dax
measure '# Items Value' = SELECTEDVALUE( '# Items'[# Items], 5 )
measure '# Pages Value' = SELECTEDVALUE( '# Pages'[# Pages], 1 )

measure 'Item Rank Filter' = 
VAR _Page = [# Pages Value]
VAR _ItemsPerPage = [# Items Value]
VAR _MinRank = ( _Page - 1 ) * _ItemsPerPage + 1
VAR _MaxRank = _Page * _ItemsPerPage
VAR _Rank = [Item Rank CY]
RETURN IF( _Rank >= _MinRank && _Rank <= _MaxRank, 1, 0 )
```

*Apply `[Item Rank Filter] = 1` to the Visual Level Filter pane.*

---

## 4. KPI Card Gradient Overlay & Glassmorphism

Create executive KPI cards with subtle gradient overlays or glassmorphism background cards.

```dax
measure 'KPI Status Background' = 
VAR _Pct = [Sales YoY%]
RETURN SWITCH( TRUE(),
    _Pct >= 0.10, "#16A34A15",  // 15% opacity green tint
    _Pct >= 0.00, "#316BF315",  // 15% opacity blue tint
    "#BA1A1A15"                 // 15% opacity red tint
)
```

---

## 5. Slide-Out Slicer Panel & Bookmark Navigators

Create responsive slide-out slicer panels using Bookmark Navigators and Shape containers.
- **Bookmarks**: `SlicerPanel_Open`, `SlicerPanel_Close`.
- **Bookmark Navigator**: Bind a Button Visual to Bookmark group `SlicerPanel`.
- **Clear Filters Button**: Create a button with Action = `Bookmark: ClearAllFilters`.

## Best Practices
- Group overlaid visuals together in the **Selection Pane** so they move and scale as a single unit.
- Maintain consistent padding (`16px` gutters, `8px` container margin) across layered card components.
