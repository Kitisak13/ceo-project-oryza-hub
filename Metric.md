# 📊 Semantic Model Metrics & Business Logic Reference

This document serves as the official reference for DAX measures, analytical calculation frameworks, decision matrices, and business logic used within the **World Rice Price Analytics (PBIP)** project.

---

## 🎯 1. Market Movement & Sentiment Decision Matrix

The measure **`'DAX'[Market Movement Summary Text]`** evaluates price momentum across dual timeframes (Short-term **WoW** vs. Medium-term **MoM**) to generate automated executive commentary.

### 📐 Decision Matrix

| Order | Market Sentiment Status | Mathematical Condition | Business & Trading Interpretation |
| :---: | :--- | :--- | :--- |
| **1** | 🟢 **Strong Bullish**<br>*(Accelerating Upward)* | `[Avg Price WoW%] > +2.0%`<br>**AND**<br>`[Avg Price MoM%] > +5.0%` | **Strong Bull Market:** Accelerated buying momentum across both short and medium term. High buying pressure / supply shortage signal. |
| **2** | 🟢 **Mild Bullish**<br>*(Upward Momentum)* | `[Avg Price WoW%] > 0.0%` | **Positive Momentum:** Weekly price higher than previous week, indicating steady demand or initial recovery. |
| **3** | 🔴 **Strong Bearish**<br>*(Sharp Correction)* | `[Avg Price WoW%] < -2.0%`<br>**AND**<br>`[Avg Price MoM%] < -5.0%` | **Strong Bear Market:** Accelerated downward correction across both timeframes. Heavy selling pressure or harvest influx. |
| **4** | 🔴 **Mild Bearish**<br>*(Downward Pressure)* | `[Avg Price WoW%] < 0.0%` | **Weakening Price:** Weekly price softening compared to prior week, indicating subdued demand or buyer resistance. |
| **5** | ⚪ **Stable / Neutral** | All other cases<br>*(e.g., `WoW = 0.0%`)* | **Consolidation / Range-Bound:** Flat market with minimal price movement or absence of directional catalyst. |

### 💻 DAX Implementation

```dax
measure 'Market Movement Summary Text' =
    VAR _WoW = [Avg Price WoW%]
    VAR _MoM = [Avg Price MoM%]
    VAR _Status =
        SWITCH(
            TRUE(),
            _WoW > 0.02 && _MoM > 0.05, "Strong Bullish (Accelerating Upward)",
            _WoW > 0, "Mild Bullish (Upward Momentum)",
            _WoW < -0.02 && _MoM < -0.05, "Strong Bearish (Sharp Correction)",
            _WoW < 0, "Mild Bearish (Downward Pressure)",
            "Stable / Neutral"
        )
    RETURN
        "Market Sentiment: " & _Status & " (WoW: " & FORMAT( _WoW, "+0.0%;-0.0%;0.0%" ) & " | MoM: " & FORMAT( _MoM, "+0.0%;-0.0%;0.0%" ) & ")"
```

---

## ⏱️ 2. Core Time Intelligence Metrics

All Time Intelligence measures are standardized using trading date lookups anchored to `'Dm_Date'` and `'Fct_Oryza Price'[HeaderDate]`.

| Measure Name | Display Folder | Calculation Formula / Concept | Purpose |
| :--- | :--- | :--- | :--- |
| **`Avg Price Last Day`** | `01. Core Price Metrics` | Latest trading date price per filtered context | Current spot price |
| **`Avg Price Previous Day`** | `01. Core Price Metrics` | Price on the latest trading date *prior* to `Avg Price Last Day` | Baseline for Daily Change |
| **`△Day`** | `07. UI & System Helpers` | `[Avg Price Last Day] - [Avg Price Previous Day]` | Daily nominal price change ($/MT) |
| **`Avg Price WoW%`** | `02. Time Intelligence` | `DIVIDE([Avg Price Last Day] - [Avg Price ~7D Prior], [Avg Price ~7D Prior], 0)` | Week-over-Week % change |
| **`Avg Price Prev Month`** | `02. Time Intelligence` | Price on the nearest trading date 1 month prior (`EDATE(-1)`) | Baseline for MoM% |
| **`%△Month` / `Avg Price MoM%`** | `02. Time Intelligence` | `DIVIDE([Avg Price Last Day] - [Avg Price Prev Month], [Avg Price Prev Month], 0)` | Month-over-Month % change |
| **`Annual Change` / `%△Year`** | `07. UI & System Helpers` | `DIVIDE([Anual 365 CY] - [Anual 365 LY], [Anual 365 LY], 0)` | 365-day rolling annual change |
| **`52 Week L` / `52 Week H`** | `03. Trend & Moving Averages` | `MIN`/`MAX` of price over trailing 365 days | 52-Week Range Boundaries |

---

## 🎨 3. In-Line SVG & Visualization Helper Measures

| Measure Name | Data Category | Resolution | Description |
| :--- | :--- | :--- | :--- |
| **`52W Range Bar SVG`** | `ImageUrl` | 100 × 14 px | Horizontal bullet range bar showing position of current price relative to 52W High and Low, color-coded by price percentile. |
| **`30D Trend Sparkline SVG`** | `ImageUrl` | 80 × 14 px | Ultra-compact 30-day price trendline with auto-scaled Y coordinates and momentum stroke color. |
| **`Text_color Day`** | General | Hex String | Returns `#089BAB` (Green), `#D83B01` (Red), or `#64748B` (Grey) based on `△Day`. |
| **`Text_color WoW`** | General | Hex String | Returns `#089BAB`, `#D83B01`, or `#64748B` based on `Avg Price WoW%`. |
| **`Text_color Month`** | General | Hex String | Returns `#089BAB`, `#D83B01`, or `#64748B` based on `%△Month`. |
| **`Text_color Year`** | General | Hex String | Returns `#089BAB`, `#D83B01`, or `#64748B` based on `Annual Change`. |

---

## 🏷️ 4. Dynamic Text & Executive Commentary Measures

| Measure Name | Display Folder | Sample Output |
| :--- | :--- | :--- |
| **`Title Rice Category`** | `10. Text` | `Oryza Global Rice Price : Broken White Rice` |
| **`Title Rice Category & Product`** | `10. Text` | `Oryza Global Rice Price : Parboiled Rice (Thailand 100% sortexed)` |
| **`Subtitle Latest Date & Price`** | `10. Text` | `Latest As Of: 30-Jul-2026 \| Price: $462.00/MT (▲ +0.7% WoW)` |
| **`Top Moving SKU Text`** | `10. Text` | `Top Mover: Vietnam 100% broken (+1.7% WoW)` |
| **`52W Range Summary Text`** | `10. Text` | `52W Range: $309 - $362 (Current at 100.0% of band)` |

---

## 📂 5. DAX Display Folder Architecture

All measures in table `'DAX'` are categorized into 10 structured folders:

1. **`01. Core Price Metrics`** — Base pricing aggregations and ATH/ATL extremes.
2. **`02. Time Intelligence`** — Periodic comparisons (WoW, MoM, YoY, YTD, MTD, QTD).
3. **`03. Trend & Moving Averages`** — Moving averages (90D, 120D) and 52-week ranges.
4. **`04. Market Volatility`** — Price Standard Deviation and Coefficient of Variation (CV).
5. **`05. Benchmarks & Price Spreads`** — Country benchmarks (e.g., India 5%) and cross-country spreads.
6. **`06. Rankings`** — Country and SKU price ranking.
7. **`07. UI & System Helpers`** — Field parameter helpers and conditional formatting colors.
8. **`08. Volatility Metrics`** — Specialized volatility indicators.
9. **`09. Variance Analysis`** — Regional spread variances (Thai vs. Viet, Thai vs. India).
10. **`10. Text`** — Dynamic card titles, analytical subtitles, executive summaries, and SVG graphics.
