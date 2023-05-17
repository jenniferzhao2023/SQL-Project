What issues will you address by cleaning the data?

I plan to address the following issues through data cleaning: 

create visitorsentiment table( visitid,visitorsentimentscore, sentimentmagnetude,sentimentratio)

drop the duplicate column(sentimentscore, sentimentmagnetude,sentimentratio)

dropped columns that were entirely null and has no relevance with other value
    
drop userid /itemrevenue /itemquantity/productrefundamount column as column are en null and can not import from other table


Data type : convert the datatype 
	convert the time and visitstarttime form string type to timestamp;
	convert the totaltransactionrevenue to numeric type.

edit the column name from properties, make them meaningful
    edit name to productname
	ratio to sentimentratio
	tale sale_report to monthly_sales_report 

Inaccurate value:  Find the inaccurate data and correct it:

	unit_price need to divided by 1,000,000
	product price need to divided by 1,000,000

Missing data: 
    
    autofill some value to missing value

	set USD for currencycode for transaction in United stated
	set 'Home/Appare/Women's' as productcategory  when productname contains 'Women's 
	set 'Home/Appare/Men's' as productcategory  when productname contains 'Men's 
	set the city value as country 


Check range constrains: 
	Sentimentscore value should between -1 and 1
	Sentimentmagnitude value should great than 0

Makes the data set understandable:
	 Update city value 'not available in demo dataset'  or '(not set)' to null
	UPDATE productvariant in allsessions table from  '(not set)' to null
	Update socialengagementtype from 'Not Socially Engaged'   to 'no'

create visitorsentiment table ,drop the duplicate columns from products table and sales_report table


Queries:
Below, provide the SQL queries you used to clean your data.

--create visitorsentiment table
SELECT  a.visitid, s.sentimentscore, s.sentimentmagnitude,s.ratio INTO visitsentimentscore
FROM sales_report s
JOIN allsessions a
USING (PRODUCTSKU)

--drop the duplicate column
ALTER TABLE sales_report drop column sentimentscore
ALTER TABLE products drop column sentimentscore

--drop the column that the entire values all null
ALTER TABLE sales_report drop column userid
ALTER TABLE products drop column itemrevenue

--identify the schema and datatype of tables and primarykey
SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'allsessions';


--Get column data types and nullability for the allsessions table.
SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'allsessions';




----convert the time from string type to timestamp
UPDATE allsessions SET time = to_timestamp(time::bigint);
ALTER TABLE allsessions 
ALTER COLUMN time TYPE timestamp 
USING to_timestamp(time, 'YYYY-MM-DD HH24:MI:SS');

----convert the visittime from string type to timestamp
UPDATE analytics SET visitstarttime = to_timestamp(visitstarttime::bigint);
ALTER TABLE analytics 
ALTER COLUMN visitstarttime TYPE timestamp 
USING to_timestamp(visitstarttime, 'YYYY-MM-DD HH24:MI:SS');

--convert the totaltransactionrevenue to numeric type
ALTER TABLE allsessions ALTER COLUMN totaltransactionrevenue SET DATA TYPE numeric
USING totaltransactionrevenue::numeric

--convert the visitid column to numeric type
ALTER TABLE allsessions ALTER COLUMN visitid SET DATA TYPE numeric
USING visitid::numeric

----check the range constraints
SELECT sentimentscore
FROM sales_report
WHERE sentimentscore >= 1 OR sentimentscore <=-1

--check the range constraints
SELECT sentimentmagnitude
FROM sales_report
WHERE sentimentmagnitude  <=0

--check primary key prodctsku is not null
SELECT productsku
FROM sales_by_sku
WHERE productsku IS NULL

--make the value more meaningful
UPDATE all_sessions SET city = NULL WHERE city = 'not available in demo dataset' OR city = ‘(not set)’

--Makes the data set understandable
UPDATE analytics SET socialengagementtype = 'No'
WHERE socialengagementtype = 'Not Socially Engaged'

--make the value anaccrate, unit_price, productprice and revenu /1,000,000 

UPDATE analytics SET unit_price = unit_price/1000000

UPDATE allsessions SET productprice = unit_price/1000000

---replace the anomalies of productcategory 

--check the productcategory
SELECT COUNT(productcategory) AS countid, productcategory
FROM allsessions
GROUP BY productcategory
HAVING  COUNT(productcategory) > 0
ORDER BY countid
DESC

--find the outlier from timeonsite
SELECT MAX(timeonsite)
FROM allsessions

--max timeonsite is 4661,delete this record

--find the anomalies from the list 
--find 1 record product name from the anomalies categorya to decide the category
UPDATE allsessions SET productcategory = 'HOME/Accessories' WHERE productcategory = '${escCatTitle}'

--Set city name as country when city is null
UPDATE allsessions table_name
SET city = 'country'
WHERE city IS NULL;

--Set productcategory to for productname including 'Men's or Women's'
UPDATE allsessions 
SET productcategory = 'Home/Apparel/Women's' 
WHERE productname like '%Women%'

UPDATE allsessions 
SET productcategory = 'Home/Apparel/Men's' 
WHERE productname like '%Men%'

--add USD as curreny for country = 'United States'
select count(*) from all_sessions where country='United States';
-- -- returns 8727 

UPDATE all_sessions
SET currencyCode = 'USD'
WHERE Country = 'United States';
--UPDATE 8727


