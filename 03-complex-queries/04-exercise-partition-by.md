# Exercise: Partition By

You may wish to reuse your answers from the group by section, and expand them to use partition by, as many of these queries are very similar.

1. How many SalesOrders were placed each month of each year, and what was the average `totaldue` of them (again by month and year)? Use partition by to include the average `totaldue` by month aggregated over all the years on record, to easily determine if sales are "above average" for each given month.
3. Join the `salesperson` table to the `Salesorderheader` table, then calculate the average `totaldue` by year, month, and salesperson along with the salespersons name. Use partition by to include the best performing salespersons average for that particular month/year in a column.
4. Make a query that determines how many unique products each vendor supplies to AdventureWorks. Use partition by to include the average number of products per vendor across all vendors, to easily compare each vendor to this global average.
6. Make a query that determines the average, minimum, and maximum `listprice` of products by `class`, use partition by to include the global maximum, minimum, and average across all classes. 
7. Update the previous query to only include products that have a `listprice` of more than $200 in the aggregates, keep the partition by the same.