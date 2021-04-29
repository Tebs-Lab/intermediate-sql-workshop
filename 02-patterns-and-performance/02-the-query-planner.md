# The Query Plan

Whenever a new query is made, the DBMS has to decide how to best execute that query. That is, it has to decide how to fetch, search, and return the requested data from disk. Query planners are complex systems that have a number of algorithms and strategies at their disposal.

## Query Plan Examples:

Consider the process of joining two tables based on a foreign key relationship, e.g.

```sql
select * from production.Product p
join production.ProductSubcategory sp
  on sp.productsubcategoryid = p.productsubcategoryid;
```

* **What SHOULD postgres do here?**
    * based on the query, Product must be scanned, there's no way to avoid that since we're very potentially asking for every record in that table.
    * But, the join is happening with an index, so for all the records in Product, we take the join condition's value (`productsubcategoryid`) and look it up in the PK index for `ProductSubcategory`
    * Double But! Since the join is on a not null field (the primary key) the query planner already knows it will have to look at every row in both tables...
        * so it will use the index to quickly match records from the left table to the right table, but it will perform a scan on the left table, hash the productsubcategory value for each, then scan the right table and use the hash iteratively to perform the join.
    * Triple BUT!
        * It *could* use the index to perform this join, but because of the above and because the index is in another memory location and would have to be loaded, the query planner decided it would be faster to essentially REBUILD the hash index than look it up from disk.

* Lets see what actually happens:

```sql
explain select * from production.Product p
join production.ProductSubcategory sp
  on sp.productsubcategoryid = p.productsubcategoryid;

   QUERY PLAN                                     
-----------------------------------------------------------------------------------
 Hash Join  (cost=1.83..18.28 rows=504 width=203)
   Hash Cond: (p.productsubcategoryid = sp.productsubcategoryid)
   ->  Seq Scan on product p  (cost=0.00..15.04 rows=504 width=139)
   ->  Hash  (cost=1.37..1.37 rows=37 width=64)
         ->  Seq Scan on productsubcategory sp  (cost=0.00..1.37 rows=37 width=64)
```

* indeed, a hash join is used. We can see both seq scans.
* Some notes:
    * cost=1.83..18.28 From the docs about this cost: "The most critical part of the display is the estimated statement execution cost, which is the planner's guess at how long it will take to run the statement (measured in cost units that are arbitrary, but conventionally mean disk page fetches). Actually two numbers are shown: the start-up cost before the first row can be returned, and the total cost to return all the rows." [https://www.postgresql.org/docs/13/sql-explain.html](https://www.postgresql.org/docs/13/sql-explain.html)
    * Rows and Width: rows are the number of rows impacted by the seq scan. Width is the size of each row. 
        * Width can be reduced by selecting fewer rows.

* Generally seq scans are best avoided when possible, but in some cases (like this one) they are better than an index scan or index lookup since the data is stored sequentally and every row must be used...

* Lets look at another example

```sql
explain select p.name, sp.name from production.product p
join production.productsubcategory sp on position(sp.name in p.name) > 0;
```

```
QUERY PLAN                                     
-----------------------------------------------------------------------------------
 Nested Loop  (cost=0.00..342.84 rows=6216 width=51)
   Join Filter: ("position"((p.name)::text, (sp.name)::text) > 0)
   ->  Seq Scan on product p  (cost=0.00..15.04 rows=504 width=19)
   ->  Materialize  (cost=0.00..1.56 rows=37 width=32)
         ->  Seq Scan on productsubcategory sp  (cost=0.00..1.37 rows=37 width=32)
(5 rows)
```

* There's no index for these names (and one might not help b/c we're joining on the position function)
    * It's just a nested loop of sequential scans.
    * Materialize means a temporary table is stored in memory (RAM rather than disk)
        * This happens to increase the query speed by storing an intermediate result, in this case just the names being selected from of the product table.

* Planner will also tell you if its using an index:

```
explain select * from sales.salesorderheader where salesorderid=10;;
                                                 QUERY PLAN                                                  
-------------------------------------------------------------------------------------------------------------
 Index Scan using "PK_SalesOrderHeader_SalesOrderID" on salesorderheader  (cost=0.29..8.30 rows=1 width=431)
   Index Cond: (salesorderid = 10)
(2 rows)
```

* Vs...

```
AdventureWorks=# explain select * from sales.salesorderheader where orderdate='2011-10-14';
                                 QUERY PLAN                                 
----------------------------------------------------------------------------
 Seq Scan on salesorderheader  (cost=0.00..1115.31 rows=21 width=431)
   Filter: (orderdate = '2011-10-14 00:00:00'::timestamp without time zone)
(2 rows)
```

* Cost of 8.3 vs 1115.31 (WOW!)