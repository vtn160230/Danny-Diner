# Dannys-Diner
Week 1 of the 8 Week SQL Challenge - Danny's Diner

##  Table of Contents
- [Business Task] (#Business-Task)
- [Entity Relationship Diagram] (#Entity-Relatioship-Diagram)
- [Case Study Questions] (#Case-Study-Questions)
- [Solution] (#Solution)


## Business Task




## Entity Relationship Diagram

![entitydg](https://user-images.githubusercontent.com/122754787/216842489-7b493ebb-fee6-44b4-b8a5-58351b7a871f.png)



## Case Study Questions
  
- What is the total amount each customer spent at the restaurant?
- How many days has each customer visited the restaurant?
- What was the first item from the menu purchased by each customer?
- What is the most purchased item on the menu and how many times was it purchased by all customers?
- Which item was the most popular for each customer?
- Which item was purchased first by the customer after they became a member?
- Which item was purchased just before the customer became a member?
- What is the total items and amount spent for each member before they became a member?
- If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
- In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?


## Solution 
### Q1: What is the total amount each customer spent at the restaurant?
<details>

````sql
SELECT s.customer_id, SUM(price) AS total_amount_spent FROM sales s
JOIN menu ON s.product_id = m.product_id
GROUP BY customer_id
ORDER BY customer_id
````

- I used the SUM and GROUP BY to figure out the total_amount_spent that each customer spent
- Used a JOIN to combine the sales and menu table on product_id that are from both tables
- Ended with an ORDER BY on customer_id to get an ascending table

Answer: 
<br>
![q1answer](https://user-images.githubusercontent.com/122754787/216840816-1676169f-e90f-4528-abbd-03c240d7242d.png)
</details>

***

### Q2: How many days has each customer visited the restaurant?
<details>

````sql
SELECT customer_id, COUNT(DISTINCT(order_date)) as days_visited FROM sales s
GROUP BY customer_id
````
<br>

- I used COUNT on the order_date to get each entry for the order_date, and DISTINCT to remove the duplicates of the same dates
- Finished with GROUP BY to get the customers in ascending order

Answer: 
<br>
![q2answer](https://user-images.githubusercontent.com/122754787/216841374-7dcbccce-3a06-4093-a864-df90981651c3.png)
</details>

***
