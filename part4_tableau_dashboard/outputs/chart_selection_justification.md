# Chart Selection Justification
**Part 4 — Tableau Executive Dashboard & Data Storytelling**
**Student:** Kauseyo Basak | **ID:** bitsom_ba_2511071
**Date:** 2026-06-18

---

## Overview

Every chart in the Executive Dashboard was selected to answer a specific business question using the most appropriate visual encoding. This document explains the reasoning behind each chart type, the design principles applied, and the mistakes deliberately avoided.

---

## Chart 1 — Monthly Sales Trend (Line Chart)

**Business Question:** How are sales changing over time? Are there seasonal patterns and growth trends?

**Chart Type:** Line chart

**Why This Chart Type Is Appropriate:**
A line chart is the canonical choice for showing a continuous quantitative variable (sales in ₹M) over a sequential time dimension (months). The connected line makes it easy to track the direction, pace, and volatility of change. A bar chart over 24 time periods would create visual clutter and make trend detection harder. A scatter plot would break the visual continuity that conveys the time-series nature of the data.

**Fields Used:**
- X-axis (Columns): `YEAR([order_date])` and `MONTH([order_date])` — ordered temporal hierarchy
- Y-axis (Rows): `SUM([sales])` — aggregated monthly revenue
- Color (secondary line): `AVG([sales])` 3-month moving average for smoothing short-term noise
- Reference line: overall average to contextualise monthly performance

**Design Principle Applied:**
Reducing noise while preserving signal. The 3-month moving average overlay helps leadership see the underlying trend without being distracted by single-month spikes or dips. The fill under the line reinforces the directional interpretation (rising = more area, falling = less area).

**Mistake Avoided:**
Dual-axis charts that mix a line for Sales and bars for a secondary metric were avoided because they require readers to mentally track two different scales simultaneously. The single-axis line with an overlay is cleaner for a leadership audience.

---

## Chart 2 — Regional Performance (Horizontal Bar Chart + Colour-Coded Margin)

**Business Question:** Which region performs best in sales, and how do profit margins compare?

**Chart Type:** Horizontal bar chart with colour encoding for profit margin

**Why This Chart Type Is Appropriate:**
A bar chart is ideal for comparing a single metric (total sales) across a small number of named categories (4 regions). The horizontal orientation allows the region name labels to read clearly without rotation. A pie chart was explicitly rejected here because pie charts distort perception of proportional differences when slices are similar in size (all four regions are within 25% of one another). A map was considered but rejected because the business uses custom Indian region groupings (North/South/East/West) that do not correspond to standard state-level geographic boundaries, making a map misleading.

**Fields Used:**
- Y-axis (Rows): `[region]` — categorical dimension
- X-axis (Columns): `SUM([sales])` — total revenue per region
- Color: `[Calc_ProfitMargin]` (continuous scale, green = higher margin) — adds a second information layer without requiring a separate chart

**Design Principle Applied:**
Maximising information density without clutter. The dual encoding (length for sales, colour for margin) allows leadership to see two variables in one chart. The bar chart is sorted by sales value so the ranking is immediately readable.

**Mistake Avoided:**
Using a 3D bar chart, which introduces perspective distortion that makes the bars' actual heights visually misleading. All charts use 2D encoding only.

---

## Chart 3 — Category and Sub-Category Profitability (Grouped Bar + Sorted Bar)

**Business Question:** Which categories and sub-categories drive profit, and which are underperforming?

**Chart Type:** Grouped bar chart (category level) + Horizontal sorted bar chart (sub-category level)

**Why This Chart Type Is Appropriate:**
The grouped bar at category level shows Sales vs Profit side by side, allowing leadership to immediately see the ratio of profit to sales for each category — revealing Furniture's low profit despite high sales. The sorted horizontal bar at sub-category level ranks all 13 sub-categories from lowest to highest profit, making it immediately obvious which sub-categories are the profit drivers and which are not.

**Fields Used:**
- Category chart: X-axis `[category]`, Y-axis grouped `SUM([sales])` and `SUM([profit])`, text annotation: `[Calc_ProfitMargin]`
- Sub-category chart: Y-axis (Rows) `[sub_category]` sorted by `SUM([profit])`, X-axis `SUM([profit])`, color: positive = green, negative = would be orange (no negative values in this dataset)

**Design Principle Applied:**
Appropriate sorting. Sub-categories are sorted by profit (ascending) so the viewer reads from weakest to strongest performance — a common analytical sorting convention. Category grouping uses distinct colours that are consistent with the dashboard's colour palette (blue for Sales, green for Profit).

**Mistake Avoided:**
A treemap was considered for sub-category profitability. However, treemaps encode information in area, which is a less precise encoding than length (position on a common scale). For a precision-oriented leadership audience comparing specific numbers, the sorted bar chart is more readable and more honest.

---

## Chart 4 — Customer Segment Performance (Side-by-Side Bar)

**Business Question:** Which customer segment generates the most sales and profit? Are margins different across segments?

**Chart Type:** Side-by-side horizontal bar chart

**Why This Chart Type Is Appropriate:**
With only three customer segments, a bar chart provides a clean and legible comparison. Side-by-side bars allow leadership to simultaneously compare the absolute magnitude of sales and profit for each segment, and visually estimate the profit-to-sales ratio. A stacked bar was considered but rejected because it makes comparing the actual segment sizes harder (readers would need to mentally subtract the stacked portions).

**Fields Used:**
- Y-axis: `[customer_segment]`
- X-axis: `SUM([sales])` and `SUM([profit])` side by side
- Color: `[customer_segment]` — categorical palette, one colour per segment, consistent throughout the dashboard

**Design Principle Applied:**
Consistent colour encoding. Each customer segment uses the same colour in this chart and in any other chart on the dashboard that includes segments as a colour dimension. This allows leadership to visually track a segment's performance across multiple charts without re-learning the encoding each time.

**Mistake Avoided:**
Using a pie chart to show segment share. Although "share" is a natural framing for segment analysis, the three segments are very similar in size (all within 5% of one another), making a pie chart nearly uninformative — the slices look nearly identical and the differences are imperceptible.

---

## Chart 5 — Discount vs Profit Margin (Scatter Plot + Trend Line)

**Business Question:** Does higher discounting reduce profit margin? What is the relationship?

**Chart Type:** Scatter plot with colour by category and trend line

**Why This Chart Type Is Appropriate:**
A scatter plot is the definitive chart type for showing the relationship between two continuous quantitative variables — discount rate and profit margin. Each point represents one order, allowing the viewer to see both the direction and strength of the relationship, as well as outliers. A bar chart cannot show this relationship at the individual order level. A line chart would imply a sequential ordering that does not exist in this data.

**Fields Used:**
- X-axis: `AVG([discount])` — discount percentage
- Y-axis: `AVG([Calc_ProfitMargin])` — profit as percentage of sales
- Color: `[category]` — categorical, three colours, reveals whether the discount-margin relationship differs by category
- Size: `SUM([sales])` — bubble size encodes revenue, adding a third dimension without clutter
- Trend line: linear regression line showing the overall directional relationship

**Design Principle Applied:**
Adding meaningful dimensionality. The three-variable encoding (discount on X, margin on Y, category as colour) answers multiple questions simultaneously: (1) What is the discount-margin relationship? (2) Does it differ by category? (3) Where is most of the revenue sitting in the discount space? This avoids needing three separate charts.

**Mistake Avoided:**
Using bubble size as a purely decorative element. The size encoding carries genuine information (total sales volume at that discount-margin combination), and the legend clearly explains what size represents.

---

## Chart 6 — Shipping Performance (Bar Chart + Delay Distribution)

**Business Question:** Which shipping modes are slowest? How many orders fall into problematic delay categories?

**Chart Type:** Horizontal bar chart (mode comparison) + Vertical bar chart (delay bucket distribution)

**Why This Chart Type Is Appropriate:**
The horizontal bar comparing average delivery days by ship mode gives an immediate ranked comparison of the four modes. The vertical bar showing delay bucket distribution (0–1 days, 2–3 days, 4–5 days, 6+ days) shows the frequency distribution of delay severity across all orders — a different and complementary perspective. Together, they answer both "which mode is slowest on average" and "how many orders are actually experiencing bad delays."

**Fields Used:**
- Mode chart: Y-axis `[ship_mode]`, X-axis `AVG([delivery_days])`, color `[Calc_DelayBucket]`
- Delay chart: X-axis `[Calc_DelayBucket]` (calculated field), Y-axis `COUNT([order_id])`, color: green → blue → orange → red (fast → slow)

**Design Principle Applied:**
Intuitive colour semantics. Green = fast/good, red = slow/bad. This colour encoding requires no legend explanation — the connotative meaning of green/red is universal. This is appropriate when the business interpretation of the dimension is clearly directional (delay is always undesirable).

**Mistake Avoided:**
Using a histogram with equal bin widths for delivery days. Equal bins (0–2, 2–4, 4–6, 6–8) do not align with the business's natural SLA buckets. The calculated `Shipping Delay Bucket` field creates business-meaningful groupings that translate directly to operational categories.

---

## Chart 7 — Return Analysis (Highlight Table / Bar)

**Business Question:** Which categories and segments have the highest return rates? Where is return risk concentrated?

**Chart Type:** Horizontal bar chart (return rate by category) + Cross-tab highlight table (category × segment)

**Why This Chart Type Is Appropriate:**
The bar chart for return rates by category gives a clear ranking. The highlight table (matrix) of return rates at the category × segment intersection allows leadership to identify whether specific combinations (e.g., Furniture × Consumer vs Furniture × Corporate) show disproportionately high return rates — which would be invisible in a simple bar chart.

**Fields Used:**
- Bar: Y-axis `[category]`, X-axis `AVG([Calc_ReturnRate])`, color: continuous scale from green (low returns) to red (high returns)
- Highlight table: Rows `[customer_segment]`, Columns `[category]`, Color/Label `AVG([return_flag])`

**Design Principle Applied:**
Using the right chart for the right question. The bar chart answers "which category has the highest return rate overall?" The highlight table answers "is the high return rate evenly distributed across segments or concentrated in specific cells?" — which is a more nuanced analytical question that requires the cross-tab format.

**Mistake Avoided:**
Stacking return rates on top of sales in a dual-axis chart. Return rate and sales volume are measured on incompatible scales, and combining them in a dual axis creates visual ambiguity and scale-distortion risks. They are presented separately.

---

## KPI Cards — Design Rationale

The dashboard includes four KPI cards at the top: Total Sales (₹217M), Total Profit (₹33.3M), Profit Margin (15.3%), and Return Rate (4.55%).

**Why KPI Cards:**
Leadership dashboards should begin with the answer, then support it with detail. KPI cards place the most important summary numbers in the most prominent position (top of the dashboard, full-width). This follows the F-pattern reading convention — western readers scan from top-left and the most important information should be encountered first.

**Design Principle Applied:**
Colour coding for semantic meaning: blue for neutral/sales metrics, green for positive/profit, orange/red for risk metrics (return rate). The four-card layout creates visual balance and allows all four metrics to be read simultaneously.

**Mistake Avoided:**
Including too many KPIs. More than 5–6 KPI cards creates cognitive overload. The four selected are the minimum set that covers revenue, profitability, efficiency, and risk — the four dimensions a leadership audience cares about most.
