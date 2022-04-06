# Review of Basic SQL

In this section we'll review the basics of SQL. Since this ought to be review, ask the students a lot of questions and let them answer and guide the review session.

## Big Picture Stuff

* **What is a database?**
    * It's a combination of software and data storage to persist and query large amounts of data.

* **What's the difference between a "database" and a "database server"?**
    * A database is a collection of data that, at least in theory, should all be logically associated.
    * A database server is a collection of at least one database.
    * Database servers also provide mechanisms to connect remotely, access the individual databases, and manage access.

* **What is SQL?**
    * SQL, or Standard Query Language, is a standard specification for accessing relational databases.
    * Many specific database technologies implement and expand upon the SQL standard. 
        * Postgres, MySQL, Oracle...

* **How have you used SQL in the past?**
    * There are a billion right answers. Use this as a chance to understand the students background and experience.

## A Few Specific SQL Queries

* Show a few typical queries, ask the students what each does in abstract terms.
    * Try not to let them get away with circular definitions...
        * "what exactly does it mean to join?"
        * "what is group by?"
        * "How does having work?"

```sql
select * from production.Product;

select 
  ProductID, 
  Name,
  ListPrice 
from production.Product;

select 
  Class,
  avg(ListPrice)
from production.Product
group by Class;

select 
  psc.name,
  avg(p.ListPrice)
from production.Product p
join production.ProductSubcategory psc on psc.ProductSubcategoryID=p.ProductSubcategoryID
group by psc.name
having count(1) > 10;
```

* Suggest that these are some example queries that the students can run when they restore their copy of the AdventureWorks database.
* Now, break into teams and complete the first exercise.