# Exercise: Case/When

1. Use a case statement to select the name of each product in the `product` table as well as a column named "customer_segment" that contains:
    1. "Luxury" if the list price is more than $1500
    2. "HENRY" if the list price is between $1500 and $500
    3. "Standard" if the list price is less than $500

2. AdventureWorks is initiating a mandatory vacation policy for employees who have accumulated too many vacation hours. Write a query that uses case/when and does the following:
    1. Ignores inactive employees.
    2. Includes the employee's name, role, number of accumulated vacation hours and a column which reads:
        * "Mandatory vacation" if the vacation hours are > 40
        * "gentle reminder" if the vacation hours are between 39 and 30
        * "No action" otherwise.

3. Use an aggregate and a case statement to select all `purchasing.productvendors` and a column that indicates for each entry if that productvendor has a higher `averageleadtime` than the global average across all products. That column should read "More Lead Time" if so, and "Standard Lead Time" if not.