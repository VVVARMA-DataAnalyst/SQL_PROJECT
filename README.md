**Retail Sales Analysis (SQL Project)**


**Overview**

This project focuses on analyzing retail sales data using SQL to uncover insights into customer behavior, product performance, and sales patterns. It demonstrates how structured queries can be used for data cleaning, validation, and exploratory analysis within a relational database.
Objectives
• Create and manage a retail sales database.
• Load and validate transactional data.
• Identify null or missing values.
• Explore customer demographics and purchasing trends.
• Build a foundation for further business analysis (e.g., revenue forecasting, product performance).

**Database Schema**

Database: sql_p1
Table: retail_sales
Column Name	        Data Type	    Description
transactions_id	     INT	        Unique transaction identifier
sale_date            DATE	        Date of the sale
sale_time	           TIME	        Time of the sale
customer_id	         INT	        Unique customer identifier
gender	          VARCHAR(10)	    Gender of the customer
age	                 INT	        Customer’s age
category	        VARCHAR(15)	    Product category
quantity	           INT	        Quantity of items sold
price_per_unit	    FLOAT	        Price per item
cogs	              FLOAT	        Cost of goods sold
total_sale	        FLOAT	         Total sale amount

**Steps Performed**

1. Created a new database `sql_p1`.
2. Created table `retail_sales` with transactional data columns.
3. Validated data integrity and null values.
4. Explored customer and category diversity.
5. Analyzed sales performance and trends using 10 analytical SQL queries.

**Detailed Analysis Queries**

Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05

SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';


Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022

SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    date_format(sale_date, '%Y-%m') = '2022-11'
    AND
    quantity >= 4;


Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.

SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1;

Q.4 Write a SQL query to find the average age of customers who purchased items in each category.

SELECT
    category,
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
group by category;

Q.5 Write a SQL query to display category,quantity of all transactions where the total_sale is greater than 1000.

select category,quantity,total_sale from retail_sales
where total_sale > 1000;

Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.

SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1;


Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year

SELECT 
    year,
    month,
    avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date) 
            ORDER BY AVG(total_sale) DESC
        ) AS rnk
    FROM retail_sales
    GROUP BY 1, 2
) AS t
WHERE rnk = 1
ORDER BY year, avg_sale DESC;


Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 

SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.


SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;



Q.10 Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)

WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;


**Technologies Used**

• SQL (MySQL / PostgreSQL)    — for data querying and analysis
• Retail Dataset              — containing transactional data (sales, demographics, etc.)
• DBMS Tools                  — MySQL Workbench or pgAdmin for query execution and visualization

**Key Insights**

• Top-selling categories and high-performing months were identified.
• Customer demographics were correlated with purchase preferences.
• Category-wise and time-shift-based analysis revealed demand distribution.
• Top customers were identified for potential loyalty programs.

**How to Run the Project**

1. Clone the repository:
   git clone https://github.com/<your-username>/retail-sales-analysis.git
   cd retail-sales-analysis
2. Open MySQL or PostgreSQL and execute the SQL file:
   SOURCE retail_sales_analysis.sql;
3. Run each query sequentially to reproduce analysis results.

**Future Enhancements**
• Integrate Python for data visualization (Matplotlib / Seaborn).
• Automate report generation using ETL pipelines.
• Build a Power BI dashboard for interactive retail analytics.
• Add more customer behavior KPIs such as average purchase frequency and lifetime value.

**Author**
Venkata Varma Vatsavayi
Passionate about Data Analytics and SQL-based Insights


