# Daewoo-Ticketing-Sales-dashboard
Tracked revenue by date, terminal, and booking channel, surfacing seasonal demand patterns for revenue reporting
# TMS Online Sales — Power BI Project Documentation

> **Project:** TMS – Online  
> **Platform:** Microsoft Power BI  
> **Business Domain:** Intercity Bus Transportation / Online Ticket Sales  
> **Organization:** Daewoo Express Bus Service  
> **Documentation generated from:** `TMS - Online.pbix`  
> **PBIX model version metadata:** Power BI model/report artifacts indicate a 2025-era release; the PBIX metadata reports `CreatedFrom: Cloud` and `CreatedFromRelease: 2025.11`.

---

## 1. Executive Summary

**TMS – Online** is a Power BI reporting solution designed to monitor and analyze **online ticket sales and online ticket quantities** across time, sales mode, and terminal.

The report is intentionally focused on a small number of high-value business questions:

1. How much online sales revenue was generated?
2. How many online tickets were sold?
3. How is online sales distributed across sales modes?
4. Which terminals generate the most online sales?
5. Which terminals generate the highest online ticket volume?
6. How are online sales and ticket quantities changing over time?
7. How does the current period compare with the previous year?

The PBIX contains **two report pages**:

- **Online Sale Summary** — management-level overview.
- **Date Wise Online Sale** — time-series/detail analysis.

The report uses a dedicated **Date Table** and a business-oriented **Online Measures** table, with supporting dimensions/reference tables for modes, terminals, codes and seating capacity.

---

# 2. Business Problem

Traditional operational reporting can make it difficult for management to quickly identify:

- overall online sales performance,
- contribution of different sales modes,
- terminal-level online sales performance,
- online ticket volumes,
- daily/monthly sales patterns,
- year-over-year performance.

This Power BI solution addresses the problem by consolidating these indicators into an interactive dashboard.

### Business Objective

The primary objective is to provide a **single analytical view of online sales performance** that can be filtered by year/date and evaluated at summary, terminal, mode and daily/monthly levels.

---

# 3. Project Scope

### Included

- Online sales revenue monitoring
- Online ticket quantity monitoring
- Sales-mode analysis
- Terminal-level sales analysis
- Terminal-level ticket-volume analysis
- Monthly trend analysis
- Daily trend analysis
- Year filtering
- Date filtering
- Previous-year comparison measures
- Management summary KPIs

### Report Pages

| Page | Purpose |
|---|---|
| Online Sale Summary | High-level management dashboard |
| Date Wise Online Sale | Daily/monthly trend and detail analysis |

---

# 4. PBIX Architecture

The PBIX contains the following model entities identified from the report/model artifacts:

### Core analytical entities

- `Online Measures`
- `Date Table`
- `Mode / Terminals`

### Supporting/reference entities

- `Terminal Codes`
- `Seating Capacity`
- `Codes`
- `MOde`
- Power BI-generated `LocalDateTable_*` structures

The report also contains a relationship/model diagram layout. The available PBIX metadata confirms the existence of these entities, although the compressed internal `DataModel` cannot be fully decoded in this environment.

---

# 5. Data Model Overview

A simplified conceptual representation is:

```text
                    ┌─────────────────────┐
                    │      Date Table     │
                    │ Date                │
                    │ Year                │
                    │ Month               │
                    │ Day                 │
                    └──────────┬──────────┘
                               │
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Online Measures   │
                    │ Online Sales KPIs   │
                    │ Ticket KPIs         │
                    │ LY KPIs             │
                    └──────────┬──────────┘
                               │
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Mode / Terminals  │
                    │ Mode                │
                    │ Terminal            │
                    │ Sale                │
                    └─────────────────────┘

Supporting/reference:
    ┌────────────────┐
    │ Terminal Codes │
    └────────────────┘

    ┌────────────────┐
    │Seating Capacity│
    └────────────────┘

    ┌───────────────┐
    │     Codes     │
    └───────────────┘

    ┌───────────────┐
    │     MOde      │
    └───────────────┘
```

> **Important:** The PBIX's `DataModel` is stored using Microsoft's XPress9-compressed internal format. The report layout and diagram metadata expose the analytical entities and visual queries, but the raw storage engine metadata/DAX definitions were not directly recoverable here. Therefore, this documentation distinguishes between **confirmed PBIX evidence** and **business interpretation**.

---

# 6. Confirmed Tables / Entities

## 6.1 Date Table

The report explicitly references a table named:

`Date Table`

Fields observed in visual queries include:

- `Date`
- `Year`
- `Month`
- `Day`
- Power BI Date Hierarchy

### Role

The Date Table provides the temporal analytical axis for:

- yearly filtering,
- monthly analysis,
- daily analysis,
- trend charts,
- year-over-year measures.

The report also references a Power BI-generated local date table:

`LocalDateTable_a88e690b-c6c2-4001-8fde-71c5090037f7`

This local date structure is used by some slicers and visual queries.

---

## 6.2 Online Measures

`Online Measures` is the main KPI/measure table used by the report.

Confirmed measures referenced by visuals include:

### Sales Measures

- `Total Online Sale`
- `Medium Sale Online`
- `Medium Sale BM`
- `Medium Sale ST`
- `Medium Sale Other`

### Ticket Measure

- `TKT QTY`

### Previous-Year Measures

- `LY_Online Sale`
- `LY_Online`
- `LY_BM`
- `LY_ST`
- `LY_Others`

This table is effectively the **semantic KPI layer** of the report.

---

## 6.3 Mode / Terminals

The report references:

`Mode / Terminals`

Confirmed fields used by visuals:

- `Mode`
- `Terminal`
- `Sale`

### Role

This entity supplies categorical dimensions for:

- online sales by mode,
- online sales by terminal,
- ticket quantities by terminal.

---

## 6.4 Terminal Codes

`Terminal Codes` appears in the model diagram as a supporting/reference table.

Likely purpose:

- terminal master/reference data,
- terminal code normalization,
- terminal mapping.

The report layout itself does not expose enough information to safely document all fields of this table.

---

## 6.5 Seating Capacity

`Seating Capacity` appears in the model diagram.

Likely business purpose:

- bus/terminal seating reference,
- capacity-related analysis,
- supporting operational reference data.

No seating-capacity measure is directly used by the two report pages identified in the PBIX layout.

---

## 6.6 Codes

`Codes` appears as a supporting model entity.

Likely purpose:

- business-code mapping,
- reference/master-data classification.

Its complete column structure could not be safely recovered from the compressed DataModel.

---

## 6.7 MOde

`MOde` appears as a separate model entity.

The report visuals use the business dimension:

- `Mode`

from `Mode / Terminals`.

The exact relationship and column structure of `MOde` should be verified in Power BI Model view before changing the production model.

---

# 7. KPI Dictionary

## 7.1 Total Online Sale

**Measure:** `Total Online Sale`

### Business meaning

Total sales revenue generated through the online sales channel within the current filter context.

### Used in

- Summary table
- Terminal sales chart
- KPI card
- Date-wise sales trend
- Date-wise table

### Analytical grain

Depends on the filter context:

- year,
- month,
- day,
- terminal,
- mode,
- date.

---

## 7.2 Medium Sale Online

**Measure:** `Medium Sale Online`

Represents online sales attributed to the **Online** medium/category.

Used in:

- summary table,
- KPI card,
- date-wise table.

---

## 7.3 Medium Sale BM

**Measure:** `Medium Sale BM`

Represents sales attributed to the `BM` sales medium/category.

Used in:

- summary table,
- KPI card,
- date-wise table.

---

## 7.4 Medium Sale ST

**Measure:** `Medium Sale ST`

Represents sales attributed to the `ST` sales medium/category.

Used in:

- summary table,
- KPI card,
- date-wise table.

---

## 7.5 Medium Sale Other

**Measure:** `Medium Sale Other`

Represents sales that fall into the remaining/other sales-medium category.

Used in:

- summary table,
- KPI card,
- date-wise table.

---

## 7.6 TKT QTY

**Measure:** `TKT QTY`

Represents online ticket quantity.

Used in:

- terminal ticket-volume chart,
- date-wise ticket trend.

This KPI provides a volume perspective alongside revenue.

---

# 8. Year-over-Year KPI Layer

The report contains the following previous-year measures:

- `LY_Online Sale`
- `LY_Online`
- `LY_BM`
- `LY_ST`
- `LY_Others`

`LY` is interpreted as **Last Year** based on the naming convention and report purpose.

These measures are used in the KPI/card visual on both pages.

### Purpose

The LY layer allows management to compare current online performance against the corresponding previous-year performance.

> The exact DAX expressions behind these measures could not be safely extracted because the internal DataModel is XPress9-compressed.

---

# 9. Report Page 1 — Online Sale Summary

## Objective

The **Online Sale Summary** page is the management overview.

It provides:

- year filtering,
- date filtering,
- month-level sales table,
- sales by mode,
- sales by terminal,
- tickets by terminal,
- current and previous-year KPI measures.

---

## Visual Inventory

The page contains **14 visual containers**, including:

- shapes/layout elements,
- title textbox,
- year slicer,
- date slicer,
- analytical table,
- mode distribution donut,
- terminal sales bar chart,
- terminal ticket bar chart,
- KPI/card visual.

---

## 9.1 Page Header

The report contains the title:

**DAEWOO EXPRESS BUS SERVICE**

The title is formatted using Times New Roman and centered.

---

## 9.2 Year Slicer

Visual type:

`advancedSlicerVisual`

Field:

`Date Table[Year]`

### Default/filter context

The visual query contains:

`2025`

Therefore, the report currently opens with **2025** as the explicit filter context in the stored visual queries.

---

## 9.3 Date Slicer

Visual type:

`slicer`

Field:

`Date Table[Date]`

Configuration:

- Dropdown
- Date-based filtering

This allows users to narrow the dashboard to a specific date/range.

---

## 9.4 Monthly Online Sales Table

Visual type:

`tableEx`

Measures:

- Total Online Sale
- Medium Sale Online
- Medium Sale BM
- Medium Sale ST
- Medium Sale Other

Time dimension:

- Month

### Business use

This visual provides a structured monthly breakdown of online sales and sales-medium contribution.

---

## 9.5 Sales by Mode

Visual type:

`donutChart`

Dimension:

`Mode / Terminals[Mode]`

Measure:

`Online Measures[Sale]`

### Business question

> What proportion of online sales is generated by each sales mode?

This is useful for understanding the composition of the online business.

---

## 9.6 Online Sales by Terminal

Visual type:

`barChart`

Dimension:

`Mode / Terminals[Terminal]`

Measure:

`Online Measures[Total Online Sale]`

### Business question

> Which terminals generate the highest online sales?

This enables terminal benchmarking and identification of high/low-performing locations.

---

## 9.7 Online Tickets by Terminal

Visual type:

`barChart`

Dimension:

`Mode / Terminals[Terminal]`

Measure:

`Online Measures[TKT QTY]`

### Business question

> Which terminals generate the highest number of online tickets?

This complements revenue analysis.

### Why this matters

A terminal may have:

- high ticket quantity but lower average revenue,
- lower ticket quantity but higher-value sales.

Therefore, ticket volume and sales revenue should be evaluated together.

---

## 9.8 KPI Card

The KPI visual references:

### Current period

- Total Online Sale
- Medium Sale Online
- Medium Sale BM
- Medium Sale ST
- Medium Sale Other

### Previous year

- LY_Online Sale
- LY_Online
- LY_BM
- LY_ST
- LY_Others

This creates a compact management-level performance view.

---

# 10. Report Page 2 — Date Wise Online Sale

## Objective

The **Date Wise Online Sale** page focuses on time-series performance.

It enables users to examine:

- monthly sales,
- daily sales,
- monthly ticket quantity,
- daily ticket quantity,
- sales-medium contribution,
- current vs previous-year KPI context.

---

## Visual Inventory

The page contains **13 visual containers**, including:

- page header,
- year slicer,
- date slicer,
- detailed table,
- online sales trend chart,
- online ticket trend chart,
- KPI card,
- layout shapes.

---

## 10.1 Date-wise Detail Table

Visual type:

`tableEx`

Measures:

- Total Online Sale
- Medium Sale Online
- Medium Sale BM
- Medium Sale ST
- Medium Sale Other

Time hierarchy:

- Month
- Day

### Purpose

This provides a detailed drill-down structure from:

```text
Year
  ↓
Month
  ↓
Day
```

---

## 10.2 Online Sales Trend

Visual type:

`lineChart`

Measure:

`Total Online Sale`

Time hierarchy:

- Day
- Month

### Business purpose

Used to identify:

- sales growth,
- sales decline,
- spikes,
- weak periods,
- seasonality,
- daily fluctuations.

---

## 10.3 Online Ticket Trend

Visual type:

`lineChart`

Measure:

`TKT QTY`

Time hierarchy:

- Month
- Day

### Business purpose

Shows ticket-volume movement over time.

---

## 10.4 KPI Card

The same KPI layer is reused:

### Current

- Total Online Sale
- Medium Sale Online
- Medium Sale BM
- Medium Sale ST
- Medium Sale Other

### Last Year

- LY_Online Sale
- LY_Online
- LY_BM
- LY_ST
- LY_Others

This creates consistency between the summary and detailed pages.

---

# 11. Filter Architecture

The report uses two major filter controls:

## Year

Source:

`Date Table[Year]`

Purpose:

- annual reporting,
- year-over-year analysis.

## Date

Source:

`Date Table[Date]`

Purpose:

- daily/monthly slicing,
- custom period analysis.

---

# 12. Default Reporting Context

The stored visual queries contain an explicit year condition:

```text
Year = 2025
```

This appears across the major analytical visuals.

Therefore, **2025 is the documented default query context** in this PBIX artifact.

This should be reviewed before publishing a future-year version of the report.

---

# 13. Analytical Flow

The dashboard's analytical workflow can be represented as:

```text
                User selects Year / Date
                         │
                         ▼
                  Date Table Filter
                         │
                         ▼
              Online Sales Model Context
                         │
          ┌──────────────┼───────────────┐
          ▼              ▼               ▼
      Sales KPIs     Ticket KPIs     LY KPIs
          │              │               │
          └──────────────┼───────────────┘
                         ▼
              Business Dimensions
                  │            │
                  ▼            ▼
                Mode        Terminal
                  │            │
                  └──────┬─────┘
                         ▼
                Power BI Visuals
                         │
        ┌────────────────┼─────────────────┐
        ▼                ▼                 ▼
    Summary         Trend Analysis     Detail Table
```

---

# 14. Management Questions Answered

The report is capable of supporting the following management questions:

### Revenue

- What is total online sales?
- How much is generated by different sales mediums?
- Which terminals generate the most online revenue?

### Volume

- How many online tickets were sold?
- Which terminals have the highest online ticket volume?

### Trend

- What is the monthly online sales trend?
- What is the daily online sales trend?
- What is the monthly/daily ticket trend?

### Composition

- What is the sales contribution by mode?
- How does online sales split across Online, BM, ST and Other categories?

### Benchmarking

- How is current online performance compared with last year?
- Which terminal or period is underperforming?

---

# 15. Key Business Metrics

A useful GitHub-level KPI framework for this project is:

| KPI | Purpose | Grain |
|---|---|---|
| Total Online Sale | Revenue performance | Date / Terminal / Mode |
| Online Medium Sale | Online channel contribution | Date |
| BM Sale | BM contribution | Date |
| ST Sale | ST contribution | Date |
| Other Sale | Other contribution | Date |
| TKT QTY | Ticket volume | Date / Terminal |
| LY Online Sale | Prior-year revenue benchmark | Date |
| LY Online | Prior-year online contribution | Date |
| LY BM | Prior-year BM benchmark | Date |
| LY ST | Prior-year ST benchmark | Date |
| LY Others | Prior-year other benchmark | Date |

---

# 16. Dashboard Design

The PBIX uses a corporate dashboard style featuring:

- Daewoo Express branding,
- blue primary visual elements,
- red accent styling,
- Times New Roman typography in major report components,
- rounded/dropdown slicers,
- management-style KPI cards,
- bar charts for terminal comparison,
- donut chart for mode composition,
- line charts for time-series analysis.

The report uses a consistent two-page analytical structure:

```text
Page 1 → Executive Summary
Page 2 → Time-Series / Detail Analysis
```

---

# 17. Technical Power BI Components

The PBIX contains:

- Power BI report layout
- Semantic model
- Date hierarchy
- DAX measures
- Visual semantic queries
- Report filters
- Slicers
- Tables
- Bar charts
- Line charts
- Donut chart
- KPI/card visual
- Model diagram
- Theme/resource package

The report references a shared theme resource:

`BaseThemes/CY24SU10.json`

---

# 18. Visual-to-Measure Lineage

## Online Sale Summary

```text
Date Table
   │
   ├── Year
   └── Date
        │
        ▼
Online Measures
   ├── Total Online Sale
   ├── Medium Sale Online
   ├── Medium Sale BM
   ├── Medium Sale ST
   ├── Medium Sale Other
   ├── TKT QTY
   ├── LY_Online Sale
   ├── LY_Online
   ├── LY_BM
   ├── LY_ST
   └── LY_Others
        │
        ├── Monthly Table
        ├── Mode Donut
        ├── Terminal Sales Bar
        ├── Terminal Ticket Bar
        └── KPI Card
```

## Date Wise Online Sale

```text
Date Table
   │
   ├── Year
   ├── Month
   └── Day
        │
        ▼
Online Measures
        │
        ├── Date Detail Table
        ├── Online Sales Line Chart
        ├── Ticket Quantity Line Chart
        └── KPI Card
```

---

# 19. Data Modeling Assessment

## Strengths

### 1. Dedicated Date Table

The use of a dedicated Date Table is appropriate for:

- time intelligence,
- monthly reporting,
- daily reporting,
- year-over-year analysis.

### 2. Centralized Measures

The `Online Measures` table provides a clean semantic layer.

This is preferable to scattering DAX measures throughout transactional tables.

### 3. Separation of Dimensions

Terminal and mode analysis are separated from KPI calculations.

### 4. Consistent Visual Logic

Both report pages reuse the same KPI measures, which improves consistency.

### 5. Executive + Detail Architecture

The two-page structure is logical:

- summary first,
- detail second.

---

# 20. Potential Improvements

## 20.1 Remove Hard-Coded Year Filters

The visual queries contain:

```text
Year = 2025
```

If this is only intended as an initial view, it should ideally be replaced with a more maintainable approach.

### Recommended

Use:

- relative date logic,
- latest available year,
- report/page filter,
- or a controlled default slicer state.

---

## 20.2 Add Explicit KPI Definitions

For production governance, document each measure with:

- business definition,
- DAX formula,
- source column,
- filter behavior,
- owner.

Example:

```text
KPI: Total Online Sale
Definition: Total monetary value of online ticket sales.
Owner: Operations / MIS
Refresh: Daily
```

---

## 20.3 Add Growth KPIs

Recommended additions:

```text
YoY Sales Growth %
YoY Ticket Growth %
Average Online Fare
Online Sales per Ticket
Online Sales Share %
```

Potential formulas conceptually:

```text
YoY Growth % =
(Current Year - Last Year) / Last Year
```

---

## 20.4 Add Terminal Benchmarking

Recommended KPIs:

- Sales per terminal
- Tickets per terminal
- Average ticket value
- Terminal contribution %
- YoY terminal growth %

---

## 20.5 Add Mode Benchmarking

Recommended:

```text
Mode Sales
Mode Ticket Qty
Mode Share %
Mode YoY Growth %
```

---

## 20.6 Add Dynamic Period Comparison

Instead of only year-based comparison, consider:

- MTD
- YTD
- Previous Month
- Previous Year
- Same Month Last Year
- Rolling 7 Days
- Rolling 30 Days

---

# 21. Recommended Future-State Dashboard

A more advanced version could contain:

### Page 1 — Executive Overview

- Total Online Sales
- Online Tickets
- Average Online Fare
- Online Share %
- YoY Growth %
- Sales by Mode
- Sales by Terminal
- Current vs LY

### Page 2 — Sales Trend

- Daily Sales
- Monthly Sales
- YTD Sales
- MTD Sales
- Rolling 30-day trend

### Page 3 — Terminal Performance

- Terminal Sales Ranking
- Ticket Ranking
- Average Fare
- YoY Growth
- Contribution %

### Page 4 — Sales Mode Analysis

- Online
- BM
- ST
- Other
- Mode contribution
- Mode growth

### Page 5 — Data Quality

- Missing dates
- Missing terminal
- Missing mode
- Duplicate transactions
- Negative/zero sales
- Ticket quantity anomalies

---

# 22. Recommended GitHub Repository Structure

```text
tms-online-powerbi/
│
├── README.md
│
├── PowerBI/
│   └── TMS - Online.pbix
│
├── Documentation/
│   ├── Data_Model.md
│   ├── KPI_Dictionary.md
│   └── Dashboard_Documentation.md
│
├── DAX/
│   ├── Sales_Measures.md
│   ├── Ticket_Measures.md
│   └── YoY_Measures.md
│
├── Screenshots/
│   ├── online-sale-summary.png
│   └── date-wise-online-sale.png
│
└── README.md
```

---

# 23. Suggested GitHub README Summary

```text
TMS Online Sales Analytics — Power BI

An interactive Power BI analytics solution developed to monitor
online ticket sales performance for an intercity bus operation.

The dashboard provides:
- Online sales KPIs
- Online ticket volume
- Sales-medium analysis
- Terminal performance
- Daily/monthly trends
- Previous-year comparison
- Interactive date/year filtering

Tools:
- Power BI
- DAX
- Power Query
- Data Modeling
- Time Intelligence

Business Domain:
Transportation / Bus Operations / Sales Analytics
```

---

# 24. Skills Demonstrated by This Project

This project demonstrates practical capability in:

### Power BI

- Dashboard development
- Report design
- Visual configuration
- Interactive slicers
- Semantic queries
- KPI reporting

### DAX

- Measure-based reporting
- Current-period calculations
- Previous-year calculations
- Time-intelligence-oriented KPI design

### Data Modeling

- Date dimension
- Business dimensions
- Measure table
- Reference/master tables
- Analytical relationships

### Business Analysis

- Sales performance
- Terminal benchmarking
- Channel/mode analysis
- Trend analysis
- Management reporting

### Data Visualization

- KPI cards
- Bar charts
- Donut charts
- Line charts
- Detail tables

---

# 25. Project Impact

The solution converts online ticketing data into a management-ready analytical layer.

Instead of manually reviewing raw operational data, users can move through:

```text
Overall Performance
        ↓
Sales Composition
        ↓
Terminal Performance
        ↓
Ticket Volume
        ↓
Daily / Monthly Trend
        ↓
Previous-Year Benchmark
```

This supports faster identification of:

- strong terminals,
- weak terminals,
- sales trends,
- volume trends,
- channel contribution,
- potential performance gaps.

---

# 26. Limitations of This Reverse Engineering

This documentation was generated from the PBIX package internals available in the uploaded file.

The following were directly identifiable:

- report pages,
- page names,
- visual types,
- visual fields,
- measure names,
- dimension names,
- filters,
- model diagram entities,
- theme/resource metadata,
- report-level structural information.

The following were **not safely recoverable** from the available PBIX internals:

- complete source connection/query definitions,
- complete Power Query M scripts,
- exact DAX formulas for every measure,
- complete column metadata for every model table,
- row-level data values.

The primary technical limitation is that the internal `DataModel` is stored in Microsoft's compressed XPress9 format.

Therefore, this document intentionally does **not invent** source-system details or DAX formulas.

---

# 27. Recommended Next Step for a Production Documentation Package

For a fully governed GitHub project, export these additional artifacts from Power BI:

1. **DAX Studio / Tabular Editor model metadata**
2. All DAX measures
3. Power Query M scripts
4. Table and column dictionary
5. Relationship diagram
6. Data source documentation
7. Refresh schedule
8. Data-quality rules
9. Dashboard screenshots
10. Business KPI definitions

These can then be added to:

```text
Documentation/
DAX/
PowerQuery/
DataModel/
Screenshots/
```

---

# 28. Final Project Assessment

### Overall maturity: **Intermediate Power BI Business Analytics Project**

The project demonstrates a solid reporting foundation with:

- a dedicated date dimension,
- centralized KPI measures,
- terminal and mode dimensions,
- summary and detail pages,
- revenue and volume KPIs,
- year-over-year measures,
- interactive filtering.

The strongest part of the design is its **business-oriented analytical flow**: management can start from total online sales, move into sales composition, compare terminals, and then drill into date-wise performance.

The biggest opportunity for improvement is to evolve the dashboard from a **reporting dashboard** into a **performance management product** by adding:

- explicit YoY growth percentages,
- MTD/YTD metrics,
- average fare,
- terminal ranking,
- contribution percentages,
- automated data-quality monitoring,
- documented DAX/M queries,
- and a formal semantic model/data dictionary.

---

## Project Classification

| Category | Assessment |
|---|---|
| Business Domain | Transportation / Online Ticket Sales |
| BI Platform | Power BI |
| Modeling | Dimensional / semantic-model based |
| Visualization | Management dashboard |
| Time Intelligence | Present |
| YoY Analysis | Present |
| Terminal Analysis | Present |
| Mode Analysis | Present |
| Ticket Analysis | Present |
| Interactive Filtering | Present |
| DAX Measures | Present |
| Data Quality Layer | Not evident from report layout |
| Advanced Forecasting | Not present |
| Machine Learning | Not present |
| Automated Insights | Not evident |
| Overall Level | Intermediate |

---

## Author / Portfolio Positioning

For a professional portfolio, this project can be positioned as:

> **Power BI Transportation Sales Analytics Dashboard** — Developed an interactive business intelligence solution for monitoring online ticket sales, ticket volume, terminal performance, sales-mode contribution and year-over-year performance using Power BI, DAX, data modeling and time-based analytics.

