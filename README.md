# 🌾 World Rice Price Analytics (PBIP) - Strategic & Trading Intelligence Dashboard

## 📋 ภาพรวมโปรเจกต์ (Project Overview)

โปรเจกต์นี้คือ **Power BI Project (`.pbip`)** สำหรับวิเคราะห์ราคาร้าวโลก (World Rice Price Analytics) โดยถูกออกแบบตามมาตรฐานสถาปัตยกรรม Fabric PBIR / TMDL และหลักการออกแบบ **3-30-300 Detail Gradient** เพื่อรองรับผู้ใช้งาน 2 กลุ่มหลัก:

1. **Executive View (Strategic & Macro Level):** สำหรับผู้บริหารในการติดตามแนวโน้มราคา ภาพรวมพอร์ตสินค้า 6 หมวด และทิศทางการเติบโต MoM/YoY
2. **Trader Team View (Tactical & Operational Level):** สำหรับทีมเทรดเดอร์และผู้จัดการความเสี่ยงในการวิเคราะห์ความผันผวน (Volatility), ส่วนต่างราคา (Price Spreads & Arbitrage), แพทเทิร์นฤดูกาล (Seasonality) และสัญญาณเตือนเทรด (Trading Signals)

### 📦 ขอบเขตสินค้าและหมวดหมู่ (31 SKUs Across 6 Categories)
1. **Long grain white rice - High quality:** 10 SKUs
2. **Long grain white rice - Low quality:** 5 SKUs
3. **Long grain parboiled rice:** 6 SKUs
4. **Long grain fragrant rice:** 2 SKUs
5. **Brokens:** 5 SKUs
6. **Medium grain milled:** 2 SKUs

---

## 📑 สรุปรายละเอียดรายงานและ Visuals ทั้ง 8 หน้า (Page & Visual Breakdown)

### 📌 Page 1: `Summary` (Executive Overview)
* **วัตถุประสงค์:** สรุปภาพรวมราคาร้าวและตารางเปรียบเทียบการเปลี่ยนแปลงราคาตามช่วงเวลา
* **Visuals ภายในหน้า:**
  1. **Dynamic Metric Field Parameter Slicer:** สวิตช์เลือกมุมมองตัวชี้วัดราคา
  2. **Product Price Summary Table (`tableEx`):** ตารางแสดงราคาเฉลี่ยล่าสุด (`Avg Price Last Day`), การเปลี่ยนแปลงรายวัน (`△Day`), การเปลี่ยนแปลงรายสัปดาห์ (`WoW%`), รายเดือน (`%△Month`), รายปี (`Annual Change`), กรอบ 52 สัปดาห์ (`52 Week Low/High`) และราคาเฉลี่ยปี 2025 vs 2026

---

### 📌 Page 2: `Compare` (Benchmark Comparison)
* **วัตถุประสงค์:** เปรียบเทียบแนวโน้มราคาระหว่างเกรดสินค้าและประเทศผู้ส่งออก
* **Visuals ภายในหน้า:**
  1. **Category Filter Slicer:** เมนูดรอปดาวน์เลือกหมวดหมู่ข้าว (`Dm_Category.Category Eng`)
  2. **Price Trend Comparison Line Chart:** กราฟเส้นเปรียบเทียบราคาเฉลี่ยตามเกรดสินค้าในหมวดหมู่ที่เลือก
  3. **Price Ranking Bar Chart / Matrix:** ตาราง/กราฟเปรียบเทียบอันดับราคาสินค้า

---

### 📌 Page 3: `Executive Portfolio Overview` (Strategic Macro View)
* **วัตถุประสงค์:** หน้าแดชบอร์ดผู้บริหาร แสดงภาพรวมประสิทธิภาพของพอร์ตสินค้าข้าวทั้ง 6 หมวด
* **Visuals ภายในหน้า:**
  1. **Header & Category Slicer:** ชื่อหน้าและเมนูเลือกกรองหมวดภาษาอังกฤษ (`Category Eng`)
  2. **KPI Card 1 - Latest Price:** `Avg Price Last Day` ($/MT)
  3. **KPI Card 2 - Weekly Change:** `Avg Price WoW%` (%)
  4. **KPI Card 3 - Monthly Change:** `Avg Price MoM%` (%)
  5. **KPI Card 4 - Price Volatility Index:** `Price Volatility% (CV)` (%)
  6. **KPI Card 5 - 52-Week Price Range:** `52W Range (Low - High)` ($/MT)
  7. **Small Multiples Line Chart:** กราฟเส้น 6 กราฟย่อยแยกตาม 6 หมวดสินค้า แสดงแนวโน้มราคารายเดือนย้อนหลัง
  8. **Detailed Portfolio Matrix:** ตารางสรุปรายละเอียดลำดับชั้น `Category Eng` -> `Product Name` พร้อมราคาล่าสุด, WoW%, MoM%, YoY%, และ Volatility CV%

---

### 📌 Page 4: `Country & Origin Benchmark` (Tactical Regional View)
* **วัตถุประสงค์:** วิเคราะห์การแข่งขันและส่วนต่างราคาระหว่างประเทศผู้ส่งออกข้าวหลัก (ไทย, เวียดนาม, อินเดีย, ปากีสถาน, สหรัฐฯ)
* **Visuals ภายในหน้า:**
  1. **Header Banner:** ชื่อหน้าแสดงความเชื่อมโยงกับดัชนีประเทศ
  2. **KPI Card 1 - Thailand Benchmark:** `Thailand 5% Avg Price` ($/MT)
  3. **KPI Card 2 - Vietnam Benchmark:** `Vietnam 5% Avg Price` ($/MT)
  4. **KPI Card 3 - India Global Benchmark:** `Benchmark Price (India)` ($/MT)
  5. **KPI Card 4 - Thai vs Viet Spread:** `Thai vs Viet Spread` ($/MT)
  6. **KPI Card 5 - Thai vs India Spread:** `Thai vs India Spread` ($/MT)
  7. **Country Benchmark Line Chart:** กราฟเส้นเปรียบเทียบระดับราคาข้าวของแต่ละประเทศผู้ส่งออกตามช่วงเวลา
  8. **Country Ranking & Differential Matrix:** ตารางแสดงราคาเฉลี่ยเทียบกับดัชนีอินเดียและส่วนต่างราคาเชิงลึก

---

### 📌 Page 5: `Price Spreads & Grade Differentials` (Trading Arbitrage View)
* **วัตถุประสงค์:** วิเคราะห์ส่วนต่างราคาระหว่างเกรดสินค้า (Arbitrage & Quality Premiums)
* **Visuals ภายในหน้า:**
  1. **Header Banner:** ชื่อหน้าวิเคราะห์ส่วนต่างราคา
  2. **KPI Card 1 - Quality Gap:** `High vs Low Quality Spread` ($/MT)
  3. **KPI Card 2 - Fragrant Premium:** `Fragrant Rice Premium %` (%)
  4. **KPI Card 3 - Parboiled Spread:** `Parboiled vs White Spread` ($/MT)
  5. **KPI Card 4 - Regional Spread:** `Thai vs Viet Spread` ($/MT)
  6. **KPI Card 5 - Spread Deviation:** `Price Volatility (SD)` ($/MT)
  7. **Spread Corridor Line Chart:** กราฟเส้นแสดงกรอบและทิศทางส่วนต่างราคาย้อนหลัง
  8. **Grade Differential Arbitrage Matrix:** ตารางสรุปส่วนต่างราคาพรีเมียมและค่าเบี่ยงเบนมาตรฐานรายสินค้า

---

### 📌 Page 6: `Volatility & Risk Analytics` (Exposure Management View)
* **วัตถุประสงค์:** ติดตามความผันผวนและบริหารความเสี่ยงด้านราคา
* **Visuals ภายในหน้า:**
  1. **Header Banner:** ชื่อหน้าบริหารความเสี่ยง
  2. **KPI Card 1 - Volatility CV:** `Price Volatility% (CV)` (%)
  3. **KPI Card 2 - Rolling 30D Volatility:** `Rolling 30D Volatility %` (%)
  4. **KPI Card 3 - Price StdDev:** `Price StdDev` ($/MT)
  5. **KPI Card 4 - 52W High:** `52 Week H` ($/MT)
  6. **KPI Card 5 - 52W Low:** `52 Week L` ($/MT)
  7. **52-Week High/Low Channel Chart:** กราฟเส้นแสดงราคาเฉลี่ยเทียบกับกรอบเพดานและพื้นราคาในรอบ 52 สัปดาห์
  8. **Product Risk Matrix:** ตารางจัดลำดับความเสี่ยงสินค้าเรียงตามค่า Volatility CV%

---

### 📌 Page 7: `Seasonality & Historical Cycles` (Pattern Recognition View)
* **วัตถุประสงค์:** วิเคราะห์แพทเทิร์นฤดูกาลและวงจรราคาย้อนหลัง (ปี 2020-2026)
* **Visuals ภายในหน้า:**
  1. **Header Banner:** ชื่อหน้าวิเคราะห์ฤดูกาล
  2. **KPI Card 1 - Recent Month Price:** `Avg Price Last Month` ($/MT)
  3. **KPI Card 2 - MoM % Change:** `Avg Price MoM%` (%)
  4. **KPI Card 3 - YoY % Change:** `Avg Price YoY%` (%)
  5. **KPI Card 4 - 2024 Average:** `2024 Avg Price` ($/MT)
  6. **KPI Card 5 - 2025 Average:** `2025 Avg Price` ($/MT)
  7. **Year-Over-Year Monthly Comparison Chart:** กราฟเส้นเปรียบเทียบแนวโน้มราคารายเดือน (Jan-Dec) แยกเส้นตามปี 2020-2026
  8. **Quarterly & Monthly Seasonality Matrix:** ตาราง Matrix แสดงราคาเฉลี่ยตัดตามไตรมาสและเดือนรายปี

---

### 📌 Page 8: `Trading Signals & Momentum Alert` (Operational Signal View)
* **วัตถุประสงค์:** ส่งสัญญาณเตือนการซื้อขายและตรวจจับความผิดปกติของราคา (Moving Average Crossovers & Outliers)
* **Visuals ภายในหน้า:**
  1. **Header Banner:** ชื่อหน้าสัญญาณเทรดเดอร์
  2. **KPI Card 1 - MA 90D Signal Flag:** `MA 90D Signal Flag` (Bullish / Bearish Trend)
  3. **KPI Card 2 - 90D Moving Average:** `Avg Price 90D MA` ($/MT)
  4. **KPI Card 3 - Gap to 52W High:** `Gap to 52W High %` (%)
  5. **KPI Card 4 - Gap to 52W Low:** `Gap to 52W Low %` (%)
  6. **KPI Card 5 - Daily Change:** `△Day` ($/MT)
  7. **Moving Average Crossover Chart:** กราฟเส้นเปรียบเทียบราคาจริงเทียบกับเส้นค่าเฉลี่ย 90D MA และ 120D MA
  8. **Trading Signal & Outlier Matrix:** ตารางแสดงสัญญาณเทรดเดอร์และระยะห่างจากจุดสูงสุด/ต่ำสุดรายสินค้า

---

## 🛠️ โครงสร้างทางเทคนิค (Technical Architecture)

* **Semantic Model (`Project_Oryza_Price.SemanticModel`):**
  - **Data Language:** TMDL (Tabular Model Definition Language)
  - **Star Schema Tables:** `Fct_Oryza Price`, `Dm_Category`, `Dm_Product Name`, `Dm_Date`, `DAX`
  - **DAX Folder Structure:** Organized into 9 Display Folders (`01. Core Price Metrics`, `02. Time Intelligence`, `03. Trend & Moving Averages`, `04. Market Volatility`, `05. Benchmarks & Price Spreads`, `06. Rankings`, `07. UI & System Helpers`, `08. Volatility Metrics`, `09. Variance Analysis`)
* **Report Definition (`Project_Oryza_Price.Report`):**
  - **Format:** Fabric Enhanced Report Format (PBIR Schema v2.1.0)
  - **Grid Layout Standard:** 1280x720 (16:9), 24px Margin, 16px Spacing between visuals
