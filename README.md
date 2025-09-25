# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `sql_projects`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `sql_projects`.
- **Table Creation**: A table named `Retail_Sales_Analysis` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sql_projects;

CREATE TABLE Retail_Sales_Analysis
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM Retail_Sales_Analysis;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM Retail_Sales_Analysis
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM Retail_Sales_Analysis
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT * FROM Retail_Sales_Analysis
 WHERE sale_date= '2022-11-05'
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
 SELECT * FROM Retail_Sales_Analysis
	   WHERE category='clothing' 
	   and sale_date >='2022-11-01'
	   and  sale_date < '2022-12-01'
	   and quantity >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT category,sum(total_sale) as totalam, 
                COUNT(*) as totallorder
                FROM Retail_Sales_Analysis
                GROUP BY by category
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT avg(age)as agg 
               FROM Retail_Sales_Analysis
               WHERE category='Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
 SELECT * FROM Retail_Sales_Analysis
            WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT
CATEGORY,
GENDER,
 COUNT(*) as total_trans
FROM Retail_Sales_Analysis
 GROUP BY by category,gender
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
 SELECT  
        YEAR(sale_date) as year,
        MONTH(sale_date) as month,
        AVG(total_sale) as avgsale
        FROM Retail_Sales_Analysis
        GROUP by year(sale_date),month(sale_date)
        ORDER BY 1,3 desc

---cte

SELECT * 
        FROM (
              SELECT 
               YEAR(sale_date) AS year,
               MONTH(sale_date) AS month,
               AVG(total_sale) AS avgsale,
                 RANK() OVER (PARTITION BY YEAR(sale_date)
                              order by AVG(total_sale) DESC
                              ) AS rank
                         FROM Retail_Sales_Analysis
                         GROUP by Year(sale_date), MONTH(sale_date)
                        ) AS t1
                        WHERE rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT top 5
         customer_id,
                     SUM(total_sale) as totsale
                     FROM Retail_Sales_Analysis
                     GROUP BY customer_id
                     ORDER by 2 desc
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT
   category,
   COUNT(distinct customer_id) as c_unique_cus
   FROM Retail_Sales_Analysis
    GROUP by category
                   
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql

	With hourly_sale as (
    SELECT *,
        CASE
            WHEN datepart(HOUR, sale_time) < 12 THEN 'Morning'
            WHEN datepart(HOUR, sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
                END AS shift
            
			FROM Retail_Sales_Analysis)
                SELECT 
                      shift,
                 COUNT(*) AS total_orders    
                 FROM hourly_sale
                 GROUP BY shift;
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

