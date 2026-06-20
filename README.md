# Revenue & Profitability Analysis for a UK Date Syrup Manufacturer

## Project Overview

Designed and developed an end-to-end Power BI reporting solution for a UK-based date syrup manufacturer to provide visibility into revenue performance, profitability, customer behaviour, and the underlying drivers of business growth.

The solution transformed operational and transactional data into actionable business intelligence through executive reporting and diagnostic analytics.

---

## Business Challenge

Following steady growth since launching in 2021, the business required a reporting solution capable of answering two critical questions:

### 1. How is the business performing?

* Revenue performance
* Profitability trends
* Customer growth
* Product performance

### 2. Why is the business performing this way?

* Which products drive revenue?
* Which customer segments generate the most value?
* Is revenue driven by order volume or order value?
* Where are potential revenue concentration risks?

The objective was to create a single source of truth that would support both strategic decision-making and operational analysis.

---

## Solution

Developed a two-page Power BI reporting solution consisting of:

### Dashboard 1 — Executive Performance Overview

<img width="1243" height="794" alt="image" src="https://github.com/user-attachments/assets/f1023472-a52c-456f-98b3-f3d5c9fa31ec" />
Designed for leadership and senior stakeholders to monitor:

* Revenue
* Gross Profit
* Profit Margin %
* Customer Growth
* Revenue Growth %
* Revenue Trends
* Profit Trends
* Product Performance
* Customer Segment Performance

### Dashboard 2 — Revenue Driver Analysis
<img width="973" height="788" alt="image" src="https://github.com/user-attachments/assets/d074abad-646b-438a-9429-cdea949e426b" />

Designed to identify the underlying drivers of revenue performance through:

* Revenue Share Analysis
* Order Share Analysis
* Average Order Value Analysis
* Product Revenue Drivers
* Customer Revenue Drivers
* Revenue Concentration Analysis

---

## Data Preparation & Modelling Approach

The source data was initially provided as a single flat table containing transactional, production, customer, product, and date-related information.

To improve reporting performance, maintainability, and analytical flexibility, the dataset was transformed into a dimensional model.

### Data Preparation Activities

* Reviewed and validated source data quality.
* Standardised column names and data types.
* Created a dedicated Date dimension to support time intelligence calculations.
* Separated transactional and production data into fact tables.
* Created Product and Customer dimension tables.
* Established relationships between fact and dimension tables.
* Organised DAX measures into dedicated measure tables to improve model governance and maintainability.

### Data Modelling Approach

As the organisation maintained a dedicated Data Engineering team, recommendations were provided regarding dimensional modelling best practices and the benefits of implementing a star schema architecture within the enterprise data platform.

Key benefits discussed included:

* Improved query performance
* Simplified reporting logic
* Enhanced scalability
* Improved maintainability
* Consistent KPI calculations across reports

Due to database governance restrictions, direct modification of the SQL database was not authorised. As a result, the dimensional model was implemented within Power BI while preserving the integrity of the source system.

The final solution utilised a **Star Schema with multiple fact tables and shared conformed dimensions (Fact Constellation/Galaxy Schema)**, enabling efficient reporting and analytical flexibility without requiring changes to the underlying database infrastructure.

---

## Data Model & Technical Architecture

The solution was built using a **Star Schema data model with multiple fact tables and shared conformed dimensions (Fact Constellation / Galaxy Schema).**

### Fact Tables

#### Fact_Transactions

* Revenue
* Gross Profit
* Orders
* Units Sold

#### Fact_Production

* Production Metrics
* Production Costs

### Dimension Tables

#### Dim_Date

* Date
* Month
* Quarter
* Year

#### Dim_Product

* Product Information

#### Dim_Customer

* Customer Information
* Customer Segment

Shared dimensions were utilised across both fact tables to ensure consistent KPI calculations, improved performance, and scalable reporting.

---

## Dashboard Structure

### Executive Performance Overview

**Purpose:** What happened?

Provides leadership with a high-level view of company performance through KPIs, profitability trends, revenue trends, and contribution analysis.

#### Key KPIs

* Total Revenue
* Gross Profit
* Profit Margin %
* Revenue Growth %
* Total Customers

#### Key Visuals

* Revenue Trend
* Gross Profit Trend
* Revenue Contribution by Product
* Revenue Contribution by Customer Segment
* Gross Profit Contribution by Product
* Gross Profit Contribution by Customer Segment
* Annual Performance Summary
* Dynamic Executive Insights

---

### Revenue Driver Analysis

**Purpose:** Why did it happen?

Explains the factors influencing revenue performance using revenue share, order share, and average order value analysis.

#### Key KPIs

* Total Revenue
* Total Orders
* Units Sold
* Average Order Value

#### Key Visuals

* Revenue Share by Product
* Revenue Share by Customer Segment
* Order Share by Product
* Order Share by Customer Segment
* Average Order Value by Product
* Average Order Value by Customer Segment
* Dynamic Revenue Driver Insights

---

## Key Business Insights

### Revenue & Profitability

* Revenue increased by **11.6% year-over-year**, reaching **£54.6M**.
* Gross Profit increased by **11.7%**, reaching **£24.5M**.
* Profit Margin remained stable at approximately **45%**.

### Product Performance

* **Original Date Syrup** contributed **28.8%** of total revenue.
* Original Date Syrup generated **33.3%** of total orders, indicating that performance was primarily driven by transaction volume.
* **Premium Date Syrup** achieved the highest Average Order Value (**£754.62**), demonstrating stronger pricing power despite lower transaction volumes.

### Customer Performance

* **Distributor customers** generated **62.9%** of total revenue.
* Distributors accounted for only **15%** of total orders.
* Distributor Average Order Value (**£2,288.50**) significantly exceeded Wholesale (**£553.10**) and Retail (**£65.60**).
* Retail customers generated **55%** of total orders but contributed only **6.6%** of total revenue.

### Strategic Insight

Revenue concentration was driven primarily by high-value distributor transactions, while product performance was influenced by a combination of transaction volume and pricing strategy.

* Original Date Syrup led revenue generation through order volume.
* Premium Date Syrup demonstrated premium-value positioning through higher transaction values.

---

## Key DAX Measures

### Revenue Growth Label

Creates a dynamic KPI label that compares current revenue growth against the previous year's growth and displays an upward or downward indicator.

```DAX
Revenue Growth Label =
VAR Growth = [Revenue growth]
VAR PrevGrowth =
    CALCULATE(
        [Revenue growth],
        DATEADD('Date table'[Date], -1, YEAR)
    )
VAR Arrow =
    IF(
        Growth >= PrevGrowth,
        UNICHAR(9650),
        UNICHAR(9660)
    )
RETURN
Arrow & " " &
FORMAT(Growth, "0.0%") &
" vs PY"
```

### Best Customer Insight

Identifies the highest revenue-generating customer segment and generates a dynamic business insight.

```DAX
Best Customer =
VAR TopCustomer =
    TOPN(
        1,
        VALUES(dim_customer_type[CustomerType]),
        [Total Revenue],
        DESC
    )
RETURN
"The top-performing customer was " &
MAXX(TopCustomer, dim_customer_type[CustomerType]) &
" based on revenue."
```

### Revenue Distribution

Calculates each category's percentage contribution to total revenue while respecting report filters and slicers.

```DAX
Revenue distribution =
DIVIDE(
    [Total revenue],
    CALCULATE(
        [Total revenue],
        ALLSELECTED(fact_transactions)
    )
)
```

---

## DAX Concepts Demonstrated

* Time Intelligence
* Dynamic KPI Indicators
* Context Transition
* Filter Context Manipulation
* Contribution Analysis
* Dynamic Business Narratives
* TOPN Ranking Logic
* CALCULATE
* DATEADD
* ALLSELECTED
* Interactive Reporting

---

## Technical Skills Demonstrated

* Power BI
* Power Query
* DAX
* Time Intelligence
* KPI Development
* Data Modelling & Architecture
* Dimensional Modelling
* Star Schema Design
* Fact Constellation (Galaxy Schema)
* Semantic Model Development
* Revenue Analysis
* Profitability Analysis
* Dynamic Insights
* Dashboard Navigation
* Business Intelligence Reporting
* Data Storytelling
* Stakeholder Engagement
* BI Solution Design
* Data Governance Awareness

---

## Business Value Delivered

The solution enabled stakeholders to:

* Monitor business performance through executive-level reporting.
* Track revenue and profitability trends over time.
* Identify the products and customer segments driving revenue.
* Understand whether performance was driven by volume or pricing.
* Detect revenue concentration risks.
* Support data-driven commercial and strategic decision-making.

---

## Project Outcome

This project demonstrates the application of Business Intelligence best practices by combining data modelling, DAX development, analytical reporting, and business storytelling to transform raw data into actionable business insights.
