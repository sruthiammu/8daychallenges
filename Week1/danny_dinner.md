# Answers

## 1.What is the total amount each customer spent at the restaurant?

```sql
select s.customer_id,sum(m.price) from sales s inner join menu m on s.product_id=m.product_id
group by s.customer_id;
```

![sql11](https://user-images.githubusercontent.com/67575229/204203487-b43cc18c-d040-41b6-9ae4-01f6365b75ea.png)

## 2.How many days has each customer visited the restaurant?

```sql
select customer_id, count(order_date) as no_of_days from sales group by customer_id;
```

![sql12](https://user-images.githubusercontent.com/67575229/204204790-5d5ddc53-23a5-4ca3-b630-6339440b7a09.png)

## 3.What was the first item from the menu purchased by each customer?

```sql
select distinct(s.customer_id), s.product_id,s.order_date,m.product_name 
from sales s left join menu m on s.product_id=m.product_id 
where s.order_date = any (select min(order_date) from sales);
```
![sql13](https://user-images.githubusercontent.com/67575229/204206798-d3b64f3d-b776-429e-8d88-e2045a1fd7c2.png)

## 4.What is the most purchased item on the menu and how many times was it purchased by all customers?

```sql
select m.product_name,count(m.product_name) as count
 from sales s inner join menu m on s.product_id=m.product_id 
 group by m.product_name
 order by  count desc
 limit 1;
```
![sql14](https://user-images.githubusercontent.com/67575229/204208462-08d261e7-df14-4102-9b64-5bf1989c9150.png)

## 5.Which item was the most popular for each customer?
```sql

```

