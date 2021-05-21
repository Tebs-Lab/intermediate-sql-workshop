# Exercise: Using The Query Planner

For each of the following described queries involving the Adventure Works database:

1. Write the query.
2. Invoke the query planner.
3. Discuss what the planner tells you, and what it might mean for the performance of the query.

## The Queries:

1. Join the Store table with the Customer table using the storeid field.
2. Use `select`, `group by`, and `count` to determine how many Products there are in each Color.
3. Join the `SalesOrderHeader` table with both the `SalesPerson` and `Customer` tables (meaning you'll need 2 joins), selecting every possible column.
4. `Select * from` one of the views you created in the exercises from the previous section. 
5. Explore for yourself! Write a query that you think is interesting, and look at the query plan. These are some potentially interesting factors...
    * Indexes on the fields.
    * where clauses
    * Subqueries
    * Regex
    * group by / having clauses


