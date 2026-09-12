# Contoso Global Retail — Power BI Executive Dashboard

> A complete Power BI project analyzing **2.35M+ sales transactions, 105K customers, and 74 stores** across a **$2.31B USD** global retail business. Built using Microsoft Fabric PBIP and TMDL for version control.

---

## Executive Summary & Key Numbers

This project turns 2.35 million raw retail transactions into an interactive 5-page dashboard for Contoso, a global electronics retailer. It converts multi-currency sales into USD and tracks revenue, profits, customer shopping habits, store efficiency, and delivery performance.

| Metric | Value | What It Means |
| :--- | :---: | :--- |
| **Gross List Revenue** | **$2.46B** | Total catalog price before discounts |
| **Discounts Given** | **$145.3M** | Average discount rate of **5.92%** |
| **Net Revenue** | **$2.31B** | Actual revenue collected (in USD) |
| **Product Cost (COGS)** | **$1.02B** | Total cost to make/buy products |
| **Net Operating Profit** | **$1.29B** | Total profit earned across all channels |
| **Profit Margin** | **55.89%** | Strong overall profitability |
| **Total Orders** | **980,666** | Total customer orders placed |
| **Total Buying Customers** | **86,908** | Customers who made at least 1 purchase |
| **Average Order Value (AOV)** | **$2,355.39** | Average amount spent per order |
| **Customer Lifetime Value (LTV)** | **$26,578.14** | Average total spend per customer |

---

## Report Overview (5 Pages)

The dashboard uses a modern widescreen layout (1450 × 800 px) designed for clear, easy reading:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        CONTOSO EXECUTIVE DASHBOARD (5 PAGES)                           │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Page 1: Executive Overview & Growth Trends                                             │
│   • 6 KPI cards showing revenue, profit, orders, and year-over-year changes.           │
│   • 10-year monthly revenue trend comparing current year vs. prior year.               │
│   • Year-to-date (YTD) cumulative revenue and profit chart.                            │
│   • Top product categories, top brands, and online vs. store revenue split.            │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Page 2: Channel Performance & Customer Insights                                        │
│   • 6 KPI cards tracking online sales ($891M), store sales ($1.42B), and repeat rate.  │
│   • The shift to online: how online grew from 13% (2016) to 60.8% of sales (2024).     │
│   • Customer count and average spend broken down by age group.                         │
│   • Country-by-country breakdown of online vs. physical store sales.                   │
│   • Customer comparison: Omnichannel buyers ($29.8K spend) vs. single-channel buyers.  │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Page 3: Store Fleet & Real Estate Efficiency                                           │
│   • 6 KPI cards tracking active stores (65), sales per square meter ($13K), and profit.│
│   • Scatter plot comparing store size (m²) vs. revenue to find over/underperformers.   │
│   • Sales per square meter by store size (Boutiques $22K/m² vs Flagships $12K/m²).     │
│   • Audit table highlighting top US stores ($19.4K/m²) vs struggling stores in Italy.  │
│   • Store revenue ranked by country (US leads with $724M).                              │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Page 4: Product Portfolio & 80/20 Profit Analysis                                      │
│   • 6 KPI cards: total SKUs (2,517), top profit SKUs (~639), units sold, and ASP.     │
│   • 80/20 Pareto chart: finding the top categories that drive 80% of revenue.          │
│   • Profit margins across budget, mid-range, and premium price bands.                  │
│   • Top 10 most profitable brands.                                                     │
│   • Drill-down table: Category -> Subcategory revenue, cost, and profit margins.       │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Page 5: Pricing, Delivery Speed & Currency Exposure                                    │
│   • 5 KPI cards: gross revenue, discounts, lead times, and same-day delivery rate.     │
│   • Profit waterfall: Gross Revenue -> Discounts -> Cost -> Net Profit.                │
│   • Margin check: verifies all 8 categories keep healthy margins above 50%.            │
│   • Monthly delivery volume vs. average delivery days (shows stable ~1.3-day delivery).│
│   • Currency risk: sales split by USD (51%), EUR (23.4%), GBP (13.4%), CAD, and AUD.   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Model (Star Schema)

The model uses a standard **Star Schema**. The central `Sales` table connects to four dimension tables using one-to-many (`1:*`) relationships:

| Table | Type | Rows | Description |
| :--- | :---: | :---: | :--- |
| **Sales** | **Fact** | **2,349,091** | Central transaction table with dates, quantities, prices, and costs |
| **Customer** | Dimension | 104,990 | Customer names, countries, age groups, and shopping habits |
| **Product** | Dimension | 2,517 | Product names, brands, categories, and price tiers |
| **Store** | Dimension | 74 | Store locations, size in square meters, and store status |
| **Date** | Dimension | ~3,600 | Calendar table supporting year-over-year and month-over-month trends |
| **Waterfall Steps** | Supporting | 4 | Helper table used to sort and display the profit waterfall chart |

---

## Data Cleaning & ETL (Power Query)

The raw CSV data was cleaned and prepped in Power Query before modeling:

* **Customers:** Removed duplicate customer IDs, capitalized names, and replaced empty company names with `"Unknown"`.
* **Stores:** Filled blank store statuses with `"Open"`, grouped stores into size categories (`<500 m²`, `500–1000 m²`, `>1000 m²`), and separated the online store from physical stores.
* **Sales:** Calculated delivery time in days (`Delivery Date - Order Date`), and set correct currency and decimal formats to prevent rounding issues.

---

## Key DAX Calculations

The model includes **59 DAX measures** organized into clean folders. Here are three key examples:

### 1. Converting Sales into USD
Converts sales from 5 different currencies into USD using the daily exchange rate:
```dax
Total Revenue = 
SUMX (
    Sales,
    Sales[Quantity] * DIVIDE ( Sales[NetPrice], Sales[ExchangeRate] )
)
```

### 2. Omnichannel Customer Value (LTV)
Calculates the average spend of customers who buy from **both** online and physical stores:
```dax
Omnichannel LTV = 
VAR OmnichannelCustomers = 
    FILTER (
        VALUES ( Sales[CustomerKey] ),
        CALCULATE ( COUNTROWS ( Sales ), Sales[StoreKey] = 999999 ) > 0
            && CALCULATE ( COUNTROWS ( Sales ), Sales[StoreKey] <> 999999 ) > 0
    )
RETURN
    DIVIDE (
        CALCULATE ( [Total Revenue], OmnichannelCustomers ),
        COUNTROWS ( OmnichannelCustomers ),
        0
    )
```

### 3. Dynamic 80/20 Hero Products Count
Calculates how many top products make up 80% of profit, updating automatically when slicers (like Category or Year) are selected:
```dax
Core 80% Profit SKUs = 
VAR TotalProfitAll = [Total Profit]
VAR TargetProfit = TotalProfitAll * 0.80
VAR ProductSummary = 
    ADDCOLUMNS (
        SUMMARIZE ( Sales, Product[ProductKey] ),
        "@Profit", [Total Profit]
    )
VAR RunningProfitTable = 
    ADDCOLUMNS (
        ProductSummary,
        "@CumProfit",
        VAR CurrentP = [@Profit]
        VAR CurrentK = Product[ProductKey]
        RETURN
        SUMX (
            FILTER (
                ProductSummary,
                [@Profit] > CurrentP || ( [@Profit] = CurrentP && Product[ProductKey] <= CurrentK )
            ),
            [@Profit]
        )
    )
RETURN
    COUNTROWS (
        FILTER (
            RunningProfitTable,
            [@CumProfit] - [@Profit] < TargetProfit
        )
    )
```

---

## Key Findings from the Data

1. **Omnichannel Customers are Most Valuable:**
   * **86.3% of buyers (75,015 people)** shop both online and in-store.
   * These shoppers spend an average of **$29,867**, which is **4.5× more** than store-only buyers ($6,550) and **8.4× more** than online-only buyers ($3,551). They also place 5× more orders.

2. **Small Stores are More Productive:**
   * Boutique stores (<500 m²) make **$22,012 per m²**, compared to large flagship stores (>1,000 m²) which make **$15,320 per m²** (+43.7% higher efficiency).
   * Several large stores in Italy and Australia underperform (<$5,500 per m²), showing opportunities to downsize and save lease costs.

3. **The 80/20 Rule in Products:**
   * Just **25.5% of products (639 SKUs)** generate **80% of total company profit ($1.03B)**.
   * Computers and Cell Phones drive nearly 60% of all sales.
   * Every single product in the catalog has had at least one sale (no dead inventory).

4. **The Market Has Shifted Online:**
   * In 2016, 86.8% of revenue came from physical stores.
   * In 2023, online sales passed physical stores for the first time (54.3%).
   * By 2024, online sales reached **60.8% of total revenue ($195M online vs. $126M in-store)**.

5. **Strong Price Discipline:**
   * Contoso rarely gives deep discounts: the highest discount ever given was **14.0%**, and the company average is **5.92%**.
   * Because of this, profit margins stay strong at over **52%** across all product categories.

6. **Fast Delivery Keeps Customers Coming Back:**
   * **58.57% of orders** are delivered the same day, with an overall average delivery time of **1.35 days**.
   * Customers who receive fast deliveries (within 2 days) have a **93.9% repeat purchase rate**.

---

## Recommendations for Management

* **1. Focus on Digital First:** Since over 60% of revenue is now online, shift store expansion budgets toward website scalability, app improvements, and warehouse automation. Turn physical stores into pickup hubs and showrooms rather than large retail spaces.
* **2. Turn Store-Only Shoppers into Omnichannel Shoppers:** Encourage in-store customers to download the app or use online ordering. Converting just 30% of store-only shoppers could add **$63M+ in customer spend**.
* **3. Open Smaller Boutique Stores:** Future stores should be smaller (<500 m²), which generate 43% more revenue per square meter. Downsize or renegotiate leases on oversized stores in Italy and Australia.
* **4. Protect Stock for the Top 25% Hero Products:** Ensure the 639 products that drive 80% of profit never go out of stock, while using on-demand ordering for slower-moving items.
* **5. Make Online Shopping Easier for Older Customers:** Customers aged 60+ generate 37% of sales ($856M). Keeping web navigation simple and providing clear support protects Contoso's most loyal demographic.

---

## Author
Shawon Mandal

*Built with Microsoft Fabric, Power BI, DAX, and Python.*
