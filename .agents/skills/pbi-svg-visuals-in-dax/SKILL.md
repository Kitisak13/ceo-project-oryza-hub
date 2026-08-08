---
name: pbi-svg-visuals-in-dax
description: Patterns for generating in-line SVG graphics using DAX (data:image/svg+xml;utf8,...), including SVG sparklines, bullet charts, rating stars, progress bars, and SVG matrix tables.
---

# Power BI SVG Visualizations in DAX

Power BI supports displaying dynamically generated SVG graphics directly inside Table, Matrix, and New Card (`cardVisual`) visuals. Measures marked as Data Category = **Image URL** render raw SVG code into responsive vector charts.

## Core Data URI Syntax

All SVG DAX measures must return string content prefixed with:
`"data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' ... </svg>"`

---

## 1. SVG Progress Bar Measure

Generate a custom horizontal progress bar with percentage fill and target indicator line.

```dax
measure 'SVG Progress Bar' = 
VAR _Pct = MIN( MAX( [Completion Pct], 0 ), 1 )
VAR _Width = 100
VAR _Height = 16
VAR _FillWidth = _Pct * _Width
VAR _BarColor = IF( _Pct >= 1, "%2316A34A", "%230051D5" ) // URL encoded hex #
VAR _BgColor = "%23F2F4F6"
RETURN
    "data:image/svg+xml;utf8," &
    "<svg xmlns='http://www.w3.org/2000/svg' width='" & _Width & "' height='" & _Height & "'>" &
        "<rect width='" & _Width & "' height='" & _Height & "' rx='4' fill='" & _BgColor & "'/>" &
        "<rect width='" & _FillWidth & "' height='" & _Height & "' rx='4' fill='" & _BarColor & "'/>" &
    "</svg>"
```

---

## 2. SVG Line Sparkline Measure

Generate a 12-month line sparkline inside table cells.

```dax
measure 'SVG Sparkline' = 
VAR _Width = 120
VAR _Height = 30
VAR _Values = ALLSELECTED( 'dimDate'[MonthNo] )
VAR _MinVal = MINX( _Values, [Total Sales] )
VAR _MaxVal = MAXX( _Values, [Total Sales] )
VAR _Range = IF( _MaxVal = _MinVal, 1, _MaxVal - _MinVal )

// Build SVG points string: X,Y X,Y X,Y
VAR _Points = 
    CONCATENATEX(
        _Values,
        VAR _X = ( 'dimDate'[MonthNo] - 1 ) * ( _Width / 11 )
        VAR _Y = _Height - ( ( [Total Sales] - _MinVal ) / _Range * ( _Height - 6 ) + 3 )
        RETURN _X & "," & _Y,
        " "
    )

RETURN
    "data:image/svg+xml;utf8," &
    "<svg xmlns='http://www.w3.org/2000/svg' width='" & _Width & "' height='" & _Height & "'>" &
        "<polyline points='" & _Points & "' fill='none' stroke='%230051D5' stroke-width='2'/>" &
    "</svg>"
```

---

## 3. SVG Bullet Chart Measure

Create a compact Bullet Chart with qualitative range bands (Poor, Satisfactory, Good), actual value bar, and target marker line.

```dax
measure 'SVG Bullet Chart' = 
VAR _Actual = [Sales Total]
VAR _Target = [Sales Target]
VAR _MaxVal = MAX( _Target * 1.2, _Actual * 1.1 )
VAR _W = 150
VAR _H = 20
VAR _ActualW = ( _Actual / _MaxVal ) * _W
VAR _TargetX = ( _Target / _MaxVal ) * _W
RETURN
    "data:image/svg+xml;utf8," &
    "<svg xmlns='http://www.w3.org/2000/svg' width='" & _W & "' height='" & _H & "'>" &
        "<rect width='" & _W & "' height='" & _H & "' fill='%23ECEEF0'/>" &
        "<rect width='" & ( _W * 0.7 ) & "' height='" & _H & "' fill='%23E0E3E5'/>" &
        "<rect width='" & _ActualW & "' height='8' y='6' fill='%23000D33'/>" &
        "<line x1='" & _TargetX & "' y1='2' x2='" & _TargetX & "' y2='18' stroke='%23BA1A1A' stroke-width='3'/>" &
    "</svg>"
```

---

## 4. SVG Rating Star Indicator

Display 5-star visual rating indicators dynamically based on a metric value (1 to 5).

```dax
measure 'SVG Star Rating' = 
VAR _Rating = MIN( MAX( ROUND( [Health Score], 0 ), 0 ), 5 )
VAR _StarPath = "M12 .587l3.668 7.431 8.2 1.192-5.934 5.784 1.399 8.165L12 18.896l-7.333 3.863 1.399-8.165-5.934-5.784 8.2-1.192z"
VAR _FilledStars = REPT( "<path d='" & _StarPath & "' fill='%23316BF3'/>", _Rating )
VAR _EmptyStars = REPT( "<path d='" & _StarPath & "' fill='%23C5C6D2'/>", 5 - _Rating )
RETURN
    "data:image/svg+xml;utf8," &
    "<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 120 24' width='100' height='20'>" &
        _FilledStars & _EmptyStars &
    "</svg>"
```

## Best Practices
- URL encode `#` symbol as `%23` inside SVG strings to prevent rendering issues in Power BI Desktop / Service.
- Set measure **Data Category** to `Image URL` in Tabular Editor or Power BI Desktop property pane.
- Keep SVG width and height proportional to visual row height (e.g. height 20–30px for table rows).
