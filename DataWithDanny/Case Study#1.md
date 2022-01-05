# Case Study #1 - Danny's Diner's Diner
## Problem Statement
Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:

* **Sales:** The sales table captures all *customer_id* level purchases with an corresponding *order_date* and *product_id* information for when and what menu items were ordered.
* **Menu:** The menu table maps the *product_id* to the actual *product_name* and *price* of each menu item.
* **Members:** The final members table captures the *join_date* when a *customer_id* joined the beta version of the Danny’s Diner loyalty program.


## Study Case Questions
1. What is the total amount each customer spent at the restaurant?
1. How many days has each customer visited the restaurant?
1. What was the first item from the menu purchased by each customer?
1. What is the most purchased item on the menu and how many times was it purchased by all customers?
1. Which item was the most popular for each customer?
1. Which item was purchased first by the customer after they became a member?
1. Which item was purchased just before the customer became a member?
1. before they became a member?
1. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
1. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

## Part 1
### What is the total amount each customer spent at the restaurant?