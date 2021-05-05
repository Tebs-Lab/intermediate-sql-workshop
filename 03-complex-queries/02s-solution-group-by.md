# Exercise: Group By

Use `group by` and `having` to create the following queries / answer the following questions:

1. How many SalesOrders were placed each year, and what was the average `totaldue` of them (again by year)?

```sql
select 
  extract(year from orderdate) as year,
  count(1) as sales, 
  avg(totaldue) as average_sale_value
from sales.salesorderheader
group by extract(year from orderdate);
```

2. How many SalesOrders were placed each month of each year, and what was the average `totaldue` of them (again by month and year)?

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
  extract(year from orderdate),
  extract(month from orderdate);
```

3. Join the `salesperson` table to the `Salesorderheader` table, then calculate the average `totaldue` by year, month, and salesperson along with the salespersons name. (you will need to join more tables to get the name... checkout `person.person`)

```sql
select
  concat(p.firstname, ' ', p.middlename, ' ', p.lastname),
  extract(year from orderdate) as year,
  extract(month from orderdate) as month,
  count(1) as sales, 
  avg(totaldue) as average_sale_value
from sales.salesorderheader soh
  join sales.salesperson sp on sp.businessentityid=soh.salespersonid
  join person.person p on p.businessentityid=sp.businessentityid
group by
  extract(year from orderdate),
  extract(month from orderdate),
  p.businessentityid
order by
  concat(p.firstname, ' ', p.middlename, ' ', p.lastname),
  extract(year from orderdate),
  extract(month from orderdate);
```

4. Make a separate query (or use a subquery) to determine the global average `totaldue` and then limit the above list to only records with a monthly per-salesperson average that is above the global average.

```sql
select avg(totaldue) from sales.salesorderheader; -- => 3915.9951093564277769

-- You can use that number directly in the having clause, or as a subquery. A subquery is more robust for something such as automated reporting purposes, since it will always be up to date. For a one time exploration of the data, making two queries is perfectly fine.

select
  concat(p.firstname, ' ', p.middlename, ' ', p.lastname),
  extract(year from orderdate) as year,
  extract(month from orderdate) as month,
  count(1) as sales, 
  avg(totaldue) as average_sale_value
from sales.salesorderheader soh
  join sales.salesperson sp on sp.businessentityid=soh.salespersonid
  join person.person p on p.businessentityid=sp.businessentityid
group by
  extract(year from orderdate),
  extract(month from orderdate),
  p.businessentityid
having avg(totaldue) > (select avg(totaldue) from sales.salesorderheader)
order by
  concat(p.firstname, ' ', p.middlename, ' ', p.lastname),
  extract(year from orderdate),
  extract(month from orderdate);
```

5. Make a query that determines how many unique products each vendor supplies to AdventureWorks.

```sql
select 
  v.name,
  count(p.productid) as product_count
from purchasing.vendor v
  join purchasing.productvendor pv on pv.businessentityid=v.businessentityid
  join production.product p on p.productid=pv.productid
group by
  v.businessentityid
order by
  count(p.productid) desc;
```

6. Make a query that determines the average, minimum, and maximum `listprice` of products by `class`. 

```sql
select 
  class,
  max(listprice),
  min(listprice),
  avg(listprice)
from production.product
group by class
order by class;
```

7. Update the previous query to only include products that have a `listprice` of more than $200 in the aggregates.

```sql
select 
  class,
  max(listprice),
  min(listprice),
  avg(listprice)
from production.product
where listprice > 200 -- note: we use where NOT having for this. Filter before grouping!
group by class
order by class;
```