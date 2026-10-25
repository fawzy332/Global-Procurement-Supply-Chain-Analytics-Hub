# 📊 Global Procurement & Supply Chain Analytics Hub

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Data_Analysis_Expressions-blue?style=for-the-badge)
![Power Query](https://img.shields.io/badge/Power_Query-Data_Transformation-orange?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Procurement_%26_Supply_Chain-green?style=for-the-badge)

## 📌 Executive Summary
In legacy corporate environments, procurement and purchasing teams often operate on fragmented, unvalidated Excel workbooks. This reliance on manual reporting causes severe visibility bottlenecks, unmonitored vendor quality risks, and unmanaged budget overruns.

The **Global Procurement Analytics Hub** is an enterprise-grade Power BI reporting platform designed to transform raw purchasing logs into real-time, dynamic decision systems. It enables procurement executives, category managers, and finance teams to monitor financial commitments, evaluate supplier SLA compliance, isolate backlog risks, and track budget burn rates across all purchasing streams[cite: 1, 2, 3, 4, 5].

---

## 🚀 Key Features & Report Pages

### 1️⃣ Executive Landing Page (High-Level Overview)
* **Target Audience:** C-Suite Executives & Procurement Directors.
* **Business Value:** Provides an instant snapshot of total financial exposure ($49.30M Spend), total negotiated value ($45.37M), and negotiated savings ($3.93M) alongside top-level supplier and order fulfillment distributions[cite: 1].
* **Key Visuals:** Executive KPI cards, Quick Navigation portal, Total Spend by Supplier (Donut Chart), and Order Status Distribution[cite: 1].

### 2️⃣ Spend & Savings Analysis
* **Target Audience:** Category Managers & Strategic Sourcing Leads[cite: 2].
* **Business Value:** Pinpoints spending velocity across spend buckets and evaluates negotiation effectiveness per category and vendor over time[cite: 2].
* **Key Visuals:** Category Savings Matrix (Total Spend, Savings, Savings Rate %), Monthly Spend vs. Negotiated Value line-column chart, and Supplier Spend ranking[cite: 2].

### 3️⃣ Delivery & Vendor Quality (Supplier Scorecard)
* **Target Audience:** Supplier Relationship Managers & Quality Engineers[cite: 3].
* **Business Value:** Enforces vendor accountability by benchmarking On-Time In-Full (OTIF) delivery performance against quality defect rates[cite: 3].
* **Key Visuals:** Supplier Scorecard Table, Category Defect Breakdown (horizontal bar chart), and **Supplier Quality vs. Lead Time Scatter Matrix**[cite: 3].

### 4️⃣ Fulfillment Backlog & Supply Chain Risk
* **Target Audience:** Logistics Operations & Risk Analysts[cite: 4].
* **Business Value:** Identifies single-source supplier dependencies and calculates the financial exposure of pending order backlogs ($3M) to prevent production downtime[cite: 4].
* **Key Visuals:** Monthly Fulfillment Status trend line, Supplier Concentration Risk Treemap, and Order Backlog Breakdown drill-down[cite: 4].

### 5️⃣ Category Budget vs. Actuals (Variance Analysis)
* **Target Audience:** Finance Directors & Procurement Category Owners[cite: 5].
* **Business Value:** Monitors budget burn rate percentages across categories (e.g., Office Supplies vs. Electronics) to prevent year-end budget overruns and support capital reallocation[cite: 5].
* **Key Visuals:** Dynamic Budget Variance KPIs, Budget Burn Rate % Matrix, and visual variance status indicators[cite: 5].

---

## 📐 Data Model & Architecture

The report relies on a clean **Star Schema** data architecture to ensure low memory consumption, fast visual cross-filtering, and seamless scaling.

* **Fact Table:** `Fact_Procurement_Orders` (contains order-level details, purchase costs, delivery metrics, defect logs, and fulfillment timestamps).
* **Dimension Tables:**
  * `Dim_Suppliers` (Supplier profile, location, contract SLA defaults)
  * `Dim_Item_Categories` (Product categorization, category budget allocations)
  * `Dim_Date` (Custom calendar table supporting time-intelligence functions)

---

## 🧮 Key DAX Measures (Sample Snippets)

```dax
// Total Spend
Total Spend = SUM(Fact_Procurement_Orders[Total_Cost])

// Negotiated Savings Rate %
Savings Rate % = 
DIVIDE(
    SUM(Fact_Procurement_Orders[Negotiated_Savings]), 
    SUM(Fact_Procurement_Orders[Total_Spend]), 
    0
)

// On-Time In-Full (OTIF) Rate %
OTIF Rate % = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_Procurement_Orders), Fact_Procurement_Orders[OTIF_Flag] = TRUE()),
    COUNTROWS(Fact_Procurement_Orders),
    0
)

// Budget Burn Rate %
Budget Burn Rate % = 
DIVIDE(
    [Total Spend], 
    SUM(Dim_Item_Categories[Category_Budget]), 
    0
)
