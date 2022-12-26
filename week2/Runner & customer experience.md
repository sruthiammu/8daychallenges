# 1) How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)

```sql
with tem as (select
case when week(registration_date)=0 then 'week1'
	when week(registration_date)=1 then 'week2'
    when week(registration_date)=2 then 'week3'
    else 'week4'
    end as registration_date,runner_id
from runners)

select registration_date,count(runner_id) no_of_runners
from tem
group by registration_date
```
![p21](https://user-images.githubusercontent.com/67575229/209537108-fb9b5ad0-00ee-41ad-a4e6-8cef41be5be7.png)

# 2) What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?

```sql
select r.runner_id,timestampdiff(minute,c.order_time,r.pickup_time) runner_pic_time,
round(avg(timestampdiff(minute,c.order_time,r.pickup_time)),2) avg_pickup_time
from customer_order_cln c join runner_orders_cln r on c.order_id=r.order_id
where r.cancellation ='No Cancellation'
group by r.runner_id
```
![p22](https://user-images.githubusercontent.com/67575229/209544519-a90a569a-f434-4045-9168-ae36843f6fd3.png)

# 3) Is there any relationship between the number of pizzas and how long the order takes to prepare?
```sql
with tem as(select count(c.order_id) pizza_count,timestampdiff(minute,c.order_time,r.pickup_time) preperation_tim_min
from customer_order_cln c join runner_orders_cln r on c.order_id=r.order_id
where r.cancellation ='No Cancellation'
group by c.order_id)

select pizza_count,round(avg(preperation_tim_min)) avg_time_preperation
from tem
group by pizza_count
```
![p23](https://user-images.githubusercontent.com/67575229/209545954-e2dc88a4-cb5b-4c51-8ab5-1194664ac92e.png)

# 4)What was the average distance travelled for each customer?

```sql
select c.customer_id, round(avg(r.distance),2) avg_distance from
customer_order_cln c join runner_orders_cln r on c.order_id=r.order_id
where r.distance is not null
group by c.customer_id;
```
![p24](https://user-images.githubusercontent.com/67575229/209561697-d80755e9-1f15-4d21-a0f1-2511db1563a0.png)

# 5) What was the difference between the longest and shortest delivery times for all orders?
```sql
select max(duration) max_duration,min(duration) min_duration,(max(duration)-min(duration)) as duration_diff
 from  runner_orders_cln 
where duration is not null
```
![p25](https://user-images.githubusercontent.com/67575229/209563047-9ebc0f4c-3e15-4aa3-ab32-6c3119bfbba0.png)

# 6) What was the average speed for each runner for each delivery and do you notice any trend for these values?





