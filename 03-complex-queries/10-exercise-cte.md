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

3. This query using `intersect` will identify all the lastnames that are shared by an employee and a customer that is an individual (rather than a store).

```sql
select 
  p.lastname
from humanresources.employee e
join person.person p on p.businessentityid=e.businessentityid
intersect
	select 
	  p.lastname
	from sales.customer c
	join person.person p on c.personid=p.businessentityid;
```

Update the query to use `intersect all` and an aggregate to count how many people share that name. 

SOLUTION:

```sql
select lastname, count(1) from 
  (select 
    p.lastname
  from humanresources.employee e
  join person.person p on p.businessentityid=e.businessentityid
  intersect all
    select 
      p.lastname
    from sales.customer c
    join person.person p on c.personid=p.businessentityid) as shared_names
group by lastname;

-- or

with shared_names as (select 
  p.lastname
from humanresources.employee e
join person.person p on p.businessentityid=e.businessentityid
intersect all
	select 
	  p.lastname
	from sales.customer c
	join person.person p on c.personid=p.businessentityid)
	
select lastname, count(1) from shared_names
group by lastname;

```

