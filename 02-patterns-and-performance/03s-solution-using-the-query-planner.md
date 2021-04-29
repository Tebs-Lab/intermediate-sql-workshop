# Exercise Solution: Using The Query Planner

For each of the following described queries involving the Adventure Works database:

1. Write the query.
2. Invoke the query planner.
3. Discuss what the planner tells you, and what it might mean for the performance of the query.

## The Queries:

1. Join the Store table with the Customer table using the customer.storeid/store.businessentityid field.

```sql
explain select
  c.customerid, s.name
from sales.customer c
join sales.store s on s.businessentityid=c.storeid;
                              QUERY PLAN                               
-----------------------------------------------------------------------
 Hash Join  (cost=59.77..476.02 rows=19820 width=25)
   Hash Cond: (c.storeid = s.businessentityid)
   ->  Seq Scan on customer c  (cost=0.00..364.20 rows=19820 width=8)
   ->  Hash  (cost=51.01..51.01 rows=701 width=25)
         ->  Seq Scan on store s  (cost=0.00..51.01 rows=701 width=25)
```

2. Use `select`, `group by`, and `count` to determine how many Products there are in each Color.

```sql
explain select 
    color,
    count(1)
from production.product
group by color;

                           QUERY PLAN                           
----------------------------------------------------------------
 HashAggregate  (cost=17.56..17.65 rows=9 width=14)
   Group Key: color
   ->  Seq Scan on product  (cost=0.00..15.04 rows=504 width=6)
(3 rows)
```

* Note: Hash aggregate means we hash on the value of color.
    * during the scan, each color gets hashed and then associated with the matching color value so the count can be incremented.

3. Join the `SalesOrderHeader` table with both the `SalesPerson` and `Customer` tables (meaning you'll need 2 joins), selecting every possible column.

```sql
explain select 
    * 
from sales.salesorderheader soh
join sales.customer c on c.customerid = soh.customerid
join sales.salesperson sp on sp.businessentityid = soh.salespersonid;

                                       QUERY PLAN                                        
-----------------------------------------------------------------------------------------
 Hash Join  (cost=613.33..1817.68 rows=31465 width=663)
   Hash Cond: (soh.salespersonid = sp.businessentityid)
   ->  Hash Join  (cost=611.95..1731.22 rows=31465 width=471)
         Hash Cond: (soh.customerid = c.customerid)
         ->  Seq Scan on salesorderheader soh  (cost=0.00..1036.65 rows=31465 width=431)
         ->  Hash  (cost=364.20..364.20 rows=19820 width=40)
               ->  Seq Scan on customer c  (cost=0.00..364.20 rows=19820 width=40)
   ->  Hash  (cost=1.17..1.17 rows=17 width=192)
         ->  Seq Scan on salesperson sp  (cost=0.00..1.17 rows=17 width=192)
```

4. Explore for yourself! Write a query that you think is interesting, and look at the query plan. These are some potentially interesting factors...
    * Indexes on the fields.
    * where clauses
    * Subqueries
    * Regex
    * group by / having clauses