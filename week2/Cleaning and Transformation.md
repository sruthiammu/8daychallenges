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
create view customer_order_clean as
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
