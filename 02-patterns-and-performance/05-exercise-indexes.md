# Exercise: Using And Evaluating Indexes

All of the following instructions are with respect to the `SalesOrderHeader` table in the AdventureWorks database.

1. Use a query to determine all the unique OrderDates.
    1. Use the results of this query to construct an `in` clause for a `select` statement such that this statement would `select * from SalesOrderHeader where OrderDate in (___every unique date___);`
    2. The purpose of this is to give us a robust stress test for "point to point" queries.
    3. Determine the earliest and latest data as well, and use this information to construct a range query that would include every row.
    4. This statement would be in the format `select * from SalesOrderHeader where OrderDate between ____ and _____;`

2. Execute each of these queries on the SalesOrderHeader table, and note their execution times. Also, examine the query plan for each of the queries.
3. Add a hash-based index to the OrderDate column.
4. Repeat step 2.
5. Delete the hash based index and add a B-Tree based index.
6. Repeat step 2.

Finally, discuss your results and findings. How do the different index strategies impact the performance of each query?