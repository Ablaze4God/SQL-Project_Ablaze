What issues will you address by cleaning the data?


1. Some columns on some of the tables have only Null Values. I will need to identify and exclude columns that are entirely NULL.
2. Some tables had missing values which would be problematic for analysis. I will need to update those with either interpolated values or zero (0).
3. Inconsistent Data Types:I need to ensure that data types are appropriate for their respective columns. 
4. Formatting Issues: Cleaning data involves standardizing the formatting for consistency.
   



Queries:
Below, provide the SQL queries you used to clean your data.


-- DATA CLEANING

-- A. sales_report table
-- 1. View table and check that all data on the CSV was imported successfully.
SELECT * 
FROM sales_report

-- 2. Count distinct values in productSKU column to check if unique & is not null for primary key.
-- a. Count and count distinct should equal each other.
SELECT 
	COUNT("productSKU") as total_productSKU, 
	COUNT(DISTINCT "productSKU") as total_unique_productSKU
FROM sales_report

-- b. Check if any other column (apart from the primary key column) has null values
SELECT *
FROM sales_report
WHERE total_ordered is NULL 
	OR "name" is NULL 
	OR "stockLevel" is NULL
	OR "restockingLeadTime" is NULL
	OR "sentimentScore" is NULL
	OR "sentimentMagnitude" is NULL
	OR ratio is NULL
	
-- Since ratio is actually total_ordered / stockLevel, 
-- Null values in the ratio column are justified as 0 / 0 is undefined. (Null = undefined)



-- B. sales_by_sku table
-- 1. View table and check that all data on the CSV was imported successfully.
SELECT * 
FROM sales_by_sku

-- 2. Count distinct values in productSKU column to check if unique & is not null for primary key.
-- a. Count and count distinct should equal each other.
SELECT 
	COUNT("productSKU") as total_productSKU, 
	COUNT(DISTINCT "productSKU") as total_unique_productSKU
FROM sales_by_sku

-- b. Check if any other column (apart from the primary key column) has null values
SELECT *
FROM sales_by_sku
WHERE total_ordered is NULL 
	
-- I don't think there is any cleaning to be done here. Data type correct, no null values.


-- C. products table
-- 1. View table and check that all data on the CSV was imported successfully.
SELECT * 
FROM products

-- 2. Rename Primary Column Name (SKU) to match the same primary key names 
-- for the other tables (sales_by_sku and sales_report)

ALTER TABLE products
RENAME COLUMN "SKU" TO "productSKU"

-- b. Check that update was successful.
SELECT * 
FROM products

-- 3. Count distinct values in productSKU column to check if unique & is not null for primary key.
-- a. Count and count distinct should equal each other.
SELECT 
	COUNT("productSKU") as total_productSKU, 
	COUNT(DISTINCT "productSKU") as total_unique_productSKU
FROM products

-- b. Check if any other column (apart from the primary key column) has null values
SELECT *
FROM products
WHERE "name" is NULL
	OR "orderedQuantity" is NULL 
	OR "stockLevel" is NULL
	OR "restockingLeadTime" is NULL
	OR "sentimentScore" is NULL
	OR "sentimentMagnitude" is NULL
	
-- 4. Assign 0.0 to sentimentScore and sentimentMagnitude columns that have null valaues 
-- for the first row with Primary Key (SKU) = GGADFBSBKS42347
UPDATE products
SET "sentimentScore" = 0.0, "sentimentMagnitude" = 0.0
WHERE "productSKU" = 'GGADFBSBKS42347';

-- 5. Check that update was successful.
SELECT *
FROM products
WHERE "productSKU" = 'GGADFBSBKS42347';

-- 6. Final table.
SELECT *
FROM products



-- D. analytics table
-- 1. View table and check that all data on the CSV was imported successfully.
SELECT * 
FROM analytics

-- 2. Reviewed the data on the table, 
-- no column has data that is unique (i.e. no column available for use as primary key).

-- 3. Check for null columns (i.e columns where all contained data are null values)
-- Total count of rows for the table is 4301122, any columns that has this number of nulls is a null column 

SELECT 
COUNT(CASE WHEN userid IS NULL THEN 1 END) AS null_count_userid,
COUNT(CASE WHEN units_sold IS NULL THEN 1 END) AS null_count_units_sold,
COUNT(CASE WHEN timeonsite IS NULL THEN 1 END) AS null_count_timeonsite,
COUNT(CASE WHEN revenue IS NULL THEN 1 END) AS null_count_revenue

FROM analytics

-- From results, userid column is entirely null, units_sold column is 98% null values 
-- and revenue column is 99.6% null values. Suggest this columns are not useful and should be removed.
-- Create a view for this table in this case that excludes the columns: userid, units_sold and revenue.

CREATE VIEW analytics_view AS
SELECT "visitNumber", "visitId", "visitStartTime", date, "fullvisitorId", "channelGrouping", 
	"socialEngagementType", pageviews, timeonsite, bounces, unit_price
FROM analytics

--4. Reviewed the distinct data on the table view named analytics_view. 
-- Always call this distinct view to do any analysis 
SELECT Distinct *
FROM analytics_view



-- E. all_sessions table
-- 1. View table and check that all data on the CSV was imported successfully.
SELECT * 
FROM all_sessions

-- 2. Reviewed the data on the table, 
-- no column has data that is unique (i.e. no column available for use as primary key).

-- 3. Check for null columns (i.e columns where all contained data are null values)
-- Total count of rows for the table is 15134, any columns that has this number of nulls is a null column 

SELECT 
COUNT(CASE WHEN "totalTransactionRevenue" IS NULL THEN 1 END) AS null_count_totalTransactionRevenue,
COUNT(CASE WHEN transactions IS NULL THEN 1 END) AS null_count_transactions,
COUNT(CASE WHEN "sessionQualityDim" IS NULL THEN 1 END) AS null_count_sessionQualityDim,
COUNT(CASE WHEN "productRefundAmount" IS NULL THEN 1 END) AS productRefundAmount,
COUNT(CASE WHEN "productQuantity" IS NULL THEN 1 END) AS null_count_productQuantity,
COUNT(CASE WHEN "productRevenue" IS NULL THEN 1 END) AS productRevenue,
COUNT(CASE WHEN "itemQuantity" IS NULL THEN 1 END) AS itemQuantity,
COUNT(CASE WHEN "itemRevenue" IS NULL THEN 1 END) AS itemRevenue,
COUNT(CASE WHEN "transactionRevenue" IS NULL THEN 1 END) AS transactionRevenue,
COUNT(CASE WHEN "transactionId" IS NULL THEN 1 END) AS transactionId,
COUNT(CASE WHEN "searchKeyword" IS NULL THEN 1 END) AS searchKeyword,
COUNT(CASE WHEN "eCommerceAction_option" IS NULL THEN 1 END) AS eCommerceAction_option

FROM all_sessions

-- From results, productRefundAmount, itemQuantity, itemRevenue columns are entirely null, productRevenue column is 99.9% null values 
-- Suggest these columns are not useful and should be removed.
-- Create a view for this table in this case that excludes the columns stated above.

CREATE VIEW all_sessions_view AS
SELECT "fullVisitorId", "channelGrouping", time, country, city,"totalTransactionRevenue", transactions, 
"timeOnSite",  pageviews, "sessionQualityDim", date, "visitId", "type", "productQuantity",
"productPrice", "productSKU", "v2ProductName", "v2ProductCategory", "productVariant", 
"currencyCode", "transactionRevenue", "transactionId", "pageTitle",
"searchKeyword", "pagePathLevel1", "eCommerceAction_type", "eCommerceAction_step", "eCommerceAction_option"
FROM all_sessions

--4. Reviewed the distinct data on the table view named all_sessions_view. 
-- Always call this distinct view to do any analysis 
SELECT Distinct *
FROM all_sessions_view

-- I could other Data cleaning like setting "not available in demo dataset" under city to NULL, 
-- setting "(not set)" under "productVariant" to NULL, 
-- making visitId and fullVisitorID as NUMERIC in millions etc.
