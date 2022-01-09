# 183. Customers Who Never Order

## Schema

Table: Customers
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
id is the primary key column for this table.
Each row of this table indicates the ID and name of a customer.

Table: Orders

+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| customerId  | int  |
+-------------+------+
id is the primary key column for this table.
customerId is a foreign key of the ID from the Customers table.
Each row of this table indicates the ID of an order and the ID of the customer who ordered it.


## Problem
Write an SQL query to report all customers who never order anything.

## MySQL query statement

    SELECT 
        c.name as Customers
    FROM Customers c
    LEFT JOIN Orders o
    ON c.id = o.CustomerId
    WHERE o.CustomerId IS NULL;