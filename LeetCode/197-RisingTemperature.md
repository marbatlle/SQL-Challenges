# 197. Rising Temperature

## Schema

Table: Weather

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id is the primary key for this table.
This table contains information about the temperature on a certain day.

## Problem
Write an SQL query to find all dates' Id with higher temperatures compared to its previous dates (yesterday).

## MySQL query statement

SELECT t1.id 
FROM Weather t1 
LEFT JOIN Weather t2
on t1.recordDate = date_add(t2.recordDate,interval 1 day) 
where t1.temperature > t2.temperature 