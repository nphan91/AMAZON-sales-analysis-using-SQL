# AMAZON-sales-analysis-using-SQL
Image
### SQL Query 
## Business Prob;ems 
## 1. Top Selling Products 
-- Query the top 10 products by total sales value. 
-- Challenge: Incude product name, total quantity sold, and total sales value.
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
