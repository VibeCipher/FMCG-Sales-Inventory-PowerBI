# FMCG Sales & Inventory Performance Dashboard

A Power BI dashboard analyzing 2 years of FMCG personal-care sales and inventory data across 5 regions, 14 cities, and 4 sales channels — built to surface revenue trends, product performance, channel mix, and inventory health.

---

## Dashboard Pages

| Page | What it shows |
|---|---|
| Executive Overview | KPI cards, monthly revenue trend (2024 vs 2025), revenue by region, channel mix |
| Product Performance | Top 10 SKUs by revenue, category breakdown matrix, discount vs volume scatter |
| Regional & Channel Deep Dive | Region × channel revenue matrix, city-level bar, channel trend over time |
| Inventory Health Monitor | Stock vs reorder level by category, stockout risk table with conditional formatting |

---

## Screenshots

### Executive Overview
![Executive Overview](screenshots/1_executive_overview.png)

### Product Performance
![Product Performance](screenshots/2_product_performance.png)

### Regional & Channel Deep Dive
![Regional Channel](screenshots/3_regional_channel.png)

### Inventory Health Monitor
![Inventory Health](screenshots/4_inventory_health.png)

---

## Tech Stack

- **Power BI Desktop** — report building, visual design
- **Power Query (M)** — data cleaning and transformation
- **DAX** — calculated measures including time intelligence

---

## Data Model

Star schema with 2 fact tables and 2 dimension tables:

```
DateTable (dim) ──────── Sales_Transactions (fact)
                    │
Product_Master (dim) ────┤
                    │
                    └─── Inventory_Snapshot (fact)
```

---

## DAX Measures (key ones)

```dax
-- Year-over-Year growth
Revenue YoY % = DIVIDE([Total Revenue] - [Revenue PY], [Revenue PY])

-- Prior year comparison using time intelligence
Revenue PY = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(DateTable[Date]))

-- Stockout risk flag based on reorder level
Stockout Risk Flag = IF([Closing Stock] < SUM(Inventory_Snapshot[ReorderLevel]), "At Risk", "Healthy")

-- Months of inventory cover remaining
Months of Cover = DIVIDE([Closing Stock], CALCULATE(AVERAGE(Inventory_Snapshot[Sold]), 
    ALLEXCEPT(Inventory_Snapshot, Inventory_Snapshot[ProductID], Inventory_Snapshot[Region])))
```

---

## Data

Synthetic dataset modeled on realistic FMCG distribution patterns:
- **23,000+ sales transactions** across Jan 2024 – Dec 2025
- **22 SKUs** across 8 product categories (razors, trimmers, shaving cream, gift sets, etc.)
- **5 regions**, 14 cities, 4 sales channels (General Trade, Modern Trade, E-commerce, Pharmacy)
- **2,640 monthly inventory snapshots** by region and SKU
- Data was intentionally seeded with quality issues (duplicates, casing inconsistencies, blank values) — all cleaned in Power Query

---

## Key Insights

- **General Trade dominates** at ~46% of total revenue; E-commerce growing steadily
- **Trimmers are the highest-revenue category** despite lower unit volumes — driven by high MRP
- **South region leads** in total revenue across both years
- **Festive season spike visible** in Oct–Dec on the monthly trend (2025 especially)
- **All inventory is healthy** — Months of Cover consistently 20–35 months across all SKUs and regions, well above reorder levels

---

## Note on YoY % with no year filter

When no year is selected in the slicer, Revenue YoY % shows an inflated number because SAMEPERIODLASTYEAR is comparing the full 2-year total against itself. Filter to 2025 to see the correct 7.14% growth figure. This is expected DAX behavior with time intelligence functions.

---

## What I'd improve with more time

- Add a dedicated **Brand-level performance** page (3 brands in the data: VELORA, SHARPEDGE, CLEANCUT, GROOMIQ)
- Build a **forecast visual** using Power BI's built-in analytics pane (trend line + forecast)
- Add **drill-through** from the region bar chart to a city-level detail page
- Replace synthetic data with real aggregated public FMCG data if available

---

## Connect

- LinkedIn: [sohamchatterjee-data](https://linkedin.com/in/sohamchatterjee-data)
- GitHub: [VibeCipher](https://github.com/VibeCipher)
