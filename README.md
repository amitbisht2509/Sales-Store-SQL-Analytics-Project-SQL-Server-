**🛒 Sales Store – SQL Analytics Project (SQL Server)**

**📘 Introduction**

This project delivers a complete **SQL Server**–based analysis of a retail store’s sales dataset.
The objective is to convert raw transactional data into meaningful insights centered around product performance, customer behavior, payment patterns, cancellations, and operational efficiency.

All insights are generated using optimized T-SQL queries running in Microsoft SQL Server. To see whole project click **sqlproject1.sql** file 

**🧩 Project Background**

Although the store records daily sales transactions, it lacks clarity on key business metrics:

#1 Which products sell the most.

#2 Who are the highest-value customers.

#3 Which categories generate the most revenue.

#4 Which products/category have high cancellation rates.

#5 Payment mode usage patterns.

#6 Peak purchasing times

#7 Customer demographic insights.



**❗ Business Problems & SQL Solutions**

Each problem below includes the SQL Server query used to solve it.

1️⃣ Problem: No visibility of top-selling products


✅ Query: Top 5 Best-Selling Products (by quantity)

```sql
SELECT TOP 5 
    p.product_name,
    SUM(o.quantity) AS total_quantity_sold
FROM orders o
JOIN products p ON o.product_id = p.product_id
WHERE o.order_status = 'Completed'
GROUP BY p.product_name
ORDER BY total_quantity_sold DESC;
```


2️⃣ Problem: Store doesn't know which products get cancelled the most
✅ Query: Most Frequently Cancelled Products
SELECT 
    p.product_name,
    COUNT(*) AS cancel_count
FROM orders o
JOIN products p ON o.product_id = p.product_id
WHERE o.order_status = 'Cancelled'
GROUP BY p.product_name
ORDER BY cancel_count DESC;

3️⃣ Problem: No clarity on top-spending customers
✅ Query: Top 5 Highest Spending Customers
SELECT TOP 5
    c.customer_name,
    SUM(o.quantity * o.price) AS total_spend
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE o.order_status = 'Completed'
GROUP BY c.customer_name
ORDER BY total_spend DESC;

4️⃣ Problem: No understanding of category revenue
✅ Query: Revenue by Product Category
SELECT
    p.category,
    SUM(o.quantity * o.price) AS total_revenue
FROM orders o
JOIN products p ON o.product_id = p.product_id
WHERE o.order_status = 'Completed'
GROUP BY p.category
ORDER BY total_revenue DESC;

5️⃣ Problem: High returns/cancellations but store doesn't know where
✅ Query: Cancellation Rate by Category
SELECT
    p.category,
    (SUM(CASE WHEN o.order_status = 'Cancelled' THEN 1 ELSE 0 END) * 100.0) 
        / COUNT(*) AS cancellation_rate
FROM orders o
JOIN products p ON o.product_id = p.product_id
GROUP BY p.category
ORDER BY cancellation_rate DESC;

6️⃣ Problem: No insight into preferred payment modes
✅ Query: Most Preferred Payment Mode
SELECT 
    payment_mode,
    COUNT(*) AS usage_count
FROM payments
GROUP BY payment_mode
ORDER BY usage_count DESC;

7️⃣ Problem: Age group buying patterns not known
✅ Query: Sales by Age Group
SELECT 
    CASE 
        WHEN c.age BETWEEN 18 AND 25 THEN '18-25'
        WHEN c.age BETWEEN 26 AND 35 THEN '26-35'
        WHEN c.age BETWEEN 36 AND 50 THEN '36-50'
        ELSE '50+'
    END AS age_group,
    SUM(o.quantity * o.price) AS total_sales
FROM customers c
JOIN orders o ON o.customer_id = c.customer_id
WHERE o.order_status = 'Completed'
GROUP BY 
    CASE 
        WHEN c.age BETWEEN 18 AND 25 THEN '18-25'
        WHEN c.age BETWEEN 26 AND 35 THEN '26-35'
        WHEN c.age BETWEEN 36 AND 50 THEN '36-50'
        ELSE '50+'
    END
ORDER BY total_sales DESC;

8️⃣ Problem: No monthly trend visibility
✅ Query: Monthly Sales Trend
SELECT 
    FORMAT(order_date, 'yyyy-MM') AS month,
    SUM(quantity * price) AS monthly_sales
FROM orders
WHERE order_status = 'Completed'
GROUP BY FORMAT(order_date, 'yyyy-MM')
ORDER BY month;

9️⃣ Problem: No gender-based buying insights
✅ Query: Gender-wise Category Preference
SELECT 
    c.gender,
    p.category,
    SUM(o.quantity) AS units_purchased
FROM orders o
JOIN customers c ON c.customer_id = o.customer_id
JOIN products p ON p.product_id = o.product_id
WHERE o.order_status = 'Completed'
GROUP BY c.gender, p.category
ORDER BY units_purchased DESC;

🔟 Problem: Store doesn't know peak shopping hours
✅ Query: Peak Purchase Hours
SELECT 
    DATEPART(HOUR, order_time) AS hour_of_day,
    COUNT(*) AS total_orders
FROM orders
WHERE order_status = 'Completed'
GROUP BY DATEPART(HOUR, order_time)
ORDER BY total_orders DESC;

**💼 Key Analytical Insights (Summary)**

Based on the SQL analysis:

Certain products dominate overall sales volume

Some categories show high cancellation rates, impacting revenue

A small set of customers generate a large % of total sales

Payment modes vary by customer demographic

Evening hours show significantly higher order volume

Certain age groups (26–35 typically) contribute most revenue

Gender influences purchasing category choices

Monthly trends show seasonal spikes that can be leveraged

**💡 Business Recommendations (How Store Can Increase Revenue)**

These recommendations translate SQL insights into actionable strategy:

1. Prioritize Stocking Top-Selling Products

Avoid stockouts → directly increases sales volume.

2. Fix High-Cancellation Items

Investigate:

Delivery delays

Poor product quality

Wrong item images/descriptions

Incorrect pricing

Reducing cancellations = instant profit improvement.

3. Launch Loyalty Programs for High-Value Customers

Offer:

Cashback

Exclusive discounts

Early access to sales

Boosts repeat purchases.

4. Promote High-Revenue Categories

Run:

Bundled deals

Category-specific offers

Sponsored ads

5. Improve Low-Performing Categories

Check:

Inventory issues

Low visibility

Price competitiveness

Customer feedback

6. Time-Based Discounting

Example:

High peak in evenings → run flash deals

Low morning sales → introduce morning offers

7. Personalize Marketing by Age Group

18–25 → gadgets, fashion
26–35 → home & lifestyle
36–50 → family essentials

8. Encourage Digital Payments

If COD is high →
Offer “5% discount on prepaid orders” to reduce delivery failures.
