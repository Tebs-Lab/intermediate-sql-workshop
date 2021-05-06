# Exercise: Set Operations

1. You've been charged with creating a query that powers a dropdown menu that can be used to select the name of every product individually as well as an option for "all products". Use a union statement to create such a query.

2. Something bad happened and now you need to send an email to all of your customers as well as all of your employees, but not to any of your vendors. Create a query using `union` and `except` to select all the email addresses that belong to employees and customers as well as explicitly removing any email addresses that might belong to someone who is both a customer and a vendor or an employee and a vendor.

3. Create a query using `intersect` to identify all the lastnames that are shared by an employee and a customer that is an individual (rather than a store).