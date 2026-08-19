# Agent Directives: World Rice Price Analytics (PBIP)

## 🌾 Project Domain & Objective

This Power BI Project (PBIP) serves as a **Strategic & Trading Intelligence Dashboard** for Executives and the Trader Team to monitor global rice price movements, forecast trends, and unlock actionable market insights.

### Product Categories & SKU Scope (Total 31 SKUs):

1. **Long grain white rice - High quality:** 10 items
2. **Long grain white rice - Low quality:** 5 items
3. **Long grain parboiled rice:** 6 items
4. **Long grain fragrant rice:** 2 items
5. **Brokens:** 5 items
6. **Medium grain milled:** 2 items

---

## 🎯 Target Audience & Analytical Requirements

### 1. Executive View (Strategic & Macro Level)

- Focus on high-level KPIs, MoM/YoY trends, macro price drivers, and portfolio performance.
- Highlight price spreads between categories (e.g., Fragrant vs. White Rice premium, High Quality vs. Low Quality gap).

### 2. Trader Team View (Tactical & Operational Level)

- Focus on daily/weekly price dynamics, volatility, momentum, and arbitrage opportunities.
- Require historical comparisons, seasonality patterns, price forecasting, and outlier/out-of-norm detection.

---

## 🛠️ Technical Rules & Guidelines for AI Agents

### 1. PBIP Integrity & Architecture

- **Directory Structure:**
  - `*.SemanticModel/`: Contains dataset definitions, tables, DAX measures, and relationships (TMDL/BIM).
  - `*.Report/`: Contains visual layouts, pages, bookmark definitions, and theme styling.
- **Safety Rule:** Never modify structural metadata or GUIDs in JSON files without explicit request. Ensure TMDL syntax is strictly validated to prevent project corruption.

### 2. DAX Coding Standards

- **Safe Calculation:** Always use `DIVIDE(numerator, denominator, 0)` to prevent division-by-zero errors.
- **Explicit Measures:** Do not use implicit measures. All aggregations (Price Averages, Spreads, Volatility % Change) must be explicit measures.
- **Time Intelligence:** Standardize Time Intelligence measures (DoD, WoW, MoM, YoY, YTD, Rolling Averages) using a dedicated Date Dimension table (`'Dim_Date'`).
- **Formatting:** Keep DAX clean, formatted, and readable with proper indentation.

### 3. Skill Execution Guidance

- Use **DAX Skills** when adding/modifying calculations in `*.SemanticModel/definition/tables/`.
- Use **Data Model / Power Query Skills** for data transformation or schema modifications in `expressions.tmdl`.
- Use **PBIP Visual/Report Skills** when designing layouts or theme styling in `*.Report/definition/`.

---

## 📄 Report Page Structure & Roadmap Strategy

### Multi-Page Architecture:
- **Page 1: `Main` (Executive Global Overview):** Global market overview, Category & Product filtering, Daily summary table with in-line SVG sparklines & 52W range bars, dynamic executive narrative, and 2020–2026 historical trend with X-axis zoom slider.
- **Page 2: `Compare` (Arbitrage & Comparison Playground):** Interactive head-to-head analysis where users can pair any 2 of the 31 global rice SKUs via independent disconnected slicers (`Dm_Product Name` vs. `Dm_Product_Compare`), examining price spreads, price ratios, historical spread divergence, and automated arbitrage commentary.
- **Phase 3 (Subsequent Pages):** Seasonality (12-month recurring patterns), Origin Parity Heatmap, and Market Volatility / Risk Matrix.

---

## 📊 Business Logic & Key Calculation Patterns

When asked to write measures or design visuals, prioritize these analytical frameworks:

- **Category Spreads:** Price difference between grades (e.g., `Price[High Quality] - Price[Low Quality]`).
- **Volatility Metrics:** Standard deviation of price changes over standard rolling windows (e.g., 7-day, 30-day).
- **Price Benchmarking:** Grade price relative to the overall market average or category benchmark.

