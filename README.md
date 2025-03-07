
# Student Performance Analysis Project

This project analyzes student performance data to derive insights about academic trends, strengths, and areas for improvement using SQL.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Table Schema](#table-schema)
- [SQL Queries](#sql-queries)
  - [Easy Queries (Q1 - Q10)](#easy-queries-q1---q10)
  - [Medium Queries (Q11 - Q15)](#medium-queries-q11---q15)
  - [Hard Queries (Q16 - Q20)](#hard-queries-q16---q20)
- [Usage](#usage)
- [License](#license)

---

## Project Overview

**Title:** Student Performance Analysis  
**Objective:** Analyze student performance data to identify academic trends and pinpoint areas for improvement.  
**Compiler:** MySQL

---

## Dataset

The dataset used in this project is the [Student Performance Dataset](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams) from Kaggle. It contains detailed information about students’ demographics, academic performance, and other related factors.

---

## Table Schema

The project uses a table named `Testfile` with the following columns:

| Column Name               | Data Type |
| ------------------------- | --------- |
| `gender`                  | text      |
| `NationalITy`             | text      |
| `PlaceofBirth`            | text      |
| `StageID`                 | text      |
| `GradeID`                 | text      |
| `SectionID`               | text      |
| `Topic`                   | text      |
| `Semester`                | text      |
| `raisedhands`             | numeric   |
| `VisITedResources`        | numeric   |
| `AnnouncementsView`       | numeric   |
| `Discussion`              | numeric   |
| `ParentAnsweringSurvey`   | text      |
| `ParentschoolSatisfaction`| text      |
| `StudentAbsenceDays`      | text      |
| `Class`                   | text      |

---

## SQL Queries

The following queries are divided into three levels of difficulty: **Easy**, **Medium**, and **Hard**.


## Solutions 
### Easy Queries (Q1 - Q10)

1. Retrieve all student records from the dataset.
   
```sql
SELECT * FROM Testfile;
```
2. List all distinct values in the gender column.
   
```sql
SELECT DISTINCT gender FROM Testfile;
```
3. Count the total number of rows in the table.

```sql
SELECT COUNT(*) AS total_rows FROM Testfile;
```
4. Count the number of students for each gender.
   
```sql
SELECT gender, COUNT(*) AS count FROM Testfile GROUP BY gender;
```
5. Calculate the average number of raisedhands.
```sql
SELECT AVG(raisedhands) AS avg_raisedhands FROM Testfile;
```

6. Retrieve all rows where raisedhands is greater than 50.
```sql
SELECT * FROM Testfile WHERE raisedhands > 50;
```

7. Show only gender, NationalITy, and Class.
```sql
   SELECT gender, NationalITy, Class FROM Testfile;
```

8. List all rows ordered by raisedhands in descending order.
```sql
SELECT * FROM Testfile ORDER BY raisedhands DESC;
```
9. Find the maximum value in the VisITedResources column.
```sql
SELECT MAX(VisITedResources) AS max_resources FROM Testfile;
```
10. Count how many students are in each Class.
```sql
SELECT Class, COUNT(*) AS count FROM Testfile GROUP BY Class;
```
11. Identify the least selling product in each country for each year based on total units sold.
```sql	
WITH product_rank AS(
    SELECT 
        st.country,
        p.product_name,
        SUM(s.quantity) AS total_qty_sold,
        RANK() OVER(PARTITION BY st.country ORDER BY SUM(s.quantity)) AS least_sold_product
    FROM sales AS s
    JOIN stores AS st
    ON s.store_id = st.store_id
    JOIN products AS p
    ON s.product_id = p.product_id
    GROUP BY 1, 2
)
SELECT * FROM product_rank WHERE least_sold_product = 1;
```
12. Calculate how many warranty claims were filed within 180 days of a product sale.
```sql
SELECT 
    COUNT(*) AS total_warranty_claimed
FROM warranty AS w
LEFT JOIN sales AS s
ON w.sale_id = s.sale_id
WHERE w.claim_date - s.sale_date <= 180;
```
13. Determine how many warranty claims were filed for products launched in the last two years.
```sql
SELECT
    p.product_name,
    COUNT(w.claim_id) as total_no_claim,
    COUNT(s.sale_id) as total_no_sales
FROM warranty AS w
RIGHT JOIN sales AS s
ON w.sale_id = s.sale_id
JOIN products AS p
ON p.product_id = s.product_id
WHERE launch_date >= CURRENT_DATE - INTERVAL '2 years'
GROUP BY 1
HAVING COUNT(w.claim_id) > 0;
```
14. List the months in the last three years where sates exceeded units in the United States.
```sql
SELECT
    TO_CHAR(sale_date, 'MM-YYYY') AS months,
    SUM(s.quantity) AS no_of_units_sold
FROM sales AS s
JOIN stores AS st
ON s.store_id = st.store_id
WHERE country = 'United States' 
  AND s.sale_date >= CURRENT_DATE - INTERVAL '3 years'
GROUP BY 1
HAVING SUM(s.quantity) > 5000;
```
15. Identify the product category with the most warranty claims filed in the last two years.
```sql
SELECT 
    c.category_name,
    COUNT(w.claim_id) AS total_claims
FROM warranty AS w
LEFT JOIN sales AS s
ON w.sale_id = s.sale_id
JOIN products AS p
ON p.product_id = s.product_id
JOIN category AS c
ON c.category_id = p.category_id
WHERE w.claim_date >= CURRENT_DATE - INTERVAL '2 years'
GROUP BY 1
ORDER BY 2 DESC;
```
16. Determine the percentage chance of receiving warranty claims after each purchase for each country.
```sql
SELECT
    country,
    total_units,
    total_claim,
    COALESCE((total_claim::NUMERIC / total_units::NUMERIC) * 100,0) AS percentage_of_risk
FROM
(
    SELECT
        st.country,
        SUM(s.quantity) AS total_units,
        COUNT(w.claim_id) AS total_claim
    FROM sales AS s
    JOIN stores AS st
    ON st.store_id = s.store_id
    LEFT JOIN warranty AS w
    ON w.sale_id = s.sale_id
    GROUP BY 1
) tr
ORDER BY 4 DESC;
```
17. Analyze the year-by-year growth ratio for each store.
```sql
WITH yearly_sales AS
(
    SELECT
        S.store_id,
        st.store_name,
        EXTRACT(YEAR FROM sale_date) AS year_of_sale,
        SUM(p.price * s.quantity) AS total_sale
    FROM sales AS s
    JOIN products AS p
    ON s.product_id = p.product_id
    JOIN stores AS st
    ON st.store_id = s.store_id
    GROUP BY 1, 2, 3
    ORDER BY 1, 2, 3
),

growth_ratio AS
(
    SELECT
        store_name,
        year_of_sale,
        LAG(total_sale, 1) OVER(PARTITION BY store_name ORDER BY year_of_sale) AS last_year_sale,
        total_sale AS current_year_sale
    FROM yearly_sales
)

SELECT
    store_name,
    year_of_sale,
    last_year_sale,
    current_year_sale,
    ROUND((current_year_sale - last_year_sale)::NUMERIC / last_year_sale::NUMERIC * 100, 2) AS growth_ratio_yoy
FROM growth_ratio
WHERE last_year_sale IS NOT NULL;
```

18. Calculate the correlation between product price and warranty claims for products sold in the tast five years, segmented by price range.
```sql
SELECT 
    CASE
        WHEN p.price < 500 THEN 'Basic'
        WHEN p.price BETWEEN 500 AND 1000 THEN 'Mid-range'
        ELSE 'Premium'
    END AS price_segment,
    COUNT(w.claim_id) AS total_claim
FROM warranty AS w
LEFT JOIN sales AS s
ON s.sale_id = w.sale_id
JOIN products AS p
ON p.product_id = s.product_id
WHERE claim_date >= CURRENT_DATE - INTERVAL '5 years'
GROUP BY 1
ORDER BY 2 DESC;
```
19. Identify the store with the highest percentage of "Completed" claims relative to total claims filed.
    
```sql
WITH completed AS
(
    SELECT
        s.store_id,
        COUNT(w.claim_id) AS completed
    FROM sales AS s
    RIGHT JOIN warranty AS w
    ON s.sale_id = w.sale_id
    WHERE w.repair_status = 'Completed'
    GROUP BY 1
), 

total_claim AS
(
    SELECT
        s.store_id,
        COUNT(w.claim_id) AS total_claim
    FROM sales AS s
    RIGHT JOIN warranty AS w
    ON s.sale_id = w.sale_id
    GROUP BY 1
)

SELECT 
    tc.store_id,
	st.store_name,
    tc.total_claim,
    c.completed,
    ROUND(c.completed::NUMERIC / tc.total_claim::NUMERIC * 100, 2) AS percentage_of_completed
FROM completed AS c
JOIN total_claim AS tc
ON c.store_id = tc.store_id
JOIN stores AS st
ON tc.store_id=st.store_id;
```

20. Write a query to calculate the monthly running total of sales for each store over the past four years and compare trends during this period.
```sql
WITH monthly_sales AS
(
    SELECT
        store_id,
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        SUM(p.price * s.quantity) AS total_profit
    FROM sales AS s
    JOIN products AS p
    ON s.product_id = p.product_id
    GROUP BY 1, 2, 3
    ORDER BY 1, 2, 3
)

SELECT
    store_id, 
    year, 
    month, 
    total_profit, 
    SUM(total_profit) OVER(PARTITION BY store_id ORDER BY year, month) AS running_total
FROM monthly_sales;
```
---


   
