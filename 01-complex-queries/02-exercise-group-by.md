# Exercise: Group By

Use `group by` and `having` to create the following queries / answer the following questions:

1. How many SalesOrders were placed each year, and what was the average `totaldue` of them (again by year)?
2. How many SalesOrders were placed each month of each year, and what was the average `totaldue` of them (again by month and year)?
3. Join the `salesperson` table to the `Salesorderheader` table, then calculate the average `totaldue` by year, month, and salesperson along with the salespersons name.
4. Make a separate query (or use a subquery) to determine the global average `totaldue` and then limit the above list to only records with a monthly per-salesperson average that is above the global average.
5. Make a query that determines how many unique products each vendor supplies to AdventureWorks.
6. Make a query that determines the average, minimum, and maximum `listprice` of products by `class`. 
7. Update the previous query to only include products that have a `listprice` of more than $200 in the aggregates.