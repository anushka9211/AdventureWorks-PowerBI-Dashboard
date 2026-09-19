# 🚴 AdventureWorks Business Intelligence Executive Dashboard

An end-to-end **Power BI Executive Dashboard** engineered for **AdventureWorks**, a global manufacturer of bicycles, components, accessories, and apparel. This project transforms raw, multi-year transactional CSV datasets into a unified Business Intelligence platform to analyze **$24.9M in Total Revenue**, **$10.5M in Total Profit**, **25.2K Orders**, and **2.2% Return Rates** across global markets.

---

## 🎯 Executive Business Summary

AdventureWorks leadership faced a core operational challenge: transactional sales data was fragmented across multiple annual spreadsheets, returns were tracked in isolation, and customer demographics were disconnected from sales performance.

This dashboard answers the primary executive question:
> **"What are the key drivers of AdventureWorks' revenue, net profit, and product returns across global sales territories and customer demographic cohorts?"**

### Key Strategic Findings
* **Volume vs. Profitability Divergence**: **Accessories** drive overall customer acquisition volume (**17.0K orders**, ~43% of total orders), whereas **Bikes** generate over **80% of total company revenue ($20M+)** despite lower transaction counts (**13.9K orders**).
* **Quality & Return Rate Risk**: While the company-wide return rate is healthy at **2.2%**, apparel items—specifically **Shorts** and **Helmets**—exhibit elevated return rates (>3.3%), signaling potential vendor sizing or fabric defect issues.
* **Core Customer Profile**: High-value buyers are concentrated in **Professional** and **Skilled Manual** occupations with **Bachelor's degrees** and annual incomes between **$60,000 – $100,000**.
* **Geographic Revenue Leadership**: The **United States** and **Australia** represent the top regional revenue drivers, while European territories (UK, Germany, France) offer significant expansion potential.

---

## 🏗️ Data Architecture & Star Schema Model

The data model follows a **Star Schema** architecture linking **2 Fact Tables** to **6 Dimension Lookup Tables** using **1-to-Many (`1:*`) single-direction filter propagation**.

```
                         [ Calendar Lookup ]      [ Customer Lookup ]
                                  \                     /
                                   \                   /
                                    v                 v
                              +-------------------------------+
                              |    Sales Data  (Fact Table)   |
                              +-------------------------------+
                                    ^                 ^
                                   /                   \
                                  /                     \
                          [ Territory Lookup ]   [ Product Lookup ]
                                                        |
                                            [ Subcategories Lookup ]
                                                        |
                                              [ Categories Lookup ]
```

### Table Breakdown
* **Fact Tables**:
  * `Sales Data`: ~55,800 transaction lines (2020–2022) detailing `OrderDate`, `OrderNumber`, `ProductKey`, `CustomerKey`, `TerritoryKey`, `OrderQuantity`.
  * `Returns Data`: ~1,810 records detailing returned quantities by date, territory, and product.
* **Dimension Tables**:
  * `Calendar Lookup`: Continuous date dimension (2020-01-01 to 2022-06-30).
  * `Customer Lookup`: 18,148 customer profiles with demographic attributes.
  * `Product Lookup`: 293 product SKUs with cost and retail price points.
  * `Product Subcategories` & `Product Categories`: Product hierarchy mappings.
  * `Territory Lookup`: 10 sales regions across North America, Europe, and Pacific.

---

## 🧮 Core DAX Measures

All metrics are organized inside a dedicated `Measure Table` using optimized DAX formulas:

### Financial & Volume Metrics
```dax
Total Revenue = SUMX('Sales Data', 'Sales Data'[OrderQuantity] * RELATED('Product Lookup'[ProductPrice]))
Total Cost    = SUMX('Sales Data', 'Sales Data'[OrderQuantity] * RELATED('Product Lookup'[ProductCost]))
Total Profit  = [Total Revenue] - [Total Cost]
Total Orders  = DISTINCTCOUNT('Sales Data'[OrderNumber])
Total Quantity= SUM('Sales Data'[OrderQuantity])
Total Returns = SUM('Returns Data'[ReturnQuantity])
Return Rate   = DIVIDE([Total Returns], [Total Quantity], 0)
```

### Time Intelligence & Target Tracking
```dax
Previous Month Revenue = CALCULATE([Total Revenue], DATEADD('Calendar Lookup'[Date], -1, MONTH))
Previous Month Orders  = CALCULATE([Total Orders], DATEADD('Calendar Lookup'[Date], -1, MONTH))
Revenue Target         = [Previous Month Revenue] * 1.05
Revenue Target Gap     = [Total Revenue] - [Revenue Target]
```

### Customer Value Metrics
```dax
Total Customers             = DISTINCTCOUNT('Sales Data'[CustomerKey])
Average Revenue per Customer = DIVIDE([Total Revenue], [Total Customers], 0)
```

---

## 📊 Dashboard Pages Overview

The report consists of **4 interactive pages** designed for executive and operational workflows:

1. **Exec Dashboard**: High-level C-suite overview featuring core KPI cards ($24.9M Rev, $10.5M Profit, 25.2K Orders, 2.2% Return Rate), weekly revenue trending line chart, orders by category bar chart, Top 10 Products matrix with conditional formatting, and monthly growth indicators.
2. **Map**: Interactive geographic visualization using Bing Maps to track order volume bubbles across North America, Europe, and Pacific with dynamic continent slicers.
3. **Product Detail**: Granular single-product analyzer featuring gauge visuals comparing actual monthly orders, revenue, and profit against +5% target goals, paired with What-If price adjustment scenario sliders.
4. **Customer Detail**: Demographic breakdown of customer sales by Education Level, Occupation, Top 100 customer lifetime spend table, and dynamic Top Customer spotlight card.

---

## 🛠️ Project Repository Structure

```
├── AdventureWorks PBIX Files/
│   └── AdventureWorks Report_FINAL.pbix    # Master Power BI Desktop File
├── PowerBI_AdventureWorks (1).pdf           # Executive PDF Dashboard Export
├── README.md                                # Project Documentation
└── .gitignore                               # Git Exclusions
```

---

## 🚀 How to Open and Use the Report

1. Clone or download this repository.
2. Download and install [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
3. Open `AdventureWorks PBIX Files/AdventureWorks PBIX Files/AdventureWorks Report_FINAL.pbix` in Power BI Desktop.

---

## 👤 Author & Acknowledgments
* **Author**: anushka9211 (23uec520@lnmiit.ac.in)
* **Project Type**: Portfolio Business Intelligence & Data Analytics Case Study
