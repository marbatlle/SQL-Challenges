196. Delete Duplicate Emails

## Schema

Table: Courses

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student     | varchar |
| class       | varchar |
+-------------+---------+
(student, class) is the primary key column for this table.
Each row of this table indicates the name of a student and the class in which they are enrolled.
 
## Problem
Write an SQL query to report all the classes that have at least five students.

## MySQL query statement

SELECT
    name,
    population,
    area
FROM world
WHERE area >= 3000000 
    OR population >= 25000000; 