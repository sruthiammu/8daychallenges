Before you start writing your SQL queries however - you might want to investigate the data, you may want to do something with some of those null values and data types in the customer_orders and runner_orders tables!

# customer_orders -CLEANING

```sql
select * from customer_orders
```

![p1](https://user-images.githubusercontent.com/67575229/208702651-23dc2368-8415-4c86-8dc1-b5b19a718b81.png)

### changing null to zero(0)
### here in `exclusions` and `extras` have null values and spaces
### removing null and space values by `0`

```sql
create table customer_order_cln as
select order_id,customer_id,pizza_id,
case
	when exclusions= 'null' then '0'
    when exclusions='' then '0'
    else exclusions
    end as exclusions,
case
	when extras is null then '0'
    when extras='null' then '0'
    when extras='' then '0'
    else extras
    end as extras,
order_time from customer_orders;
```

![p2](https://user-images.githubusercontent.com/67575229/208718733-4e7e2c41-df69-4b74-9ef5-60cfbd0ed9f4.png)

# runner_orders -CLEANING

```sql
select * from runner_orders;
```
![p3](https://user-images.githubusercontent.com/67575229/208719136-9eb38a3e-0219-4f77-8bc9-7995d66d12b1.png)

### here `pickup_time`,`distance`,`duration` and `cancellation` columns having null values, empty columns and worng data types
### all changed to null values,removed km from distance minute from duration and put no cancellation in cancellation

```sql

create table runner_orders_cln as
select order_id,runner_id,
case
	when pickup_time='null' then null
    when pickup_time='' then null
    else pickup_time
    end as pickup_time,
case
	when distance like 'null' then null
    when distance like '%km' then trim('km' from distance)
    else distance
    end as distance,
case
	when duration like '%minutes' then null
    when duration like '%mins' then null
    when duration like 'null' then null
    when duration like '%minute' then null
    else duration
    end as duration,
case
	when cancellation='' then 'No Cancellation'
    when cancellation is null then 'No Cancellation'
    when cancellation ='null' then 'No Cancellation'
    else cancellation
    end as cancellation
from runner_orders

```


![p4](https://user-images.githubusercontent.com/67575229/208722191-0dcfa26d-85b4-40a5-9fe2-e278dfd125f6.png)

### changing data types of columns in runner_orders_cln table

```sql
alter table runner_orders_cln
modify pickup_time timestamp,
modify distance float,
modify duration int
```

### click here [solution](https://github.com/sruthiammu/8daychallenges/commit/23a3832269d553f9a174ae3337ffc258b8ba2d1a)  for A.pizza matrics
