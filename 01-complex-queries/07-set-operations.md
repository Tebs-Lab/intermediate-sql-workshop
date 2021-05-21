# Set Operations

* Set operations can be used to combine the results of separate queries.
* They are similar to joins in a way, except that instead of adding columns to the ultimate result, they add rows to the ultimate result. 
* The three set operations are
    * union (all results from both queries)
    * intersect (only results that appear in both queries)
    * except (all results from the first query minus results that appear in the second query)
* By default set operations remove duplicate rows from the resulting table.
    * The `all` keyword can be added to any of the set operations to prevent that deduplication from happening.
* Result sets must have the same number of columns.
* Lets look at some examples:

```sql
select 1
union 
select 2;
```
* Note: as basic as you can get...

```sql
select 1
union 
select 1;

-- vs

select 1
union all
select 1
```
* Note: deduplication in action, or not.

```sql
select 1
union all
select 1, 2;
```
* Note: produces an error.

* Okay, lets do some real-er examples:

```sql
select 
	concat(p.firstname, ' ', p.middlename, ' ', p.lastname)
from humanresources.employee e
join person.person p on p.businessentityid=e.businessentityid
union 
	select 
		concat(p.firstname, ' ', p.middlename, ' ', p.lastname)
	from sales.customer c
	join person.person p on p.businessentityid=c.personid;
```
* Note: all full names of all employees and customers
* Change the union to intersect and except.
    * Intersect is names of people who are both customers AND employees.
    * Except is all the names of employees NOT shared by any customers
    * adding all removes duplicate records.