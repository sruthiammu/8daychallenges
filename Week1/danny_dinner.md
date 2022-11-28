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
with subtable as (
select s.customer_id,m.product_name,count(m.product_id)as count,DENSE_RANK() OVER(partition by s.customer_id 
order by count(m.product_id) desc) as rn from sales s inner join menu m on s.product_id=m.product_id 
group by s.customer_id,m.product_name)

select customer_id,product_name,count from subtable where rn=1;
```
![sql15](https://user-images.githubusercontent.com/67575229/204222159-982f9b81-9ba7-4981-a00b-8505c456cb32.png)

## 6.Which item was purchased first by the customer after they became a member?
```sql
with purchase as (select mm.customer_id,m.product_name,mm.join_date,s.order_date,
dense_rank() over(partition by mm.customer_id order by s.order_date asc) as first_purchase from members  mm join sales s 
using(customer_id)
join menu m on s.product_id=m.product_id
where s.order_date>= mm.join_date)

select customer_id,product_name from purchase where first_purchase=1
```

![sql16](https://user-images.githubusercontent.com/67575229/204245955-4d887ec4-2c38-4a0f-a297-beb7d405c7cc.png)


