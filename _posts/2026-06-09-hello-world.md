---
layout: post
title: "Hello World — What This Blog Is About"
date: 2026-06-09
description: "First post: why I'm writing about dbt, DuckDB, SQL, Python, and the analytics engineering stack."
tags: [meta, dbt, sql]
published: false
---

This is a placeholder post. Replace this content with your own — the structure below is here so you can see how **headings**, **code blocks**, and **inline code** render.

## Why I'm writing this

I'm a Data Analyst with a background in BI and data warehousing, moving toward analytics engineering. Most of what I learn happens by doing — so writing forces me to slow down and understand things properly.

Topics I plan to cover:

- **dbt** — data modeling, testing, documentation
- **DuckDB** — fast local analytics, replacing pandas for some workflows
- **SQL** — query optimization, window functions, anti-patterns
- **Python** — pandas, scripting, light ML for data work
- **Data engineering** — pipelines, warehouse design, layered architectures

## Code examples

### SQL

A simple example using a window function to calculate a running total:

```sql
SELECT
    order_date,
    revenue,
    SUM(revenue) OVER (
        ORDER BY order_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total
FROM orders
ORDER BY order_date;
```

A dbt-style model with a CTE:

```sql
-- models/marts/finance/fct_revenue.sql
WITH source AS (
    SELECT * FROM {{ ref('stg_orders') }}
),

aggregated AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        SUM(amount)                     AS total_revenue,
        COUNT(DISTINCT customer_id)     AS unique_customers
    FROM source
    WHERE status = 'completed'
    GROUP BY 1
)

SELECT * FROM aggregated
```

### Python

Loading a Parquet file with DuckDB and querying it directly:

```python
import duckdb

con = duckdb.connect()

result = con.execute("""
    SELECT
        order_month,
        SUM(revenue)   AS total_revenue,
        COUNT(*)       AS num_orders
    FROM 'data/orders.parquet'
    GROUP BY order_month
    ORDER BY order_month
""").df()

print(result.head())
```

Profiling a DataFrame with pandas before building a dbt model:

```python
import pandas as pd

df = pd.read_csv("raw_orders.csv", parse_dates=["order_date"])

# Quick profiling
print(df.dtypes)
print(df.isnull().sum())
print(df["status"].value_counts())
```

### YAML (dbt schema file)

```yaml
version: 2

models:
  - name: fct_revenue
    description: "Monthly revenue aggregated from completed orders."
    columns:
      - name: month
        description: "First day of the month."
        tests:
          - not_null
          - unique
      - name: total_revenue
        description: "Sum of order amounts for the month."
        tests:
          - not_null
```

---

That's it for the placeholder. Delete everything from *Why I'm writing this* onward and replace with your real content.
