# SQL Retail sales
Retail Sales Analysis SQL Project

Project Overview
Project Title: Retail Sales Analysis
Level: Beginner
Database: p1_retail_db

This project demonstrates SQL skills commonly used by data analysts to explore, clean, and analyze retail sales data. You'll learn to set up a database, perform exploratory data analysis (EDA), and answer business questions using SQL queries. It's ideal for those beginning their data analysis journey and looking to solidify foundational SQL skills.

Objectives
1. Set up a retail sales database: Create and populate the p1_retail_db database.
2. Data Cleaning: Identify and handle records with missing or null values.
3. Exploratory Data Analysis (EDA): Gain a deeper understanding of the dataset.
4. Business Analysis: Answer specific business questions using SQL queries and derive actionable insights

Project Structure
1. Database Setup
Database Name: p1_retail_db
Table: retail_sales
Table Structure:
The retail_sales table contains the following columns:
transactions_id (INT, Primary Key)
sale_date (DATE)
sale_time (TIME)
customer_id (INT)
gender (VARCHAR)
age (INT)
category (VARCHAR)
quantity (INT)
price_per_unit (FLOAT)
cogs (FLOAT)
total_sale (FLOAT)


SQL scripts for creating the table:

DROP TABLE IF EXISTS retail_sales;

CREATE TABLE retail_sales (
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


2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

--- data cleaning and exploration 

--total coloumns 

select count (*) from retail_sales 

select distinct(category)from retail_sales

select count(distinct(transactions_id)) from retail_sales

--- Data cleaning (removing null values from table) 

select * from retail_sales 
where transactions_id is null -- It is an primary key so it does not contain any null values

select * from retail_sales 
where gender is null          -- To check each column it is time waste method and not an appropirate way 

select * from retail_sales --- To check all columns at atime it is the best approach
where transactions_id is null
      or 
      sale_date is null
      or 
      sale_time is null
      or 
      customer_id is null
      or 
      gender is null
      or 
      age is null  
      or 
      category is null
      or 
      quantity is null
      or 
      price_per_unit is null
      or 
      cogs is null
      or 
      total_sale is null

-- After running these we got  values with three columns  and three rows has an null values

-- To delete those null values --

delete from retail_sales 
      where transactions_id is null
      or 
      sale_date is null
      or 
      sale_time is null
      or 
      customer_id is null
      or 
      gender is null
      or 
      age is null  
      or 
      category is null
      or 
      quantity is null
      or 
      price_per_unit is null
      or 
      cogs is null
      or 
      total_sale is null

--Data Exploration --
	select * from retail_sales;

--1. how many sales we have
	
 select count(*) 
	as total_sales 
	from retail_sales;

--2. How many unique customers we have 

select count(distinct(customer_id)) 
	as total_customers 
	from retail_sales;

--3. How many categories we have 

select count(distinct(category))
	as total_categories
from retail_sales;

--4 Write a SQL query to retrieve all columns for sales made on '2022-11-05'

select * from retail_sales 
where sale_date = '2022-11-05';

--5 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4
--in the month of Nov-2022.

select * from retail_sales 
where 
	category = 'Clothing'
    and
	to_char(sale_date , 'YYYY-MM') = '2022-11'
	and 
	quantity >=4;

--6 Write a SQL query to calculate the total sales (total_sale) for each category

select category,
	sum(total_sale) as total_sales,
	count(*) as total_orders
	from retail_sales
group by category
	order by total_sales;

--7 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category round it 2 decimals.

select 
     round(avg(age) ,2) as average_age
from retail_sales
where category = 'Beauty';

--8 Write a SQL query to find all transactions where the total_sale is greater than 1000

select * from retail_sales
	where total_sale >= 1000;

--9 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category

select gender,
	category,
	count(transactions_id) as i
	from retail_sales
group by gender,
	category
	order by i;


select * from retail_sales

--10 write a query to find out the average sale for each month and year .

select 
extract (month from sale_date) as month,
	extract (year from sale_date) as year,
avg(total_sale) as avg_sales
from retail_sales
group by 1,2
	order by 2, 3 desc;

--11 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year


select  month,
	year,
	avg_sales
	from 
(select 
extract (month from sale_date) as month,
	extract (year from sale_date) as year,
avg(total_sale) as avg_sales,
	rank () over(partition by extract (year from sale_date) order by avg(total_sale) desc ) as rank
from retail_sales
group by 1,2 ) as t1
where rank = 1;

--12 Write a SQL query to find the top 5 customers based on the highest total sales 

select customer_id ,
	sum(total_sale) as max 
from retail_sales
	group by 1
	order by 2 desc
	limit 5;

--13 Write a SQL query to find the number of unique customers who purchased items from each category.

select category,
count(distinct(customer_id))
from retail_sales
	group by category;

--14 Write a SQL query to create each shift. (Example Morning <12, Afternoon Between 12 & 17, Evening >17)

select customer_id,
	sale_time,
case 
	when extract (hour from sale_time) < 12 then 'morning shift' 
    when extract (hour from sale_time) between 12 and 17 then 'Afternoon'
    else 'Evening'
    end as shift
from retail_sales
	order by 1;

--15 Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)


with 
	hourly_sale as (
select customer_id,
	sale_time,
case 
	when extract (hour from sale_time) < 12 then 'morning shift' 
    when extract (hour from sale_time) between 12 and 17 then 'Afternoon'
    else 'Evening'
    end as shift
from retail_sales
	order by 1)

select shift,
count(*) as total_orders
from hourly_sale
	group by shift;

 Results & Insights
1. Top Performing Categories: Identified product categories contributing the most to total sales.
2. Customer Behavior: Observed customer purchasing patterns, including top customers and purchase timings.
3. Sales Trends: Analyzed average monthly sales to determine peak and off-peak seasons.
4. Operational Efficiency: Insights into shifts and their respective order volumes.

   Author - sarath surya

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!


   
