
# 📊 Retail Sales Analysis SQL Project

![SQL](https://img.shields.io/badge/SQL-PostgreSQL-blue)
![Project Level](https://img.shields.io/badge/Level-Beginner-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 📌 Project Overview

This project demonstrates how SQL can be used to analyze retail sales data, clean datasets, and extract meaningful business insights. It simulates a real-world retail environment where transaction-level data is used to understand customer behavior, sales trends, and category performance.

---

## 🎯 Objectives

- Design and create a relational database
- Clean and validate raw data
- Perform exploratory data analysis (EDA)
- Answer business-driven questions using SQL
- Generate insights for decision-making

---

## 🗂️ Dataset Description

The dataset represents retail transactions with the following attributes:

- Transaction ID  
- Sale Date & Time  
- Customer ID  
- Gender & Age  
- Product Category  
- Quantity Purchased  
- Price Per Unit  
- Cost of Goods Sold (COGS)  
- Total Sale  

---

## 🏗️ Database Setup

```sql
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

---

## 🧹 Data Cleaning

- Removed records with NULL values
- Ensured completeness of key fields
- Validated dataset integrity before analysis

```sql
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

---

## 🔍 Key Analysis Queries

### 1. Sales on a Specific Date
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

### 2. Clothing Purchases in November 2022
```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
AND quantiy >= 4;
```

### 3. Total Sales by Category
```sql
SELECT category,
       SUM(total_sale) AS net_sale,
       COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

### 4. Average Age of Beauty Customers
```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

### 5. High-Value Transactions
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

### 6. Transactions by Gender and Category
```sql
SELECT category,
       gender,
       COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender;
```

### 7. Best Performing Month per Year
```sql
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

### 8. Top 5 Customers by Sales
```sql
SELECT customer_id,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

### 9. Unique Customers per Category
```sql
SELECT category,
       COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

### 10. Sales by Time of Day (Shift Analysis)
```sql
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

---

## 📈 Key Insights

- Certain product categories generate higher revenue than others  
- A small group of customers contributes significantly to total sales  
- High-value transactions indicate premium purchasing behavior  
- Sales patterns vary across months and time of day  
- Customer demographics influence purchasing decisions  

---

## 🧠 Skills Demonstrated

- SQL Database Design  
- Data Cleaning Techniques  
- Aggregations & Grouping  
- Window Functions (RANK)  
- Date & Time Functions  
- Conditional Logic (CASE WHEN)  
- Exploratory Data Analysis (EDA)  

---

## 📷 Project Preview

> Add screenshots of your queries and results here for better presentation.

---

## 🚀 How to Use

1. Create the database using the provided SQL script  
2. Run the table creation query  
3. Insert or import your dataset  
4. Execute the analysis queries  
5. Modify queries to explore further insights  

---

## 📌 Conclusion

This project showcases how raw transactional data can be transformed into meaningful insights using SQL. It highlights patterns in customer behavior, sales performance, and business operations that can support data-driven decision-making.

---

## 🤝 Author

**Kolapo Newton**  
Data Analyst | SQL | Data Engineering Enthusiast  

---

