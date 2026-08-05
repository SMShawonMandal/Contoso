# Contoso Retail Analytics — Power BI Report

This repository contains the **Contoso Retail Analytics** Power BI report — a Microsoft sample/demo retail dashboard built on the fictional Contoso retail company dataset. The report was exported from Power BI Service (Fabric) and extracted for analysis.

---

## 📊 Report Overview

| Property | Value |
|---|---|
| **Report Name** | Contoso Retail Analytics |
| **Source** | Microsoft sample dataset (Contoso) |
| **Created With** | Power BI Desktop / Fabric |
| **Release Version** | 2026.07 |
| **Format Version** | 1.32 |
| **Page Count** | 2 pages (+ 3rd page referenced in DAX docs) |
| **Dimensions** | 1450 × 800 px (both pages) |

---

## 📄 Pages

### Page 1: Store & Regional Performance
**Page ID:** `5395111fe40d797884d9`  
**Visual Count:** 54  

**KPI Cards (7)**
- Active Stores: **65**
- Revenue: **$133.95M**
- Revenue per Store: **$2.06M**
- Sales per Square Meter: **$1.23K**
- Top Store Revenue: **$4.88M**
- Physical Customers
- Profit Margin %

**Charts & Visuals**
- **Clustered Column Chart** — Top 5 Stores by Revenue
- **Line Chart** — Revenue Trend (Physical Revenue + Previous Year)
- **Scatter Chart** — Store Efficiency Matrix (Revenue vs. Sales/Sq Meter, sized by SquareMeters)
- **Treemap** — Revenue by Country
- **Slicers** — Continent, State, Country dropdowns

**Key Measure Tables Referenced**
- `03_Channel KPIs` (Physical Revenue, Physical Store Profit, Physical Customers)
- `04_Store KPIs` (Active Stores, Revenue per Store, Sales per Square Meter, Top Store Revenue)
- `06_Time Intelligence` (Previous Year Revenue)

---

### Page 2: Executive Overview
**Page ID:** `7e9e1d17501259382d3e`  
**Visual Count:** 52  

**KPI Cards with YoY Conditional Formatting (14)**
| KPI | Value | YoY Change | Format |
|---|---|---|---|
| Total Revenue | $325.04M | **-27.69%** | 🔴 Red |
| Total Profit | $181.69M | **-27.70%** | 🔴 Red |
| Profit Margin % | 55.90% | -0.01% | 🔴 Red |
| AOV | 1.99K | **-13.32%** | 🔴 Red |
| Total Orders | 164K | **-16.58%** | 🔴 Red |
| Total Customers | 62K | **-5.95%** | 🔴 Red |
| Revenue Growth % | — | — | Line on combo |
| Customer Growth % | — | — | Card |

**Charts & Visuals**
- **Combo Chart** — Revenue Trend (Total Revenue + Previous Year as columns, Revenue Growth % as line)
- **Stacked Area Chart** — Revenue & Profit YTD
- **Donut Chart** — Revenue by Channel (Physical vs. Online)
- **Bar Charts** — Top 5 Category by Revenue, Top 5 Brands by Revenue
- **Table** — Top 8 Products by Revenue
- **Slicers** — Channel, Category, Date Range (2024-01-01 to 2024-12-31)

**Key Measure Tables Referenced**
- `01_Foundation` (Total Revenue, Total Profit, Profit Margin %, Total Customers, Total Orders)
- `02_Customer KPIs` (AOV, YoY AOV Growth %, YoY Orders Growth %, Customer Growth %)
- `06_Time Intelligence` (Revenue YTD, YoY Revenue Growth %)
- `07_Profit Intelligence` (Profit YTD, YoY Profit Growth %, Profit Margin YoY Change)
- `10_Executive KPIs` (Revenue Growth %, Customer Growth %)

---

## 🗄️ Data Model (Star Schema)

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

## 📁 Repository Contents

```
├── contotso.pbix                    # Original Power BI report (81 MB)
├── contotso.pdf                     # Exported PDF report (969 KB)
├── DAX_Product_Pricing_Performance.md  # DAX reference for 3rd page (Product & Pricing)
├── extracted_pbix/                  # PBIX extracted to folder structure
│   ├── DataModel                    # Binary data model (81 MB)
│   ├── DiagramLayout                # Visual diagram layout (JSON)
│   ├── Metadata                     # Report metadata (JSON)
│   ├── Report/
│   │   └── definition/
│   │       └── pages/               # Page definitions + visual containers
│   └── [Content_Types].xml          # Package manifest
└── ss/                              # Screenshots (7 PNGs)
    ├── 1.png  (Executive Overview)
    ├── 2.png  (Store & Regional)
    ├── 3.png
    ├── 4.png
    ├── 5.png
    ├── 6.png
    └── 7.png
```

---

## 📝 DAX Reference: Product & Pricing Performance (Page 3)

The file [`DAX_Product_Pricing_Performance.md`](DAX_Product_Pricing_Performance.md) contains copy-paste ready DAX formulas for a **third page** covering:

### Calculated Columns
- **Price Band** (Low/Mid/High/Premium) with sort column
- **Discount Band** (No Discount / Low / Medium / High) with sort column

### Calculated Tables
- **Waterfall Steps** — Disconnected bridge table for waterfall charts
- **Brand Slicer** — Top 20 brands + "Other" bucket

### Core Measures
- Gross Revenue, Discount Amount, Average Discount %

### Pareto / 80-20 Measures
- Revenue Contribution %, Revenue Cumulative %
- Profit Contribution %, Profit Cumulative %

### Waterfall Measures
- Waterfall Value (driven by Waterfall Steps table)

### Scatter / Median Measures
- Median Discount %, Median Margin %
- Quadrant Label (Stars / Volume Drivers / Commoditized / Margin Killers)

### Ranking & Contribution
- Product Revenue Rank, Product Profit Rank
- Revenue/Profit Contribution %

### Conditional Formatting
- Discount Font Color (red if > 20%)
- Margin Background Color (diverging: red → yellow → green)
- Discount Amount Font Color (always red)

### Drill-Through Configuration
- Page 3 setup with `Product[CategoryName]` and `Product[ProductName]`
- Source page linking from Executive Overview & Store pages

---

## 🛠️ Technical Details

- **Schema Versions:** Page 2.1.0, Visual Container 2.11.0
- **Compression:** XPress9 (standard PBIX compression)
- **Authoring Tool:** Power BI Desktop 2.156.603.0
- **Extraction Tool:** `pbi-tools` / standard zip extraction

### Extracted Structure
The `extracted_pbix/` folder follows the standard PBIX package structure:
- `DataModel` — VertiPaq binary (not human-readable)
- `DiagramLayout` — JSON with node positions for model diagram
- `Metadata` — Version, auto-created relationships, source info
- `Report/definition/pages/` — Each page has `page.json` + `visuals/{id}/visual.json`

---

## 🚀 Usage

### View the Report
1. Open `contotso.pbix` in **Power BI Desktop** (Feb 2024 or later)
2. Or publish to **Power BI Service / Fabric**

### Explore Extracted Files
```bash
# View page definitions
cat extracted_pbix/Report/definition/pages/5395111fe40d797884d9/page.json

# View a visual container
cat extracted_pbix/Report/definition/pages/5395111fe40d797884d9/visuals/016ff7d3a1b8a57a907b/visual.json

# View diagram layout
cat extracted_pbix/DiagramLayout
```

### Apply DAX Measures
Copy measures from `DAX_Product_Pricing_Performance.md` into the appropriate measure tables in Power BI Desktop:
1. **Modeling** → **New measure** (or **New column** / **New table**)
2. Paste DAX formula
3. Assign to suggested measure table (see checklist in the doc)

---

## 📸 Screenshots

| Page | Screenshot |
|---|---|
| Executive Overview | `ss/1.png` |
| Store & Regional Performance | `ss/2.png` |
| Additional views | `ss/3.png` – `ss/7.png` |

---

## 📚 References

- [Contoso Sample Dataset (Microsoft)](https://github.com/microsoft/powerbi-desktop-samples)
- [pbi-tools](https://github.com/pbi-tools/pbi-tools) — for PBIX extraction/manipulation
- [DAX Reference Guide](https://learn.microsoft.com/dax/)
- [Power BI Visual Container Schema](https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.11.0/schema.json)

---

## 📄 License

This report uses Microsoft's **Contoso sample dataset** which is provided for demonstration and educational purposes. Check Microsoft's sample data license terms for redistribution.

---

*Generated from PBIX extraction and analysis — August 2026*