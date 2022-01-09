# 181. Employees Earning More Than Their Managers

## Schema
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| salary      | int     |
| managerId   | int     |
+-------------+---------+
id is the primary key column for this table.
Each row of this table indicates the ID of an employee, their name, salary, and the ID of their manager.

## Problem
Write an SQL query to find the employees who earn more than their managers. Return the result table in any order.

## MySQL query statement

    WITH employee_vs_manager AS (
    WITH tmp_dict AS (
    SELECT 
        name,
        id,
        salary
    FROM Employee
    )
    SELECT
        Employee.name as Employee,
        Employee.salary as EmployeeSalary,
        tmp_dict.name as managerName,
        tmp_dict.salary as managerSalary
    FROM Employee
    INNER JOIN tmp_dict
    ON Employee.managerId = tmp_dict.id
    )
    SELECT 
        Employee
    FROM employee_vs_manager
    WHERE EmployeeSalary > managerSalary;