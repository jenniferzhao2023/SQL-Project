What are your risk areas? Identify and describe them.

My primary risk areas are data integrity and result accuracy. One of the data integrity issues I have encountered is the presence of numerous null values in the database, which could potentially lead to data inconsistencies or inaccuracies. For result accuracy, there are some values that lack clear definition, and some columns have similar names, making it difficult to determine the correct field to use in a query. In these cases, I may need to make assumptions, which can impact the accuracy of the results.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

--Uniqueness: check if the productsku in different tables contains null value. 
SELECT name, productsku
FROM productS
WHERE productsku IS NULL

--check if fullvisitorid is unique

SELECT COUNT(DISTINCT fullvisitorid)
FROM allsessions;

--The result is 14223, while we should get 15134

--get the duplicates fullvisitorid list
SELECT COUNT(visitid) AS countid, visitid
FROM allsessions
GROUP BY visitid
HAVING  COUNT(visitid) > 0
ORDER BY countid
DESC

--check the 10 same fullvisitorid and compare 

SELECT *
FROM allsessions
WHERE fullvisitorid ='3608475193341679870'

--check if visitid is unique

SELECT COUNT(DISTINCT fullvisitorid)
FROM allsessions;

--he results is 14862

--get the duplicates visitid list

SELECT COUNT(visitid) AS countid, visitid
FROM allsessions
GROUP BY visitid
HAVING  COUNT(visitid) > 0
ORDER BY countid
DESC

--check the 4 same visitid and compare it

SELECT visitid, * FROM allsessions
WHERE visitid = '1471780519'

--check distinct(productcategory)

SELECT DISTINCT(productcategory)
FROM allsessions

--Accuracy check:
--Check the revenue value is accurate.

SELECT * 
FROM analytics
WHERE units_sold IS NOT NULL
AND unit_price IS NOT NULL
AND revenue IS NOT NULL

Choose 3 diffenrent productsku to verify
Revenue ~~ unit_price * units_sold * 1.05%??

--check numeric values,if it make sense

SELECT MAX(pageviews)
FROM allsessions

SELECT MAX(timeonsite)
FROM allsessions