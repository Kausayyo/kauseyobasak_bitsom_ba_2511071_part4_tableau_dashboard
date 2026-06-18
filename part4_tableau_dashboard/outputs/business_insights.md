# Business Insights
**Part 4 — Tableau Executive Dashboard & Data Storytelling**
**Student:** Kauseyo Basak | **ID:** bitsom_ba_2511071
**Date:** 2026-06-18

---

## Dataset Summary

| Metric | Value |
|---|---|
| Total Orders | 4,200 |
| Total Sales | ₹217.0M |
| Total Profit | ₹33.3M |
| Overall Profit Margin | 15.3% |
| Return Rate | 4.55% |
| Average Delivery Days | 3.6 days |
| Date Range | January 2024 – December 2025 |
| Regions | North, South, East, West |
| Categories | Technology, Furniture, Office Supplies |

---

## Calculated Fields Used

| Calculated Field | Formula | Purpose |
|---|---|---|
| **Profit Margin** | `[profit] / [sales]` | Shows what fraction of revenue converts to profit; comparable across store sizes |
| **Cost** | `[sales] - [profit]` | Separates revenue into profit and cost components |
| **Average Order Value** | `[sales] / [quantity]` | Tracks unit value per item sold; useful for monitoring basket composition |
| **Return Rate** | `SUM([return_flag]) / COUNT([order_id])` | Measures the fraction of orders returned; a quality and satisfaction signal |
| **Shipping Delay Bucket** | `IF [delivery_days] <= 1 THEN "0-1 days (Fast)" ELSEIF [delivery_days] <= 3 THEN "2-3 days (Normal)" ELSEIF [delivery_days] <= 5 THEN "4-5 days (Slow)" ELSE "6+ days (Very Slow)" END` | Groups orders into four delivery speed categories for filtering and colour-coding |

---

## Insight 1 — Sales Trend: Seasonal Peaks in Q4 Drive Revenue

**Observation:** Monthly sales in Q4 (October–December) consistently exceeded ₹10M both in 2024 and 2025. The weakest months were April–June with sales falling below ₹8M. The year-over-year comparison shows 2025 tracking higher than 2024 from July onwards.

**Data Evidence:** 2024 Q4 average monthly sales were approximately ₹12–14M; 2025 Q3 onwards showed sustained growth above ₹10M per month.

**Business Interpretation:** Retail sales follow a predictable seasonal pattern driven by year-end purchasing cycles, festive season shopping, and corporate procurement cycles in Q4. The April–June trough likely reflects post-Q1 procurement slowdowns and pre-monsoon retail lulls.

**Recommended Action:** Leadership should pre-plan inventory build-ups by September to maximise availability during the Q4 peak. Marketing budgets should be front-loaded into September–November rather than spread evenly throughout the year.

---

## Insight 2 — Regional Performance: South Region Leads in Volume but Margin Varies

**Observation:** South region generated the highest total sales (₹64.7M, 29.8% of total), outperforming North (₹54.6M), West (₹48.9M), and East (₹48.9M). However, regional profit margins are close to one another (all between 14.5% and 16%), suggesting the South advantage is primarily volumetric rather than structural.

**Data Evidence:** Regional sales: South ₹64.7M → North ₹54.6M → West ₹48.9M → East ₹48.9M. Margin range across regions: approximately 14.5%–16.0%.

**Business Interpretation:** South's volume lead may reflect a larger customer base, denser retailer coverage, or stronger campaign effectiveness. The near-equal margins across regions mean that profit improvement must come from category mix and cost management rather than relying on regional factors.

**Recommended Action:** Investigate whether South's volume advantage comes from higher customer acquisition or higher repeat purchase rates. If it is acquisition, replicating those campaign strategies in East and West could close the gap.

---

## Insight 3 — Category Performance: Technology Dominates, Office Supplies Under-Indexed

**Observation:** Technology contributed ₹153.9M in sales (70.9% of total) and ₹28.0M in profit (84.2% of total profit), making it the dominant engine of the business. Furniture contributed ₹51.6M in sales but only ₹3.6M in profit (a 6.9% margin). Office Supplies contributed the lowest sales at ₹11.5M but with an 14.9% margin, close to the overall average.

**Data Evidence:** Technology margin: ₹28.0M / ₹153.9M = 18.2%. Furniture margin: ₹3.6M / ₹51.6M = 6.9%. Office Supplies margin: ₹1.7M / ₹11.5M = 14.9%.

**Business Interpretation:** Technology is the profit engine. Furniture generates substantial revenue but very thin margins — it is possible the business is using furniture as a traffic driver (with aggressive discounting) but not managing its margin discipline carefully. Office Supplies is small but healthy.

**Recommended Action:** Conduct a product mix review within Furniture to identify which sub-categories (Tables, Bookcases, Chairs, Furnishings) are margin-dilutive. Consider pricing or volume changes that improve Furniture margin toward at least 10%.

---

## Insight 4 — Sub-Category Profitability: Copiers and Accessories Are the Core Profit Drivers

**Observation:** The top 4 profit contributors are Copiers (₹7.3M), Accessories (₹7.2M), Phones (₹7.1M), and Machines (₹6.4M) — all Technology sub-categories. The lowest performers are Paper (₹0.32M), Binders (₹0.33M), and Art (₹0.35M) — all Office Supplies — each generating less than ₹0.4M profit on meaningful sales volumes.

**Data Evidence:** Copiers: ₹40.6M sales, ₹7.3M profit (18.0% margin). Paper: ₹2.0M sales, ₹0.32M profit (16.0% margin). No sub-category showed negative total profit — the business does not have a loss-making product line at the aggregate level.

**Business Interpretation:** The Technology sub-categories are the primary value creation engine. Office Supplies sub-categories are relatively small and low-margin but are not destroying value. The concern is the long-tail of Office Supplies products consuming working capital and shelf space for minimal profit contribution.

**Recommended Action:** Review the Office Supplies portfolio for rationalisation opportunities. Products that occupy warehouse space but contribute under ₹0.5M in profit annually may not be worth maintaining unless they serve a customer retention function.

---

## Insight 5 — Discount Impact: Discounts Above 25% Erode Profit Margins Significantly

**Observation:** Average profit margin for orders with 0–5% discount is approximately 18–20%. For orders with 26–35% discount, average margin drops to approximately 10–12%. For orders with 36%+ discount, margins fall further, in some cases below 5%.

**Data Evidence:** Discount–profit margin correlation: −0.266 (moderate negative). Mean discount across dataset: 13.7%. High-discount orders (>30%) show meaningfully lower average margins than low-discount orders.

**Business Interpretation:** The negative correlation between discount and margin confirms that discounting above 25% is a margin dilutant. This is a pricing discipline issue: if discounts are being offered to close deals without a clear ROI rationale (e.g., to acquire high-LTV customers), they are eroding profitability.

**Recommended Action:** Implement discount guardrails. Any discount above 20% should require manager approval with a documented business justification. A/B testing of discount levels by segment could quantify whether heavy discounting actually drives incremental volume or simply transfers margin to customers.

---

## Insight 6 — Shipping Performance: Standard Class Dominates Usage but Creates Delay Risk

**Observation:** Standard Class accounts for 56.6% of all orders (by sales: ₹122.8M) with an average delivery time of 4.7 days. Same Day delivers in 0.4 days on average and has the lowest volume (6.7% of sales). 21.4% of all orders take more than 5 days to deliver, putting them in the "Slow" or "Very Slow" delay bucket.

**Data Evidence:** Avg delivery by mode: Same Day 0.4 days → First Class 1.8 days → Second Class 2.7 days → Standard Class 4.7 days. 464 orders took 6–7 days; 45 orders took 8–9 days.

**Business Interpretation:** Over 21% of orders experiencing 5+ day delivery is a customer satisfaction risk. For technology products (which dominate sales), delivery expectations are high, and slow shipping may contribute to returns and lower customer ratings.

**Recommended Action:** Identify the order characteristics associated with the slowest deliveries (region, category, city). For orders where customers paid for First Class or Second Class but received Standard Class delivery speed, investigate whether SLA breaches are occurring. A 2-day SLA guarantee for Technology orders could be a differentiator.

---

## Insight 7 — Return Rate: Furniture Returns Are 2.5× Higher Than Technology

**Observation:** Furniture has a return rate of 7.67% — the highest across all categories. Technology's return rate is 3.03% and Office Supplies is 3.65%. The overall return rate is 4.55%.

**Data Evidence:** Furniture returns: 7.67% of Furniture orders returned. Technology returns: 3.03%. The gap (4.64 percentage points) is statistically and operationally significant given 4,200 orders and ₹51.6M in Furniture sales.

**Business Interpretation:** Furniture returns are expensive: they involve logistics cost, restocking cost, and potential value deterioration. A 7.67% return rate on ₹51.6M in sales means approximately ₹3.96M of furniture orders are being returned annually — a meaningful operational burden. Possible causes: product quality issues, wrong size/fit, misleading product descriptions, or delivery damage.

**Recommended Action:** Conduct root cause analysis on Furniture returns. If damage-in-transit is a contributor, improved packaging or specialist furniture logistics would help. If product description mismatch is the cause, improving imagery and measurement detail on product pages would reduce returns.

---

## Insight 8 — Business Risk: Revenue Concentration in Technology Creates Vulnerability

**Observation:** Technology represents 70.9% of total sales and 84.2% of total profit. If a single large category experiences a market disruption — such as a supply chain shock, a price war driven by new competitors, or a shift in customer purchasing platforms — the business's profitability would be severely impacted.

**Data Evidence:** Without Technology, the remaining business (Furniture + Office Supplies) generates ₹63.1M in sales and only ₹5.3M in profit (8.4% margin). This is well below the business's stated overall margin of 15.3%.

**Business Interpretation:** The business is heavily dependent on a single category that happens to have good margins currently. This is a strategic risk: if Technology margins compress (due to competition or commoditisation), the business has no other category generating adequate returns to compensate.

**Recommended Action:** The leadership team should evaluate whether to (a) increase investment in Technology to maintain the advantage, (b) invest in improving Furniture margins to reduce concentration risk, or (c) expand Office Supplies to build a more balanced portfolio. At minimum, a scenario analysis should be conducted to stress-test the business's profitability under a 5-point Technology margin compression.

---

## Insight 9 — Customer Segment: Home Office Shows Highest Profit, Consumer Slightly Lower

**Observation:** Home Office segment generated the highest total sales (₹74.5M, 34.3%) and profit (₹11.6M, 34.7%). Consumer and Corporate segments are very similar in both sales and profit. Profit margins are virtually identical across all three segments (approximately 15.3%).

**Data Evidence:** Home Office: ₹74.5M sales, ₹11.6M profit. Corporate: ₹70.6M sales, ₹10.7M profit. Consumer: ₹71.9M sales, ₹11.0M profit.

**Business Interpretation:** Segment performance is nearly homogeneous — there is no segment that is dramatically more profitable or more at risk. This is actually a positive sign: the business does not have a segment subsidising another. However, it also means that segment-based targeting decisions should be driven by acquisition cost and lifetime value analysis rather than current transaction profitability.

**Recommended Action:** Enrich the dataset with customer acquisition cost (CAC) and lifetime value (LTV) data by segment. If Home Office customers have lower churn and higher repeat purchase frequency, they may warrant a dedicated loyalty programme or subscription offer.

---

## Insight 10 — Campaign Channel Effectiveness

**Observation:** The dataset includes a `campaign_channel` field with values: Organic, Social, Referral, Paid, Email (and some nulls). Initial analysis shows differences in average order value and profit margin across channels, suggesting that channel mix has an impact on order quality.

**Data Evidence:** The campaign_channel field has some null values (approximately 5%). Organic and Referral channels may show higher average order values as customers arrive with higher intent.

**Business Interpretation:** Understanding which marketing channels drive the most profitable orders (not just the most orders) is critical for marketing ROI optimisation. High-discount-dependent channels may look good in volume but poor in margin.

**Recommended Action:** Build a channel attribution analysis segmented by profit margin and discount rate. Channels that drive orders with margins significantly below average should be evaluated for ROI. Email and Referral channels typically have lower CAC and should be prioritised if they show comparable order quality.
