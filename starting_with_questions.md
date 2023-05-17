Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT SUM(totaltransactionrevenue) AS total
, city, country
FROM allsessions
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY city, country
ORDER BY total DESC




Answer: United states has the highest level of transaction revenues as country for highest and  San Francisco as the city.




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

SELECT  city, country,
        ROUND(AVG(orderedquantity)) as avg_productordered
FROM allsessions as a
JOIN products as p
ON a.productsku=p.productsku
WHERE city is not null
GROUP BY city,country
ORDER BY ROUND(AVG(orderedquantity)) DESC


Answer:Based on the data, the top 10 average number of products ordered from visitors in each city and country is as follows:

city	country	avg_productordered
Council Bluffs	United States	7589
Bellflower	United States	3786
Cork	Ireland	3786
Montenegro	Montenegro	3786
Mali	Mali	3786
Santiago	Chile	3607
Bellingham	United States	2836
Detroit	United States	2748
Papua New Guinea	Papua New Guinea	2558
RÃ©union	RÃ©union	2538




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

SELECT city,country,ROUND(AVG(units_sold))AS avg_produ
ct_ordered,"productcategory"
FROM allsessions AS s
INNER JOIN analytics AS a
ON s.visitid = a.visitid
WHERE units_sold IS NOT NULL
GROUP BY city,country,"productcategory"
ORDER BY avg_product_ordered DESC



Answer:

  There are a few product categories that appear to be more popular based on the average number of products ordered:

- "Home/Apparel/Headgear" appears to be a popular category in multiple countries, including the United States, Canada, Germany, and India.
- "Home/Bags" and "Home/Bags/Backpacks" are also popular categories in multiple countries.
- "Home/Accessories/Stickers" is another category that appears to be popular in multiple countries, including Australia, Canada, Switzerland, and the United States.






**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
City:

SELECT  productsku, productname, city, country, orderedquantity
FROM (
    SELECT
        p.productsku, p.name as productname, a.city, a.country,
        SUM(orderedquantity) as orderedquantity,
        ROW_NUMBER() OVER (PARTITION BY city ORDER BY SUM(orderedquantity) DESC) as rn
    FROM allsessions as a
    JOIN products as p
    ON a.productsku=p.productsku
    WHERE city is not null
    GROUP BY p.productsku, city, country, p.name
    ORDER BY SUM(orderedquantity) DESC) as subquery
WHERE rn = 1

Country:

SELECT  productsku, productname, country, orderedquantity
FROM (
    SELECT
        p.productsku, p.name as productname, a.country,
        SUM(orderedquantity) as orderedquantity,
        ROW_NUMBER() OVER (PARTITION BY country ORDER BY SUM(orderedquantity) DESC) as rn
    FROM allsessions as a
    JOIN products as p
    ON a.productsku=p.productsku
    WHERE city is not null
    GROUP BY p.productsku, country, p.name
    ORDER BY SUM(orderedquantity) DESC) as subquery
WHERE rn = 1


Answer:
Based on the provided data for orders with a quantity over 10,000, we can identify the top-selling product from each city/country as follows:

- United States: Custom Decals (223,374)
- Mountain View (United States): Kick Ball (75,850)
- India: Custom Decals (49,218)
- United Kingdom: Custom Decals (37,860)
- San Francisco (United States): Kick Ball (30,340)
- New York (United States): Kick Ball (30,340)
- Chicago (United States): Kick Ball (30,340)
- Germany: Custom Decals (26,502)
- Palo Alto (United States): Cam Outdoor Security Camera - USA (19,033)
- Canada: Twill Cap (18,982)
- Sunnyvale (United States): Cam Outdoor Security Camera - USA (16,314)
- Taiwan: Kick Ball (15,170)
- Russia: Kick Ball (15,170)
- Minato (Japan): Kick Ball (15,170)
- Kirkland (United States): Kick Ball (15,170)
- Los Angeles (United States): Kick Ball (15,170)
- Santiago (Chile): Kick Ball (15,170)
- Santa Clara (United States): Kick Ball (15,170)
- San Diego (United States): Kick Ball (15,170)
- San Bruno (United States): Kick Ball (15,170)
- Portugal: Kick Ball (15,170)
- Council Bluffs (United States): Kick Ball (15,170)
- Dublin (Ireland): Kick Ball (15,170)
- Romania: Custom Decals (15,144)
- France: Custom Decals (15,144)
- Japan: Custom Decals (15,144)
- Italy: Custom Decals (15,144)
- London (United Kingdom): Twill Cap (13,778)
- Brazil: Custom Decals (11,358)
- Georgia: Custom Decals (11,358)
- San Jose (United States): Custom Decals (11,358)
- Detroit (United States): 22 oz Water Bottle (10,075)



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:


SELECT al.city, al.country, an.revenue,an.units_sold, an.unit_price,al.productsku, al.productname
FROM allsessions al
JOIN analytics an
USING (fullvisitorid)
WHERE revenue > 0
ORDER BY an.revenue DESC

Answer:
we cannot summarize the impact of revenue generated from each city/country based on this data. 









