
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

### Easy Queries (Q1 - Q10)

1. **Retrieve all student records from the dataset.**
   ```sql
   SELECT * FROM Testfile;
2. **List all distinct values in the gender column.**
   ```sql
   SELECT DISTINCT gender FROM Testfile;
