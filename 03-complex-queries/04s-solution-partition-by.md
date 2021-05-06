# Exercise: Partition By


1. Select the date and `totaldue` from every record from `salesorderheader` and use a window function to additionally select the global average `totaldue`.


```sql
select 
  orderdate,
  totaldue,
  avg(totaldue) over (partition by (1))
from sales.salesorderheader;  
```

2. Update the previous query to now include both the global and monthly average `totaldue` in each row.

```sql
select 
  orderdate,
  totaldue,
  avg(totaldue) over (partition by 1) as global_avg,
  avg(totaldue) over (partition by extract(month from orderdate)) as month_avg
from sales.salesorderheader;
```

3. Update the previous query to now include the month/year average `totaldue` in each row.

```sql
select 
  orderdate,
  totaldue,
  avg(totaldue) over (partition by 1) as global_avg,
  avg(totaldue) over (partition by extract(month from orderdate)) as month_avg,
  avg(totaldue) over (partition by 
					    extract(month from orderdate),
					    extract(year from orderdate)
					 ) as month_year_avg
from sales.salesorderheader;
```

4. Write a query that selects the listprice of each product as well as the average list price by class, productline, and class/productline combined.

```sql
select 
  class, 
  productline,
  listprice,
  avg(listprice) over(partition by productline) as productline_avg,
  avg(listprice) over(partition by class) as class_avg,
  avg(listprice) over(partition by class, productline) as productline_class_avg
from production.product
order by 
  productline,
  class;
```
