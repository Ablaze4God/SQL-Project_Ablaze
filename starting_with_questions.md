Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

-- Answering the questions under "starting with questions"
-- 1. Question 1: Which cities and countries have the highest level of transaction revenues on the site?
-- View the 5 tables to see which ones have the data we need.
SELECT * 
FROM sales_report

SELECT *
FROM sales_by_sku

SELECT *
FROM products

SELECT Distinct *
FROM analytics_view

SELECT Distinct *
FROM all_sessions_view

-- answering question #1 using a CTE to ensure I have only distinct rows in the view (all_sessions_view)
WITH cte_all_sessions_view AS (
    SELECT Distinct *
	FROM all_sessions_view
)
SELECT country, city, SUM(CAST("transactionRevenue" AS double precision))
FROM cte_all_sessions_view
GROUP BY country, city
ORDER BY SUM(CAST("transactionRevenue" AS double precision)) ASC



Answer:

Country = United States
City: Sunnyvale


![image](https://github.com/Ablaze4God/SQL-Project_Ablaze/assets/143281877/c6d8d651-cdd1-4e2e-8aea-ef2400f21776)



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
-- 2. Question #2: What is the average number of products ordered from visitors in each city and country?
-- View the 5 tables to see which ones have the data we need.
SELECT * 
FROM sales_report


SELECT DISTINCT *
FROM sales_by_sku

SELECT *
FROM products

SELECT Distinct *
FROM analytics_view

SELECT Distinct *
FROM all_sessions_view

-- answering question #2 using a CTE to ensure I have only distinct rows in the view (all_sessions_view)
WITH cte_all_sessions_view AS (
    SELECT Distinct *
	FROM all_sessions_view
)

SELECT country, city, AVG(CAST("orderedQuantity" AS double precision))
FROM products
JOIN cte_all_sessions_view USING ("productSKU")
WHERE country != '(not set)'        -- Filtering out the rows that have country as (not set) 
GROUP BY country, city
ORDER BY country




Answer:
Answer in the image below

![image](https://github.com/Ablaze4God/SQL-Project_Ablaze/assets/143281877/ab5674ae-74bd-44cb-8855-0492a7ecb119)



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:


-- 3. Question #3: Is there any pattern in the types (product categories) of 
-- products ordered from visitors in each city and country?
-- View the 5 tables to see which ones have the data we need.
SELECT * 
FROM sales_report


SELECT DISTINCT *
FROM sales_by_sku

SELECT *
FROM products

SELECT Distinct *
FROM analytics_view

SELECT Distinct *
FROM all_sessions_view

-- answering question #3 using a CTE to ensure I have only distinct rows in the view (all_sessions_view)
WITH cte_all_sessions_view AS (
    SELECT Distinct *
	FROM all_sessions_view
)

SELECT country, city, "v2ProductCategory", SUM(CAST("orderedQuantity" AS double precision))
FROM products
JOIN cte_all_sessions_view USING ("productSKU")
WHERE country != '(not set)'        -- Filtering out the rows that have country as (not set) 
GROUP BY country, city, "v2ProductCategory"
ORDER BY country




Answer:

Answer in the image / snapshot below

![image](https://github.com/Ablaze4God/SQL-Project_Ablaze/assets/143281877/be6135e7-5067-4f13-ba01-dccfa167e908)





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

-- 4. Question #4: What is the top-selling product from each city/country? 
-- Can we find any pattern worthy of noting in the products sold?
-- View the 5 tables to see which ones have the data we need.
SELECT * 
FROM sales_report


SELECT DISTINCT *
FROM sales_by_sku

SELECT *
FROM products

SELECT Distinct *
FROM analytics_view

SELECT Distinct *
FROM all_sessions_view

-- answering question #4 using 1 CTE to ensure I have only distinct rows in the view (all_sessions_view)

WITH cte_all_sessions_view AS (
    SELECT Distinct *
	FROM all_sessions_view
)

SELECT country, city, "v2ProductName", SUM(CAST("orderedQuantity" AS double precision)) as sum_orderedQty
FROM products
JOIN cte_all_sessions_view USING ("productSKU")
WHERE country != '(not set)'        -- Filtering out the rows that have country as (not set) 
GROUP BY country, city, "v2ProductName"
ORDER BY country, city, SUM(CAST("orderedQuantity" AS double precision)) DESC
-- I would have loved to delinate only the max value for sum_orderedQty
-- per group by using the MAX function with SUM function but it doesn't allow me 
-- perform a nested aggregrate function)


Answer:

Answer shown in image / snap shot below
![image](https://github.com/Ablaze4God/SQL-Project_Ablaze/assets/143281877/4b14edd4-77cc-463d-b4df-7fa98eefd26e)





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:


-- 5. Question #5: Can we summarize the impact of revenue generated from each city/country?
-- View the 5 tables to see which ones have the data we need.
SELECT * 
FROM sales_report

SELECT DISTINCT *
FROM sales_by_sku

SELECT *
FROM products

SELECT Distinct *
FROM analytics_view

SELECT Distinct *
FROM all_sessions_view

-- checking what the total sum value of totalTransactionRevenue is
SELECT SUM(CAST("totalTransactionRevenue" AS  double precision))
	FROM all_sessions_view
-- total sum value of totalTransactionRevenue = 14281310000
-- answering question #5 using 1 CTE to ensure I have only distinct rows in the view (all_sessions_view)

WITH cte_all_sessions_view AS (
    SELECT Distinct *
	FROM all_sessions_view
)

SELECT 
	country, 
	city, 
	SUM(CAST("orderedQuantity" AS double precision)) as sum_orderedQty,
	(((CAST("totalTransactionRevenue" AS double precision )) / (CAST(14281310000 AS double precision))) * 100) AS percent_of_total_revenue
FROM products
JOIN cte_all_sessions_view USING ("productSKU")
WHERE country != '(not set)'        -- Filtering out the rows that have country as (not set)
	AND (((CAST("totalTransactionRevenue" AS double precision )) / (CAST(14281310000 AS double precision))) * 100) IS NOT NULL
GROUP BY country, city, "totalTransactionRevenue"
ORDER BY country, city


Answer:

Answer in the snapshot below
![image](https://github.com/Ablaze4God/SQL-Project_Ablaze/assets/143281877/2706db8b-a6cf-4e2a-972b-7a5f70998433)






