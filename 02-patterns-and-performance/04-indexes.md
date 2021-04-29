# Indexing

In this section we're going to look under the hood of the two most popular index types — btrees and hash tables — and explain how they're used.

## The Basics of Indexing

* [Use these slides!](https://slides.com/tebba-von-mathenstein/learned-indexing/#/3)
    * Stop at the "learned indexes" segment.
    * You can also do this on the whiteboard if you prefer.

* Key takaways:
    * Indexes are an external structure, they take up additional memory.
    * Indexing speeds up most queries, but slows down insertion time.
    * Hash indexes are better for "point" based queries, btrees are better for range queries.
    * btrees are the default index type for almost all psql situations.

## Creating Indexes and Their Impact

Take a look at this self join that returns sales orders that have been made on the same date...

```sql
explain select * from sales.salesorderheader soh1
join sales.salesorderheader soh2 on soh1.orderdate=soh2.orderdate;
                                        QUERY PLAN                                        
------------------------------------------------------------------------------------------
 Hash Join  (cost=3181.96..36554.62 rows=1626391 width=862)
   Hash Cond: (soh1.orderdate = soh2.orderdate)
   ->  Seq Scan on salesorderheader soh1  (cost=0.00..1036.65 rows=31465 width=431)
   ->  Hash  (cost=1036.65..1036.65 rows=31465 width=431)
         ->  Seq Scan on salesorderheader soh2  (cost=0.00..1036.65 rows=31465 width=431)
(5 rows)
```

* Huge cost! 
    * We can see the planner wanted to use a hash... so

```sql
create index soh_orderdate on sales.salesorderheader using hash (orderdate);
```

```sql
explain select * from sales.salesorderheader soh1
join sales.salesorderheader soh2 on soh1.orderdate=soh2.orderdate;
                                             QUERY PLAN                                             
----------------------------------------------------------------------------------------------------
 Nested Loop  (cost=0.00..28720.70 rows=1626391 width=862)
   ->  Seq Scan on salesorderheader soh1  (cost=0.00..1036.65 rows=31465 width=431)
   ->  Index Scan using soh_orderdate on salesorderheader soh2  (cost=0.00..0.60 rows=28 width=431)
         Index Cond: (orderdate = soh1.orderdate)
(4 rows)
```

* The hash step was skipped, instead the index was used, saved us 1036 units of cost, plus saved a second table scan, another 1036 units of work. Amazing!!

* But... because we used a hash index, this won't help us at all for range queries. Lets see what I mean:

```sql
explain select * from sales.salesorderheader where orderdate between '01-01-2010' and '01-01-2012';

---
Seq Scan on salesorderheader  (cost=0.00..1193.97 rows=1700 width=431)
   Filter: ((orderdate >= '2010-01-01 00:00:00'::timestamp without time zone) AND (orderdate <= '2012-01-01 00:00:00'::timestamp without time zone))
```

* Lets improve this with a btree!

```sql
create index soh_btree_orderdate on sales.salesorderheader using btree (orderdate);

explain select * from sales.salesorderheader where orderdate between '01-01-2010' and '01-01-2012';


Index Scan using soh_btree_orderdate on salesorderheader  (cost=0.29..88.23 rows=1702 width=431)
   Index Cond: ((orderdate >= '2010-01-01 00:00:00'::timestamp without time zone) AND (orderdate <= '2012-01-01 00:00:00'::timestamp without time zone))
```

* Reduced the cost from 1193 to 88.23!!
