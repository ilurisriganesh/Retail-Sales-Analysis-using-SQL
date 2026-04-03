# 📊 Retail Sales Analysis using SQL
“Focused on SQL-based data extraction, transformation, and analysis”
## 👤 Author  
**SRI GANESH I**

---

## 📌 Project Overview  
This project focuses on analyzing retail sales data using SQL to extract meaningful business insights. It demonstrates data cleaning, exploratory data analysis (EDA), and solving real-world business problems using SQL queries.

---

## 🗂️ Dataset Structure  

```sql
CREATE TABLE retail_sales
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

---

## 🧹 Data Cleaning  

### 🔍 Check for NULL values
```sql
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### ❌ Remove NULL records
```sql
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

## 🔍 Key SQL Analysis  

### 📅 Sales on Specific Date
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

---

### 👕 Clothing Sales (Nov 2022, Quantity ≥ 4)
```sql
SELECT *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND quantity >= 4;
```

---

### 💰 Total Sales by Category
```sql
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

---

### 👥 Average Age (Beauty Category)
```sql
SELECT
    ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

---

### 💸 High-Value Transactions (>1000)
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

---

### 👨‍👩‍👧 Transactions by Gender & Category
```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

---

### 📊 Best Selling Month Each Year
```sql
SELECT 
       year,
       month,
       avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(MONTH FROM sale_date) AS month,
    AVG(total_sale) AS avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
FROM retail_sales
GROUP BY 1, 2
) AS t1
WHERE rank = 1;
```

---

### 🏆 Top 5 Customers
```sql
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

---

### 👤 Unique Customers per Category
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

---

### ⏰ Sales by Time of Day
```sql
WITH hourly_sale AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders    
FROM hourly_sale
GROUP BY shift;
```

---

## 🛠️ Tools & Technologies  
- SQL (PostgreSQL / MySQL)  
- Data Cleaning  
- Aggregations & Window Functions  

---

## 🚀 Key Learnings  
- Data cleaning using SQL  
- Writing analytical queries  
- Using **GROUP BY, CTEs, and Window Functions**  
- Solving real business problems with data  

---

## 📌 Conclusion  
This project demonstrates how SQL can be used to analyze retail sales data and generate actionable insights. It showcases practical skills required for a Data Analyst role.

---

## ⭐ If you like this project  
Give it a ⭐ and feel free to connect!
