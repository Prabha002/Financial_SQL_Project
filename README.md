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

