
# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project demonstrates SQL skills commonly used by data analysts to explore, clean, and analyze retail sales data. You'll learn to set up a database, perform exploratory data analysis (EDA), and answer business questions using SQL queries. It's ideal for those beginning their data analysis journey and looking to solidify foundational SQL skills.

---

## Objectives

1. **Set up a retail sales database**: Create and populate the `p1_retail_db` database.
2. **Data Cleaning**: Identify and handle records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Gain a deeper understanding of the dataset.
4. **Business Analysis**: Answer specific business questions using SQL queries and derive actionable insights.

---

## Project Structure

### 1. Database Setup
- **Database Name**: `p1_retail_db`
- **Table**: `retail_sales`
- **Table Structure**:  
  The `retail_sales` table contains the following columns:
  - `transactions_id` (INT, Primary Key)
  - `sale_date` (DATE)
  - `sale_time` (TIME)
  - `customer_id` (INT)
  - `gender` (VARCHAR)
  - `age` (INT)
  - `category` (VARCHAR)
  - `quantity` (INT)
  - `price_per_unit` (FLOAT)
  - `cogs` (FLOAT)
  - `total_sale` (FLOAT)

SQL scripts for creating the table:
```sql
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
```

---

### 2. Data Exploration & Cleaning
Key tasks include:
- Counting total records, unique customers, and product categories.
- Checking for and removing null values.

Sample Queries:
```sql
-- Count total records
SELECT COUNT(*) AS total_records FROM retail_sales;

-- Find unique product categories
SELECT DISTINCT(category) FROM retail_sales;

-- Check and delete records with null values
DELETE FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL;
```

---

### 3. Business Analysis

A set of SQL queries to answer common business questions:

1. **Total Sales**:  
   ```sql
   SELECT COUNT(*) AS total_sales FROM retail_sales;
   ```

2. **Unique Customers**:  
   ```sql
   SELECT COUNT(DISTINCT customer_id) AS total_customers FROM retail_sales;
   ```

3. **Sales by Date**:  
   ```sql
   SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';
   ```

4. **Top Customers**:  
   ```sql
   SELECT customer_id, SUM(total_sale) AS total_sales 
   FROM retail_sales 
   GROUP BY customer_id 
   ORDER BY total_sales DESC 
   LIMIT 5;
   ```

5. **Sales by Shift**:  
   ```sql
   SELECT 
       CASE 
           WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning Shift' 
           WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon' 
           ELSE 'Evening' 
       END AS shift,
       COUNT(*) AS total_orders
   FROM retail_sales
   GROUP BY shift;
   ```

More queries can be found in the project SQL script.

---

## Results & Insights

1. **Top Performing Categories**: Identified product categories contributing the most to total sales.  
2. **Customer Behavior**: Observed customer purchasing patterns, including top customers and purchase timings.  
3. **Sales Trends**: Analyzed average monthly sales to determine peak and off-peak seasons.  
4. **Operational Efficiency**: Insights into shifts and their respective order volumes.

---

## How to Run This Project

1. Clone this repository:
   ```bash
   git clone https://github.com/sarath702/SQL_retail-sales.git
   ```
2. Set up a SQL database (e.g., PostgreSQL, MySQL, SQLite).
3. Execute the SQL scripts in sequence:
   - Create the `retail_sales` table.
   - Insert sample data into the table.
   - Run the analysis queries.

---

## Contributing

Contributions are welcome! If you have ideas for additional queries or improvements:
1. Fork the repository.
2. Create a feature branch:
   ```bash
   git checkout -b feature-name
   ```
3. Commit and push your changes.
4. Open a pull request.

---

## License

This project is licensed under the [MIT License](LICENSE).
