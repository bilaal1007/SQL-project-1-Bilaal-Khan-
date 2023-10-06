--Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

--SQL Queries:
SELECT city, country, MAX(transactionrevenue) AS highest_transactionrevenue
FROM all_sessions
WHERE transactionrevenue IS NOT NULL
GROUP BY city, country, transactionrevenue 
ORDER BY transactionrevenue DESC
LIMIT 5;


--Answer:
--Highest transaction revenue is from Sunnyvale UnitedStates w 200000000 in transaction revenue.



--Question 2: What is the average number of products ordered from visitors in each city and country?**

--SQL Queries:

SELECT a.city,a.country, AVG(orderedquantity) AS avg_productsordered
FROM all_sessions a
JOIN products p ON a.productsku = p.sku 
GROUP BY a.city, a.country
ORDER BY avg_productsordered DESC;
--Answer:

--Average number of products ordered from visitors in Council Bluffs USA is 7589 products 



--Question 3: Is there any pattern in the types (product categories) of products ordered
--from visitors in each city and country?**

--SQL Queries:
SELECT city, country, v2productcategory
FROM all_sessions
WHERE country <> '(not set)' AND city <> '(not set)'
GROUP BY city, country, v2productcategory 
ORDER BY country, v2productcategory 

--Answer:

  --The most prevalent product category in each city and country is Home products 



--Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


--SQL Queries:
SELECT (s.total_ordered) AS max_total_ordered, 
a.city, 
a.country,
s.productsku AS top_selling_product 
FROM all_sessions AS a 
JOIN sales_report AS s
ON a.productsku = s.productsku
GROUP BY s.total_ordered,
a.city,
a.country,
s.productsku
HAVING 
	MAX(s.total_ordered)= (
		SELECT MAX(total_ordered)
		FROM sales_report AS s 
		WHERE s.productsku = s.productsku)
ORDER BY country, city DESC ;


--Answer:
--GGOEGOAQ012899 
--this is the top selling product in"Croatia""Germany""India""Japan""Malaysia""Mexico""Netherlands""South Korea""SpainSwitzerland""Taiwan""United States"--




--Question 5: Can we summarize the impact of revenue generated from each city/country?**
-- what is the highest revenue generated from each city and country 
--SQL Queries:
ALTER TABLE all_sessions
ALTER COLUMN totaltransactionrevenue TYPE bigint 
USING totaltransactionrevenue::bigint;

SELECT totaltransactionrevenue, city, country 
FROM all_sessions 
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY country, city, totaltransactionrevenue
ORDER BY totaltransactionrevenue DESC



--Answer:
--USA had the most revenue impact as it made the most 
--Then Israel
--Then Australia 


