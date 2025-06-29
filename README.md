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

### Q5: Determine the average, minimum, and maximum rating of categories for each city
```sql
select category, city, avg(rating), min(rating), max(rating)
from walmart
group by 1, 2
order by 3,4,5 desc
```
### Q6: Calculate the total profit for each category
```sql
SELECT 
    category,
    SUM(unit_price * quantity * profit_margin) AS total_profit
FROM walmart
GROUP BY category
ORDER BY total_profit DESC;
```

### Q7: Determine the most common payment method for each branch
```sql
with cte as
(
select branch, payment_method, count(*) as no_transaction,
rank() over (partition by branch order by count(*) desc) as ranked
from walmart
group by branch, payment_method
)
select branch, payment_method, no_transaction
from cte
where ranked=1
```

### Q8: Categorize sales into Morning, Afternoon, and Evening shifts (coba lagi)
```sql
SELECT
    branch,
    CASE 
        WHEN HOUR(TIME(time)) < 12 THEN 'Morning'
        WHEN HOUR(TIME(time)) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS shift,
    COUNT(*) AS num_invoices
FROM walmart
GROUP BY branch, shift
ORDER BY branch, num_invoices DESC;
```

### Q9: Identify the 5 branches with the highest revenue decrease ratio from last year to current year (e.g., 2022 to 2023)

