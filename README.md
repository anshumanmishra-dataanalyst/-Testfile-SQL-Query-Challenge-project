# Student Performance Analysis SQL Project

## Project Overview

**Title:** Student Performance Analysis

**Objective:**  
Analyze student performance data to derive insights about academic trends, strengths, and areas for improvement.

**Dataset:**  
Use the [Student Performance Dataset from Kaggle](https://www.kaggle.com/datasets). This dataset contains detailed information on student demographics, academic performance, and other related factors.

**Database & Table Details:**

- **Compiler:** MySQL
- **Table Name:** `Testfile`

**Table Schema:**

| Column Name                | Data Type |
|----------------------------|-----------|
| `gender`                   | text      |
| `NationalITy`              | text      |
| `PlaceofBirth`             | text      |
| `StageID`                  | text      |
| `GradeID`                  | text      |
| `SectionID`                | text      |
| `Topic`                    | text      |
| `Semester`                 | text      |
| `raisedhands`              | numeric   |
| `VisITedResources`         | numeric   |
| `AnnouncementsView`        | numeric   |
| `Discussion`               | numeric   |
| `ParentAnsweringSurvey`    | text      |
| `ParentschoolSatisfaction` | text      |
| `StudentAbsenceDays`       | text      |
| `Class`                    | text      |

---

## SQL Queries

The queries are divided into three difficulty levels: **Easy (Q1–Q10)**, **Medium (Q11–Q15)**, and **Hard (Q16–Q20)**.

### Easy Questions

#### Q1: Retrieve all student records from the dataset.
**A1:**
```sql
SELECT * FROM Testfile;

#### Q2: List all distinct values in the gender column.
**A2:**
```sql
SELECT DISTINCT gender FROM Testfile;
