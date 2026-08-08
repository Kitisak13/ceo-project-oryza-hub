---
name: pbi-dax-window-and-statistical-patterns
description: Implement DAX Window functions (INDEX, OFFSET, WINDOW) for moving averages, offset period comparisons, running totals, and building native Box Plot visuals in combo charts.
---

# Power BI DAX Window Functions & Statistical Visuals

DAX Window functions (`INDEX`, `OFFSET`, `WINDOW`) simplify order-based calculations, moving averages, offset comparisons, and dynamic ranking without complex `EARLIER` or `FILTER` iterators.

---

## 1. DAX Window Functions

### `OFFSET`: Previous Period / Row Comparison
Retrieve values from N rows relative to the current context.

```dax
measure 'Sales Prev Month (OFFSET)' = 
CALCULATE(
    [Total Sales],
    OFFSET(
        -1,
        ALLSELECTED( 'dimDate'[YearMonth], 'dimDate'[YearMonthNo] ),
        ORDERBY( 'dimDate'[YearMonthNo], ASC )
    )
)
```

### `INDEX`: Nth Row Value (Top 1, Nth Item)
Retrieve absolute Nth row value in an ordered set.

```dax
measure 'Top 1 Store Sales' = 
CALCULATE(
    [Total Sales],
    INDEX(
        1,
        ALLSELECTED( 'dimStore'[StoreName] ),
        ORDERBY( [Total Sales], DESC )
    )
)
```

### `WINDOW`: Moving Average / Rolling Range
Calculate rolling averages or running totals over a moving window of rows.

```dax
measure '3-Month Moving Average (WINDOW)' = 
AVERAGEX(
    WINDOW(
        -2, RELATIVE,
        0, RELATIVE,
        ALLSELECTED( 'dimDate'[YearMonth], 'dimDate'[YearMonthNo] ),
        ORDERBY( 'dimDate'[YearMonthNo], ASC )
    ),
    [Total Sales]
)
```

---

## 2. Statistical Box Plot in Native Combo Charts

Build Statistical Box Plots (Min, Q1, Median, Q3, Max, Outliers) using standard Line and Stacked Column Combo Charts (`lineStackedColumnComboChart`).

### DAX Measures for Box Plot Elements

```dax
measure 'Sales Quantity MIN' = MIN( fctSales[SalesQuantity] )

measure 'Sales Quantity Q1' = 
PERCENTILE.INC( fctSales[SalesQuantity], 0.25 )

measure 'Sales Quantity MEDIAN' = 
MEDIAN( fctSales[SalesQuantity] )

measure 'Sales Quantity Q3' = 
PERCENTILE.INC( fctSales[SalesQuantity], 0.75 )

measure 'Sales Quantity MAX' = MAX( fctSales[SalesQuantity] )

// Stacked Column Sections (Box Lower, Box Upper, Whisker Range)
measure 'Box Lower (Q1 - Min)' = [Sales Quantity Q1] - [Sales Quantity MIN]

measure 'Box Middle (Median - Q1)' = [Sales Quantity MEDIAN] - [Sales Quantity Q1]

measure 'Box Upper (Q3 - Median)' = [Sales Quantity Q3] - [Sales Quantity MEDIAN]

measure 'Whisker Upper (Max - Q3)' = [Sales Quantity MAX] - [Sales Quantity Q3]
```

### Visual Setup (`lineStackedColumnComboChart`)
- **Shared Axis**: `dimProduct[Category]`
- **Column Values**: Stacked measures `[Box Lower]`, `[Box Middle]`, `[Box Upper]`. Set `[Box Lower]` series color to Transparent (`#ffffff00`).
- **Line Values**: `[Sales Quantity MIN]`, `[Sales Quantity MAX]` as whisker boundary lines.

## Best Practices
- Always specify explicit `ORDERBY` and `PARTITIONBY` clauses inside `WINDOW` and `OFFSET` functions to ensure predictable sort behavior.
- Use `INC` (Inclusive) percentile functions (`PERCENTILE.INC`) for standard corporate box plot statistics.
