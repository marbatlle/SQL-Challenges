# 182. Duplicate Emails

## Schema
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id is the primary key column for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.

## Problem
Write an SQL query to report all the duplicate emails.

## MySQL query statement

    SELECT
        email as Email
    FROM Person
    GROUP BY email
    HAVING COUNT(Email) > 1;