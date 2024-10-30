# Financial_SQL_Project
# Project Overview
**Project title:** Financial Samole Sales Analysis
**Level:** Beginner
**Database:** mydatabase
This project demonstrates SQL-based analysis of financial data. Using SQL queries, it explores revenue, customer behaviour, sales trends, and transactional insights to support data-driven decision-making. 

# Objectives
**1. Set up a financial sales database:** Create and populate a retail sales database with the provided sales data.

**2. Data Cleaning:** Identify and remove any records with missing or null values.

**3. Exploratory Data Analysis (EDA):** Perform basic exploratory data analysis to understand the dataset.

**4. Sales Performance Analysis:** Evaluating total sales per segment, identifying the product with the highest gross sales, and analyzing sales variations by country.

**5. Profit Analysis:**

# Project Structure
**1. Database Setup**

-**Database Creation:** The project starts by creating a database named mydatabase.

-**Table Creation:** A table named financial is created to store the sales data. The table structure includes columns for 
IDs, Segment,	Country, Product, Discount band, Units sold, Manufacturing price, Sales price, Gross sales, Discounts, 	  Sales, COGS(Cost Of Goods Sold), Profit, Dates,	Months and years.

``` sql
--Creating database
create database mydatabase;

--Creating table
create table financial(
ids INT primary key,
segment VARCHAR(20),
country VARCHAR(30),
product VARCHAR(20),
discount_band VARCHAR(10),
units_sold INT,
manufacturing_price INT,
sale_price INT,
gross_sales FLOAT,
discounts FLOAT,
sales FLOAT,
cogs INT,
profit FLOAT,
dates DATE,
months VARCHAR(20),
years INT
);

--Viewing all records and fields
SELECT * FROM financial;
```

**2. Data Cleaning:**
- Identify unique records, which will eventually be marked as distinct so that their quality is improved.

-Replace and update months column as integer values coming from the sales_date column and detect missing values in each field to retain the integrity.
``` sql
--Data Cleaning
-- Viewing distinct records
SELECT DISTINCT * FROM financial;

--Replacing months column with int months
--Updating the months column with the extracted month
UPDATE financial
SET months = EXTRACT(MONTH FROM dates)::int;
SELECT * FROM financial;

--Selecting null values
SELECT * FROM financial
WHERE ids IS NULL
OR segment IS NULL
OR country IS NULL
OR product IS NULL
OR discount_band IS NULL
OR units_sold IS NULL
OR manufacturing_price IS NULL
OR sale_price IS NULL
OR gross_sales IS NULL
OR discounts IS NULL
OR sales IS NULL
OR profit IS NULL
OR dates IS NULL
OR months IS NULL
OR years IS NULL;
```

**3. Exploratory Data Analysis (EDA):**

Counting total records, unique segments, products, discount bands, and countries to provide an overview of the dataset.
Analyzing total units sold and overall sales to gauge business performance.
```sql
--Exploratory Data Analysis
--Counting total number of records
SELECT COUNT(*) FROM financial;

--Counting total no of segments
SELECT COUNT(DISTINCT segment) AS unique_segments FROM financial;

--Counting Unique discound bands
SELECT COUNT(DISTINCT discount_band) AS unique_bands FROM financial;

--How many unique products
SELECT COUNT(DISTINCT product) AS products FROM financial;

--How many countries
SELECT COUNT(DISTINCT country) AS countries FROM financial;

--Total units sold
SELECT sum(units_sold) AS total_unitssold FROM financial;

--Overall total sales
SELECT SUM(sales) AS total_sales FROM financial;
```

**4. Sales Performance Analysis:**
This section analyzes sales metrics across various dimensions, including total sales per segment, country performance, and product gross sales. It provides insights into sales trends and averages to inform strategic decision-making.
```sql
--Sales Performance Analysis
--Total sales amount per segment
SELECT segment,SUM(sales)AS sales_per_segment FROM financial
GROUP BY segment ORDER BY segment;

--Highest gross sales for product
SELECT product,SUM(gross_sales)AS max_gross_sales
FROM financial
GROUP BY product
ORDER BY max_gross_sales DESC
LIMIT 1;

--Total sales vary by country
SELECT country,SUM(sales) AS total_sales,
COUNT(DISTINCT product) AS total_products,
AVG(sales) AS average_sales_per_transaction
FROM financial
GROUP BY country
ORDER BY total_sales DESC;

--Average sale price for each product
SELECT product,round(AVG(sale_price),2)AS average_saleprice FROM financial
GROUP BY product;
```

**5. Profit Analysis:**
This section evaluates profitability by calculating total profit for each segment and product, and examining the relationship between manufacturing prices and profits. It provides insights into profit margins and highlights key profit-generating products.
```sql
--Profit Analysis
--Total profit for each segment
SELECT segment,SUM(profit)
FROM financial
GROUP BY 1;

--The product generates the highest profit
SELECT product,SUM(profit)AS profits
FROM financial
GROUP BY product
ORDER BY profits DESC
LIMIT 1;

--Is Manufacturing price relate to profit
SELECT manufacturing_price,AVG(profit)AS average_profit 
FROM financial 
GROUP BY 1;

--Total profit margin for each segment
SELECT segment,SUM(profit) as total_profit
FROM financial
GROUP BY segment;
```

**6. Discount Impact Analysis:**
This section shows the effects of discounts on sales performance, including the count of products with no discounts, average discounts by band, and total units sold. It provides insights into how discount strategies influence sales outcomes.

```sql
--Discount Impact Analysis
--Products with no discounts count
SELECT COUNT(discounts) FROM financial
WHERE discounts=0;

--Average discount given in each discount band
SELECT discount_band,AVG(discounts)AS average_discounts 
FROM financial
GROUP BY discount_band;

--Discounts affect units sold
SELECT discount_band,SUM(units_sold) AS total_units_sold 
FROM financial 
GROUP BY 1;
```

**7. Time Based Analysis:**
This section explores sales trends over time, analyzing total sales by month, year-over-year changes, and identifying peak and low sales periods. It offers valuable insights into seasonal patterns and overall sales growth.
```sql
--Time-Based Analysis
--Total sales by month
SELECT years,months,SUM(sales)AS total_sales
FROM financial
GROUP BY 2,1
ORDER BY 1,2;

--Sales changes over the years
SELECT years,SUM(sales)AS total_sales
FROM financial 
GROUP BY years 
ORDER BY years;

--Sales growth rate year-over-year?
SELECT years,sum(sales) as sales
from financial
group by years;

--Highest and lowest sales month
SELECT months,SUM(sales) AS max_sales_month
FROM financial
GROUP BY 1
ORDER BY 1 DESC
LIMIT 1;
SELECT months,SUM(sales) AS min_sales_month
FROM financial
GROUP BY 1
ORDER BY 1
LIMIT 1;

--How do sales trends vary month over month
SELECT months,sum(sales) as sales
from financial
group by months;
```

**8. Cost Analysis:**
This section assesses the Cost of Goods Sold (COGS) across products and segments, calculating averages and totals. It provides insights into cost structures and their impact on overall profitability.

```sql
--Cost Analysis
--Average Cost Of Goods Sold(COGS) per product
SELECT product,AVG(cogs)AS average_cogs 
FROM financial
GROUP BY product;

--Sum of Cost Of Goods Sold(COGS) per segment
SELECT segment,SUM(cogs)AS total_cogs 
FROM financial
GROUP BY 1;

--Sum of Cost Of Goods Sold(COGS) per discount band
SELECT discount_band,SUM(cogs)AS total_cogs 
FROM financial
GROUP BY 1;
```



