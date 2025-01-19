# AMAZON-sales-analysis-using-SQL
Image
### SQL Query 
## Business ProbLems 
## 1. Top Selling Products 
- Query the top 10 products by total sales value.
- Challenge: Include product name, total quantity sold, and total sales value.
```SQL
-- Create a new column 
ALTER TABLE dbo.[order_items]
ADD [total_sale] FLOAT;

-- Update the column with total sales value
UPDATE dbo.[order_items]
SET [total_sale] = [quantity] * [price_per_unit];

-- Retrieve the top 10 products by total sales value
SELECT TOP 10 
    oi.[product_id], 
    p.[product_name],
    SUM(oi.[quantity]) AS [total_quantity_sold],
    SUM(oi.[total_sale]) AS [total_sales_value]
FROM dbo.[orders] AS o
JOIN dbo.[order_items] AS oi
    ON o.[order_id] = oi.[order_id]
JOIN dbo.[products] AS p
    ON oi.[product_id] = p.[product_id]
GROUP BY oi.[product_id], p.[product_name]
ORDER BY SUM(oi.[total_sale]) DESC;
```
## 2. Revenue by Category
- Calculate total revenue generated by each product category.
- Challenge: Include the percentage contribution of each category to total revenue.
```sql
SELECT 
    p.[category_id],
    c.[category_name],
    SUM(oi.[total_sale]) AS [total_sale],
    SUM(oi.[total_sale]) / 
    (SELECT SUM([total_sale]) FROM dbo.[order_items]) * 100 AS [contribution_percentage]
FROM dbo.[order_items] AS oi
JOIN dbo.[products] AS p
    ON p.[product_id] = oi.[product_id]
LEFT JOIN dbo.[category] AS c
    ON c.[category_id] = p.[category_id]
GROUP BY 
    p.[category_id],
    c.[category_name]
ORDER BY SUM(oi.[total_sale]) DESC;
```
## 3. Average Order Value (AOV)
- Compute the average order value for each customer.
- Challenge: Include only customers with more than 5 orders
```sql
SELECT 
    c.[customer_id],
    CONCAT(c.[first_name], ' ', c.[last_name]) AS [full_name],
    SUM(oi.[total_sale]) / COUNT(o.[order_id]) AS [AOV],
    COUNT(o.[order_id]) AS [total_orders]
FROM dbo.[orders] AS o
JOIN dbo.[customers] AS c
    ON c.[customer_id] = o.[customer_id]
JOIN dbo.[order_items] AS oi
    ON oi.[order_id] = o.[order_id]
GROUP BY 
    c.[customer_id], 
    c.[first_name], 
    c.[last_name]
HAVING COUNT(o.[order_id]) > 5;
```
