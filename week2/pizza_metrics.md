# 1) How many pizzas were ordered?

```sql
select count(order_id) as pizza_order_count from customer_order_cln
```
![p4](https://user-images.githubusercontent.com/67575229/208725577-f96ff0b5-f8bd-4cdf-b27d-45795305496e.png)

# 2) How many unique customer orders were made?

```sql
select count(distinct(order_id)) as unique_order_count from customer_order_cln
```
![p6](https://user-images.githubusercontent.com/67575229/208726527-7eaeac8a-c89e-4bca-be6b-71297f2344b9.png)

# 3) How many successful orders were delivered by each runner?
```sql
select runner_id,count(order_id) as successful_order
from runner_orders_cln
where distance <>0
group by runner_id
```
![p7](https://user-images.githubusercontent.com/67575229/208727571-f041d935-223f-445e-89ae-1cb7e77eece3.png)

# 4) How many of each type of pizza was delivered?

```sql
select p.pizza_name,count(c.pizza_id) as delivered_pizza_count from customer_order_cln c join runner_orders_cln r
on c.order_id=r.order_id
 join pizza_names p on c.pizza_id=p.pizza_id
 where r.distance<>0
group by c.pizza_id
```
![p8](https://user-images.githubusercontent.com/67575229/208730357-9c392e48-f6ad-4936-a675-8c0ec14acd9a.png)

# 5) #How many Vegetarian and Meatlovers were ordered by each customer?

```sql
select c.customer_id,p.pizza_name,count(c.customer_id) order_count 
from customer_order_cln c join pizza_names p
on c.pizza_id=p.pizza_id
group by c.customer_id,p.pizza_name
order by customer_id
```
![p9](https://user-images.githubusercontent.com/67575229/208844685-eda433e1-5060-4680-9f21-764922da0ea0.png)


# 6) What was the maximum number of pizzas delivered in a single order?

```sql
with tem as (select c.order_id,count(c.pizza_id) pizza_count_perorder from customer_order_cln c join runner_orders_cln r  on c.order_id=r.order_id
where r.distance <>0
group by order_id)
select max(pizza_count_perorder) max_count from tem 
```

![pp10](https://user-images.githubusercontent.com/67575229/208848996-2cc83429-7ffa-4cbe-bc4e-5c98cd7f5eb8.png)

![p10](https://user-images.githubusercontent.com/67575229/208849038-9c4134e2-cf08-4dac-a524-24eb78353342.png)

