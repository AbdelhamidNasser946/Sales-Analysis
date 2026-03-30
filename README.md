# 📊 Sales Analysis Dashboard
> Power BI Dashboard for comprehensive sales performance analysis

---

## 🗂️ Overview

This Power BI dashboard provides a comprehensive view of sales performance metrics. It enables users to explore orders, revenue, taxes, freight, and product performance across territories, categories, and time periods. The dashboard is built using a star schema data model for optimal performance and includes interactive visualizations with drill-down capabilities.

---

## ✨ Features

| Feature | Description |
|---|---|
| 6 KPI Cards | Display key metrics: Order Details, Orders, Tax, Due, Freight, and SubTotal at a glance |
| Year Slicer | Interactive filter to analyze data across multiple years, enabling temporal comparison |
| Drill Down Capability | Explore product hierarchies from Category → SubCategory → Product level for detailed insights |
| Tooltip Page | Hover over visuals to see detailed financial breakdowns and additional context |
| Star Schema | Optimized data model with a central Fact table connected to Dimension tables for fast queries |
| DAX Measures Table | All business calculations centralized in a dedicated `_Measures` table for easy maintenance and consistency |

---

## 📁 Project File Structure

```
Sales-Analysis/
│
├── README.md                    # Project documentation and guide
├── SalesDashboard.pbix          # Main Power BI dashboard file
│
├── Data/
│   └── Sales.xlsx               # Source data containing all sales transactions
│
└── Screenshots/
    └── dashboard_overview.png   # Visual preview of the dashboard
```

**File Descriptions:**
- **SalesDashboard.pbix** - The main Power BI workbook containing all data models, measures, and visualizations
- **Sales.xlsx** - Raw source data with all order and product information
- **Screenshots** - Preview images of the dashboard for quick reference

---

## 🗃️ Data Source

**File:** `Data/Sales.xlsx`

This Excel file contains the raw sales transaction data with the following columns:

| Column | Description | Example |
|---|---|---|
| `OrderDetailID` | Unique identifier for each order line item | 1, 2, 3... |
| `OrderID` | Sales order identifier (multiple lines per order) | 43659, 43660... |
| `OrderDate` | Date when the order was placed | 2011-05-31 |
| `Status / StatusID` | Current status of the order (Shipped, Approved, Cancelled, etc.) | Shipped |
| `CustomerID` | Unique customer identifier | 29825, 29672... |
| `TerritoryID / Territory` | Geographic sales territory name and its ID | Northeast, Southwest |
| `ProductID / Product` | Product name and its unique identifier | Mountain Bike, Road Bike |
| `ProductSubCategory` | Secondary product classification | Mountain Bikes, Road Bikes |
| `ProductCategory` | Primary product grouping | Bikes, Components, Clothing |
| `OrderQty` | Number of units ordered | 1, 5, 10... |
| `UnitPrice` | Price per unit at the time of sale | $2,024.99 |
| `LineTotal` | Extended price (UnitPrice × OrderQty) - used as SubTotal | $2,024.99 |
| `TaxAmt` | Tax amount charged on this order line | $202.50 |
| `Freight` | Shipping cost for this order line | $61.66 |
| `TotalDue` | Final amount due (LineTotal + Tax + Freight) | $2,288.15 |

---

## 🧩 Data Model — Star Schema

The dashboard uses a star schema design, which separates transactional data (Fact table) from lookup data (Dimension tables). This approach improves query performance and data integrity.

| Table | Type | Purpose | Key Columns |
|---|---|---|---|
| `Fact_Sales` | Fact | Central table containing all transactions | OrderID, ProductID, TerritoryID, OrderDate |
| `Dim_Product` | Dimension | Product master data | ProductID, Product, SubCategory, Category |
| `Dim_Territory` | Dimension | Geographic sales regions | TerritoryID, Territory, TerritoryGroup |
| `Dim_Date` | Dimension | Time-based lookups for temporal analysis | Date, Month, Quarter, Year |
| `Dim_Status` | Dimension | Order status reference table | StatusID, Status |

### Product Hierarchy
The product data is organized in a three-level hierarchy for drill-down analysis:
```
Category (e.g., Bikes)
  └─> SubCategory (e.g., Mountain Bikes)
      └─> Product (e.g., Mountain-100 Black)
```

This structure allows users to start with high-level category insights and drill down to specific products.

---

## 📐 DAX Measures

All business calculations are stored in a dedicated `_Measures` table to ensure consistency and maintainability across the dashboard.

```dax
-- Order Metrics
# Orders        = DISTINCTCOUNT(Sales[OrderID])
                  -- Counts unique orders (each order counted once)
                  
# Order Details = COUNTROWS(Sales)
                  -- Counts individual line items (each SKU counted separately)

-- Financial Metrics
Total SubTotal  = SUM(Sales[LineTotal])
                  -- Sum of all line item totals before tax and freight
                  
Total Tax       = SUM(Sales[TaxAmt])
                  -- Total tax collected across all transactions
                  
Total Freight   = SUM(Sales[Freight])
                  -- Total shipping costs
                  
Total Due       = SUM(Sales[TotalDue])
                  -- Grand total revenue (SubTotal + Tax + Freight)

-- Calculated Column
Year = YEAR(Sales[OrderDate])
       -- Extracted year from order date for temporal filtering
```

**Key Concepts:**
- **# Orders** uses DISTINCTCOUNT to avoid double-counting multiple line items in the same order
- **# Order Details** counts every individual line to track volume
- All currency measures should be formatted as **Currency ($)** in Power BI for proper display

---

## ✅ Expected KPI Card Values

Use these reference values to validate your dashboard calculations:

| Card | Value | Notes |
|---|---|---|
| # Order Details | 23,603 | Total line items across all orders |
| # Orders | 1,465 | Unique orders in the dataset |
| Total SubTotal | $30,086,493.55 | Revenue before tax and freight |
| Total Tax | $2,931,096.85 | Tax collected (~9.7% of SubTotal) |
| Total Freight | $915,967.77 | Shipping costs (~3.0% of SubTotal) |
| Total Due | $33,933,558.17 | Final revenue including tax and freight |

---

## 📊 Visuals & Interactions

| Visual | Type | Fields Used | Purpose |
|---|---|---|---|
| # Orders Card | Card Visual | `# Orders` | Display total unique orders |
| # Order Details Card | Card Visual | `# Order Details` | Show total line items sold |
| Total SubTotal Card | Card Visual | `Total SubTotal` | Revenue before adjustments |
| Total Tax Card | Card Visual | `Total Tax` | Tax collected |
| Total Freight Card | Card Visual | `Total Freight` | Shipping costs |
| Total Due Card | Card Visual | `Total Due` | Final revenue amount |
| Orders by OrderDate | Line Chart | OrderDate (X-axis), # Orders (Y-axis) | Trend analysis over time |
| Orders by Status | Bar Chart | Status (Axis), # Orders (Values) | Order distribution by status |
| Qty by Category/SubCategory/Product | Bar Chart + Drill | Product Hierarchy, # Order Details | Volume analysis with drill capability |
| Orders vs Total Due by Territory | Clustered Bar | Territory, # Orders, Total Due | Regional performance comparison |
| Year Slicer | Tile Slicer | Year column | Year-based filtering across all visuals |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Microsoft Power BI Desktop** | Dashboard design and data visualization |
| **Microsoft Excel** | Source data storage and management (`Sales.xlsx`) |
| **DAX** (Data Analysis Expressions) | Custom calculations and measures |
| **Power Query** (M Language) | Data transformation and data cleaning |

---

## 📝 Implementation Notes

### Data Formatting
- `LineTotal` is used as the **SubTotal** since there is no separate SubTotal column in the source data
- All currency measures should be formatted as **Currency ($)** to ensure proper display in KPI cards

### Interactive Features
- The **Year Slicer** is embedded in the dashboard header as a **Tile Slicer** for quick access
- **Tooltip Page** is configured as a **Report Page** tooltip in the visual format settings for enhanced hover details
- **Drill-Down** functionality on the Product Category chart uses the three-level **Product Hierarchy** (Category → SubCategory → Product)

### Best Practices Applied
- Star schema design for performance optimization
- Centralized measures in dedicated table for consistency
- Clear naming conventions for all columns and measures
- Hierarchical organization for intuitive data exploration

---

## 🚀 Getting Started

1. **Open the Dashboard** - Load `SalesDashboard.pbix` in Power BI Desktop
2. **Review KPI Cards** - Compare values with the Expected KPI Card Values section
3. **Interact with Slicers** - Use the Year Slicer to filter data by time period
4. **Explore Drill-Downs** - Click on Product Category bars to drill into SubCategory and Product levels
5. **Hover for Details** - Move cursor over visuals to see tooltip information

---

## 📞 Support & Questions

For issues or questions about this dashboard:
- Review the data model structure in the Data Model view
- Validate all DAX measures match the provided formulas
- Check that source data in `Sales.xlsx` is properly connected

---

*Sales Analysis Dashboard — Power BI Project*
*Last Updated: 2026-03-30*