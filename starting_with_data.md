Question 1: What percentage of visitors actually make a purchase?

SQL Queries:

SELECT
    100 * COUNT(DISTINCT CASE WHEN units_sold > 0 THEN visitid END)/COUNT(DISTINCT visitid) AS percentage
FROM analytics;

Answer: 13% visitors make a purchase.



Question 2:  What are the most polular channels customer use to vistit the website?

SQL Queries:

SELECT COUNT(DISTINCT visitid) AS unique_visitors, channelgrouping
FROM allsessions
GROUP BY channelgrouping;
 

Answer:  Organic search, direct, and referrals are the top channels customers use.




Question 3: List all visitid that sentiment scores less than 0

SQL Queries:

SELECT visitid,sentimentscore, sentimentmagnitude, sentimentratio
FROM visitsentimentscore
WHERE sentimentscore < 0 AND sentimentmagnitude > 1


Answer: 


Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
