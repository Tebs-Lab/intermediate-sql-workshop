# Exercise: Views

Creating these views may require clever use of any number of the techniques we've learned about. There may also be more than one way to create a view that fits the needs described. 

1. Create a view for the HR department such that using `select * from YOUR_VIEW` a member of that department can know for each employee:
    * Their full name
    * Their email address
    * Their title
    * Which department they belong to currently
    * Their managers full name and email address

2. Create a view for the purchasing department such that using `select * from YOUR_VIEW` a member of that department can know about each vendor:
    * The company name
    * The point of contact's email address
    * How many products they sell to AdventureWorks
    * Whether or not the vendor is located in the US
    * Whether or not they sell any bike frames to AdventureWorks

3. Choose your own view... In your teams decide on a view that could be useful for either the sales or production department. Try to make it challenging by: 
    * including information from multiple tables.
    * including information that must be computed with an aggregate
    * using a case statement and or regular expression

4. Bonus points: see what the query planner says about selecting * from your view... could you improve the efficiency of this query somehow?