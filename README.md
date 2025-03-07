
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
### Medium Queries (Q11 - Q15)

11. Find classes that have more than 20 students.
```sql	
SELECT Class, COUNT(*) AS count FROM Testfile GROUP BY Class HAVING count > 20;
```
12. Compute the average raisedhands for each combination of NationalITy and gender.
```sql
SELECT NationalITy, gender, AVG(raisedhands) AS avg_raisedhands FROM Testfile GROUP BY NationalITy, gender;
```
13. Retrieve rows where AnnouncementsView is greater than 5 and Discussion is less than 10.
```sql
SELECT *
FROM Testfile
WHERE AnnouncementsView > 5
  AND Discussion < 10;
```
14. Add a column called Performance that shows 'High' if raisedhands is above 50 and 'Low' otherwise.
```sql
SELECT *, CASE WHEN raisedhands > 50 THEN 'High' ELSE 'Low' END AS Performance FROM Testfile;
```
15. For each NationalITy, list the top 3 rows with the highest Discussion scores (requires MySQL 8.0+ for window functions).
```sql
SELECT * FROM ( SELECT *, ROW_NUMBER() OVER (PARTITION BY NationalITy ORDER BY Discussion DESC) AS rn FROM Testfile ) AS sub WHERE rn <= 3;
```
### Hard Queries (Q16 - Q20)

16. Create a pivot-like table that shows the number of students by gender for each Class.
```sql
SELECT Class, SUM(CASE WHEN gender = 'Male' THEN 1 ELSE 0 END) AS Male_Count, SUM(CASE WHEN gender = 'Female' THEN 1 ELSE 0 END) AS Female_Count FROM Testfile GROUP BY Class;
```
17. Compare students within the same NationalITy and PlaceofBirth by joining the table to itself; list pairs where one student has a higher raisedhands value than the other.
```sql
SELECT a.gender AS Gender_A, a.raisedhands AS RaisedHands_A, b.gender AS Gender_B, b.raisedhands AS RaisedHands_B, a.NationalITy, a.PlaceofBirth FROM Testfile a JOIN Testfile b ON a.NationalITy = b.NationalITy AND a.PlaceofBirth = b.PlaceofBirth WHERE a.raisedhands > b.raisedhands;
```

18. For each row, calculate the ratio of raisedhands to VisITedResources, while avoiding division by zero.
```sql
SELECT *, 
       CASE 
         WHEN VisITedResources = 0 THEN NULL 
         ELSE raisedhands / VisITedResources 
       END AS Hands_Resources_Ratio
FROM Testfile;
```
19. Rank students by Discussion scores within each NationalITy using window functions (MySQL 8.0+ required).
    
```sql
SELECT *,
       RANK() OVER (PARTITION BY NationalITy ORDER BY Discussion DESC) AS Discussion_Rank
FROM Testfile;
```

20. Calculate a weighted score for each student using the formula
(raisedhands * 0.4 + AnnouncementsView * 0.3 + Discussion * 0.3)
and rank students by this score.

```sql
SELECT *, (raisedhands * 0.4 + AnnouncementsView * 0.3 + Discussion * 0.3) AS WeightedScore, RANK() OVER (ORDER BY (raisedhands * 0.4 + AnnouncementsView * 0.3 + Discussion * 0.3) DESC) AS ScoreRank FROM Testfile;
```
---


   
