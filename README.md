## **Mexico Toy Sales – SQL Project Report**
 1. **Project Overview**

This project analyzes toy sales data from stores across Mexico using SQL. The database consists of five main tables: Products, Stores, Sales, Inventory, and Calendar. The goal is to answer business-driven questions about product performance, revenue generation, store operations, and stock management.

Through SQL queries, we uncover insights that can help toy retailers improve their sales strategy, manage inventory effectively, and identify top-performing cities and categories.

 2. **Objectives**

• Identify top-selling toy products and categories.

• Measure sales revenue performance by store, city, and region.

• Detect peak sales periods by analyzing monthly trends.

• Evaluate store distribution and opening timelines.

• Assess inventory availability to avoid stockouts.

3. **Database Schema**

• **Products:** Product details (ID, name, category, cost, price).

• **Stores:** Store details (ID, name, city, location, opening date).

• **Sales:** Transaction records (sale ID, date, product, store, units sold).

• **Inventory:** Stock availability by store and product.

• **Calendar:** Date dimension table for time-based analysis.

 4. **Analysis & Queries**

Q1. Top 10 Best-Selling Products

    SELECT p.product_name, SUM(s.units) AS sales_quantity
    FROM products p
    JOIN sales s ON p.product_id = s.product_id 
    GROUP BY p.product_name
    ORDER BY sales_quantity DESC
    LIMIT 10;

Q2. Total Revenue by Store

    SELECT st.store_name, SUM(s.units * p.product_price) AS total_revenue
    FROM products p
    JOIN sales s ON p.product_id = s.product_id 
    JOIN stores st ON st.store_id = s.store_id 
    GROUP BY st.store_name
    ORDER BY total_revenue DESC;

Q3. Monthly Sales Trends

    SELECT p.product_name,
           TO_CHAR(s.date, 'YYYY-MON') AS month,
           SUM(s.units) AS total_sales
    FROM products p
    JOIN sales s ON p.product_id = s.product_id 
    GROUP BY p.product_name, TO_CHAR(s.date, 'YYYY-MON')
    ORDER BY total_sales DESC;

Q4. City with Highest Sales Revenue

    SELECT st.store_city, SUM(s.units * p.product_price) AS sales_revenue
    FROM sales s
    JOIN products p ON s.product_id = p.product_id
    JOIN stores st ON s.store_id = st.store_id 
    GROUP BY st.store_city 
    ORDER BY sales_revenue DESC
    LIMIT 1;

Q5. Contribution by Product Category

    SELECT p.product_category,
           ROUND((SUM(s.units * p.product_price) * 100.0) /
                 (SELECT SUM(s2.units * p2.product_price)
                  FROM sales s2
                  JOIN products p2 ON s2.product_id = p2.product_id), 2) 
           AS contribution_percentage
    FROM sales s
    JOIN products p ON s.product_id = p.product_id
    GROUP BY p.product_category
    ORDER BY contribution_percentage DESC;

Q6. Top 5 Cities by Sales Revenue

    SELECT st.store_city, SUM(s.units * p.product_price) AS total_revenue
    FROM sales s
    JOIN products p ON s.product_id = p.product_id
    JOIN stores st ON s.store_id = st.store_id
    GROUP BY st.store_city
    ORDER BY total_revenue DESC
    LIMIT 5;

Q7. Average Product Price by Category

    SELECT product_category , ROUND(AVG(product_price),2) AS avg_price 
    FROM products
    GROUP BY product_category;

Q8. Store Count by City

    SELECT store_city, COUNT(store_city) AS num_of_stores
    FROM stores
    GROUP BY store_city
    ORDER BY num_of_stores DESC;

Q9. Earliest and Latest Store Openings

    SELECT MIN(store_open_date) AS earliest_open_date, 
           MAX(store_open_date) AS latest_open_date 
    FROM stores;

Q10. Top 5 Products by Stock Availability

    SELECT p.product_name, i.stock_on_hand
    FROM products p
    JOIN inventory i ON p.product_id = i.product_id
    ORDER BY i.stock_on_hand DESC 
    LIMIT 5;


 5. **Conclusion**

This SQL project demonstrates how sales and store data can be transformed into actionable insights.

• **Product-Level Insights:** Identified best-sellers and top stock items.

• **Revenue Insights:** Ranked cities and stores by revenue contribution.

• **Time Insights:** Tracked monthly trends and store opening timelines.

• **Category Insights:** Analyzed average prices and sales contribution.
