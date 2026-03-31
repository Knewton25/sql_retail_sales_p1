# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis\
**Level**: Beginner\
**Database**: `sql_project_p1`

This project demonstrates practical SQL skills used in data analysis,
including database creation, data cleaning, exploratory data analysis
(EDA), and business-oriented querying. The dataset represents retail
transactions and is used to extract insights about customers, sales
performance, product categories, and purchasing behavior.

------------------------------------------------------------------------

## Objectives

-   Create and structure a retail sales database
-   Clean and validate the dataset
-   Perform exploratory data analysis (EDA)
-   Answer real-world business questions using SQL
-   Extract actionable insights from sales data

------------------------------------------------------------------------

## Project Structure

### 1. Database Setup

A database named `sql_project_p1` was created, along with a
`retail_sales` table to store transaction-level data.

Each record represents a single transaction with attributes such as:

-   Transaction ID
-   Sale date and time
-   Customer ID
-   Gender and age
-   Product category
-   Quantity purchased
-   Price per unit
-   Cost of goods sold (COGS)
-   Total sale amount

``` sql
CREATE DATABASE sql_project_p1;

CREATE TABLE retail_sales(
     transactions_id INT PRIMARY KEY,
     sale_date DATE,
     sale_time TIME,
     customer_id INT,
     gender VARCHAR(15),
     age INT,
     category VARCHAR(15),
     quantiy INT,
     price_per_unit FLOAT,
     cogs FLOAT,
     total_sale FLOAT
);
```

------------------------------------------------------------------------

### 2. Data Exploration & Cleaning

Initial exploration was performed to understand dataset size and
structure, followed by cleaning to ensure data quality.

``` sql
SELECT COUNT(*) FROM retail_sales;

SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

SELECT *
FROM retail_sales
WHERE age IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;

DELETE FROM retail_sales
WHERE age IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

------------------------------------------------------------------------

### 3. Data Analysis & Business Questions

#### Retrieve sales on a specific date

``` sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

#### Clothing purchases in November 2022 with quantity ≥ 4

``` sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
AND quantiy >= 4;
```

#### Total sales and orders per category

``` sql
SELECT category,
       SUM(total_sale) AS net_sale,
       COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

#### Average age of Beauty category customers

``` sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

#### High-value transactions

``` sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

#### Transactions by gender and category

``` sql
SELECT category,
       gender,
       COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender;
```

#### Best selling month per year

``` sql
SELECT year, month, avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) t1
WHERE rank = 1;
```

#### Top 5 customers

``` sql
SELECT customer_id,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

#### Unique customers per category

``` sql
SELECT category,
       COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

#### Sales by time of day

``` sql
WITH hourly_sale AS (
    SELECT *,
           CASE
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
               ELSE 'Evening'
           END AS shift
    FROM retail_sales
)

SELECT shift,
       COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

------------------------------------------------------------------------

## Key Findings

-   Sales are distributed across multiple product categories.
-   High-value transactions (\>1000) indicate premium purchases.
-   Customer demographics influence category purchases.
-   Monthly trends reveal variations in sales performance.
-   A small group of customers contributes significantly to revenue.
-   Transaction activity varies by time of day.

------------------------------------------------------------------------

## Conclusion

This project demonstrates how SQL can be used to transform raw
transactional data into meaningful business insights. Through structured
querying, aggregation, and analysis, it highlights customer behavior,
sales performance, and operational patterns that support data-driven
decision-making.
