# Contoso Retail Analytics

A Power BI executive dashboard built on Microsoft's Contoso retail sample dataset. This report delivers a two-page analytics experience for retail leadership — combining high-level KPIs with operational store intelligence.

---

## Pages

### Executive Overview
The landing page for leadership. Six headline KPIs show total revenue ($325M), profit ($182M), margin (55.9%), average order value, order volume, and customer count — each with year-over-year conditional formatting (green for growth, red for decline). A combo chart tracks revenue trend against prior year with a growth-rate line. A stacked area chart shows revenue and profit accumulating through the year. A donut breaks down revenue by channel (Physical vs. Online). Bar charts surface the top 5 categories and top 5 brands. A table lists the top 8 products by revenue. Slicers for channel, category, and a 2024 date range let viewers filter the entire page.

### Store & Regional Performance
The operational deep-dive. KPI cards show active stores (65), total revenue ($134M), revenue per store ($2.06M), sales per square meter ($1.23K), top store revenue ($4.88M), and physical customer count. A clustered column chart ranks the top 5 stores by revenue. A line chart tracks revenue trend with prior-year comparison. A scatter plot maps store efficiency — revenue versus sales per square meter, sized by store footprint. A treemap visualizes revenue by country. Slicers for continent, state, and country enable geographic drilling.

---

## Data Model

The model follows a star schema centered on a core fact table (`01_Foundation`) surrounded by dimension tables (`Customer`, `Date`, `Product`, `Store`) and specialized aggregate tables for each analytic domain:

- **01_Foundation** — Revenue, profit, margin, customers, orders
- **02_Customer KPIs** — AOV, YoY growth rates
- **03_Channel KPIs** — Physical channel revenue, profit, customers
- **04_Store KPIs** — Store count, revenue per store, sales density, top performer
- **06_Time Intelligence** — Prior-year comparisons, YTD, YoY growth
- **07_Profit Intelligence** — Profit YTD, profit growth, margin change
- **10_Executive KPIs** — Executive growth metrics

---

## Advanced Analytics (Reference Implementation)

A third page — **Product & Pricing Performance** — is documented with production-ready DAX covering:

- **Price & Discount Bands** — Calculated columns with sort orders for consistent binning
- **Pareto / 80-20** — Running contribution percentages for revenue and profit
- **Waterfall** — Gross revenue → discounts → cost → profit via a disconnected bridge table
- **Scatter Quadrants** — Median discount vs. median margin with four-quadrant labeling (Stars, Volume Drivers, Commoditized, Margin Killers)
- **Ranking** — Dense revenue and profit ranks across products
- **Conditional Formatting** — Field-value measures for font color (discounts >20% red), diverging margin backgrounds (red → yellow → green), discount amounts always red
- **Drill-Through** — Configured from category bars (Page 2) and product visuals (Page 1) landing on a filtered product detail view

---



## License

Built on Microsoft's Contoso sample dataset for demonstration and educational use. Refer to Microsoft's sample data terms for redistribution.
