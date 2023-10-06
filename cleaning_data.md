--What issues will you address by cleaning the data?--
-- 1.> Divide unit_price by 1,000,000 
-- 2.> in 'city' column change the 'notset' values to 'not available in demo data set' to keep data consistent
-- 3.>Renaming 'ratio' in sales_report to '%_of_stock_sold' and multiplying by 100 to make it readable as a percent 
-- 4.> Get rid of duplicates in .. to reduce comp load and increase readability 


-Queries:
--Below, provide the SQL queries you used to clean your data.

-- 1.> Divide unit_price by 1,000,000 
CREATE TEMP TABLE analytics_clean AS (
    SELECT visitnumber, visitid, visitstarttime, date, fullvisitorid, userid, channelgrouping, socialengagementtyper, units_sold, pageviews, timeonsite, bounces, revenue, unit_price
    FROM analytics
)
CREATE TEMP TABLE analytics_clean AS (
	SELECT *,
	(revenue::real / 1000000) as revenue_clean,
	(unit_price::real / 1000000) as unit_price_clean
	FROM analytics
)
--QA test--
SELECT *,
(revenue::real / 1000000) as revenue_clean,
(unit_price::real / 1000000) as unit_price_clean 
FROM analytics_clean LIMIT 100 
--Passed--
select city from all_sessions 
-- 2.> in 'city' column change the 'notset' values to 'not available in demo data set' to keep data consistent
UPDATE all_sessions
SET city = 'not available in data set'
where city = '(not set)'
--QA test--
select city from all_sessions 
--Passed--

-- 3.>Renaming 'ratio' in sales_report to '%_of_stock_sold' and multiplying by 100 to make it readable as a percent 
ALTER TABLE sales_report 
ADD COLUMN ratiotest numeric  

UPDATE sales_report
SET ratiotest = ratio

select ratio , ratiotest from sales_report 

UPDATE sales_report 
SET ratiotest = ratiotest * 100 

select ratio , ratiotest from sales_report 

ALTER TABLE sales_report 
ALTER COLUMN ratiotest TYPE numeric (10,2)

select ratio , ratiotest from sales_report 

ALTER TABLE sales_report 
RENAME COLUMN ratiotest to percentofstockleft
--QA test--
select percentofstockleft from sales_report 
--Passed--

-- 4.> Get rid of duplicates in .. to reduce comp load and increase readability 
SELECT DISTINCT * FROM analytics 
SELECT DISTINCT * FROM all_sessions 
SELECT DISTINCT * FROM products
SELECT DISTINCT * FROM sales_report
SELECT DISTINCT * FROM salesbysku 
