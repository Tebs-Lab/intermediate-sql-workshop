# Exercise: Common Table Expressions



1. Join any three tables and select every row. Now, using the CTE syntax select a subset of the rows from the join. (this is to practice the syntax in a simple way)

2. The following query shows how many SalesOrders were placed each month of each year, and what was the average `totaldue` of them (again by month and year). 

Use a CTE to compute the global monthly average (e.g. the average totaldue for each month across all years) and join it to the result of this query so that we can compare each yearly/monthly average and say e.g. "this is an above average January"

```sql
select 
  extract(year from orderdate) as year,
  extract(month from orderdate) as month,
  count(1) as sales, 
  avg(totaldue) as average_sale_value
from sales.salesorderheader
group by 
  extract(year from orderdate),
  extract(month from orderdate)
order by 
  extract(month from orderdate),
  extract(year from orderdate);
```

2. Create a query that uses an aggregate to determine each sales person's largest and smallest sale on a monthly basis. Then, use that query as a table with the CTE syntax, and select from it using another aggregate  to compute the average of each sales person's smallest and largest sales. (this "double aggregate" is a major benefit of CTEs)

3. Combine a CTU with Union! Make a query that involves any numerical field, then using three selects and three unions `sum` that numerical value using...
    1. All values.
    2. Odd values.
    3. Even values.

Union these three selects into a single query with 3 rows. 

