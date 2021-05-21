# Common Table Expressions

* Common table expressions are a way to use queries as though they are tables.
    * The queries are not actually "materialized" into a table and so they take up very little space on disk.
    * However, they are executed and the result of the query is held in memory until the rest of the expression is completed.
* CTE's make some things that would be impossible possible, and they make other things that might otherwise be very hard much easier.
* There are two syntaxes... lets see them:

```sql
select name from (
  select * from production.product
) as temp_table;

-- and equivilantly:

with temp_table as (
    select * from production.product
)

select name from temp_table;
```

* These are useful for many reasons, for example:
    * Improve readability with deep joins and especially recursive joins.
    * Allow for multiple levels of aggregation.
    * Aggregation over union/intersect/except operations.
    * (and others...)

* Lets see an example of a useful CTE...

```sql
with monthly_sales_revenue as (
	select 
	  sum(totaldue) as gross_sales,
	  extract(year from orderdate) as year,
	  extract(month from orderdate) as month
	from sales.salesorderheader
	group by 
	  extract(year from orderdate),
	  extract(month from orderdate)
)

select 
  month, 
  avg(gross_sales) as average_monthly_sale
from monthly_sales_revenue
group by 
  month
order by month;
```

* We can take it even further by joining these two tables to easily compare the monthly average to any particular years sales for that same month...

```sql
with monthly_sales_revenue as (
	select 
	  sum(totaldue) as gross_sales,
	  extract(year from orderdate) as year,
	  extract(month from orderdate) as month
	from sales.salesorderheader
	group by 
	  extract(year from orderdate),
	  extract(month from orderdate)
),

avg_monthly_sales as (
	select 
	  month, 
	  avg(gross_sales) as average_monthly_sale
	from monthly_sales_revenue
	group by 
	  month
	order by month
)

select 
  msr.year, 
  msr.month, 
  msr.gross_sales, 
  ams.average_monthly_sale
from monthly_sales_revenue msr
join avg_monthly_sales ams on ams.month=msr.month;
```