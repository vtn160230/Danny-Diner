# Dannys-Diner
Week 1 of the 8 Week SQL Challenge - Danny's Diner

##  Table of Contents
- [Business Task](#Business-Task)
- [Entity Relationship Diagram](#Entity-Relationship-Diagram)
- [Case Study Questions](#Case-Study-Questions)
- [Solutions](#Solutions)


## Business Task

Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.


## Entity Relationship Diagram

![entitydg2](https://user-images.githubusercontent.com/122754787/216842806-1545ba7f-8155-4efa-b514-fca62c533464.png)


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


## Solutions 
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

### Q3: What was the first item from the menu purchased by each customer?
<details>

````sql
  with t1 as(
	SELECT customer_id, order_date, product_name, 
	ROW_Number() OVER(PARTITION BY s.customer_id
	ORDER BY s.order_date) as rank
	FROM sales s
	JOIN menu m ON s.product_id = m.product_id
)

SELECT customer_id, product_name FROM t1
WHERE rank = 1
GROUP BY customer_id, product_name
````
  
- Use t1 as a temporary table and use ROW_Number to create column ranks that is partitioned by the customer_id and ORDERED BY order_date
- Write new query pulling the customer_id and product_name from t1 table WHERE the rank = 1, which will pull the rank 1 entry for each customer_id
- GROUP BY customer_id and product_name to fetch the customer_id and first item ever ordered by the customer
	
Answer: 
<br>
![q3answer](https://user-images.githubusercontent.com/122754787/216845143-43f7855d-4c28-4edf-9520-2316d43317c5.png)
  </details>

***

### Q4: What is the most purchased item on the menu and how many times was it purchased by all customers?
<details>

````sql
SELECT product_name, COUNT(s.product_id) AS most_purchased FROM sales s
JOIN menu m ON s.product_id = m.product_id
GROUP BY product_name, s.product_id
ORDER BY most_purchased DESC
````

- Use COUNT of product_id 
- GROUP BY the product_id and product_name to show all the products and the amount of times they were each purchased
- ORDER BY most_purchased DESC to get the highest to lowest

Answer:
<br>
![q4asnwer](https://user-images.githubusercontent.com/122754787/216847985-9d0cecf3-dd8f-4075-a61e-67e3c38c5db3.png)
</details>

***

### Q5: Which item was the most popular for each customer?
<details>

````sql
	with t1 as (
	SELECT customer_id, product_name, COUNT(m.product_id) AS order_count,
	DENSE_RANK() OVER(PARTITION BY s.customer_id 
					ORDER BY COUNT(s.customer_id) DESC) AS rank 
	FROM sales s
	JOIN menu m ON s.product_id = m.product_id
	GROUP BY customer_id, product_name
	ORDER BY customer_id, rank DESC
)

SELECT customer_id, product_name, order_count FROM t1
WHERE rank = 1
````
- Use t1 as a temp table and use DENSE_RANK so the tied numbers are not skipped over
- Fetch data WHERE the rank = 1 to show the most popular item for each customer

Answer:
<br>
![q5answer](https://user-images.githubusercontent.com/122754787/216849655-b547c715-ca65-4186-b32b-9e4b7e4bbea1.png)
</details>
