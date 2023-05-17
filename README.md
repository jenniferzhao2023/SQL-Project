# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
This database provides data on revenue by products, as well as detailed data on visitor interactions with the products (including the timing of their visits, their geographic location, the number of pages they viewed, the duration of their visit, and the number of transactions they entered

The objective of this project is to build up our skills in data cleaning, data transformation, and data analytics. By analyzing the data, we aim to uncover some patterns, trends, and insights that can inform decision-making and drive business value. 


## Process
Here are the steps I followed to complete the project:
Step 1: I created the ecommerce database, set the primary key and column datatype, and imported the CSV files.

Step 2: I checked the schema of the database and discovered the relationships between different tables. I also found out the meaning of each column.

Step 3: I conducted data cleaning, which involved converting the datatype, exploring unique values, finding duplicate records, identifying missing values, and auto-filling them if possible. I also identified extreme values, inaccurate dates, and irrelevant data.

Step 4: I implemented a data QA process, which involved pulling queries or manually spot-checking to check the accuracy and completeness of the data.

Step 5: I analyzed the data using different questions to make insightful observations.


## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)




## Challenges 
During my project, I faced many challenges, including:
1. Reorganizing the tables posed a significant challenge for me as some columns were stored in different tables, and the allsession table contained a lot of information. Splitting the tables into multiple tables was complicated and time-consuming.
2. Some columns, such as Fullvisitorid, Visitorid, and Userid, were not clearly defined and had similar names. Similarly, columns like Transactionrevenue, Itemrevenue, and Productrevenue were confusing. Without a subject matter expert, I had to compare them and make assumptions to run the query accordingly.
3. Dealing with null values was also a challenge. I dropped columns that were entirely null, and for columns with a few null values,for example, transactionrevenue has 4 records. I was unsure about the acceptance criteria , so just leave them as they were.
4. I identified all the duplicates of Fullvisitorid and Visitorid, but I wasn't sure how to deal with them. It seemed like duplicated Fullvisitorid still made sense.
5. I was curious about how to perform a more in-depth analysis of the and make more useful insights.


## Future Goals
(what would you do if you had more time?)
If I had more time, I would like to perform further data cleaning. My first step would be to split the "allsession" table into multiple tables with different columns, including customers, products etc.. This would make the structure of the database more readable and help to reduce data redundancy. For instance, I can set some values to null, such as the currency type according to the country. Additionally, I would create a separate table for product categories and assign different levels to different columns. This would make it easier to calculate and analyze data.

Furthermore, I would dedicate more time to exploring the relationships between different numerical values to verify their accuracy and conduct a more in-depth analysis. For instance, I would focus on analyzing product sales data, such as identifying the most popular products and forecasting inventory demand. By doing so, we can provide better business surpport and make data-driven decisions to improve overall performance.
