--Question 1 Calculate the total number of sales for each product 

--SQL query 
SELECT CAST(units_sold AS INTEGER) AS units_soldint 
FROM analytics
ALTER TABLE analytics
ADD COLUMN units_soldint INTEGER;
UPDATE analytics 
SET units_soldint = CAST(units_sold AS INTEGER);
--above i had to convert a data type that i mistakenly imported earlier in this assignement

SELECT all_sessions.productsku, SUM(analytics.units_soldint) AS total_sales 
FROM all_sessions
JOIN analytics ON all_sessions.fullvisitorid = analytics.fullvisitorid
WHERE units_soldint IS NOT null 
GROUP BY productsku; 
--Answer 
'"9180754"	2
"9180793"	26
"9180833"	5
"9181573"	181
"9182575"	1
"9182581"	6300
"9182658"	4'


--Question 2
-- WHat is the average time spend on the website per visitor that falls under organic search?
--SQL Query 
SELECT DISTINCT fullvisitorid, avg(timeonsite) AS avg_timeonsite, channelgrouping
FROM analytics 
WHERE channelgrouping = 'Organic Search'
GROUP BY fullvisitorid, channelgrouping, timeonsite 
HAVING timeonsite IS NOT null 


--Question 3 
--How many units were sold in total on July 16th 2017?
--SQL Query
SELECT COUNT (*) units_sold, date 
FROM analytics 
WHERE date = '2017-07-16'
GROUP BY date 

--Answer 
38517 units were sold on july 16 2017 

--Answer:
--USA had the most revenue impact as it made the most 
--Then Israel
--Then Australia 
