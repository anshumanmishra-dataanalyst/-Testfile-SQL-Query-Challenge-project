Student Performance Analysis
Project Overview
Title: Student Performance Analysis Objective: Analyze student performance data to derive insights about academic trends, strengths, and areas for improvement. Dataset: Student Performance Dataset from Kaggle

Dataset Structure
Table Name: Testfile The dataset has the following columns:

gender (text)

NationalITy (text)

PlaceofBirth (text)

StageID (text)

GradeID (text)

SectionID (text)

Topic (text)

Semester (text)

raisedhands (numeric)

VisITedResources (numeric)

AnnouncementsView (numeric)

Discussion (numeric)

ParentAnsweringSurvey (text)

ParentschoolSatisfaction (text)

StudentAbsenceDays (text)

Class (text)

Queries and Analysis
Easy Questions
Retrieve all student records from the dataset.

sql
SELECT * FROM Testfile;
List all distinct values in the gender column.

sql
SELECT DISTINCT gender FROM Testfile;
Count the total number of rows in the table.

sql
SELECT COUNT(*) AS total_rows FROM Testfile;
Count the number of students for each gender.

sql
SELECT gender, COUNT(*) AS count FROM Testfile GROUP BY gender;
Calculate the average number of raisedhands.

sql
SELECT AVG(raisedhands) AS avg_raisedhands FROM Testfile;
Retrieve all rows where raisedhands is greater than 50.

sql
SELECT * FROM Testfile WHERE raisedhands > 50;
Show only gender, NationalITy, and Class.

sql
SELECT gender, NationalITy, Class FROM Testfile;
List all rows ordered by raisedhands in descending order.

sql
SELECT * FROM Testfile ORDER BY raisedhands DESC;
Find the maximum value in the VisITedResources column.

sql
SELECT MAX(VisITedResources) AS max_resources FROM Testfile;
Count how many students are in each Class.

sql
SELECT Class, COUNT(*) AS count FROM Testfile GROUP BY Class;
Medium Questions
Find classes that have more than 20 students.

sql
SELECT Class, COUNT(*) AS count FROM Testfile GROUP BY Class HAVING count > 20;
Compute the average raisedhands for each combination of NationalITy and gender.

sql
SELECT NationalITy, gender, AVG(raisedhands) AS avg_raisedhands FROM Testfile GROUP BY NationalITy, gender;
Retrieve rows where AnnouncementsView is greater than 5 and Discussion is less than 10.

sql
SELECT * FROM Testfile WHERE AnnouncementsView > 5 AND Discussion < 10;
Add a column called Performance that shows 'High' if raisedhands is above 50 and 'Low' otherwise.

sql
SELECT *, CASE WHEN raisedhands > 50 THEN 'High' ELSE 'Low' END AS Performance FROM Testfile;
For each NationalITy, list the top 3 rows with the highest Discussion scores (requires MySQL 8.0+ for window functions).

sql
SELECT * FROM (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY NationalITy ORDER BY Discussion DESC) AS rn 
    FROM Testfile 
) AS sub WHERE rn <= 3;
Hard Questions
Create a pivot-like table that shows the number of students by gender for each Class.

sql
SELECT Class, 
       SUM(CASE WHEN gender = 'Male' THEN 1 ELSE 0 END) AS Male_Count, 
       SUM(CASE WHEN gender = 'Female' THEN 1 ELSE 0 END) AS Female_Count 
FROM Testfile 
GROUP BY Class;
Compare students within the same NationalITy and PlaceofBirth by joining the table to itself; list pairs where one student has a higher raisedhands value than the other.

sql
SELECT a.gender AS Gender_A, a.raisedhands AS RaisedHands_A, 
       b.gender AS Gender_B, b.raisedhands AS RaisedHands_B, 
       a.NationalITy, a.PlaceofBirth 
FROM Testfile a 
JOIN Testfile b 
ON a.NationalITy = b.NationalITy AND a.PlaceofBirth = b.PlaceofBirth 
WHERE a.raisedhands > b.raisedhands;
For each row, calculate the ratio of raisedhands to VisITedResources, while avoiding division by zero.

sql
SELECT *, 
       CASE WHEN VisITedResources = 0 THEN NULL 
            ELSE raisedhands / VisITedResources 
       END AS Hands_Resources_Ratio 
FROM Testfile;
Rank students by Discussion scores within each NationalITy using window functions (MySQL 8.0+ required).

sql
SELECT *, 
       RANK() OVER (PARTITION BY NationalITy ORDER BY Discussion DESC) AS Discussion_Rank 
FROM Testfile;
Calculate a weighted score for each student using the formula (raisedhands * 0.4 + AnnouncementsView * 0.3 + Discussion * 0.3) and rank students by this score.

sql
SELECT *, 
       (raisedhands * 0.4 + AnnouncementsView * 0.3 + Discussion * 0.3) AS WeightedScore, 
       RANK() OVER (ORDER BY (raisedhands * 0.4 + AnnouncementsView * 0.3 + Discussion * 0.3) DESC) AS ScoreRank 
FROM Testfile;
Conclusion
This project aims to provide a comprehensive analysis of student performance, offering valuable insights into academic trends, strengths, and areas for improvement. By working on these queries, you'll gain hands-on experience with SQL and data analysis techniques.
