# 🔍 Exploratory Data Analysis (EDA) — Sales Data Warehouse

![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=for-the-badge&logo=microsoft-sql-server&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![EDA](https://img.shields.io/badge/Type-EDA-blue?style=for-the-badge)

---

## 📌 Project Overview

This project performs a comprehensive **Exploratory Data Analysis (EDA)** on a sales data warehouse using **Microsoft SQL Server**. The analysis covers the full EDA lifecycle — from schema exploration and data profiling to business-driven insights across customers, products, and sales performance.

The goal is to understand the data structure, uncover patterns, identify top/bottom performers, and generate a clean analytical foundation for future reporting or machine learning projects.

---

## 🗂️ Database Structure

The analysis is built on a **Gold Layer** data warehouse schema with three core tables:

| Table | Description |
|---|---|
| `gold.dim_customers` | Customer dimension — demographics, country, gender, birthdate |
| `gold.dim_products` | Product dimension — product names, categories, pricing |
| `gold.fact_sales` | Sales fact table — orders, quantities, revenue, pricing |

---

## 🎯 Business Questions Answered

### 👥 Customer Analysis
- What is the age range of our customer base? *(youngest vs. oldest)*
- How many unique customers exist vs. customers who have actually placed orders?
- How are customers distributed across countries?
- What is the gender breakdown of our customer base?
- Which customers generate the highest revenue?
- Which customers have placed the fewest orders?

### 📦 Product Analysis
- How many products exist per category?
- What is the average price per product category?
- Which **Top 5 products** generate the highest revenue?
- Which **5 products** are the worst performers in sales?

### 💰 Sales Performance
- What is the **total revenue** generated?
- How many total items have been sold?
- What is the average selling price?
- What is the total number of orders placed?
- How is revenue distributed across product categories?
- How are sold items distributed across countries?

---

## 🔑 Key SQL Techniques Used

| Technique | Description |
|---|---|
| `SUM`, `AVG`, `COUNT` | Aggregate functions for KPI calculation |
| `DATEDIFF` + `GETDATE()` | Dynamic age calculation from birthdates |
| `GROUP BY` + `ORDER BY` | Segmentation and ranking of business dimensions |
| `JOIN` / `LEFT JOIN` | Multi-table relational queries across fact & dimension tables |
| `UNION ALL` | Combining metrics into a single report view |
| `TOP N` | Quick top/bottom performer identification |
| `RANK() OVER (ORDER BY ...)` | Window function for flexible ranking |
| Subqueries | Encapsulated logic for filtered ranking |
| `AVG() OVER()` | Window function to compare rows against overall average |
| `INFORMATION_SCHEMA` | Schema introspection and metadata exploration |

---

## 📊 Sample Insights (EDA Highlights)

```sql
-- Customer age range
SELECT
    MIN(birthdate) AS Oldest_customer,
    DATEDIFF(YEAR, MIN(birthdate), GETDATE()) AS Age,
    MAX(birthdate) AS Youngest_customer,
    DATEDIFF(YEAR, MAX(birthdate), GETDATE()) AS Age
FROM gold.dim_customers;
```

```sql
-- Top 5 revenue-generating products using RANK()
SELECT * FROM (
    SELECT
        p.product_name,
        SUM(s.sales_amount) AS revenue,
        RANK() OVER (ORDER BY SUM(s.sales_amount) DESC) AS rank_products
    FROM gold.dim_products AS p
    JOIN gold.fact_sales AS s ON p.product_key = s.product_key
    GROUP BY p.product_name
) t
WHERE rank_products <= 5;
```

```sql
-- Products priced above the overall average
SELECT * FROM (
    SELECT
        product_key,
        price,
        AVG(price) OVER() AS avg_price
    FROM gold.fact_sales
) a
WHERE price > avg_price;
```

---

## 🛠️ Tools & Technologies

- **Microsoft SQL Server** — Query execution & analysis
- **SQL Server Management Studio (SSMS)** — Development environment
- **Data Warehouse (Gold Layer)** — Star schema with fact & dimension tables

---

## 📁 Repository Structure

```
📦 sql-eda-project
 ┣ 📄 EDA_project.sql       ← Full annotated SQL script
 ┗ 📄 README.md             ← Project documentation (this file)
```

---

## 🚀 How to Run

1. Open **SQL Server Management Studio (SSMS)**
2. Connect to your SQL Server instance
3. Ensure the `DataWarehouseAnalytics` database is available
4. Open `EDA_project.sql`
5. Execute queries section by section or all at once

---

## 👤 Author

**Basmala Hassan**
Data Analyst | SQL | EDA | Data Warehousing

[![LinkedIn] https://www.linkedin.com/in/basmala-hassan-976b20224/
[![GitHub] https://github.com/Basmala157
---

## 📄 License

This project is open source and available
