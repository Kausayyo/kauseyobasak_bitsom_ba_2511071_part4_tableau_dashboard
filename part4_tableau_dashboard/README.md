# Part 4: Tableau Executive Dashboard & Data Storytelling

**Student:** Kauseyo Basak | **ID:** bitsom_ba_2511071
**Course:** Business Analytics | **Assignment Part:** 4 of 4

---

## Business Problem Summary

A retail leadership team requires an executive dashboard to monitor sales performance, profitability, customer segments, category performance, shipping performance, discount impact, and return patterns across regions, categories, and time. The dashboard must tell a clear story and support decision-making — not simply display charts.

The key business questions driving the dashboard design:
1. Where is revenue growing and where is it declining over time?
2. Which regions, categories, and segments drive the most value?
3. How do discounts affect profitability, and is the current discount strategy sustainable?
4. What are the return patterns, and where are operational losses concentrated?
5. How does shipping performance affect customer satisfaction?

---

## Dataset Description

| Property | Value |
|---|---|
| File | `data/dashboard_sales_data.xlsx` |
| Rows | 4,200 orders |
| Columns | 20 |
| Date Range | January 2024 – December 2025 (24 months) |
| Regions | North, South, East, West |
| Categories | Technology, Furniture, Office Supplies |
| Sub-categories | 13 (Copiers, Phones, Accessories, Machines, Tables, Chairs, Bookcases, Furnishings, Storage, Labels, Binders, Art, Paper) |
| Customer Segments | Consumer, Corporate, Home Office |
| Ship Modes | Standard Class, Second Class, First Class, Same Day |
| Campaign Channels | Organic, Social, Referral, Paid, Email (with ~5% nulls) |

### Key Dataset Statistics

| Metric | Value |
|---|---|
| Total Sales | ₹217.0M |
| Total Profit | ₹33.3M |
| Overall Profit Margin | 15.3% |
| Return Rate | 4.55% |
| Average Delivery Days | 3.6 |
| Mean Discount | 13.7% |

---

## Tableau Workbook Description

**File:** `tableau/executive_dashboard.twbx`
**Format:** Tableau Packaged Workbook (data embedded)
**Version:** Compatible with Tableau Desktop 2023.x and later

### Worksheets (Tabs)

| Sheet Name | Chart Type | Business Question |
|---|---|---|
| Sales Trend View | Line chart + YoY comparison | How are sales evolving over time? |
| Regional Performance View | Horizontal bar chart | Which region leads in sales and margin? |
| Category Profitability View | Grouped bar + sorted bar | Which categories and sub-categories drive profit? |
| Customer Segment View | Side-by-side bar | How do segments compare in sales and profit? |
| Shipping Performance View | Bar chart | Which shipping modes cause delays? |
| Discount vs Profit View | Scatter plot | How does discounting affect margin? |
| Return Analysis View | Bar chart | Where are return risks concentrated? |
| KPI Summary | Text/BAN | Top-level KPI reference view |

### Executive Dashboard

The main dashboard combines:
- 4 KPI cards (Total Sales, Total Profit, Profit Margin, Return Rate)
- Sales Trend View (line chart, monthly granularity)
- Regional Performance View (horizontal bar)
- Category Profitability View (sorted bar)
- Discount vs Profit View (scatter plot)
- Return Analysis View (bar chart)

---

## Calculated Fields Created

| Field Name | Formula | Type |
|---|---|---|
| **Profit Margin** | `[profit] / [sales]` | Measure (continuous %) |
| **Cost** | `[sales] - [profit]` | Measure (₹) |
| **Average Order Value** | `[sales] / [quantity]` | Measure (₹) |
| **Return Rate** | `SUM([return_flag]) / COUNT([order_id])` | Measure (%) |
| **Shipping Delay Bucket** | `IF [delivery_days] <= 1 THEN "0-1 days (Fast)" ELSEIF [delivery_days] <= 3 THEN "2-3 days (Normal)" ELSEIF [delivery_days] <= 5 THEN "4-5 days (Slow)" ELSE "6+ days (Very Slow)" END` | Dimension (string) |

---

## Dashboard Components

### Charts and Views
1. Monthly Sales Trend — line chart with 3-month moving average overlay
2. Year-over-Year Sales Comparison — multi-line chart by year
3. Sales and Profit by Region — horizontal bar with profit margin colour encoding
4. Profit by Sub-Category — horizontal sorted bar, green (positive) / orange (low margin)
5. Discount vs Profit Margin — scatter plot with category colour and sales size encoding
6. Shipping Delay Distribution — bar chart by delay bucket
7. Return Rate by Category — bar chart with categorical colour

### KPI Cards (3 required, 4 provided)
1. **Total Sales** — ₹217.0M
2. **Total Profit** — ₹33.3M
3. **Profit Margin** — 15.3%
4. **Return Rate** — 4.55%

---

## Filters and Interactions

### Dashboard Filters (Interactive — applied to all views)
| Filter | Field | Type |
|---|---|---|
| Region Filter | `[region]` | Categorical multi-select |
| Category Filter | `[category]` | Categorical multi-select |
| Customer Segment Filter | `[customer_segment]` | Categorical multi-select |
| Ship Mode Filter | `[ship_mode]` | Categorical multi-select |
| Campaign Channel Filter | `[campaign_channel]` | Categorical multi-select |

### Dashboard Actions
- **Filter Action:** Clicking a region bar in the Regional Performance View filters all other charts on the dashboard to show only that region's data (filter action from Regional Performance to all sheets).
- **Highlight Action:** Hovering over any data point highlights related marks across connected views.
- **URL Action (optional):** Category bars link to expanded category detail view.

---

## Key Business Insights

1. **Technology dominates:** 70.9% of sales, 84.2% of profit — the business is heavily dependent on one category.
2. **Furniture underperforms on margin:** 6.9% margin vs 15.3% overall — highest volume, lowest margin category.
3. **South region leads:** ₹64.7M sales (30% of total), consistent margin performance.
4. **Discounts above 25% erode margin:** −0.27 correlation between discount rate and profit margin.
5. **Furniture returns are 2.5× higher than Technology:** 7.67% vs 3.03% — operational risk area.
6. **Q4 seasonal peak:** October–December months consistently drive highest sales.
7. **Standard Class shipping creates delay risk:** 21%+ of orders take 5+ days, below modern delivery expectations.
8. **Home Office segment slightly outperforms:** Highest sales (₹74.5M) and profit (₹11.6M) across segments.

---

## Dashboard Story Summary

The business is healthy and growing, with Technology driving strong profits at 18% margin. However, three issues require immediate attention: (1) Furniture's 6.9% margin and 7.67% return rate are consuming operational resources without proportional profit return; (2) discounting above 25% is systematically diluting margins without clear evidence of volume uplift; (3) 21% of orders experiencing 5+ day delivery is a latent churn risk in a market with rising speed expectations.

Priority actions: implement discount guardrails, investigate Furniture return drivers, promote faster shipping for Technology orders, and replicate South region playbook in East and West.

See `outputs/dashboard_story.md` for the full narrative.

---

## Assumptions and Limitations

- `profit` is net margin after all costs; no COGS vs overhead breakdown is available.
- `return_flag` is binary (0/1) — no information on reason for return, return window, or resolution.
- `campaign_channel` has ~5% null values, making marketing attribution incomplete.
- The 24-month dataset covers only 2024–2025 and may not represent long-term structural trends.
- Geographic analysis uses region groupings (North/South/East/West), not state-level coordinates — a geographic map chart would be misleading.
- `delivery_days` is the difference between `order_date` and `ship_date`, not the time to customer receipt — actual delivery may be longer.
- The workbook uses `excel-direct` connection with an embedded Excel file. When opening `executive_dashboard.twbx`, Tableau will extract and use the packaged `dashboard_sales_data.xlsx` automatically.

---

## Screenshots Included

| File | Contents |
|---|---|
| `screenshots/full_dashboard.png` | Complete executive dashboard with all 5+ charts and 4 KPI cards |
| `screenshots/sales_trend_view.png` | Monthly sales trend with YoY comparison |
| `screenshots/regional_performance_view.png` | Sales and margin by region and top states |
| `screenshots/category_profitability_view.png` | Category-level and sub-category profit analysis |
| `screenshots/filter_interaction_view.png` | Dashboard filtered to Consumer segment, North region, 2024 — showing interactive filter behaviour |

---

## Repository Structure

```
part4_tableau_dashboard/
├── data/
│   └── dashboard_sales_data.xlsx
├── tableau/
│   └── executive_dashboard.twbx
├── outputs/
│   ├── dashboard_story.md
│   ├── business_insights.md
│   └── chart_selection_justification.md
├── screenshots/
│   ├── full_dashboard.png
│   ├── sales_trend_view.png
│   ├── regional_performance_view.png
│   ├── category_profitability_view.png
│   └── filter_interaction_view.png
└── README.md
```

---

## How to Open the Tableau Workbook

1. Ensure Tableau Desktop 2023.1 or later is installed.
2. Double-click `tableau/executive_dashboard.twbx` — Tableau will open the packaged workbook with the embedded data.
3. The Executive Dashboard tab will show by default. Use the tabs at the bottom to navigate to individual worksheet views.
4. Use the filter controls on the right side of the dashboard to filter by Region, Category, Customer Segment, Ship Mode, or Campaign Channel.
5. Click on a bar in the Regional Performance View to filter all other charts to that region.
