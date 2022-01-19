# 196. Delete Duplicate Emails

## Schema

Table: Cinema

+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| id             | int      |
| movie          | varchar  |
| description    | varchar  |
| rating         | float    |
+----------------+----------+
id is the primary key for this table.
Each row contains information about the name of a movie, its genre, and its rating.
rating is a 2 decimal places float in the range [0, 10]
## Problem
Write an SQL query to report the movies with an odd-numbered ID and a description that is not "boring".

Return the result table ordered by rating in descending order.

## MySQL query statement
SELECT
    *
FROM cinema
WHERE description NOT LIKE "boring"
    AND id % 2 <> 0
ORDER BY rating DESC;
