# Answers

## 1.What is the total amount each customer spent at the restaurant?

```sql
select s.customer_id,sum(m.price) from sales s inner join menu m on s.product_id=m.product_id
group by s.customer_id;
```

![sql11](https://user-images.githubusercontent.com/67575229/204203487-b43cc18c-d040-41b6-9ae4-01f6365b75ea.png)

