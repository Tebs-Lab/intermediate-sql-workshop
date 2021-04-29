# Exercise: Using And Evaluating Indexes

All of the following instructions are with respect to the `SalesOrderHeader` table in the AdventureWorks database.

1. Use this query to "randomly" sample 50 orderdates from the table, and return them as a comma separated list:

```sql
select string_agg(concat('''', to_char(orderdate, 'yyyy-mm-dd'), ''''), ',') from (
	select * from sales.salesorderheader where orderdate is not null order by rowguid limit 50
) as limited_dates;
```

> Note, this weird four quotes in a row: `''''` is called "escaping" and it actually translates into one single quote (as a string) in postgres. Different languages handle escape characters differently.

1. Use the results of this to construct an `in` clause for a `select` statement such that this statement would `select * from SalesOrderHeader where OrderDate in (___every unique date___);` The purpose of this is to give us a robust stress test for "point to point" queries.


2. Determine the earliest and latest data as well, and use this information to construct a range query that includes three sub-ranges within this overall range. The purpose of this query is to give us a robust stress test for range queries. This statement would be in the format:

```sql
select * from SalesOrderHeader where OrderDate between ____ and _____ 
or orderdate between _____ and _____
or orderdate between _____ and _____;
```

## Explain Some Queries!

1. Execute each of these queries on the SalesOrderHeader table, and note their execution times. Also, examine the query plan for each of the queries.
2. Add a hash-based index to the OrderDate column.
3. Repeat step 1.
4. Delete the hash based index and add a B-Tree based index.
5. Repeat step 1.

Finally, discuss your results and findings. How do the different index strategies impact the performance of each query?