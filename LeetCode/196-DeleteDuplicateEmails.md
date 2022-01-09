196. Delete Duplicate Emails

## Schema

Table: Person

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id is the primary key column for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.

## Problem
Write an SQL query to delete all the duplicate emails, keeping only one unique email with the smallest id.

## MySQL query statement

DELETE FROM Person
WHERE id NOT IN (
    SELECT id FROM
    (
        SELECT MIN(id) id FROM Person
        GROUP BY email
    ) AS tmp
)