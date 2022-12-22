# Amazon_Vine_Analysis
## Overview: Analyze Amzon reviews written by members of the paid Amazon Vine program. 
### The purpose of this analysis is to choose one dataset from 50 datasets. Then use PySpark to perform the ETL process to extract the dataset, transform the data, connect to an AWS RDS instance, and load the transformed data into pgAdmin. Next, I used Pandas to determine if there is any bias toward favorable reviews from Vine members in my dataset. 

## Analysis
- Before I started my analysis, I needed to pick a dataset from the [Amazon Review datasets](https://s3.amazonaws.com/amazon-reviews-pds/tsv/index.txt). For this analysis, I chose to analyze the Health Personal Care reviews. All of the datasets had the same schematas, as shown in Image 1. 

### Image 1: Amazon Dataset Schematas
![Amazon Dataset Schematas](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/644263b4c0a89838d9cc286e2ce5981661c9c3e4/images/Amazon_Dataset_Schematas.png)

- Once I picked the dataset, I needed to create a database with Amazon RDS and a database in my Amazon RDS server in pgAdmin. By connecting my Amazon RDS database to pgAdmin, when I transform the dataset into four separate DataGrames, then I can match the DataFrames to table schemas in pgAdmin. Therefore, in pgAdmin I needed to run a [query](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/6702da096dafe909c6bad6346c60324b0820f031/Starter_Code/challenge_schema.sql) to create four tables for my new database: customers_table, products_table, review_id_table, and vine_table. Then I created an [Google Colab Notebook](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/52753046c31ead9a45984606d3c8290a5e417599/Amazon_Reviews_ETL.ipynb) to extract the Amazon dataset, create a new DataFrame, and transform the dataset into four DataFrames that will match the schema in the pgAdmind tables. 

- The first DataFrame I created is the customers_table, and I wanted to aggregate the reviews by the customer_id. Therefore, I used to the groupby() function on the customer_id column, and counted all the customer ids by using the agg() function and chaining it to the groupby() function. By doing this, it creates a new column, customer_count, Image 2. 

### Image 2: Customers Table
![Customers Table](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/644263b4c0a89838d9cc286e2ce5981661c9c3e4/images/customers_table.png)

- Next, I created the products_table DataFrame. To create the products_table, I used the select() function to select the product_id and product_title. Then I needed to drop the duplicates so that I only retrieved the unique product_ids, Image 3. 

### Image 3: Products Table
![Products Table](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/644263b4c0a89838d9cc286e2ce5981661c9c3e4/images/products_table.png)

- To crete the review_id_table DataFrame, I also used the select() function to select the columns that I used in the review_id_table in pgAdmin, and converted the review_date column to a date, Image 4. I also used to the select() function to create the vine_table, and only selected the columns that are in the vine_table in pgAdmin, Image 5. 

### Image 4: Review ID Table
![Review ID Table](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/644263b4c0a89838d9cc286e2ce5981661c9c3e4/images/review_id_table.png)

### Image 5: Vine Table
![Vine Table](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/644263b4c0a89838d9cc286e2ce5981661c9c3e4/images/vine_table.png)

- Now that the tables have been created I can load the DataFrames into pgAdmin. First, I needed to make the connection to my AWS RDS instance. Then I can load the DataFrames that correspond to the tables in pgAdmin. In pgAdmin, I made sure to run a query to check that my tables were successfully populated before I exported them as csv files, Image 6. Once the tables were exported as csv files, I could then run analysis to determine if there is any bias of Vine reviews. 

### Image 6: Vine Table Populated into pgAdmin
![Vine Table Populated into pgAdmin](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/5de917fceab265f51cf80e24ae2f167b9f027dd5/images/pgAdmin_table.png)

- Using Pandas, I created a Jupyter Notebook, [Vine_Review_Analysis](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/a7dc63a4d437ba32858ee054f9301043181b7728/Vine_Review_Analysis.ipynb), and read in the vine_table.csv file as a DataFrame so that I can perform my analysis. Frist, I filtered the data and created a new DataFrame to retrieve all the rows where the total_votes count was equal to or greater than 20. By doing this, I am able to pick reviews that are more likly to be helpful and avoid having any division by zero errors. After filtering the data, there was a total of 133,416 rows where the total votes count were equal to or greater than 20. 

- Then I wanted to filter the DataFrame I just created to create a new DataFrame to retrieve all the rows where the number of helpful_votes divided by the total_votes is equal to or greater than 50%. Once I ran this analysis, there was a total of 121,360 helpful votes out of the total votes. 

- Now that I have the total number of helpful votes, I need to create two more additional DataFrames to retrieve all the rows where a review was written as part of the Vine program (paid) and one where the reviews were not part of the Vine program (unpaid). For the reviews that were part of the Vine program, there were a total of 497, whereas unpaid had 120,863. With the DataFrames for paid and unpaid reviews, I can determine the total number of reviews, the number of 5-star reviews, and the percentage of 5-star reviews for the two types of reviews. 

## Results
- The paid and unpaid analysis script and results can be found in the [Vine_Review_Analaysis](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/a7dc63a4d437ba32858ee054f9301043181b7728/Vine_Review_Analysis.ipynb)
### Paid Reviews Analysis and Results
- There were 121,360 total reviews that were helpful for this analysis. Out of the 121,360 reviews, 497 of them paid Vine reviews. Then I wanted to determine out of the 497 reviews, how many of them were 5-star reviews. Therefore, from the paid Vine review DataFrame, I ran an analysis to calculate the number of reviews that were 5-star. Out of the 497 total paid Vine reviews, 220 of them were 5-star reviews. Therefore, 44% of the total paid Vine reviews were 5-star reviews, Image 6. 

#### Image 7: Paid Review Analysis
![Paid Review Analysis](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/644263b4c0a89838d9cc286e2ce5981661c9c3e4/images/paid_review_analysis.png)

### Unpaid Reviews Analysis and Results
- Again, there were 121,360 total reviews that were helpful for this analsyis. Out of the 121,360 reviews, 120,863 of them were unpaid Vine reviews. Just like I did for the paid reviews, I wanted to know how many of the unpaid reviews were 5-star reviews. From the 120,863 unpaid reviews, 74,470 of them were 5-star reviews. Therefore, 62% of the total unpaid Vine reviews were 5-star reviews, Image 7. 

#### Image 8: Unpaid Reveiw Analysis
![Unpaid Review Analysis](https://github.com/mrma2318/Amazon_Vine_Analysis/blob/644263b4c0a89838d9cc286e2ce5981661c9c3e4/images/unpaid_review_analysis.png)

## Summary
- Overall, I wanted to see if the unpaid reviews were more genuine than the paid reviews and determine if there was any bias. To not show bias in reviews, the percentage of unpaid reviews should be larger than the paid reviews. This is indicated in the results from analyizing the total number of paid and unpaid 5-star reviews and calculating the percentages. Paid reviews were only 44% of the total paid reviews, while unpaid 5-star reviews made up 62% of the total unpaid reviews. If the paid reviews percentage was larger than the unpaid percentage, then there might have been greater bias due to being paid to possibly give 5-star reviews. Since the unpaid reviews had a higher percentage though, this lets us know that the reviews are more genuine. 

- However, due to the significant difference in total number of reviews between paid, 497, and unpaid, 120,863, could also be why the results indicate little bias in the reviews. Thus, an additional analysis that I could do to support my statement would be to collect additional Vine paid reviews. By adding additional paid reviews and getting more even paid and unpaid total reviews, might provide further insight into whether or not bias exists in the reviews for Health Personal Care items. 