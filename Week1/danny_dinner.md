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

## 7. Which item was purchased just before the customer became a member? 
```sql
with sub1 as (
select s.customer_id,m.product_name,s.order_date,mm.join_date, 
dense_rank() over(partition by s.customer_id order by s.order_date) as purchase 
from sales s inner join menu m 
on s.product_id=m.product_id
inner join members mm
on s.customer_id=mm.customer_id
where s.order_date <=mm.join_date)

select customer_id,product_name,order_date,join_date from sub1 where purchase=1;
```


![sql17](https://user-images.githubusercontent.com/67575229/204267806-82bda1bb-44a8-4c2d-bd43-ad85e2b26b3d.png)

## 8.What is the total items and amount spent for each member before they became a member?
```sql
select
s.customer_id,count(s.customer_id) as count ,sum(m.price) as total
 from sales s inner join menu m on s.product_id=m.product_id
 inner join members mm on s.customer_id=mm.customer_id
 where s.order_date< mm.join_date
 group by s.customer_id
 order by s.customer_id
```
![sql18](https://user-images.githubusercontent.com/67575229/204276185-83378e5b-2de6-499f-a072-f32ea9109d9e.png)

## 9.If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

```sql
select customer_id,sum(points) points from (
select s.customer_id,
case 
when m.product_name='sushi' then price*20
when m.product_name !='sushi' then price*10
end as points
from sales s inner join menu m on s.product_id=m.product_id) as t1
group by s.customer_id
```

![sql19](https://user-images.githubusercontent.com/67575229/204280922-7b677a4e-2ab4-43d3-9f21-97987833a24a.png)

## 10.In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

```sql

```

## 11. join all 

```sql
select s.customer_id,s.order_date,m.product_name,m.price,(case when s.order_date<mm.join_date then 'N'
else  'Y' end) members  from sales s inner join menu m 
on s.product_id=m.product_id
inner join
members mm
on s.customer_id=mm.customer_id
order by s.customer_id,order_date
```

![sql111](https://user-images.githubusercontent.com/67575229/204303557-0ff797bd-c9b6-41db-b38e-4a6b952997d1.png)

## 12. ranking all

```sql
select *,(case when memberss='Y' then (dense_rank() over(partition by customer_id,memberss order by order_date ))
else 'null' end) ranking from (select s.customer_id,s.order_date,m.product_name,m.price,(case when s.order_date<mm.join_date then 'N'
else  'Y' end) memberss   from sales s inner join menu m 
on s.product_id=m.product_id
inner join
members mm
on s.customer_id=mm.customer_id
order by s.customer_id,order_date) as t1
```
![sql112](https://user-images.githubusercontent.com/67575229/204306320-b6519ca8-8659-4de2-899c-67cc6d288791.png)
