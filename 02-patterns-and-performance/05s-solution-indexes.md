# Exercise: Using And Evaluating Indexes

All of the following instructions are with respect to the `SalesOrderHeader` table in the AdventureWorks database.

```sql
select string_agg(concat('''', to_char(orderdate, 'yyyy-mm-dd'), ''''), ',') from (
	select * from sales.salesorderheader where orderdate is not null order by rowguid limit 50
) as limited_dates;
```

> Note, this weird four quotes in a row: `''''` is called "escaping" and it actually translates into one single quote (as a string) in postgres. Different languages handle escape characters differently.

1. Use the results of this to construct an `in` clause for a `select` statement such that this statement would `select * from SalesOrderHeader where OrderDate in (___every unique date___);`

```sql
select * from sales.salesorderheader where orderdate in ('2014-05-01','2011-08-01','2013-12-15','2013-01-26','2014-04-03','2013-07-18','2013-06-12','2013-09-14','2011-12-03','2014-01-02','2014-04-16','2014-03-30','2011-12-06','2013-10-30','2012-07-08','2013-09-22','2013-10-30','2014-05-08','2014-03-01','2011-07-21','2011-09-10','2014-04-11','2014-01-29','2014-06-07','2014-03-18','2012-05-17','2014-01-16','2012-11-10','2013-05-28','2014-05-11','2013-12-20','2014-04-18','2011-08-13','2014-05-06','2012-12-31','2014-03-22','2014-05-08','2012-04-10','2013-08-30','2013-10-29','2014-05-06','2012-09-14','2012-07-09','2014-01-24','2013-11-25','2014-04-06','2013-08-30','2012-07-05','2013-06-18','2013-10-19');
```

The purpose of this is to give us a robust stress test for "point to point" queries.

2. Determine the earliest and latest data as well, and use this information to construct a range query that includes three sub-ranges within this overall range. The purpose of this query is to give us a robust stress test for range queries. This statement would be in the format:

```sql
select * from SalesOrderHeader where OrderDate between ____ and _____ 
or orderdate between _____ and _____
or orderdate between _____ and _____;
```


```sql
select min(orderdate), max(orderdate) from sales.salesorderheader;

         min         |         max         
---------------------+---------------------
 2011-05-31 00:00:00 | 2014-06-30 00:00:00

# So one such query:

select * from sales.salesorderheader where 
orderdate between '2011-05-31' and '2011-06-30'
or orderdate between '2013-01-31' and '2013-06-30'
or orderdate between '2014-05-31' and '2011-09-30';
```

## Explain Some Queries!

1. Execute each of these queries on the SalesOrderHeader table, and note their execution times. Also, examine the query plan for each of the queries.

**The two queries**

```sql
explain select * from sales.salesorderheader where orderdate in ('2014-05-01','2011-08-01','2013-12-15','2013-01-26','2014-04-03','2013-07-18','2013-06-12','2013-09-14','2011-12-03','2014-01-02','2014-04-16','2014-03-30','2011-12-06','2013-10-30','2012-07-08','2013-09-22','2013-10-30','2014-05-08','2014-03-01','2011-07-21','2011-09-10','2014-04-11','2014-01-29','2014-06-07','2014-03-18','2012-05-17','2014-01-16','2012-11-10','2013-05-28','2014-05-11','2013-12-20','2014-04-18','2011-08-13','2014-05-06','2012-12-31','2014-03-22','2014-05-08','2012-04-10','2013-08-30','2013-10-29','2014-05-06','2012-09-14','2012-07-09','2014-01-24','2013-11-25','2014-04-06','2013-08-30','2012-07-05','2013-06-18','2013-10-19');

explain select * from sales.salesorderheader where 
orderdate between '2011-05-31' and '2011-06-30'
or orderdate between '2013-01-31' and '2013-06-30'
or orderdate between '2014-05-31' and '2011-09-30';
```

**Results**

```
# Point:
 Seq Scan on salesorderheader  (cost=0.00..3003.21 rows=3193 width=431)
   Filter: (orderdate = ANY ('{...ALL THE DATES...}'::timestamp without time zone[]))

# Range:
 Seq Scan on salesorderheader  (cost=0.00..1508.62 rows=2701 width=431)
   Filter: (((orderdate >= '2011-05-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2011-06-30 00:00:00'::timestamp without time zone)) OR ((orderdate >= '2013-01-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2013-06-30 00:00:00'::timestamp without time zone)) OR ((orderdate >= '2014-05-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2011-09-30 00:00:00'::timestamp without time zone)))
(2 rows)
```

2. Add a hash-based index to the OrderDate column.

```sql
create index soh_hash_orderdate on sales.salesorderheader using hash (orderdate);
```

3. Repeat step 1.

```
# point
Bitmap Heap Scan on salesorderheader  (cost=196.80..1150.29 rows=3193 width=431)
   Recheck Cond: (orderdate = ANY ('{... ALL THE DATES...}'::timestamp without time zone[]))
   ->  Bitmap Index Scan on soh_hash_orderdate  (cost=0.00..196.00 rows=3193 width=0)
         Index Cond: (orderdate = ANY ('{... ALL THE DATES...}'::timestamp without time zone[]))

# Range
 Seq Scan on salesorderheader  (cost=0.00..1508.62 rows=2701 width=431)
   Filter: (((orderdate >= '2011-05-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2011-06-30 00:00:00'::timestamp without time zone)) OR ((orderdate >= '2013-01-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2013-06-30 00:00:00'::timestamp without time zone)) OR ((orderdate >= '2014-05-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2011-09-30 00:00:00'::timestamp without time zone)))
```

4. Delete the hash based index and add a B-Tree based index.

```sql
drop index sales.soh_hash_orderdate;
create index soh_btree_orderdate on sales.salesorderheader using btree (orderdate);
```

5. Repeat step 1.

```
# point
 Index Scan using soh_hash_orderdate on salesorderheader  (cost=0.29..262.18 rows=3193 width=431)
   Index Cond: (orderdate = ANY ('{...dates...}'::timestamp without time zone[]))

# range
 Bitmap Heap Scan on salesorderheader  (cost=50.18..840.40 rows=2703 width=431)
   Recheck Cond: (((orderdate >= '2011-05-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2011-06-30 00:00:00'::timestamp without time zone)) OR ((orderdate >= '2013-01-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2013-06-30 00:00:00'::timestamp without time zone)) OR ((orderdate >= '2014-05-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2011-09-30 00:00:00'::timestamp without time zone)))
   ->  BitmapOr  (cost=50.18..50.18 rows=2729 width=0)
         ->  Bitmap Index Scan on soh_hash_orderdate  (cost=0.00..6.05 rows=176 width=0)
               Index Cond: ((orderdate >= '2011-05-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2011-06-30 00:00:00'::timestamp without time zone))
         ->  Bitmap Index Scan on soh_hash_orderdate  (cost=0.00..36.25 rows=2396 width=0)
               Index Cond: ((orderdate >= '2013-01-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2013-06-30 00:00:00'::timestamp without time zone))
         ->  Bitmap Index Scan on soh_hash_orderdate  (cost=0.00..5.86 rows=157 width=0)
               Index Cond: ((orderdate >= '2014-05-31 00:00:00'::timestamp without time zone) AND (orderdate <= '2011-09-30 00:00:00'::timestamp without time zone))
```

Finally, discuss your results and findings. How do the different index strategies impact the performance of each query?

Although the names and applications of different strategies are certainly interesting, we really care about performance...

#### No Indexes

Point: cost=0.00..3003.21 rows=3193 width=431
Range: cost=0.00..1508.62 rows=2701 width=431

#### Hash Index

Point: cost=196.80..1150.29 rows=3193 width=431 (nearly 3x speed up)
Range: cost=0.00..1508.62 rows=2701 width=431 (it's the same query plan)

#### BTree

Point: cost=0.29..262.18 rows=3193 width=431 (EVEN BETTER!)
Range: cost=50.18..840.40 rows=2703 width=431 (3x better than either)

## Discussion:

The btree turned out to be better than the hash even for the point lookups in our case! This won't always be the case, but it's a good reason to test things out and really look carefully at use cases.

There might be many reasons for this little surprise, including the storage format / cache lookup behavior of the indexes, the fact that order date is going to have a lot of collisions, and more.

For fun, I looked up the index on the primary key (which are unique, so there will be fewer collisions). It was a btree, so I added a hash index as well...

```sql
create index soh_id_hash on sales.salesorderheader using hash (salesorderid);

explain select * from sales.salesorderheader where salesorderid = 43662;
                                      QUERY PLAN                                      
--------------------------------------------------------------------------------------
 Index Scan using soh_id_hash on salesorderheader  (cost=0.00..8.02 rows=1 width=431)
   Index Cond: (salesorderid = 43662)
```

Lo and behold, this time my hash index was preferred over the btree. If I drop the hash index...

```sql
explain select * from sales.salesorderheader where salesorderid = 43662;
                                                 QUERY PLAN                                                  
-------------------------------------------------------------------------------------------------------------
 Index Scan using "PK_SalesOrderHeader_SalesOrderID" on salesorderheader  (cost=0.29..8.30 rows=1 width=431)
   Index Cond: (salesorderid = 43662)
```

Note: it's BARELY faster. 8.3 vs 8.02. The fact of the matter is that btrees are really good, and even though they are O(log n) they generally have performance that is competitive with O(1) hash indexes even on point lookups. 