What are your risk areas? Identify and describe them.
Altering Tables - I would have alter data types in columns in order to get accurate values when cleaning data and selecting it, for example dividing unitprice by 1000000 when the unit_price data type is bigint causes severe rounding errors. Therefore I alter the data type to Numeric to account for decimals after dividing it. 
Updating table column values - When cleaning data I would have to change parameters for some columns to increase readability of data. this could be an issue if I update the raw data twice or another function acts on it. 
Making data consistent - Normalizing inconsistent data to all match can be an issue if you convert a value that contains information without knowing. for example 'notset' and 'notindata' are the same and need to be the same but 'null' values may have missing context that should be addressed.
QA Process:
Describe your QA process and include the SQL queries used to execute it.

Altering tables- I would have to ensure that the data type for the raw data when importing the table matches what I plan to do to it when cleaning it. for example when seeing large values in the unit_price column, i can predict ill change those by dividing 1000000, therefore i should forsee and import the column as a numeric with the appropriate percision and scale rather than just putting bigint 
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



Updating Column values- I would review data type when altering values to make sure data is accurate, for example if im converting a ratio to a percentage value by multiplying by 100. If my percision and scale when importing my data  is not included I will get inaccurate data since the default scale is to the whole number. 
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
