# walmart

### Exploratory Data Analyst
```sql
select * from walmart

select count(*) from walmart

select payment_method, count(*) as total
from walmart
group by payment_method
order by 2 desc

select count(distinct(branch)) from walmart

select max(quantity) from walmart

select min(quantity) from walmart
```

## Business Problem

### Q1: Find different payment methods, number of transactions, and quantity sold by payment method

```sql
select payment_method, count(*) as no_transaction, sum(quantity) as no_qty_sold
from walmart
group by 1
order by 2 desc
```

### Q2: Identify the highest-rated category in each branch --> Display the branch, category, and avg rating

```sql
with cte as
(
select branch, category, avg(rating) as avg_rating,
rank() over(partition by branch order by avg(rating) desc) as ranked
from walmart
group by branch, category
)
select * from cte
where ranked = 1
```

### Q3: Identify the busiest day for each branch based on the number of transactions
```sql
with cte as
(
select branch, dayname(date) as day_name, count(*) as no_transactions,
rank() over(partition by branch order by count(*) desc) as ranked
from walmart
group by branch, dayname(date)
)
select branch, day_name, no_transactions from cte
where ranked = 1
```

### Q4: Calculate the total quantity of items sold per payment method
```sql
select payment_method, sum(quantity) as no_qty_sold
from walmart
group by 1
order by 2 desc
```

### Q4
