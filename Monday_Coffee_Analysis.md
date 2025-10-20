# ☕️ Monday Coffee Expansion Project

## 📊 Project Overview
The **Monday Coffee Expansion Project** is a SQL-based data analysis focused on understanding customer behavior, sales performance, and market potential across different cities.  
The study helps guide **business expansion decisions** by analyzing population data, coffee consumption estimates, sales, and revenue metrics.

---

## 🧩 Database Structure

| Table Name | Description |
|-------------|--------------|
| **city** | Contains city information including population and ranking. |
| **products** | Details of all coffee products offered by Monday Coffee. |
| **customers** | Contains customer demographics and location data. |
| **sales** | Stores all coffee sales transactions with product, customer, and date info. |

---

## 🎯 Key Business Questions

1. **Coffee Consumers Count**  
   → *How many people in each city are estimated to consume coffee (25% of the population)?*

2. **Total Revenue from Coffee Sales**  
   → *What is the total revenue generated from coffee sales across all cities in the last quarter of 2023?*

3. **Sales Count for Each Product**  
   → *How many units of each coffee product have been sold?*

4. **Average Sales Amount per City**  
   → *What is the average sales amount per customer in each city?*

5. **City Population and Coffee Consumers**  
   → *Provide a list of cities along with their populations and estimated coffee consumers.*

---

## 🧮 SQL Analysis

### 1️⃣ Coffee Consumers Count
```sql
SELECT 
   city_name,
   ROUND((population * 0.25)/1000000,2) AS coffee_consumers_in_millions,
   city_rank
FROM city
ORDER BY population DESC;
```

### 2️⃣ Total Revenue from Coffee Sales (Q4 2023)
```sql
SELECT 
    ci.city_name,
    SUM(s.total) AS total_revenue
FROM sales AS s
JOIN customers AS c
    ON s.customer_id = c.customer_id
JOIN city AS ci
    ON c.city_id = ci.city_id
WHERE YEAR(s.sale_date) = 2023
  AND DATEPART(QUARTER, s.sale_date) = 4
GROUP BY ci.city_name
ORDER BY total_revenue DESC;
```

### 3️⃣ Sales Count per Product
```sql
SELECT 
    p.product_name,
    COUNT(s.sale_id) AS total_orders
FROM products AS p
LEFT JOIN sales AS s
    ON s.product_id = p.product_id
GROUP BY p.product_name
ORDER BY COUNT(s.sale_id) DESC;
```

### 4️⃣ Average Sales per Customer by City
```sql
SELECT
    ci.city_name,
    SUM(s.total) AS total_revenue,
    COUNT(DISTINCT s.customer_id) AS total_customers,
    ROUND(SUM(s.total)/COUNT(DISTINCT s.customer_id), 2) AS [Avg_Sales_per_city]
FROM sales AS s
JOIN customers AS c
    ON s.customer_id = c.customer_id
JOIN city AS ci
    ON c.city_id = ci.city_id
GROUP BY ci.city_name
ORDER BY total_revenue DESC;
```

### 5️⃣ City Population and Coffee Consumers
```sql
WITH city_table AS (
    SELECT 
        city_name,
        ROUND((population * 0.25) / 1000000, 2) AS coffee_consumers
    FROM city
),
customers_table AS (
    SELECT 
        ci.city_name,
        COUNT(DISTINCT c.customer_id) AS unique_customers
    FROM sales AS s
    JOIN customers AS c
        ON c.customer_id = s.customer_id
    JOIN city AS ci
        ON c.city_id = ci.city_id
    GROUP BY ci.city_name
)
SELECT 
    ct.city_name,
    ct.coffee_consumers,
    cu.unique_customers
FROM city_table AS ct
JOIN customers_table AS cu
    ON ct.city_name = cu.city_name;
```

---

## 📈 Insights Summary

- Cities with **higher population and coffee consumer counts** present the best expansion potential.  
- The **top revenue-generating cities in Q4 2023** should be prioritized for store openings and marketing investment.  
- Product-level analysis shows which coffee products are **top performers** and which may need improvement.  
- Cities with **lower average sales per customer** may benefit from loyalty programs or pricing adjustments.  

---

## 💡 Recommendations

1. Focus expansion on **high-revenue, high-consumption** cities.  
2. Launch awareness campaigns in **large but low-performing** markets.  
3. Optimize inventory for **top-selling coffee products**.  
4. Track **average sales per city** quarterly to identify growth opportunities.  
5. Align supply chain planning with **seasonal demand patterns** (e.g., Q4 spikes).

---

## 🧾 Conclusion

This SQL-driven analysis provides a solid foundation for **data-informed expansion decisions** for the Monday Coffee brand.  
It identifies where coffee demand is strongest, where new opportunities exist, and how to optimize sales across cities and products.

---

## 🛠️ Tools & Technologies
- **SQL Server** (T-SQL)
- **Microsoft Excel / Power BI** (for visualization)
- **GitHub** (project documentation)

---

## 📂 Project Files
| File Name | Description |
|------------|--------------|
| `monday_coffee_analysis.sql` | Main SQL analysis script |
| `coffee_data.csv` | (Optional) Source data for city, sales, customers, products |
| `README.md` | Project documentation |

---

## 👤 Author
**Sunday Ajayi**  
_Data Analyst | SQL | Excel | Power BI | R | Oracle | Odoo_  

📧 Email: (add your email)  
🔗 GitHub: [yourusername](https://github.com/yourusername)  
💼 LinkedIn: (add your LinkedIn profile)
