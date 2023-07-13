What issues will you address by cleaning the data?
During the data cleaning process, we will address the following issues identified in the dataset:

1. Missing values: We will handle missing values in columns such as "unit cost" by either imputing them or removing the corresponding rows.

2. Data inconsistencies: We will address inconsistencies in the "currency code" column, ensuring that all values are valid and consistent.

3. Data outliers: We will examine the data for any outliers in columns like "units_sold" and take appropriate steps to handle them, such as transforming or removing outliers.

4. Data duplication: We will identify and remove any duplicated records in the dataset to ensure data integrity and eliminate any biases in the analysis.

5. Data validation: We will validate the data against defined rules and constraints, such as checking for correct data types and verifying relationships between tables.

By addressing these data issues, we can ensure the accuracy and reliability of the data for further analysis and insights.




Queries:
Below, provide the SQL queries you used to clean your data.

Some codes are given below :

1. Handling missing values:
DELETE FROM all_sessions
WHERE city IS NULL OR city = '(not set)';

2. Handling special values:
UPDATE all_sessions
SET city = NULL
WHERE city = 'not available in demo dataset';

3. By deleting some irrelevant and missing values columns:
   Here is the generic code, as I deleted a couple of columns
   ALTER TABLE table_name
   DROP COLUMN column_name;

4. Changed the unit_price by dividing 1000,000
Code:
UPDATE analyticis
SET unit_cost = unit_cost / 1000000;

5. Checked the ‘total_ordered’ column to see if there is any negative number or not.
SELECT total_ordered
FROM sales_by_sku
WHERE total_ordered < 0;

6. Removing Specific Type of Product Data

EX: from product_SKU, Starting with 918 type data has been deleted 

delete from sales_report
where "productSKU" like '91%'





