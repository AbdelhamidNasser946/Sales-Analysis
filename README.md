# 📊 AdventureWorks Sales Dashboard
> Power BI Dashboard built on AdventureWorks Sales data

---

## 🗂️ Overview

This Power BI dashboard provides a comprehensive view of AdventureWorks sales performance. It enables users to explore orders, revenue, taxes, freight, and product performance across territories, categories, and time periods.

---

## ✨ Features

| Feature | Description |
|---|---|
| 6 KPI Cards | Order Details, Orders, Tax, Due, Freight, SubTotal |
| Year Slicer | Filter all visuals by year (2011–2014) |
| Drill Down | Product Category > SubCategory > Product |
| Tooltip Page | Hover tooltips with financial breakdown |
| Star Schema | Optimised data model with Fact + Dimension tables |
| DAX Measures Table | All measures centralised in `_Measures` table |

---

## 📁 Project File Structure

```
AdventureWorks-Dashboard/
│
├── README.md
├── AdventureWorks.pbix
│
├── Data/
│   └── Sales.xlsx
│
├── Screenshots/
│   └── dashboard.png
│
└── DAX/
    └── measures.md
```

---

## 🗃️ Data Source

**File:** `Sales.xlsx`

| Column | Description |
|---|---|
| `OrderDetailID` | Unique identifier for each order line |
| `OrderID` | Sales order identifier |
| `OrderDate` | Date the order was placed |
| `Status / StatusID` | Order status (Shipped, Approved, Cancelled...) |
| `CustomerID` | Customer identifier |
| `TerritoryID / Territory` | Sales territory name and ID |
| `ProductID / Product` | Product name and ID |
| `ProductSubCategory` | Product sub-category |
| `ProductCategory` | Product category (Bikes, Components, etc.) |
| `OrderQty` | Quantity ordered |
| `UnitPrice` | Price per unit |
| `LineTotal` | UnitPrice x OrderQty — used as SubTotal |
| `TaxAmt` | Tax amount per order line |
| `Freight` | Shipping cost |
| `TotalDue` | Final amount due (LineTotal + Tax + Freight) |

---

## 🧩 Data Model — Star Schema

| Table | Type | Key Columns |
|---|---|---|
| `Fact_Sales` | Fact | OrderID, ProductID, TerritoryID, OrderDate |
| `Dim_Product` | Dimension | ProductID, Product, SubCategory, Category |
| `Dim_Territory` | Dimension | TerritoryID, Territory, TerritoryGroup |
| `Dim_Date` | Dimension | Date, Month, Quarter, Year |
| `Dim_Status` | Dimension | StatusID, Status |

### Product Hierarchy
```
Category  →  SubCategory  →  Product
```

---

## 📐 DAX Measures

All measures are stored in a dedicated `_Measures` table.

```dax
-- Orders
# Orders        = DISTINCTCOUNT(Sales[OrderID])
# Order Details = COUNTROWS(Sales)

-- Financials
Total SubTotal  = SUM(Sales[LineTotal])
Total Tax       = SUM(Sales[TaxAmt])
Total Freight   = SUM(Sales[Freight])
Total Due       = SUM(Sales[TotalDue])

-- Calculated Column
Year = YEAR(Sales[OrderDate])
```

---

## ✅ Expected KPI Card Values

Use these to validate your Power BI cards are correct:

| Card | Value |
|---|---|
| # Order Details | 23,603 |
| # Orders | 1,465 |
| Total SubTotal | $30,086,493.55 |
| Total Tax | $2,931,096.85 |
| Total Freight | $915,967.77 |
| Total Due | $33,933,558.17 |

---

## 📊 Visuals

| Visual | Type | Fields Used |
|---|---|---|
| # Orders Card | Card | `# Orders` |
| # Order Details Card | Card | `# Order Details` |
| Total SubTotal Card | Card | `Total SubTotal` |
| Total Tax Card | Card | `Total Tax` |
| Total Freight Card | Card | `Total Freight` |
| Total Due Card | Card | `Total Due` |
| Orders by OrderDate | Line Chart | OrderDate (X), # Orders (Y) |
| Orders by Status | Bar Chart | Status (Axis), # Orders (Values) |
| Qty by Category / SubCategory / Product | Bar Chart + Drill Down | Product Hierarchy, # Order Details |
| Orders vs Total Due by Territory | Clustered Bar | Territory, # Orders, Total Due |
| Year Slicer | Tile Slicer | Year column |

---

## 🛠️ Tools Used

- Microsoft Power BI Desktop
- Microsoft Excel (`Sales.xlsx`)
- DAX (Data Analysis Expressions)
- Power Query (M Language)

---

## 📝 Notes

- `LineTotal` is used as **SubTotal** since no SubTotal column exists (`UnitPrice x OrderQty`)
- Format money measures as **Currency ($)** via the Measure Tools ribbon
- Year slicer is embedded inside the header banner as a **Tile slicer**
- Tooltip page is set to **Report Page** tooltip type in visual format settings
- Drill down is enabled on the Product Category chart using the **Product Hierarchy**

---

*AdventureWorks Sales Dashboard — Power BI Project*
