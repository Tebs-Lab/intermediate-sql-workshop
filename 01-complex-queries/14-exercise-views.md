# Exercise: Views

Creating these views may require clever use of any number of the techniques we've learned about. There may also be more than one way to create a view that fits the needs described. 

1. Create a view for the HR department such that using `select * from YOUR_VIEW` a member of that department can know for each employee:
    * Their full name
    * Their email address
    * Their title
    * Which department they belong to currently
    * Their managers full name and email address

> Hint/Note: The two most difficult parts of this query are getting the **current** department for each employee and figuring out who the manager is. Getting the manager is especially awkward. The information you need is encoded by the `organizationnode` column. Take a look at the values and see if you can figure out the structure. My solution uses a regular expression to match employees with managers based on this column.

2. Create a view for the purchasing department such that using `select * from YOUR_VIEW` a member of that department can know about each vendor:
    * The company name
    * The point of contact's email address
    * How many products they sell to AdventureWorks
    * Whether or not they sell any bike pedals to AdventureWorks

> Note/Hint: There may be multiple points of contact for each vendor, include them all. To determine whether or not a company sells pedals consider using the `bool_or` aggregate function ([https://www.postgresql.org/docs/13/functions-aggregate.html#FUNCTIONS-AGGREGATE-TABLE](https://www.postgresql.org/docs/13/functions-aggregate.html#FUNCTIONS-AGGREGATE-TABLE)). 

3. Bonus points: Choose your own view... In your teams decide on a view that could be useful for either the sales or production department. Try to make it challenging by: 
    * including information from multiple tables.
    * including information that must be computed with an aggregate
    * using a case statement and or regular expression