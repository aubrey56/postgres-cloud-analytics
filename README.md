# E-Commerce Cloud Analytics Project (PostgreSQL)

## Overview
This repository contains a cloud-based PostgreSQL data analysis project. The goal is to build an e-commerce database model, execute data manipulation, and perform advanced SQL analytics using Window Functions, CTEs, and aggregation.

## Database Schema
The database model contains four core tables:
- `customers`
- `products`
- `orders`
- `order_items`

## Advanced Analytical Extraction Query
```sql
WITH department_summary AS (
    SELECT 
        department,
        COUNT(project_id) AS total_projects,
        SUM(budget) AS dept_budget,
        SUM(spent) AS dept_spent,
        ROUND(AVG((spent / budget) * 100), 2) AS avg_budget_utilization_pct
    FROM cloud_projects
    GROUP BY department
)
SELECT 
    p.project_name,
    p.department,
    p.status,
    p.budget,
    p.spent,
    (p.spent - p.budget) AS variance,
    ds.avg_budget_utilization_pct AS dept_avg_spent_pct,
    CASE 
        WHEN p.spent > p.budget THEN 'OVER BUDGET'
        WHEN p.spent = p.budget THEN 'ON BUDGET'
        ELSE 'UNDER BUDGET'
    END AS financial_health
FROM cloud_projects p
JOIN department_summary ds ON p.department = ds.department
ORDER BY variance DESC;
```

## Key SQL Concepts Applied
- PostgreSQL cloud deployment (Neon.tech / AWS)
- DDL (`CREATE TABLE`) with foreign key constraints
- DML (`INSERT INTO`) & data manipulation
- CTEs (`WITH` clauses) for modular query design
- Window functions (`SUM() OVER (PARTITION BY ...)`)
- Conditional expressions (`CASE ... WHEN`)
