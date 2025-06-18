# Retail Sales Analysis with SQL

## Project Overview

This project focuses on analyzing retail sales data using SQL to derive meaningful insights into sales performance, customer behavior, and inventory trends. By establishing a normalized database schema and running targeted queries, we can understand key business metrics and identify areas for improvement.

## Objective

The primary objective is to:
* *Analyze Sales Performance*: Understand total sales, sales trends over time, and revenue growth.
* *Understand Customer Behavior*: Identify top customers and their spending habits.
* *Gain Product Insights*: Determine best-selling products and manage inventory effectively.
* *Evaluate Store Performance*: Compare sales across different store locations.
* *Improve Inventory Management*: Identify products with low stock to prevent stockouts.

## Database Schema Design

The project uses a normalized relational database schema consisting of four interconnected tables:

### 1. Customers
Stores information about individual customers.
* customer_id (INT, PK)
* name (TEXT)
* gender (TEXT)
* age (INT)
* email (TEXT, UNIQUE)
* city (TEXT)

### 2. Products
Contains details about each product sold.
* product_id (INT, PK)
* name (TEXT)
* category (TEXT)
* price (FLOAT)
* cost (FLOAT)
* stock_quantity (INT)

### 3. Stores
Holds information about the retail store locations.
* store_id (INT, PK)
* name (TEXT)
* location (TEXT)

### 4. Sales
Records each individual sales transaction, linking customers, products, and stores.
* sale_id (INT, PK)
* customer_id (INT, FK to Customers)
* product_id (INT, FK to Products)
* store_id (INT, FK to Stores)
* sale_date (DATE)
* quantity (INT)
* total_price (FLOAT)

## Sample SQL Queries and Analysis

The project includes SQL queries to perform various analyses:

### A. Sales Performance
*Query: Total sales per month*
```sql
SELECT
    DATE_TRUNC('month', sale_date) AS month,
    SUM(total_price) AS total_sales
FROM Sales
GROUP BY month
ORDER BY month;
# Retail Sales SQL Queries

## 1. Sales on Specific Date
sql
SELECT * 
FROM sales 
WHERE sale_date = '2022-11-05';


## 2. Clothing Category & Quantity > 4 in Nov 2022
sql
SELECT * 
FROM sales 
WHERE category = 'clothing' 
  AND quantity > 4 
  AND sale_date BETWEEN '2022-11-01' AND '2022-11-30';


## 3. Total Sales for Each Category
sql
SELECT category, SUM(total_sale) AS total_sales 
FROM sales 
GROUP BY category;


## 4. Average Age of Customers Who Purchased Beauty Products
sql
SELECT AVG(c.age) AS avg_age 
FROM customers c
JOIN sales s ON c.customer_id = s.customer_id
WHERE s.category = 'Beauty';


## 5. Transactions With Total Sale > 1000
sql
SELECT * 
FROM sales 
WHERE total_sale > 1000;


## 6. Total Number of Transactions by Gender in Each Category
sql
SELECT c.gender, s.category, COUNT(s.transaction_id) AS total_transactions
FROM sales s
JOIN customers c ON s.customer_id = c.customer_id
GROUP BY c.gender, s.category;


## 7. Average Sale Per Month & Best Selling Month Each Year
sql
-- Average sale per month
SELECT DATE_TRUNC('month', sale_date) AS month, AVG(total_sale) AS avg_monthly_sale
FROM sales
GROUP BY month;

-- Best selling month in each year
SELECT year, month, monthly_sale FROM (
  SELECT 
    EXTRACT(YEAR FROM sale_date) AS year,
    TO_CHAR(sale_date, 'Month') AS month,
    SUM(total_sale) AS monthly_sale,
    RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY SUM(total_sale) DESC) AS rnk
  FROM sales
  GROUP BY year, month
) ranked_sales
WHERE rnk = 1;


## 8. Top 5 Customers by Total Sales
sql
SELECT customer_id, SUM(total_sale) AS total_spent 
FROM sales 
GROUP BY customer_id 
ORDER BY total_spent DESC 
LIMIT 5;


## 9. Unique Customers Per Category
sql
SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM sales 
GROUP BY category;


## 10. Shift-wise Orders (Morning, Afternoon, Evening)
sql
SELECT 
  CASE 
    WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
    WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
    ELSE 'Evening' 
  END AS shift, 
  COUNT(order_id) AS orders_count
FROM sales
GROUP BY shift;


## 11. Total Sales in a Specific Time Period
sql
SELECT SUM(total_sale) AS total_sales 
FROM sales 
WHERE sale_date BETWEEN '2023-01-01' AND '2023-03-31';


## 12. Best Selling Products
sql
SELECT product_id, SUM(quantity) AS total_sold 
FROM sales 
GROUP BY product_id 
ORDER BY total_sold DESC 
LIMIT 10;


## 13. Seasonal Sales Trends
sql
SELECT 
  CASE 
    WHEN EXTRACT(MONTH FROM sale_date) IN (12,1,2) THEN 'Winter'
    WHEN EXTRACT(MONTH FROM sale_date) IN (3,4,5) THEN 'Spring'
    WHEN EXTRACT(MONTH FROM sale_date) IN (6,7,8) THEN 'Summer'
    ELSE 'Fall' 
  END AS season, 
  SUM(total_sale) AS seasonal_sales
FROM sales 
GROUP BY season;


## 14. Impact of Discounts on Sales
sql
SELECT discount_percent, AVG(total_sale) AS avg_sale 
FROM sales 
GROUP BY discount_percent 
ORDER BY discount_percent;


## 15. Low Selling Products
sql
SELECT product_id, SUM(quantity) AS total_sold 
FROM sales 
GROUP BY product_id 
ORDER BY total_sold ASC 
LIMIT 10;


