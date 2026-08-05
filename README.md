# Contoso Retail Analytics - Power BI Dashboard

##  Executive Summary
The **Contoso Retail Analytics** project is an enterprise-grade Power BI dashboard designed to transform raw retail data into actionable insights. The primary objective of this analysis is to diagnose the company’s macro-level financial health, evaluate geographic and physical store efficiency, and optimize product pricing and discount strategies.

---

##  Key Findings & Strategic Insights

### 1. Macro Financial Health & Channel Performance
*   **YoY Contraction:** The business is facing significant headwinds, with Total Revenue ($320.63M) and Total Profit ($179.22M) both experiencing a sharp year-over-year decline of approximately 27.7%. Total orders (164K) and customer count (62K) have also dropped.
*   **Channel Dominance:** Despite overall revenue drops, the Online channel continues to outperform Physical retail, generating $195.06M compared to the physical channel's $125.57M.
*   **Margin Resilience:** Despite the drop in revenue volume, the company has successfully protected its profitability, maintaining a highly healthy Profit Margin of 55.90% (a negligible -0.01% YoY change).

### 2. Product & Pricing Strategy (The 80/20 Rule)
*   **Heavy Reliance on Two Categories:** The Revenue Pareto chart reveals a severe concentration of revenue. "Computers" is the undisputed flagship category ($0.99bn gross), followed by "Cell phones" ($0.38bn). Together, these top categories drive over 60% of the entire company's revenue.
*   **Premium Pricing Wins:** The Price Band Analysis illustrates that "Premium" items ($500+) are not just the highest revenue generators, but they also yield the highest profit margins. Lower-tier items (<$50) contribute minimally to the bottom line.
*   **Controlled Discounting:** The company maintains strict discipline over its pricing strategy. The average overall discount rate sits at just 5.91%, which is the primary reason the profit margin has remained so stable despite top-line revenue struggles.

### 3. Regional & Store Efficiency
*   **Regional Leaders:** The United States is by far the most dominant geographical market, generating $55.78M in top country revenue.
*   **Store Footprint Optimization:** The Store Efficiency Matrix shows distinct clustering. While average sales sit at $1.20K per square meter, the scatter plot identifies outlier stores that are highly efficient, indicating that a larger physical footprint does not guarantee proportional revenue returns. Top-performing stores independently generate upwards of $3.6M.

##  Strategic Recommendations
1.  **Investigate YoY Declines:** With margins stable but volume dropping, investigate top-of-funnel marketing and customer acquisition strategies to reverse the 5.95% drop in total customers.
2.  **Diversify the Product Portfolio:** The extreme reliance on the "Computers" category represents a risk. Implement cross-selling strategies to boost mid-tier categories like Home Appliances and TV/Video.
3.  **Lean into Premium:** Since $500+ items drive the best margins and revenue, marketing spend should be heavily weighted toward these premium SKUs rather than low-tier volume drivers.

---

##  Dashboard Pages & Features

### 1. Executive Overview
A high-level diagnostic view of overall financial health and macro trends.
- **Key Metrics:** Total Revenue, Total Profit, Profit Margin %, Average Order Value (AOV), Total Orders, and Total Customers.
- **Trend Analysis:** Year-to-Date (YTD) Revenue & Profit tracking and Year-over-Year Revenue Growth.
- **Top Performers:** Revenue breakdowns by Top 5 Categories, Top 5 Brands, and Sales Channel (Online vs. Physical).

### 2. Store & Regional Performance
Geospatial and store-level efficiency tracking.
- **Store Efficiency Matrix:** A scatter plot analyzing Sales per Square Meter against Total Revenue to identify highly efficient footprint usage.
- **Geographical Breakdown:** Treemap and regional metrics highlighting top-performing Countries and States.
- **Operational Metrics:** Regional Delivery Performance tracking Same Day Delivery % and Average Delivery Days.
- **Top Stores:** Ranking the Top 5 Stores by Revenue to highlight top physical locations.

### 3. Product & Pricing Performance
Deep-dive analytics into pricing tiers, discount impact, and the 80/20 rule.
- **Revenue Pareto Chart:** A dynamic 80/20 chart highlighting the core product categories driving the vast majority of overall business revenue.
- **Margin Waterfall:** Powered by a custom disconnected bridge table to visualize the journey from Gross Revenue down to Total Profit by subtracting Discounts and Costs.
- **Discount vs. Margin Impact:** A scatter plot identifying pricing performance based on discount rates and resulting profit margins.
- **Price Band Analysis:** Evaluates revenue and margin health across custom pricing tiers (Premium, High, Mid, Low).
- **Drill-Through Detail Matrix:** A granular, SKU-level table for deep-diving into specific product performances.

---

##  Technical Implementation

To uncover these insights, a robust data model and advanced DAX techniques were engineered:
- **Data Modeling (Star Schema):** Designed a highly optimized Star Schema separating Fact tables (Foundation, Channel KPIs, Store KPIs) from Dimension tables (Customer, Date, Product, Store) to ensure scalable performance.
- **Advanced Context Management:** Engineered complex DAX measures using `ALLSELECTED()` and `FILTER()` to accurately calculate Pareto (80/20) cumulative percentages regardless of active user slicers.
- **Custom Financial Modeling:** Implemented a non-relational "Disconnected Bridge Table" (`Waterfall Steps`) to force a custom chronological sorting of financial deductions, allowing for a seamless Margin Waterfall visualization.
- **Dynamic Segmentation:** Developed calculated columns to automatically categorize thousands of SKUs into custom 'Price Bands' (Low, Mid, High, Premium) and 'Discount Bands' to enable deeper macro-analysis without manual grouping.
- **Drill-Through Architecture:** Created a seamless UX where users can identify a macro-trend (e.g., a high-performing category) and drill through directly to a granular, SKU-level matrix for root-cause analysis.
- **Dynamic Currency Conversion:** DAX measures seamlessly handle currency conversions row-by-row using an Exchange Rate table to normalize global sales.

---

##  Data Model Reference

### Fact / Measure Tables
| Table | Purpose |
|---|---|
| `01_Foundation` | Core financial measures (Revenue, Profit, Margin, Customers, Orders) |
| `02_Customer KPIs` | Customer metrics (AOV, YoY growth) |
| `03_Channel KPIs` | Channel-level KPIs (Physical Revenue, Profit, Customers) |
| `04_Store KPIs` | Store-level KPIs (Active Stores, Revenue/Store, Sales/SqMeter, Top Store) |
| `06_Time Intelligence` | Time-based measures (Previous Year Revenue, Revenue YTD, YoY Growth) |
| `07_Profit Intelligence` | Profit measures (Profit YTD, YoY Profit Growth, Margin YoY Change) |
| `10_Executive KPIs` | Executive-level KPIs (Revenue Growth %, Customer Growth %) |

### Dimension Tables
| Table | Key Columns |
|---|---|
| `Customer` | Continent, CustomerKey |
| `Date` | Date, YearMonth, Quarter, Month, Year |
| `Product` | ProductName, CategoryName, Brand |
| `Store` | StoreNo, CountryName, State, CountryCode, Channel, SquareMeters |

---
