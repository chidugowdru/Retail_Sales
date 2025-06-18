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
